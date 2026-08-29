# หน้า Login เข้าสู่ระบบ (เจ้าของร้าน/พนักงาน — desktop)

> สร้างเมื่อ 2026-08-29
> อ้างอิง Design System: [[../20260829-02-design-system|20260829-02-design-system]] (หัวข้อ 2.1-2.2 Design Tokens, 3.1 Components ร่วม, 4.4-4.5 UX Guidelines)
> อ้างอิง Requirement: [[../../../01-requirements/01-spec/20260807-02-dashboard-sales|20260807-02-dashboard-sales]], [[../../../01-requirements/01-spec/20260807-03-audit-log-pdpa-compliance|20260807-03-audit-log-pdpa-compliance]]
> อ้างอิง Feature: `AUDIT-01`, `DASH-05` (ดู [[../../../01-requirements/02-plan/20260829-01-feature-list|20260829-01-feature-list]]) / Journey step: [[../20260829-01-user-journeys#Journey 3: เจ้าของร้าน/พนักงานดู Dashboard ยอดขาย|Journey 3: เจ้าของร้าน/พนักงานดู Dashboard ยอดขาย]] ขั้นตอนที่ 1-2

> **หมายเหตุสำคัญ — สมมติฐานชั่วคราว**: ไม่มี requirement spec ฉบับใดในโปรเจกต์ระบุรายละเอียดของหน้า login โดยตรง (username/password หรือช่องทางอื่น เช่น OTP/SSO, ปุ่ม "ลืมรหัสผ่าน", ใครเป็นผู้สมัครบัญชีให้พนักงาน) มีเพียง [[../20260829-01-user-journeys#Journey 3: เจ้าของร้าน/พนักงานดู Dashboard ยอดขาย|Journey 3]] ที่กล่าวถึง "Login เข้าสู่ระบบ" เป็นขั้นตอนหนึ่งก่อนตรวจสอบสิทธิ์ role (`DASH-05`) เท่านั้น และ `DASH-05` เองก็ระบุประเด็นเปิดว่า "รายละเอียดการกำหนดสิทธิ์/บทบาทผู้ใช้ยังไม่ระบุ" — wireframe ในไฟล์นี้จึงเป็น **แนวทางขั้นต่ำสุดที่ user ยืนยันให้ทำไปก่อน (username/password ธรรมดา)** ไม่ใช่ requirement ที่ยืนยันแล้ว หากมีการสร้าง requirement spec เฉพาะสำหรับหน้า login ในอนาคต ต้องกลับมาทบทวนไฟล์นี้ใหม่

หน้าจอนี้มี 2 state หลัก (Login Form ปกติ, Login Form เมื่อกรอกข้อมูลผิด) และ 1 ส่วนอธิบาย handoff หลัง login สำเร็จไปยังหน้า Dashboard ที่มีอยู่แล้วใน [[sales-dashboard|หน้า Dashboard ยอดขาย]]

---

## Wireframe — State 1: Login Form (default)

```
+--------------------------------------------------------------------------------+
|                                                                                  |
|                                                                                  |
|                          [Wordmark: my-coffee-store]                            |
|                                                                                  |
|                    เข้าสู่ระบบสำหรับเจ้าของร้าน/พนักงาน                                |
|                    --text-h1--                                                  |
|                                                                                  |
|              +------------------------------------------------+                |
|              |  ชื่อผู้ใช้ (Username)                              |                |
|              |  --text-caption-- (label)                        |                |
|              |  [__________________________________]            |                |
|              |                                                  |                |
|              |  รหัสผ่าน (Password)                               |                |
|              |  --text-caption-- (label)                        |                |
|              |  [__________________________________] [👁]        |                |
|              |                                                  |                |
|              |              [ เข้าสู่ระบบ ]                        |  <-- Primary Button (เต็มความกว้างฟอร์ม)
|              |                                                  |                |
|              |                  ลืมรหัสผ่าน?                       |  <-- Ghost/Text link (placeholder)
|              |                  --text-caption--                |                |
|              +------------------------------------------------+                |
|                                                                                  |
+--------------------------------------------------------------------------------+
```

- ฟอร์มอยู่กึ่งกลางจอ พื้นหลัง `--color-surface` ขอบ `--color-border` (`--radius-md`) ลอยอยู่เหนือพื้นหลัง `--color-background`
- ปุ่ม "เข้าสู่ระบบ" เป็น Primary Button ตาม Design System หัวข้อ 3.1 (พื้น `--color-brand-primary` ตัวอักษรขาว) มี state disabled จนกว่าจะกรอกทั้ง 2 ช่อง
- ลิงก์ "ลืมรหัสผ่าน?" เป็น **placeholder เท่านั้น** — ยังไม่มี flow/requirement รองรับปลายทางเมื่อกด (ดูหมายเหตุสมมติฐานชั่วคราวด้านบน)
- ไม่มี field อื่นนอกจาก username/password เพราะไม่มีข้อมูลยืนยันว่าต้องรองรับช่องทางอื่น (OTP/SSO) ในสเปคปัจจุบัน

## Wireframe — State 2: Login Form (error — ข้อมูลไม่ถูกต้อง)

```
+--------------------------------------------------------------------------------+
|                                                                                  |
|                          [Wordmark: my-coffee-store]                            |
|                                                                                  |
|                    เข้าสู่ระบบสำหรับเจ้าของร้าน/พนักงาน                                |
|                    --text-h1--                                                  |
|                                                                                  |
|              +------------------------------------------------+                |
|              |  [!] ชื่อผู้ใช้หรือรหัสผ่านไม่ถูกต้อง กรุณาลองใหม่อีกครั้ง |  <-- Alert (error)
|              |  --text-caption--, สี --color-error--                          |
|              |                                                  |                |
|              |  ชื่อผู้ใช้ (Username)                              |                |
|              |  [__________________________________]            |                |
|              |                                                  |                |
|              |  รหัสผ่าน (Password)                               |                |
|              |  [__________________________________] [👁]        |                |
|              |  ขอบช่องกรอกเปลี่ยนเป็น --color-error-- ชั่วคราว    |                |
|              |                                                  |                |
|              |              [ เข้าสู่ระบบ ]                        |                |
|              |                  ลืมรหัสผ่าน?                       |                |
|              +------------------------------------------------+                |
|                                                                                  |
+--------------------------------------------------------------------------------+
```

- ใช้ Alert component ตาม Design System หัวข้อ 3.1 (icon สถานะ + ข้อความ + สีพื้นอ่อนของ `--color-error`) วางเหนือฟอร์ม ไม่ใช่ toast ลอย เพราะเป็นข้อผิดพลาดที่บล็อกการทำงานต่อ (ผู้ใช้ต้องแก้ก่อนไปต่อ)
- ข้อความ error ใช้ภาษาที่เข้าใจง่าย ไม่ระบุว่า "username ผิด" หรือ "password ผิด" แยกกัน (ตามหลักความปลอดภัยทั่วไป และสอดคล้อง Design System หัวข้อ 4.6 เรื่อง tone of voice ที่เป็นมิตร ไม่ใช่ error code ทางเทคนิค)
- ไม่สื่อสถานะด้วยสีเพียงอย่างเดียว — มี icon `[!]` และข้อความกำกับเสมอ ตาม Design System หัวข้อ 4.5 Accessibility

## หลัง Login สำเร็จ (Handoff ไปยังหน้า Dashboard)

เมื่อกรอกข้อมูลถูกต้องและกด "เข้าสู่ระบบ" สำเร็จ ระบบจะ:

1. บันทึก audit log เหตุการณ์ `dashboard.login` (`AUDIT-01`) — ไม่มี UI แสดงผลขั้นตอนนี้ เกิดขึ้นฝั่งระบบ
2. ตรวจสอบสิทธิ์ role ของผู้ใช้ (`DASH-05`) แล้วนำผู้ใช้ไปยังหนึ่งในสอง state ที่มีอยู่แล้วในไฟล์ [[sales-dashboard|หน้า Dashboard ยอดขาย]]:
   - มีสิทธิ์ (owner/staff ที่ได้รับสิทธิ์) → [[sales-dashboard#Wireframe — State 1: Authorized View (เจ้าของร้าน/พนักงานที่มีสิทธิ์)|State 1: Authorized View]]
   - ไม่มีสิทธิ์ → [[sales-dashboard#Wireframe — State 2: Access Denied View (ผู้ใช้ไม่มีสิทธิ์)|State 2: Access Denied View]]

หน้าจอนี้จึงไม่สร้าง state "เข้าสู่ระบบสำเร็จ" ซ้ำ เพราะผลลัพธ์ทั้งสองแบบถูกออกแบบไว้แล้วในไฟล์ `sales-dashboard.md`

---

## Components ที่ใช้

| Component | อ้างอิง Design System | หมายเหตุ |
|---|---|---|
| Text Input (username/password) | [[../20260829-02-design-system#2.1 Colors\|2.1 Colors]], [[../20260829-02-design-system#2.2 Typography\|2.2 Typography]] | ไม่มี component "Form Input" ระบุแยกใน Design System ปัจจุบัน (ยังไม่ใช่ touchpoint ที่ระบุไว้ตอนสร้าง) จึงยึด token สี/ตัวอักษรทั่วไปเป็นหลัก — ควรเพิ่ม component "Text Input" อย่างเป็นทางการใน Design System เมื่อมี touchpoint แบบฟอร์มเพิ่มขึ้น |
| Button (Primary) | [[../20260829-02-design-system#3.1 Components ร่วม (ใช้ทุกหน้าจอ)\|3.1 Components ร่วม]] | ปุ่ม "เข้าสู่ระบบ" — มี state default/disabled/loading ตามสเปค component |
| Button (Ghost/Text) | [[../20260829-02-design-system#3.1 Components ร่วม (ใช้ทุกหน้าจอ)\|3.1 Components ร่วม]] | ลิงก์ "ลืมรหัสผ่าน?" (placeholder) |
| Alert | [[../20260829-02-design-system#3.1 Components ร่วม (ใช้ทุกหน้าจอ)\|3.1 Components ร่วม]] | ใช้ variant error สำหรับ state 2 |

## Mapping กลับไปยัง Requirement

| ส่วนของหน้าจอ | Requirement/Business Rule | Feature ID |
|---|---|---|
| ฟอร์ม Login (username/password) | ไม่มี business rule ระบุตรงๆ — สืบทอดจาก Journey 3 ขั้นตอน "Login เข้าสู่ระบบ" เท่านั้น (สมมติฐานชั่วคราว) | — (ไม่มี Feature ID เฉพาะ) |
| การบันทึก audit log เมื่อ login | "ทุก action ในรายการต่อไปนี้ต้องถูกบันทึกเป็น audit log เสมอ...login เข้า dashboard" | `AUDIT-01` |
| การตรวจสอบสิทธิ์หลัง login (handoff ไป Authorized/Access Denied) | "การเข้าถึงหน้า dashboard ต้องจำกัดเฉพาะผู้ใช้ที่มีบทบาท 'เจ้าของร้าน' หรือ 'พนักงานที่ได้รับสิทธิ์ดูรายงาน' เท่านั้น" | `DASH-05` |

## ประเด็นที่ยังไม่ยืนยัน (สืบทอดจาก requirement ต้นทาง + สมมติฐานที่เพิ่มขึ้นในไฟล์นี้)

- **ไม่มี requirement spec เฉพาะสำหรับหน้า login** — ทุก field/element ในไฟล์นี้ (username/password, ปุ่มลืมรหัสผ่าน, ข้อความ error) เป็นแนวทางขั้นต่ำสุดที่ user ยืนยันให้ทำไปก่อนเท่านั้น แนะนำให้สร้าง requirement spec เฉพาะสำหรับหน้า login ในรอบถัดไป (ผ่าน `/create-requirement`) เพื่อยืนยันรายละเอียดจริง เช่น ช่องทาง auth ที่รองรับ, flow "ลืมรหัสผ่าน", วิธีสร้างบัญชีพนักงาน
- **รายละเอียดการกำหนดสิทธิ์/บทบาทผู้ใช้** (ใครกำหนดสิทธิ์พนักงาน, มีกี่ระดับสิทธิ์) — สืบทอดจากประเด็นเปิดของ `DASH-05` ในสเปคต้นทาง ยังไม่ระบุ
- Design System ปัจจุบัน (`20260829-02-design-system.md`) ยังไม่มี component "Form/Text Input" อย่างเป็นทางการ เพราะหน้า login ไม่ใช่ touchpoint ที่ระบุไว้ตอนสร้างเอกสารนั้น ไฟล์นี้จึงอนุมานจาก token สี/ตัวอักษรทั่วไปแทน — ควรเพิ่ม component นี้อย่างเป็นทางการถ้ามีฟอร์มเพิ่มขึ้นในอนาคต

## ประวัติการแก้ไข

| วันที่ | สาเหตุ | สรุปการแก้ไข |
|---|---|---|
| 2026-08-29 | สร้างครั้งแรก — ผู้ใช้ขอ "ทำ prototype หน้า login ต่อ" และเลือกแนวทาง A (Login แบบขั้นต่ำสุด username/password + ลืมรหัสผ่าน placeholder) หลังยืนยันว่าไม่มี requirement spec รองรับรายละเอียดหน้านี้โดยตรง | สร้าง wireframe หน้า Login 2 state (Login Form default, Login Form error) พร้อมส่วนอธิบาย handoff ไปยัง `sales-dashboard.md` ที่มีอยู่แล้ว ระบุชัดว่าเป็นสมมติฐานชั่วคราว รอ requirement spec เฉพาะในอนาคต |
