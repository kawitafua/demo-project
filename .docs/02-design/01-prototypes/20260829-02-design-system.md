# Design System

> สร้างเมื่อ 2026-08-29
> อ้างอิงความต้องการจาก: [[../../01-requirements/01-spec/20260807-01-table-qr-ordering|20260807-01-table-qr-ordering]], [[../../01-requirements/01-spec/20260807-02-dashboard-sales|20260807-02-dashboard-sales]], [[../../01-requirements/01-spec/20260807-03-audit-log-pdpa-compliance|20260807-03-audit-log-pdpa-compliance]]
> อ้างอิง Flow: [[20260829-01-user-journeys|20260829-01-user-journeys]]

เอกสารนี้กำหนด **Design System** ของโปรเจกต์ my-coffee-store ในทิศทาง **Earth Tone + Minimalist + Muji-inspired** เพื่อใช้เป็นภาษากลางระหว่างทีมออกแบบและทีมพัฒนา ครอบคลุมทุกจุดสัมผัส (touchpoint) ที่ปรากฏใน requirement spec ปัจจุบัน ได้แก่ หน้าสั่งเมนูของลูกค้า (mobile), จอ KDS ของบาริสต้า (tablet/in-store display) และหน้า Dashboard ยอดขายของเจ้าของร้าน/พนักงาน (desktop)

เอกสารนี้เป็น **tech-agnostic** (ไม่ผูกกับ framework/library เฉพาะเจาะจง) เนื่องจากโปรเจกต์ยังไม่ได้เลือก tech stack — ค่าที่กำหนด (สี, ขนาด, ฟอนต์) สามารถนำไปแปลงเป็น CSS variables, Tailwind config, หรือ design token format อื่นได้เมื่อเลือก stack แล้ว

---

## 1. Brand Identity & CI

### 1.1 แนวคิดแบรนด์ (Brand Concept)

| หัวข้อ | รายละเอียด |
|---|---|
| ชื่อโปรเจกต์ (working name) | my-coffee-store (ชื่อแบรนด์จริง/โลโก้ยังไม่กำหนด — ดูหัวข้อ "ประเด็นที่ยังไม่ชัดเจน") |
| บุคลิกแบรนด์ | อบอุ่น, เรียบง่าย, จริงใจ, ไม่ฉูดฉาด — เน้นให้ "กาแฟและประสบการณ์การสั่ง" เป็นพระเอก ไม่ใช่ UI |
| แรงบันดาลใจ | โทนสี Earth Tone ของเมล็ดกาแฟ/ไม้/ดิน ผสานความเรียบง่ายสไตล์ MUJI (ไร้ลวดลายเกินจำเป็น, เน้นฟังก์ชัน, whitespace เยอะ) |
| คำที่ไม่ควรเป็น | หรูหราเกินจริง (luxury), สีสันจัดจ้านแบบ playful/childish, ทันสมัยจ๋าแบบ neon/gradient |

### 1.2 หลักการออกแบบ 3 ข้อ (Design Principles)

1. **Calm over Loud** — ใช้สีเรียบ ไม่ฉูดฉาด ให้ความรู้สึกผ่อนคลายเหมือนนั่งจิบกาแฟ ไม่ใช่แอปที่กระตุ้นให้รีบตัดสินใจ
2. **Function over Decoration** — ทุก element มีเหตุผลในการมีอยู่ ไม่มีลวดลาย/เอฟเฟกต์ตกแต่งที่ไม่ช่วยการใช้งาน (สอดคล้องแนวคิด MUJI)
3. **Clarity for Every Role** — ลูกค้า (มือใหม่ ใช้ครั้งเดียวจบ), บาริสต้า (ใช้ซ้ำทั้งกะ ต้องเร็ว), เจ้าของร้าน (ดูข้อมูลเชิงลึก) มีบริบทต่างกัน UI ต้องชัดเจนพอที่แต่ละกลุ่มใช้ได้ทันทีโดยไม่ต้องสอน

