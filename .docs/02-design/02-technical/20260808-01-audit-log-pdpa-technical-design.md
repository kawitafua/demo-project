# Technical Design: Audit Log และ PDPA Consent

> สร้างเมื่อ 2026-08-08
> อ้างอิง: [[../../01-requirements/01-spec/20260807-03-audit-log-pdpa-compliance|20260807-03-audit-log-pdpa-compliance]] (requirement ต้นทาง)

## หมายเหตุสำคัญเรื่อง tech stack

โปรเจกต์นี้ยังไม่ได้เลือก tech stack (ภาษา/เฟรมเวิร์ก/ฐานข้อมูล) เอกสารนี้จึงออกแบบแบบ **tech-agnostic** คือระบุ data model, architecture, และ security control ในระดับตรรกะ (logical) โดยไม่ผูกกับเทคโนโลยีเฉพาะเจาะจง ส่วนการเลือกเทคโนโลยีจริง (เช่น log service ตัวไหน, ฐานข้อมูลอะไร) ให้รอจนกว่าโปรเจกต์จะเลือก stack โดยรวมก่อน (ดูหัวข้อ "ประเด็นที่ยังไม่ชัดเจน" ท้ายเอกสาร)

## 1. Architecture Overview

ออกแบบเป็น **Audit Log Service แยกออกจากระบบหลัก** ตาม business rule ที่ยืนยันไว้แล้ว (เก็บในระบบ log แยกต่างหาก ไม่ใช่ตารางในฐานข้อมูลหลัก)

```
[Ordering Service] ──┐
[Payment Service]  ──┤
[Membership Service]─┼──> AppendLogEntry() ──> [Audit Log Store] (แยกจาก DB หลัก)
[Auth/Dashboard]   ──┤
[Export Feature]   ──┘

[Retention Job] ──(scheduled, ทุกวัน)──> ตรวจสอบ entry ที่เกิน 90 วัน ──> ลบ/anonymize

[Log Access API] <── query (เฉพาะ role: เจ้าของร้าน, ผู้ดูแลระบบ)
```

หลักการสำคัญ:
- ทุกบริการ (ordering, payment, membership, auth/dashboard, export) เรียก `AppendLogEntry()` เป็นจุดเดียวในการเขียน log — ห้ามเขียน log ตรงเข้าตารางแบบกระจัดกระจาย เพื่อให้บังคับ schema/masking rule ได้จากที่เดียว
- Audit Log Store เป็นคนละที่จัดเก็บ (physically/logically separate) จากฐานข้อมูลหลักของระบบ เพื่อลดผลกระทบซึ่งกันและกันหากอีกฝั่งถูกโจมตีหรือข้อมูลเสียหาย
- การเขียน log ไม่ควรบล็อกการทำงานหลัก (เช่น การชำระเงินต้องไม่รอ log เขียนสำเร็จก่อนจึงตอบลูกค้า) — แนะนำให้ทำแบบ asynchronous/best-effort แต่ต้องมี retry หรือ dead-letter queue เพื่อไม่ให้ log หายเงียบๆ

## 2. Data Model

### 2.1 Audit Log Entry

| ฟิลด์ | ประเภท | คำอธิบาย |
|---|---|---|
| `id` | string/UUID | รหัส entry ไม่ซ้ำ |
| `occurred_at` | timestamp (UTC) | เวลาที่เกิดเหตุการณ์ |
| `actor_type` | enum: `customer` \| `staff` \| `system` | ผู้ทำเป็นใคร |
| `actor_id` | string | รหัสผู้ทำ (customer id / staff id / ชื่อ job สำหรับ system) |
| `action` | enum (ดูตาราง 2.2) | ประเภทเหตุการณ์ |
| `target_entity_type` | string | เช่น `order`, `payment`, `member`, `user_role` |
| `target_entity_id` | string | รหัสของสิ่งที่ถูกกระทำ |
| `before_state` | JSON (nullable) | ค่าก่อนแก้ไข — เฉพาะ action ที่เป็นการแก้ไข/ลบ |
| `after_state` | JSON (nullable) | ค่าหลังแก้ไข — เฉพาะ action ที่เป็นการแก้ไข/ลบ |
| `ip_address` | string (nullable) | เก็บเมื่อจำเป็น (เช่น login, export) |
| `device_info` | string (nullable) | เก็บเมื่อจำเป็น |
| `metadata` | JSON (nullable) | ข้อมูลเสริมอื่นที่ไม่เข้าฟิลด์ข้างต้น |

**กฎ masking**: `before_state`/`after_state`/`metadata` ต้องผ่านตัว mask ก่อนเขียนเสมอ — ห้ามมีเลขบัตรเครดิตเต็ม (เก็บได้แค่ last 4 หลัก ถ้าจำเป็นต้องอ้างอิง) และห้ามมีรหัสผ่านในทุกรูปแบบ (รวม hash) การ mask ต้องเกิดที่จุดเดียว (ใน `AppendLogEntry()`) ไม่ใช่หน้าที่ของผู้เรียกแต่ละที่ เพื่อไม่ให้พลาด

### 2.2 Action → Target mapping (จาก business rules ของ requirement spec)

| Action | target_entity_type ตัวอย่าง | หมายเหตุ |
|---|---|---|
| `order.created` | `order` | จากระบบสั่งกาแฟ QR ([[../../01-requirements/01-spec/20260807-01-table-qr-ordering|20260807-01-table-qr-ordering]]) |
| `payment.succeeded` / `payment.failed` | `payment` | ต้อง mask หมายเลขบัตร/wallet |
| `member.updated` / `member.deleted` | `member` | ต้องมี before/after state |
| `dashboard.login` | `staff_session` | จาก [[../../01-requirements/01-spec/20260807-02-dashboard-sales|20260807-02-dashboard-sales]] |
| `data.exported` | `export_job` | บันทึกว่า export อะไร ช่วงวันที่ไหน |
| `user_role.changed` | `user_role` | ต้องมี before/after state (สิทธิ์เดิม → สิทธิ์ใหม่) |

