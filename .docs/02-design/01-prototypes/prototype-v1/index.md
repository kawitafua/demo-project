# Prototype v1 — Dashboard ยอดขาย

> สร้างเมื่อ 2026-08-29
> สร้างโดย skill `prototype-builder`

เวอร์ชันนี้เป็น prototype ฉบับแรกของโปรเจกต์ my-coffee-store โดยรอบนี้ทำเฉพาะ scope **"หน้า Dashboard ดูยอดขาย"** ตามคำขอของ user เท่านั้น (ยังไม่ทำหน้าสั่งเมนูลูกค้า/QR ordering หรือจอ KDS บาริสต้าในเวอร์ชันนี้ — จะเพิ่มเป็นไฟล์ใหม่ในเวอร์ชันนี้หรือเวอร์ชันถัดไปเมื่อมีการร้องขอ)

## เอกสารอ้างอิงที่ใช้

- Requirement spec: [[../../../01-requirements/01-spec/20260807-02-dashboard-sales|20260807-02-dashboard-sales]]
- Feature List: [[../../../01-requirements/02-plan/20260829-01-feature-list|20260829-01-feature-list]] (เฉพาะ `DASH-01` ถึง `DASH-06`)
- User Journey: [[../20260829-01-user-journeys|20260829-01-user-journeys]] (เฉพาะ Journey 3: เจ้าของร้าน/พนักงานดู Dashboard ยอดขาย)
- Backlog: [[../../../01-requirements/backlog|backlog]]

## Design System ที่อ้างอิง

- [[../20260829-02-design-system|20260829-02-design-system]] (Earth Tone + Minimalist + Muji-inspired) — ใช้หัวข้อ 2.1-2.6 (Design Tokens), 3.1 (Components ร่วม), 3.4 (Dashboard Components), 4.x (UX Guidelines)

## รายชื่อหน้าจอในเวอร์ชันนี้

| หน้าจอ | คำอธิบาย | Feature ที่ครอบคลุม |
|---|---|---|
| [[sales-dashboard\|หน้า Dashboard ยอดขาย]] | หน้าเดียว ครอบคลุม 2 state (Authorized View / Access Denied View) + sub-state (Export Confirm Modal) | `DASH-01`, `DASH-02`, `DASH-03`, `DASH-04`, `DASH-05`, `DASH-06` |

## ประวัติการแก้ไข

| วันที่ | สาเหตุ | สรุปการแก้ไข |
|---|---|---|
| 2026-08-29 | สร้างครั้งแรก | สร้าง prototype-v1 ฉบับแรกของโปรเจกต์ เฉพาะ scope "หน้า Dashboard ดูยอดขาย" ตามคำขอ user, อ้างอิง design system ที่มีอยู่แล้ว (20260829-02-design-system.md) |