### 1.3 โลโก้ & การใช้งาน

ยังไม่มีโลโก้อย่างเป็นทางการ ระหว่างที่ยังไม่มี ให้ใช้ **wordmark ตัวอักษรล้วน** (ชื่อร้าน พิมพ์ด้วยฟอนต์หลักตามหัวข้อ 2.2 น้ำหนัก Medium/Semibold สี `--color-text-primary` หรือ `--color-brand-primary`) แทนไอคอน/สัญลักษณ์ เพื่อคงความเรียบตามแนว MUJI (MUJI เองใช้ wordmark ล้วน ไม่มีไอคอน)

---

## 2. Design Tokens / Design System

### 2.1 Colors

พาเลตหลักอิงโทนดิน/ไม้/เมล็ดกาแฟ (earth tone) จำกัดจำนวนสีเด่นให้น้อยที่สุดตามหลัก minimalist — ใช้สี accent เพียงจุดเดียวต่อหน้าจอ ที่เหลือเป็นกลาง (neutral)

#### Brand & Neutral

| Token | Hex | การใช้งาน |
|---|---|---|
| `--color-brand-primary` | `#6B4A34` (Coffee Bean Brown) | ปุ่มหลัก, header, จุดเน้นสำคัญ |
| `--color-brand-secondary` | `#A9805F` (Roasted Tan) | สถานะ hover/active ของสี primary, ไอคอนรอง |
| `--color-background` | `#F7F3EC` (Milk Foam / Oat) | พื้นหลังหลักของทุกหน้าจอ |
| `--color-surface` | `#FFFFFF` | พื้นผิวการ์ด/แผงข้อมูลที่ลอยเหนือพื้นหลัง |
| `--color-surface-muted` | `#EDE6D8` (Linen Beige) | พื้นหลังส่วนรอง เช่น แถบ filter, แถวสลับสีตาราง |
| `--color-border` | `#D9CFBF` (Sand) | เส้นขอบ/เส้นแบ่ง แทนการใช้ shadow หนักๆ |
| `--color-text-primary` | `#3A322A` (Espresso) | ข้อความหลัก |
| `--color-text-secondary` | `#7A6F62` (Warm Gray) | ข้อความรอง/คำอธิบาย |
| `--color-text-disabled` | `#B8AFA2` | ข้อความ/element ที่ปิดใช้งาน |

#### Accent (ใช้เฉพาะจุดสำคัญ — สถานะ/CTA เท่านั้น)

| Token | Hex | การใช้งาน |
|---|---|---|
| `--color-accent-terracotta` | `#B5623C` | CTA รอง, badge "ขายดี" |
| `--color-accent-sage` | `#7C8A5C` | ใช้คู่กับ success (โทนเขียวมะกอกให้เข้ากับ earth tone แทนเขียวสดทั่วไป) |

#### Semantic (สถานะ)

| Token | Hex | การใช้งาน |
|---|---|---|
| `--color-success` | `#5F7A4A` | ชำระเงินสำเร็จ, บันทึกสำเร็จ |
| `--color-warning` | `#B8863B` | รอดำเนินการ, ใกล้ครบกำหนด |
| `--color-error` | `#A6472F` | ชำระเงินไม่สำเร็จ, ปฏิเสธการเข้าถึง |
| `--color-info` | `#5C7A8A` | ข้อความแจ้งเตือนทั่วไป |

**กฎการใช้สี**: ห้ามใช้สีเป็นตัวสื่อความหมายเพียงอย่างเดียว (ดูหัวข้อ 4.5 Accessibility) เช่น สถานะออเดอร์/การชำระเงินต้องมี icon หรือ label ข้อความกำกับคู่กับสีเสมอ

### 2.2 Typography

