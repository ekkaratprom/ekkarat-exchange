# PCI DSS — สรุปครบเครื่อง

> Complete Course Summary · 2026 · มาตรฐาน v4.0

---

## สารบัญ

- [00 — Introduction to PCI DSS](#00--introduction-to-pci-dss)
- [01 — Cardholder Data vs SAD](#01--cardholder-data-vs-sad)
- [02 — Goal 1: Secure Network](#02--goal-1-build--maintain-secure-network)
- [03 — Goal 2: Protect Data](#03--goal-2-protect-account-data)
- [04 — Goal 3: Vulnerability Management](#04--goal-3-vulnerability-management)
- [05 — Goal 4: Access Control](#05--goal-4-access-control)
- [06 — Goal 5: Monitor & Test](#06--goal-5-monitor--test)
- [07 — Goal 6: Security Policy](#07--goal-6-security-policy)
- [08 — Password Security](#08--password-security)
- [09 — Malware & Phishing](#09--malware--phishing)
- [10 — Do's & Don'ts](#10--payment-card-dos--donts)
- [Quiz + เฉลย](#quiz--เฉลย)

**Highlights:** 6 Goals · 12 Requirements · v4.0 · 5 Quiz Questions

---

## 00 — Introduction to PCI DSS

**PCI DSS (Payment Card Industry Data Security Standard)** คือมาตรฐานความปลอดภัยสำหรับธุรกิจที่ **ประมวลผล / เก็บ / ส่ง** ข้อมูลบัตรชำระเงิน (เครดิต + เดบิต)

สร้างปี **2004** โดย 5 แบรนด์ใหญ่ (the "five brands"): `Visa` `MasterCard` `American Express` `Discover` `JCB`

### ประโยชน์ของการ Comply

- ลูกค้าเชื่อใจ — ระบบปลอดภัย
- ลดความเสี่ยงข้อมูลรั่ว ทั้งวันนี้และอนาคต
- รักษาความสัมพันธ์กับธนาคาร / payment brands

### ถ้าไม่ Comply จะเจออะไร?

- ชื่อเสียงพัง เสียลูกค้า
- ถูกฟ้องร้อง โดนปรับจากผู้ออกบัตรและรัฐ
- ถูกยกเลิกสิทธิ์รับบัตร

### PCI DSS v4.0 — เวอร์ชันล่าสุด

ออกเดือนเมษายน 2024 อัปเดตเพื่อรับมือภัยใหม่ๆ — บังคับ **MFA**, กฎรหัสผ่านที่เข้มขึ้น, ป้องกัน Phishing & Social Engineering ดีขึ้น

### Timeline v4.0

| วันที่ | เหตุการณ์ |
|---|---|
| มี.ค. 2022 | PCI DSS v4.0 ประกาศออกมา |
| มี.ค. 2022 – มี.ค. 2024 | Transition period — ใช้ v3.2.1 หรือ v4.0 ก็ได้ |
| 31 มี.ค. 2024 | v3.2.1 ถูก retire — ต้องใช้ v4.0 เท่านั้น |
| 31 มี.ค. 2025 | Requirements ใหม่ที่เป็น "best practice" → กลายเป็น **mandatory** |

---

## 01 — Cardholder Data vs SAD

ข้อมูลที่ PCI DSS ปกป้องแบ่งเป็น **2 ประเภทหลัก**

| | Cardholder Data (CD) | Sensitive Auth Data (SAD) |
|---|---|---|
| **สถานะ** | ✅ เก็บได้ (อย่างปลอดภัย) | ❌ ห้ามเก็บเด็ดขาด! |
| **ประกอบด้วย** | PAN (เลข 16 หลัก), Cardholder Name, Expiration Date, Service Code | Full Magnetic Stripe, CVV/CSC/CID/CAV2, PIN/PIN Block |

> **เคล็ดจำ:** ของที่ **เห็นได้ด้วยตา** (หน้าบัตร + ชื่อ) = CD เก็บได้ / ของที่ **ใช้ยืนยันตอนจ่าย** (CVV, PIN, magnetic stripe) = SAD ห้ามเก็บ

### "Store" หมายถึงเก็บที่ไหนบ้าง?

ทุกที่ ทุกรูปแบบ — Database, อีเมล, คอม, มือถือ, **กระดาษ** (ใบเสร็จก็นับ!)

### 3 วิธีปกป้อง Stored Cardholder Data

| วิธี | ทำอะไร |
|---|---|
| **Encryption** | เข้ารหัสข้อมูล อ่านไม่ได้ถ้าไม่มี key |
| **Tokenization** | แทน PAN ด้วย token อิเล็กทรอนิกส์ |
| **Truncation** | ปิดบัง PAN เหลือ 4 ตัวแรกหรือ 6 ตัวท้าย (เช่น `xxxx xxxx xxxx 1234`) |

---

## 02 — Goal 1: Build & Maintain Secure Network

### Requirement 1 — Network Security Controls

ติดตั้ง + ดูแล firewall, router ป้องกัน CDE จากภายนอก (ครอบคลุม virtual devices, cloud, container ด้วย)

### Requirement 2 — Secure Configurations

เปลี่ยน default password, ลบ software/account ที่ไม่ใช้, ปิด service ที่ไม่จำเป็น

> **Mindset:** Req 1 = กำแพงเมือง · Req 2 = ล็อกประตูทุกบาน

---

## 03 — Goal 2: Protect Account Data

### Requirement 3 — Protect Stored Data

**Data at Rest** — เก็บเฉพาะที่จำเป็น, ห้ามเก็บ SAD, ใช้ encryption/truncation/tokenization

### Requirement 4 — Protect Data in Transit

**Data in Transit** — เข้ารหัสตอนส่งผ่าน public network, ปกป้อง wireless

> **เปรียบเทียบ:** Req 3 = ใส่ข้อมูลใน "ตู้เซฟ" / Req 4 = ใส่ใน "รถหุ้มเกราะ"

---

## 04 — Goal 3: Vulnerability Management

### Requirement 5 — Protect from Malware

Anti-virus, anti-phishing training, update + patch ตลอด

### Requirement 6 — Secure Systems & Software

Secure coding, จัดลำดับ vulnerability, WAF, change management

### Vulnerability Management คืออะไร?

กระบวนการ **ค้นหา → ประเมิน → แก้ไข → รายงาน** จุดอ่อนในระบบอย่างต่อเนื่อง — เพราะภัยใหม่เกิดขึ้นทุกวัน

---

## 05 — Goal 4: Access Control

### Requirement 7 — Need to Know

**Least Privilege** — ให้สิทธิ์เท่าที่งานต้องใช้ Deny by default

### Requirement 8 — Unique ID + MFA

1 คน 1 ID — accountability + traceability พร้อม MFA และ password เข้มแข็ง

### Requirement 9 — Physical Access

Badge reader, visitors ต้องมีคนพา, ทำลายข้อมูลอย่างปลอดภัย, ตรวจ POS ป้องกัน tampering

> **3 Authentication Factors:**
> - Something you **know** (รหัส)
> - Something you **have** (โทรศัพท์/token)
> - Something you **are** (ลายนิ้วมือ)
>
> รวม 2+ = **MFA**

---

## 06 — Goal 5: Monitor & Test

### Requirement 10 — Log & Monitor Access

เปิด audit logs, review ทุกวัน, เก็บ 12 เดือน, ใช้ time-sync

### Requirement 11 — Test Security Regularly

Vulnerability scan ทุกไตรมาส, Pen test ปีละครั้ง, IDS/IPS

### Cheat Sheet — ความถี่ที่ต้องจำ

| กิจกรรม | ความถี่ |
|---|---|
| Log Review | Daily (ทุกวัน) |
| Vulnerability Scan | Quarterly (ทุกไตรมาส) |
| Penetration Test | Yearly (ปีละครั้ง) |
| Log Retention | 12 months |
| Password Change | 90 days |
| Security Training | Yearly |

> **Mnemonic:** "V-Q, P-Y" — **V**uln scan = **Q**uarterly · **P**en test = **Y**early

---

## 07 — Goal 6: Security Policy

### Requirement 12 — Information Security Policy

Acceptable use policy, ทำ risk analysis ปีละครั้ง, security training (รับเข้า + ทุกปี), screen พนักงาน, Incident Response Plan, ตรวจสอบ third-party

### "Personnel" ครอบคลุมใคร?

Full-time + Part-time + Temporary + **Contractors** + **Consultants** + Third parties

> **Mindset:** Goal 1-5 เน้น **เทคโนโลยี** · Goal 6 เน้น **คน + กระบวนการ** เพราะคนคือ weakest link เสมอ

---

## 08 — Password Security

### PCI Requirements สำหรับ Password

| กฎ | รายละเอียด |
|---|---|
| ความยาว | อย่างน้อย **12 ตัว** (v4.0) / 7 ตัว (v3.2.1) |
| ส่วนผสม | ต้องมีทั้ง **ตัวเลข + ตัวอักษร** |
| เปลี่ยน | ทุก **90 วัน** |
| ห้ามซ้ำ | ต่างจากของเก่า |
| Default | ห้ามใช้ default ของผู้ผลิต |
| Lockout | ล็อก account หลังพยายามผิดไม่เกิน **6 ครั้ง** |

### Top รหัสผ่านที่โดนแฮกบ่อยที่สุด

`123456` · `123456789` · `qwerty` · `password` · `111111` · `12345678` · `abc123`

> **😱 ข้อเท็จจริง:** รหัส `123456` เพียงตัวเดียว — บัญชีโดนแฮก **23 ล้านบัญชี** ในปีเดียว

### Best Practice

- ผสม upper/lower/เลข/สัญลักษณ์ ยาวอย่างน้อย 12 ตัว
- ไม่ใช้ข้อมูลส่วนตัวที่หาได้บน social media
- ไม่ใช้ทริคสลับตัวอักษรง่ายๆ (เช่น `P@ssw0rd`)
- **ห้าม share — ห้ามจด — ห้ามส่งทางอีเมล**

> **"1 คน · 1 Unique ID"** — ห้าม share credentials เด็ดขาด เพราะ accountability!

---

## 09 — Malware & Phishing

### ประเภทของ Malware

| ประเภท | ทำอะไร | ระดับ |
|---|---|---|
| **Ransomware** | เข้ารหัสไฟล์ เรียกค่าไถ่ (Bitcoin) | 🔴🔴🔴 สูงมาก |
| **Spyware** | แอบเก็บข้อมูล / รหัส / keylog | 🔴🔴 สูง |
| **Virus** | ฝังในไฟล์ปกติ แพร่ผ่านการเปิดไฟล์ | 🔴🔴 สูง |
| **Trojan** | ปลอมเป็นโปรแกรมดีๆ (มักปลอมเป็น antivirus) | 🔴🔴 สูง |
| **Adware** | โฆษณากวนใจ + redirect เว็บ | 🔴 น้อย-กลาง |

### สัญญาณว่าเครื่องติด Malware

- กิจกรรมแปลกๆ ตอนเปิดเครื่อง/browser
- มีโปรแกรม/ไฟล์แปลกปลอม
- เครื่องช้าลง รีสตาร์ทแล้วก็ยังช้า
- **Antivirus ถูกปิดเอง** เปิดกลับไม่ได้
- เครื่อง crash บ่อย
- Popup โฆษณาเพิ่มขึ้นผิดปกติ

### Phishing — การหลอกออนไลน์

ปลอมตัวเป็นคน/องค์กรที่ไว้ใจ เพื่อขโมยรหัส บัญชี เงิน

**🚩 Red Flags ของ Phishing:**
- ผู้ส่งน่าสงสัย (โดเมนแปลกๆ)
- คำทักทายทั่วไป "Dear Customer"
- สร้างความเร่งด่วน / ขู่
- Link / Attachment น่าสงสัย
- สะกดผิด ไวยากรณ์แย่
- ขอข้อมูลส่วนตัว/รหัส

> **กฎทอง:** ธนาคาร / Tech company / องค์กรจริง — **ไม่เคย** ขอรหัสหรือข้อมูลส่วนตัวทางอีเมล!

> **Phishing Response:** "STOP → THINK → VERIFY" — หยุด · คิด · ตรวจสอบ

---

## 10 — Payment Card Do's & Don'ts

| ✓ Do's — สิ่งที่ควรทำ | ✗ Don'ts — สิ่งที่ห้ามทำ |
|---|---|
| เก็บใบเสร็จใน **locked storage** | ทิ้งเอกสารในที่ไม่ปลอดภัย |
| แสดง PAN แบบ **truncated** (xxxx xxxx xxxx 1234) | ทิ้งใบเสร็จลงถังขยะปกติ (ใช้ **shredder**) |
| โต๊ะทำงาน **เรียบร้อย** (Clean Desk) | **เขียน/เก็บ CVV เด็ดขาด!** |
| อ่าน + เข้าใจ **security policy** | ส่ง PAN เต็มทางอีเมล (ต้อง encrypted/truncated) |

> **⚠️ จำให้แม่น:** **CVV / CSC / PIN** = SAD = **ห้ามเก็บในรูปแบบใดๆ ทั้งสิ้น** ไม่ว่าจะเขียน, save, email, screenshot หรืออะไรก็ตาม

---

## Quiz + เฉลย

### Q1 — Vulnerability Scanning Frequency

**Question:** According to PCI, vulnerability scanning should be performed:

- ✗ Yearly
- ✅ **Quarterly (ทุกไตรมาส)**
- ✗ Half-yearly
- ✗ Monthly

**คำอธิบาย:** ตาม **PCI DSS Requirement 11** (Goal 5) — ต้องทำ vulnerability scan ทั้งภายในและภายนอก **ทุก 3 เดือน** เพื่อค้นหาช่องโหว่ใหม่ๆ ส่วน penetration test ทำปีละครั้ง

> 💡 **ทริคจำ:** "V-Q, P-Y" — Vuln scan = Quarterly, Pen test = Yearly

---

### Q2 — Security Policy Scope

**Question:** An information security policy should apply to:

- ✗ Employees only
- ✗ Contractors only
- ✅ **Employees and contractors**
- ✗ Managers only

**คำอธิบาย:** ตาม **PCI DSS Requirement 12** — คำว่า **"Personnel"** ครอบคลุมทุกคน ทั้งพนักงาน full-time, part-time, temporary, **contractors, consultants** และ third parties

> 💡 ถ้ายกเว้นใครคนหนึ่ง = สร้างจุดอ่อนในระบบ

---

### Q3 — Customized Approach (v4.0)

**Question:** Which best describes PCI 4.0's "customized approach" to compliance?

- ✗ The customized approach allows you to choose which requirements are best suited to your organization
- ✗ The customized approach allows you to substitute any of the 12 requirements for customized requirements
- ✅ **The customized approach allows you to substitute your own control to meet the objective of any PCI DSS requirement**

**คำอธิบาย:** **Customized Approach** เป็นฟีเจอร์ใหม่ใน v4.0 — คุณสามารถใช้ **control ของตัวเอง** แทน control มาตรฐาน แต่ต้องบรรลุ **objective** เดียวกัน

> ⚠️ ไม่ใช่การ "เลือก" requirement หรือ "เปลี่ยน" requirement — ต้องทำครบทั้ง 12 ข้อเสมอ

---

### Q4 — Vulnerability Management Definition

**Question:** What is meant by the term 'Vulnerability Management'?

- ✗ Managing information system's log files
- ✗ Ensuring your company's premises is physically secure
- ✅ **Finding weaknesses in your company's network infrastructure**
- ✗ Scanning emails for viruses

**คำอธิบาย:** **Vulnerability Management** = กระบวนการ **ค้นหา → ประเมิน → แก้ไข → รายงาน** จุดอ่อนในระบบอย่างต่อเนื่อง ตรงกับ **Goal 3** (Req 5, 6) และ Req 11

---

### Q5 — Cardholder Data (Multi-Select)

**Question:** Which of the following is considered cardholder data?

- ✅ **A. PAN**
- ✅ **B. Expiry date**
- ✗ C. Magnetic stripe data ← นี่คือ **SAD** ห้ามเก็บ!
- ✅ **D. Cardholder name**

**คำอธิบาย:** **Cardholder Data (CD)** = PAN, ชื่อ, วันหมดอายุ, service code — ส่วน **Magnetic Stripe Data = SAD** ห้ามเก็บเด็ดขาด

> 💡 **ทริคจำ:** ของที่ **เห็นได้ด้วยตา** หน้าบัตร = CD / ของที่ **ซ่อนอยู่** (stripe, chip, CVV) = SAD

---

*PCI DSS Study Guide · Compiled 2026 · For Educational Use*