### 2.3 Member Consent (PDPA)

เพิ่มฟิลด์ในข้อมูลสมาชิก (ไม่ต้องมีตารางแยก เพราะ scope นี้เป็น consent แบบง่าย):

| ฟิลด์ | ประเภท | คำอธิบาย |
|---|---|---|
| `privacy_consent_given` | boolean | ต้องเป็น `true` ก่อนสร้างบัญชีสมาชิกได้ |
| `privacy_consent_at` | timestamp | เวลาที่กดยินยอม |
| `privacy_notice_version` | string | เวอร์ชันของข้อความ privacy notice ที่ลูกค้าเห็นตอนกดยินยอม (เผื่อแก้ข้อความในอนาคต จะรู้ว่าใครยินยอมเวอร์ชันไหน) |

การเปลี่ยนแปลงค่า consent (ถ้ามีในอนาคต) ต้องถูกบันทึกเป็น audit log entry ประเภท `member.updated` ด้วยเช่นกัน

## 3. Access Control

- สิทธิ์ query audit log จำกัดเฉพาะ role `owner` และ `system_admin` (คนละ role กับ `sales_viewer` ที่ใช้เข้าถึง dashboard ยอดขายใน [[../../01-requirements/01-spec/20260807-02-dashboard-sales|20260807-02-dashboard-sales]])
- Audit Log Store ต้องไม่ให้สิทธิ์ `UPDATE`/`DELETE` แก่ตัวแอปพลิเคชันหลักหรือ role ทั่วไปใดๆ (append-only ในทางปฏิบัติ) — มีเพียง Retention Job ที่รันแบบ scheduled/system credential เท่านั้นที่ลบ/anonymize ข้อมูลที่เกิน 90 วันได้
- การกระทำใดๆ ต่อ Audit Log Store เอง (เช่น การรัน Retention Job) ก็ควรมี record เหตุการณ์นั้นไว้ (เช่นสรุปจำนวน entry ที่ถูกลบ/anonymize ในรอบนั้น) เพื่อให้ตรวจสอบย้อนหลังได้ว่า retention job ทำงานจริงและไม่มีใครแทรกแซง

## 4. Retention Job

- รันเป็นรอบ (แนะนำ: รายวัน) ตรวจหา entry ที่ `occurred_at` เกิน 90 วัน
- ค่าเริ่มต้นที่แนะนำ: **ลบถาวร (hard delete)** entry ที่เกินกำหนด เพราะ scope นี้ไม่ต้องการเก็บ log ระยะยาวเพื่อวิเคราะห์ (ถ้าต้องเก็บสถิติระยะยาว ควร aggregate เป็นข้อมูลที่ไม่มี PID ก่อนแล้วเก็บแยก ไม่ใช่หน้าที่ของ audit log)
- ต้องมี log/สรุปผลการรัน retention job ไว้ตรวจสอบได้ (ดูข้อ 3)

## 5. Interfaces (Logical)

ไม่ผูกกับ REST/gRPC/message queue ที่เจาะจง — ระบุเป็น operation ระดับตรรกะที่ทุก service ต้องมี:

- `AppendLogEntry(entry)` — เขียน log entry ใหม่ (เรียกจากทุก service ที่เกิด action ในตาราง 2.2), ทำ masking ภายในฟังก์ชันนี้เสมอ
- `QueryLogEntries(filters, requestor)` — อ่าน log entry, ต้องตรวจสอบสิทธิ์ผู้เรียกเป็น `owner`/`system_admin` ก่อนคืนผลเสมอ
- `PurgeExpiredEntries()` — เรียกจาก Retention Job เท่านั้น (system credential), ไม่เปิดให้ service หรือ user อื่นเรียก

## 6. ประเด็นที่ยังไม่ชัดเจน (รอตัดสินใจในรอบถัดไป)

- **เทคโนโลยีที่ใช้ทำ Audit Log Store จริง** (เช่น self-hosted log aggregator, managed cloud logging service, หรือฐานข้อมูลแยก instance) — รอจนกว่าโปรเจกต์จะเลือก tech stack โดยรวมก่อน
- **กลไก async delivery ที่ใช้เขียน log** (message queue, event bus, หรือ synchronous call แบบ fire-and-forget) — ขึ้นกับ stack ที่เลือก
- **UI/เครื่องมือสำหรับ owner/admin ในการ query audit log** — ยังไม่อยู่ใน scope requirement (ดู [[../../01-requirements/01-spec/20260807-03-audit-log-pdpa-compliance|20260807-03-audit-log-pdpa-compliance]] หัวข้อ "ไม่ทำ") จึงยังไม่ออกแบบ interface ระดับ UI ในเอกสารนี้

## เอกสารที่เกี่ยวข้อง

- [[../../01-requirements/01-spec/20260807-03-audit-log-pdpa-compliance|20260807-03-audit-log-pdpa-compliance]]
- [[../../01-requirements/01-spec/20260807-01-table-qr-ordering|20260807-01-table-qr-ordering]]
- [[../../01-requirements/01-spec/20260807-02-dashboard-sales|20260807-02-dashboard-sales]]
- [[../../03-testing/01-test-plan/index|01-test-plan]]