| หัวข้อ | รายละเอียด |
|---|---|
| แนวทาง | Humanist sans-serif เรียบ อ่านง่ายทั้งภาษาไทย/อังกฤษ ไม่มี serif หรือ display font ตกแต่ง (สอดคล้อง MUJI ที่ใช้ตัวอักษรเรียบตรงไปตรงมา) |
| ฟอนต์ภาษาไทย (แนะนำ) | `Noto Sans Thai` (fallback: `IBM Plex Sans Thai`, `sans-serif`) |
| ฟอนต์ภาษาอังกฤษ/ตัวเลข (แนะนำ) | `Inter` (fallback: `IBM Plex Sans`, `-apple-system`, `sans-serif`) |
| Line-height ภาษาไทย | ขั้นต่ำ 1.6 สำหรับ body text (สระบน-ล่าง/วรรณยุกต์ภาษาไทยต้องการพื้นที่มากกว่าอังกฤษ) |

**Type Scale** (ฐาน 16px, อัตราส่วน ~1.25):

| Token | ขนาด | น้ำหนัก | ใช้กับ |
|---|---|---|---|
| `--text-display` | 32px / line-height 1.3 | Semibold (600) | หัวข้อหน้า Dashboard, ยอดรวมเด่นๆ |
| `--text-h1` | 24px / 1.35 | Semibold (600) | หัวข้อหลักของหน้า |
| `--text-h2` | 20px / 1.4 | Medium (500) | หัวข้อ section/การ์ด |
| `--text-body` | 16px / 1.6 | Regular (400) | เนื้อหาทั่วไป, รายการเมนู |
| `--text-caption` | 14px / 1.5 | Regular (400) | คำอธิบายรอง, timestamp, label ฟอร์ม |
| `--text-micro` | 12px / 1.4 | Medium (500) | badge, tag, ตัวเลขในตาราง |

จำกัดจำนวนระดับตัวอักษรต่อหน้าจอไม่เกิน 3-4 ระดับ ตามหลัก minimalist (อย่าใช้ทุก scale พร้อมกันในหน้าเดียว)

### 2.3 Spacing & Layout

ใช้ฐาน **4px** และ scale ทวีคูณ เพื่อความสม่ำเสมอ:

| Token | ค่า |
|---|---|
| `--space-1` | 4px |
| `--space-2` | 8px |
| `--space-3` | 12px |
| `--space-4` | 16px |
| `--space-5` | 24px |
| `--space-6` | 32px |
| `--space-8` | 48px |
| `--space-10` | 64px |

**หลักการใช้**: whitespace คือส่วนหนึ่งของดีไซน์ ไม่ใช่พื้นที่ว่างที่ต้อง "เติม" — เว้นระยะให้มากกว่าที่รู้สึกว่าจำเป็นเล็กน้อยเสมอ (แนว MUJI)

### 2.4 Radius, Border & Elevation

| Token | ค่า | ใช้กับ |
|---|---|---|
| `--radius-sm` | 4px | input, badge, ปุ่มขนาดเล็ก |
| `--radius-md` | 8px | การ์ด, ปุ่มหลัก |
| `--radius-lg` | 16px | modal, bottom sheet (ตะกร้าสินค้าบนมือถือ) |
| `--border-width` | 1px solid `--color-border` | เส้นแบ่ง/ขอบการ์ด — ใช้แทน shadow เป็นหลัก |
| `--shadow-float` | `0 2px 8px rgba(58,50,42,0.08)` | เฉพาะ element ที่ "ลอย" เหนือเนื้อหา เช่น floating cart bar, modal เท่านั้น |

**กฎ**: หลีกเลี่ยง drop shadow ที่หนัก/หลายชั้น — ใช้เส้นขอบบางๆ (`--color-border`) แบ่งพื้นที่แทนเป็นค่าเริ่มต้น ใช้ shadow เฉพาะกรณีที่ต้องสื่อว่า element นั้นลอยอยู่เหนือ layer อื่นจริงๆ

### 2.5 Iconography

