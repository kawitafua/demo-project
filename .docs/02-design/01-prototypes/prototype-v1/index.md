# Prototype v1

> สร้างเมื่อ 2026-08-29
> สร้างโดย skill `prototype-builder`

เวอร์ชันนี้เป็น prototype ฉบับแรกของโปรเจกต์ my-coffee-store เริ่มจาก scope **"หน้า Dashboard ดูยอดขาย"** แล้วต่อยอดเพิ่ม **"หน้าสั่งเมนูลูกค้า (QR Ordering)"** เข้ามาในเวอร์ชันเดียวกัน (ยังไม่ทำจอ KDS บาริสต้าในเวอร์ชันนี้ — จะเพิ่มเป็นไฟล์ใหม่เมื่อมีการร้องขอ)

## เอกสารอ้างอิงที่ใช้

- Requirement spec: [[../../../01-requirements/01-spec/20260807-02-dashboard-sales|20260807-02-dashboard-sales]], [[../../../01-requirements/01-spec/20260807-01-table-qr-ordering|20260807-01-table-qr-ordering]]
- Feature List: [[../../../01-requirements/02-plan/20260829-01-feature-list|20260829-01-feature-list]] (`DASH-01` ถึง `DASH-06`, `QR-01` ถึง `QR-04`, `AUDIT-05`)
- User Journey: [[../20260829-01-user-journeys|20260829-01-user-journeys]] (Journey 1: ลูกค้าสั่งกาแฟผ่าน QR Code, Journey 3: เจ้าของร้าน/พนักงานดู Dashboard ยอดขาย)
- Backlog: [[../../../01-requirements/backlog|backlog]]

## Design System ที่อ้างอิง

- [[../20260829-02-design-system|20260829-02-design-system]] (Earth Tone + Minimalist + Muji-inspired) — ใช้หัวข้อ 2.1-2.6 (Design Tokens), 3.1 (Components ร่วม), 3.2 (หน้าสั่งเมนู mobile), 3.4 (Dashboard Components), 4.x (UX Guidelines)

## รายชื่อหน้าจอในเวอร์ชันนี้

| หน้าจอ | คำอธิบาย | Feature ที่ครอบคลุม |
|---|---|---|
| [[sales-dashboard\|หน้า Dashboard ยอดขาย]] | หน้าเดียว ครอบคลุม 2 state (Authorized View / Access Denied View) + sub-state (Export Confirm Modal) | `DASH-01`, `DASH-02`, `DASH-03`, `DASH-04`, `DASH-05`, `DASH-06` |
| [[customer-menu-page\|หน้าสั่งเมนูลูกค้า (QR Ordering — mobile)]] | 4 state: เมนูหลัก, ตะกร้า/Checkout Drawer, สมัครสมาชิก+Consent, ผลการชำระเงิน (สำเร็จ/ไม่สำเร็จ) | `QR-01`, `QR-02`, `QR-03`, `AUDIT-05` |

## ประวัติการแก้ไข

| วันที่ | สาเหตุ | สรุปการแก้ไข |
|---|---|---|
| 2026-08-29 | สร้างครั้งแรก | สร้าง prototype-v1 ฉบับแรกของโปรเจกต์ เฉพาะ scope "หน้า Dashboard ดูยอดขาย" ตามคำขอ user, อ้างอิง design system ที่มีอยู่แล้ว (20260829-02-design-system.md) |
| 2026-08-29 | ผู้ใช้ขอ "ทำ prototype หน้าสั่งเมนูลูกค้าต่อ" — เลือกเพิ่มไฟล์ในเวอร์ชันเดิม (ไม่สร้าง prototype-v2) เพราะเป็นการเพิ่มหน้าจอใหม่ที่ไม่ทับซ้อน/ไม่กระทบ sales-dashboard.md เดิม | เพิ่มไฟล์ `customer-menu-page.md` (หน้าสั่งเมนูลูกค้า QR Ordering ครอบคลุม `QR-01`~`QR-03`, `AUDIT-05` และ Journey 1) เข้าเวอร์ชันนี้ พร้อมอัปเดตเอกสารอ้างอิง/design system ที่อ้างอิงและรายชื่อหน้าจอด้านบน |