- ใช้ **line icon (outline)** น้ำหนักเส้นสม่ำเสมอ ไม่ใช้ icon แบบ filled/gradient/หลายสี
- สีไอคอนเริ่มต้น: `--color-text-secondary`, ไอคอนที่ interactive ใช้ `--color-brand-primary`
- ขนาดมาตรฐาน: 16px (inline กับข้อความ), 20px (ปุ่ม/nav), 24px (จุดเน้น เช่น empty state)

### 2.6 Grid & Breakpoints

โปรเจกต์มี 3 กลุ่มหน้าจอที่บริบทต่างกันชัดเจนตาม [[20260829-01-user-journeys|user journeys]]:

| Breakpoint | ช่วงความกว้าง | ใช้กับ |
|---|---|---|
| `mobile` | < 768px | หน้าสั่งเมนู/ตะกร้าของลูกค้า (เข้าผ่าน QR) — ออกแบบแบบ **mobile-first** เป็นหลัก |
| `tablet` | 768–1279px | จอ KDS ในร้าน (แสดงต่อเนื่อง ไม่ scroll แนวนอน) |
| `desktop` | ≥ 1280px | หน้า Dashboard ยอดขายของเจ้าของร้าน/พนักงาน |

---

## 3. UI Components & Patterns

แบ่งตามกลุ่มผู้ใช้ 3 กลุ่มตาม journey หลัก

### 3.1 Components ร่วม (ใช้ทุกหน้าจอ)

| Component | Anatomy | สถานะที่ต้องมี |
|---|---|---|
| **Button** | Primary (`--color-brand-primary` พื้น, ตัวอักษรขาว), Secondary (พื้น `--color-surface` ขอบ `--color-border`), Ghost/Text (ไม่มีพื้น/ขอบ) | default, hover, pressed, disabled, loading (spinner แทน label) |
| **Badge/Tag** | พื้นหลังอ่อนของสี semantic + ตัวอักษรสีเข้มของสีเดียวกัน (เช่น badge "ขายดี" = พื้น terracotta 10% opacity + ตัวอักษร terracotta เข้ม) | default เท่านั้น (ไม่ interactive) |
| **Alert / Toast** | icon สถานะ + ข้อความ + สีพื้นอ่อนของ semantic color ที่เกี่ยวข้อง | success, warning, error, info — ต้องปิดเองได้ (dismissible) ยกเว้น error สำคัญ |
| **Modal / Dialog** | overlay สีเข้มโปร่งแสงบนพื้นหลัง + panel `--radius-lg` กึ่งกลางจอ (มือถือ: bottom sheet เต็มความกว้าง) | ใช้กับ consent dialog (สมัครสมาชิก/PDPA), ยืนยัน export |
| **Empty State** | ไอคอน line 24px + ข้อความอธิบายสั้น + CTA (ถ้ามี) | ใช้เมื่อยังไม่มีออเดอร์ใน KDS, ยังไม่มีข้อมูลยอดขายในช่วงที่เลือก |

### 3.2 หน้าสั่งเมนู (ลูกค้า — mobile)

อ้างอิง [[../../01-requirements/01-spec/20260807-01-table-qr-ordering|table-qr-ordering]]

| Component | รายละเอียด |
|---|---|
| **Table Indicator** | แถบเล็กด้านบนสุดของหน้า แสดง "โต๊ะ #{n}" ตลอดเวลาที่ลูกค้าอยู่ในหน้าสั่งเมนู เพื่อความมั่นใจว่าออเดอร์จะไปโต๊ะถูกต้อง |
| **Category Tabs** | แท็บแนวนอน scroll ได้ พื้นหลัง `--color-surface-muted`, tab ที่เลือกใช้ `--color-brand-primary` |
| **Menu Item Card** | รูปภาพ (radius `--radius-md`) + ชื่อเมนู (`--text-body` medium) + ราคา (`--text-body`) + ปุ่ม "+" เพิ่มลงตะกร้า มุมขวาล่าง badge "ขายดี" ถ้ามี (ใช้ accent terracotta) |
| **Floating Cart Bar** | แถบลอยด้านล่างจอ (`--shadow-float`) แสดงจำนวนชิ้น + ยอดรวม + ปุ่ม "ดูตะกร้า" — ปรากฏเฉพาะเมื่อมีสินค้าในตะกร้า |
| **Cart / Checkout Drawer** | Bottom sheet (`--radius-lg`) รายการสินค้า + ปุ่มปรับจำนวน + ยอดรวม + ปุ่ม "ชำระเงิน" |
| **Membership/Consent Checkbox** | Checkbox + label ลิงก์ "อ่าน privacy notice" แยกจากปุ่มยืนยันชัดเจน — ปุ่ม "สมัครสมาชิก" ต้อง disabled จนกว่าจะติ๊กยินยอม (สอดคล้อง business rule เรื่อง consent ใน [[../../01-requirements/01-spec/20260807-03-audit-log-pdpa-compliance|audit-log-pdpa]]) |
| **Payment Result State** | เต็มจอ/modal ใหญ่ ไอคอน success (`--color-success`) หรือ error (`--color-error`) ขนาดใหญ่ + ข้อความชัดเจน + ปุ่ม action ถัดไป (กลับเมนู / ลองชำระใหม่) |

### 3.3 จอ KDS (บาริสต้า — tablet/in-store display)

| Component | รายละเอียด |
|---|---|
| **Order Ticket Card** | การ์ดแนวตั้ง: หมายเลขโต๊ะ (`--text-h2` เด่นสุดของการ์ด) + เวลาที่สั่ง (`--text-caption`) + รายการเมนู (`--text-body`) + จำนวนต่อรายการ |
| **New Order Highlight** | การ์ดออเดอร์ใหม่ที่เพิ่งเข้ามาแบบเรียลไทม์ ใช้ขอบ `--color-brand-secondary` หนา 2px ชั่วคราว (ไม่ใช้ animation กระพริบ/สั่น เพราะรบกวนสายตาขณะทำงาน) แล้วจางกลับเป็นขอบปกติหลังบาริสต้าเปิดดู |
| **Ticket Grid** | จัดเรียงเป็น grid หลายคอลัมน์ตามลำดับเวลาเข้า (เก่าสุดซ้ายบน) ไม่ scroll แนวนอน — ออกแบบให้อ่านได้จากระยะไกล 1-2 เมตร (ตัวอักษรไม่เล็กกว่า `--text-body`) |

### 3.4 Dashboard ยอดขาย (เจ้าของร้าน/พนักงาน — desktop)

อ้างอิง [[../../01-requirements/01-spec/20260807-02-dashboard-sales|dashboard-sales]]

| Component | รายละเอียด |
|---|---|
| **Stat Card (KPI Tile)** | พื้นหลัง `--color-surface` ขอบ `--color-border` — label รอง (`--text-caption`) ด้านบน + ตัวเลขเด่น (`--text-display`) ด้านล่าง ใช้สี accent เฉพาะตัวเลขที่ต้องการเน้น (เช่น ยอดขายรวม) |
| **Date Range Picker** | ปุ่มแสดงช่วงวันที่ปัจจุบัน กดแล้วเปิด calendar popover เลือกช่วงแบบอิสระ (ไม่จำกัด preset) ตาม business rule ที่ต้องรองรับ custom range |
| **Bar Chart (ยอดขายตามช่วงเวลา)** | แท่งกราฟใช้ `--color-brand-primary` เป็นสีหลักเพียงสีเดียว (ไม่ไล่สี/ไม่หลายสีต่อ series เพราะมีมิติเดียว) เส้น gridline บางสี `--color-border`, tooltip พื้น `--color-text-primary` ตัวอักษรขาว |
| **Data Table (ยอดขายแยกตามเมนู)** | แถวสลับสี `--color-surface` / `--color-surface-muted`, คอลัมน์ตัวเลขชิดขวา, แถวเมนูขายดีอันดับ 1-3 มี badge อันดับกำกับ |
| **Export Button** | Secondary button + icon download มุมขวาบนของตารางที่เกี่ยวข้อง กดแล้วต้องมี confirm modal สั้นๆ ก่อน export จริง |
| **Access Denied State** | ใช้เมื่อ role ไม่มีสิทธิ์เข้าหน้านี้ — ไอคอน lock + ข้อความอธิบายสั้น (ไม่ใช้โทน error สีแดงจัด ให้ใช้ `--color-text-secondary` + ไอคอนกลางๆ เพราะนี่ไม่ใช่ "ความผิดพลาด" ของผู้ใช้) |

---

## 4. UX Guidelines & Rules

### 4.1 หลักการทั่วไป (Minimalist / MUJI-inspired)

- **สีต่อหน้าจอ**: ใช้สี accent ได้สูงสุด 1 สีเด่นต่อหน้าจอ นอกนั้นเป็นสี neutral/earth tone พื้นฐาน
- **Whitespace ก่อนเสมอ**: เมื่อไม่แน่ใจว่าจะเพิ่ม element หรือเพิ่มระยะห่าง ให้เลือกเพิ่มระยะห่างก่อน
- **ไม่มี element ที่ไม่มีหน้าที่**: ทุกไอคอน/เส้น/สี ต้องสื่อความหมายหรือช่วยการใช้งานจริง ไม่ใช่ตกแต่ง
- **ความสม่ำเสมอ**: ปุ่ม/ฟอร์ม/การ์ดที่ทำหน้าที่เดียวกันต้องหน้าตาเหมือนกันทุกหน้าจอ (เช่น ปุ่ม primary ต้องเป็นสี/ทรงเดียวกันทั้งหน้าเมนูและ dashboard)

### 4.2 Mobile-first สำหรับหน้าสั่งเมนู

- ลูกค้าเข้าเว็บผ่าน QR ครั้งแรกโดยไม่มีการสอนการใช้งานล่วงหน้า → ทุกปุ่มสำคัญ (เพิ่มสินค้า, ดูตะกร้า, ชำระเงิน) ต้องอยู่ในระยะเอื้อมนิ้วโป้งได้ (thumb zone — ครึ่งล่างของจอ)
- ขนาดพื้นที่กดขั้นต่ำ **44×44px** ตามมาตรฐาน touch target
- ไม่ใช้ hover state เป็นตัวสื่อข้อมูลสำคัญ (มือถือไม่มี hover)

### 4.3 ความเร็วในการอ่านสำหรับบาริสต้า (KDS)

- บาริสต้าดูจอระหว่างทำงาน ไม่มีเวลาอ่านข้อความยาว → ข้อมูลสำคัญที่สุด (หมายเลขโต๊ะ) ต้องเป็นสิ่งแรกที่เห็นบนการ์ดเสมอ ขนาดใหญ่สุดในการ์ด
- หลีกเลี่ยง animation ที่ดึงความสนใจถี่เกินไป (เช่นกระพริบต่อเนื่อง) เพราะจะรบกวนสมาธิระหว่างชงกาแฟ — ใช้การเปลี่ยนแปลงแบบ static (เปลี่ยนสีขอบครั้งเดียว) แทน

### 4.4 ความชัดเจนของสถานะ (Feedback States)

- สถานะที่กระทบเงิน/ธุรกรรม (ชำระเงินสำเร็จ/ไม่สำเร็จ, export สำเร็จ) ต้องแสดงผลแบบ **บล็อกความสนใจทั้งหมด** (full-screen หรือ modal) ไม่ใช่แค่ toast เล็กๆ ที่พลาดง่าย
- สถานะที่ไม่กระทบเงิน (เช่น เพิ่มสินค้าลงตะกร้าสำเร็จ) ใช้ toast/inline feedback เบาๆ พอ ไม่ต้อง block การใช้งาน

### 4.5 Accessibility

- Contrast ratio ของตัวอักษรกับพื้นหลังต้องผ่านเกณฑ์ **WCAG AA** (อย่างน้อย 4.5:1 สำหรับ body text) — ต้องตรวจสอบเป็นพิเศษเพราะโทน earth tone มักมีความต่างของค่าความสว่าง (contrast) น้อยกว่าสีสดโดยธรรมชาติ
- ห้ามสื่อสถานะด้วยสีเพียงอย่างเดียว (เช่น สถานะชำระเงิน/ออเดอร์) ต้องมี icon หรือ label ข้อความกำกับเสมอ เพื่อรองรับผู้ใช้ตาบอดสี
- ปุ่ม/ลิงก์ทุกตัวต้องมี label ที่สื่อความหมายชัดเจน ไม่ใช้ "คลิกที่นี่" ลอยๆ

### 4.6 ภาษาและ Tone of Voice

- เนื้อหาในระบบเป็นภาษาไทยเป็นหลัก ใช้ภาษาที่เป็นกันเอง สุภาพ ไม่ทางการจนแข็ง (เช่น "สั่งสำเร็จแล้ว" แทน "การทำธุรกรรมเสร็จสมบูรณ์")
- ข้อความ error ต้องบอกสาเหตุ/ทางแก้แบบเข้าใจง่าย ไม่ใช้ error code ทางเทคนิคปนกับผู้ใช้ทั่วไป (เก็บ error code ไว้ใน log ฝั่งระบบเท่านั้น อ้างอิง [[../../01-requirements/01-spec/20260807-03-audit-log-pdpa-compliance|audit-log-pdpa]])

---

## 5. ประเด็นที่ยังไม่ชัดเจน (รอตัดสินใจในรอบถัดไป)

- **ชื่อแบรนด์และโลโก้จริง** — ยังไม่มีการตัดสินใจ เอกสารนี้ใช้แนวทาง wordmark ชั่วคราวตามหัวข้อ 1.3
- **ฟอนต์ที่เลือกใช้จริง** — `Noto Sans Thai` / `Inter` เป็นข้อเสนอแนะเบื้องต้น รอยืนยันเมื่อเลือก tech stack (บาง stack อาจมีข้อจำกัดเรื่อง font loading/licensing)
- **Component library/design tool ที่จะใช้สร้าง mockup จริง** (เช่น Figma) — ยังไม่ระบุ เอกสารนี้เป็น token/guideline level ยังไม่ใช่ pixel-perfect mockup
- **Dark mode** — ยังไม่อยู่ใน scope requirement ปัจจุบัน ไม่มี token สำหรับ dark theme ในเอกสารนี้

---

## เอกสารที่เกี่ยวข้อง

- [[20260829-01-user-journeys|20260829-01-user-journeys]]
- [[../../01-requirements/01-spec/20260807-01-table-qr-ordering|20260807-01-table-qr-ordering]]
- [[../../01-requirements/01-spec/20260807-02-dashboard-sales|20260807-02-dashboard-sales]]
- [[../../01-requirements/01-spec/20260807-03-audit-log-pdpa-compliance|20260807-03-audit-log-pdpa-compliance]]
- [[../../01-requirements/02-plan/20260829-01-feature-list|20260829-01-feature-list]]
- [[../02-technical/index|02-technical]]

## ประวัติการแก้ไข

| วันที่ | สาเหตุ | สรุปการแก้ไข |
|---|---|---|
| 2026-08-29 | สร้างครั้งแรก | สร้าง Design System ฉบับแรก (Earth Tone + Minimalist + Muji) ครอบคลุม Brand Identity, Design Tokens, UI Components, UX Guidelines ตาม requirement spec ทั้ง 3 ฉบับที่มีอยู่ ณ ขณะนั้น |
