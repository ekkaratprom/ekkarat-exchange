# Secure Coding 2025 — สรุปบทเรียน

> A Field Guide · vol. 01 · 2025 | PCI DSS v4.0

---

## สารบัญ

**Foundation**
- [Chapter 01 — Threats, Attacks & Vulnerabilities](#chapter-01--threats-attacks--vulnerabilities)
- [Chapter 02 — The OWASP Top 10](#chapter-02--the-owasp-top-10)

**Process**
- [Chapter 03 — The SDLC](#chapter-03--the-sdlc)
- [Chapter 04 — Security in SDLC](#chapter-04--security-in-the-sdlc)

**Compliance**
- [Chapter 05 — PCI DSS](#chapter-05--pci-dss)
- [Chapter 06 — PCI DSS Developer's Cut](#chapter-06--pci-dss-developers-cut)
- [Chapter 07 — Tooling: SonarQube](#chapter-07--tooling--sonarqube)

**Assessment**
- [Quiz — 5 Questions + เฉลย](#quiz--5-questions--เฉลย)

---

## Chapter 01 — Threats, Attacks & Vulnerabilities

ก่อนจะป้องกัน เราต้องเข้าใจก่อนว่าศัตรูคืออะไร — และอะไรคือสิ่งที่เปิดประตูให้พวกเขาเข้ามา

### Vulnerability vs Exploit

**Vulnerability** คือจุดอ่อนในระบบ ไม่ว่าจะเป็นบั๊กในโค้ด การตั้งค่าผิด กระบวนการที่บกพร่อง หรือ algorithm ที่ล้าสมัย เปรียบเทียบเหมือนประตูที่ไม่ได้ล็อก

**Exploit** คือวิธีการที่แฮกเกอร์ใช้เพื่อเข้าระบบผ่านจุดอ่อนนั้น เช่น hacking tool, malware, malicious script หรือ social engineering อย่าง phishing เปรียบเทียบเหมือนการเดินเข้าประตูบานนั้น

> **จำไว้:** Vulnerability = *"ช่องโหว่อยู่ที่ไหน"* — Exploit = *"ใช้ช่องโหว่นั้นยังไง"*

### กรณีศึกษาจริง

**Adobe Server Exposure (2019)**
ข้อมูลผู้ใช้ 7.5 ล้านบัญชีรั่วสู่อินเทอร์เน็ตเพราะ **misconfiguration** ของ development environment ทำให้ใครก็ตามที่มี web browser สามารถเข้าถึงได้

**Epic Games Hack (2016)**
ผู้ใช้ forum 800,000 บัญชีถูกขโมยข้อมูลผ่าน **SQL injection** ในระบบ forum รุ่นเก่าที่ยังไม่ได้อัปเดต

### 10 Secure Coding Tips

1. **Validate Inputs** — ด่านแรกของการป้องกัน ตรวจ length, format, character set
2. **Manage Authentication** — ระบบยืนยันตัวตนและจัดการรหัสผ่านที่แข็งแรง
3. **Sanitize Data** — ลบอักขระอันตรายด้วย whitelist/blacklist/escape
4. **Least Privilege** — ให้สิทธิ์น้อยที่สุดเท่าที่จำเป็น เฉพาะช่วงที่ต้องใช้
5. **Secure Architecture** — ออกแบบเอง แบ่ง subsystem ใช้ plugin ที่ปลอดภัย
6. **Keep it Simple** — ยิ่งซับซ้อน ยิ่งเสี่ยงพลาด
7. **Default "Deny"** — ตั้งค่าเริ่มต้นเป็นปฏิเสธ แล้วค่อยให้สิทธิ์
8. **Multiple Layers** — Defense in depth — firewall + hashing + MFA
9. **Use Encryption** — เข้ารหัสทั้ง at rest และ in transit
10. **Quality Checks** — code review, มาตรฐาน, threat modeling

> *อย่าเชื่อ input, ให้สิทธิ์น้อย, เข้ารหัสเสมอ, และทำงานเป็นชั้นๆ*

---

## Chapter 02 — The OWASP Top 10

OWASP (Open Web Application Security Project) จัดอันดับช่องโหว่ตามความถี่ในการพบ ความง่ายในการตรวจจับ ความง่ายในการโจมตี และผลกระทบทางเทคนิค ฉบับ **2021** มีดังนี้

| # | ชื่อ | คำอธิบาย |
|---|---|---|
| 01 | **Broken Access Control** | ผู้ใช้เข้าถึงข้อมูล/ฟังก์ชันที่ไม่ควรเข้าได้ |
| 02 | **Cryptographic Failures** | ไม่ป้องกันข้อมูลด้วยการเข้ารหัสที่เหมาะสม |
| 03 | **Injection** | SQL, Shell, HTML script — ฉีดโค้ดผ่าน input |
| 04 | **Insecure Design** | ช่องโหว่จาก architecture ตั้งแต่ต้น |
| 05 | **Security Misconfiguration** | ซอฟต์แวร์ล้าสมัย, default password, port เปิด |
| 06 | **Vulnerable Components** | Library/framework ที่มีช่องโหว่ |
| 07 | **Auth Failures** | ระบบยืนยันตัวตนอ่อนแอ ไม่มี MFA |
| 08 | **Integrity Failures** | โค้ด/ข้อมูลไม่ถูกตรวจสอบ — SolarWinds |
| 09 | **Logging Failures** | ไม่ log ไม่ monitor ผู้โจมตีอยู่นาน 200+ วัน |
| 10 | **Server-Side Request Forgery** | บังคับ server ยิง request โดยไม่ validate URL |

### ตัวอย่าง Injection — SQL Injection

สมมติแอปใช้ SQL function แบบนี้:

```sql
Query = "SELECT * FROM accounts WHERE custID='" + request.getParameter("id") + "'";
```

ผู้โจมตีใส่ `' or '1'='1` เป็น parameter `id` → query กลายเป็นคืนข้อมูลทั้ง table

**วิธีป้องกัน:** Input validation + Input/Output encoding + ใช้ parameterized queries

> **CSRF vs SSRF — อย่าสับสน:**
> - **CSRF** = หลอก browser ของผู้ใช้ให้ทำ action แทนเขา
> - **SSRF** = หลอก server ของเป้าหมายให้ยิง request ไปที่ภายใน

---

## Chapter 03 — The SDLC

Software Development Life Cycle คือกระบวนการผลิตซอฟต์แวร์ — คุณภาพสูงสุด ต้นทุนต่ำสุด ใช้เวลาน้อยที่สุด

### 5 Phases

| Phase | กิจกรรม Security |
|---|---|
| **01 Requirements** | เก็บความต้องการและกำหนด security requirements ตาม corporate policy/PCI DSS |
| **02 Design** | ออกแบบและทำ risk assessment ของ architecture ที่เสนอ |
| **03 Implementation** | เขียนโค้ดและใช้ automated static source code checking (SAST) |
| **04 Testing / QA** | ทดสอบโดยใช้ OWASP Top 10 / SANS Top 25 เป็นแนวทาง |
| **05 Maintenance / Deployment** | Monitor data leakage 24/7 + ใช้ WAF/IPS ป้องกันชั่วคราว |

> **Cost of fixing:** การแก้ปัญหาบน production แพงกว่าการแก้ตั้งแต่ Design/Requirements **ถึง 100 เท่า** นี่คือเหตุผลที่แนวคิด **"Shift Left"** สำคัญมาก

---

## Chapter 04 — Security in the SDLC

SDLC ที่ปลอดภัยไม่ใช่แค่เรื่องเครื่องมือ แต่ต้องครบทั้งสามด้าน — **People, Processes, Technologies**

### The Human Factor

แม้จะมีเครื่องมือดีแค่ไหน ถ้าคนไม่ได้รับการอบรม security ก็ไม่มีประโยชน์ Security awareness training สำคัญสำหรับ **ทุกคน** ที่เกี่ยวข้องกับการพัฒนา ไม่ใช่แค่ developer

> *Build security in, don't bolt it on*

### กิจกรรมสำคัญในแต่ละ Phase

| Phase | กิจกรรม Security หลัก |
|---|---|
| Requirements | กำหนด security requirements + รายการ misuse cases |
| Design | Risk assessment + ออกแบบ controls |
| Implementation | SAST tools + spot check โค้ด |
| Testing/QA | OWASP Top 10 / SANS Top 25 แยก env |
| Deployment | Monitor, data leak detection, IPS/WAF |

---

## Chapter 05 — PCI DSS

Payment Card Industry Data Security Standard — มาตรฐานความปลอดภัยสำหรับองค์กรที่ process, store หรือ transmit ข้อมูลบัตรเครดิต สร้างโดย Visa, MasterCard, American Express, Discover และ JCB

### ทำไม PCI DSS ถึงสำคัญ?

จาก Verizon 2022 Data Breach Investigations Report:
- **84%** ของเคส data breach เกี่ยวข้องกับ payment card data
- **93%** ของ breach มี financial motive — ทุกคนในระบบ payment คือเป้า

### 6 Goals · 12 Requirements

| Goal | Requirements |
|---|---|
| **1. Build Secure Network** | 1. NSCs (Firewalls) · 2. Secure configurations |
| **2. Protect Data** | 3. Stored data · 4. Cryptography in transit |
| **3. Vulnerability Mgmt** | 5. Anti-malware · 6. Secure development |
| **4. Access Control** | 7. Need-to-know · 8. Auth · 9. Physical |
| **5. Monitor & Test** | 10. Logging · 11. Testing |
| **6. Security Policy** | 12. Organizational policies |

### The Continuous Cycle

1. **Assess** — ระบุที่ตั้งข้อมูลบัตร, inventory IT, วิเคราะห์ช่องโหว่
2. **Remediate** — แก้ไขช่องโหว่, ลบข้อมูลที่ไม่จำเป็น
3. **Report** — เขียนเอกสารและส่งให้ acquirer/payment brands
4. **Monitor & Maintain** — ตรวจสอบ control ตลอดปี

---

## Chapter 06 — PCI DSS Developer's Cut

หาก code ของคุณแตะข้อมูลบัตรเครดิต — นี่คือข้อบังคับที่ต้องปฏิบัติตาม

### Requirement 3 — Protect Stored Account Data

**3.4** Render PAN unreadable ทุกที่ที่ถูกเก็บ — encrypt, hash, truncate หรือ tokenize

### Requirement 6 — Secure Development ⭐

| ข้อ | รายละเอียด |
|---|---|
| **6.1** | มีกระบวนการระบุ vulnerability + จัด risk ranking (high/medium/low) |
| **6.2** | Critical patches ต้องลงภายใน **1 เดือน** หลังออก |
| **6.2.2** | Developer ต้องได้รับการอบรม **ทุก 12 เดือน** |
| **6.3** | พัฒนาตาม industry best practices · ฝัง security ตลอด SDLC |
| **6.4** | แยก dev/test/prod · ไม่ใช้ production data ในการทดสอบ |
| **6.5.1** | อบรม secure coding อย่างน้อย **ปีละครั้ง** |
| **6.6** | Public-facing apps ต้องมี WAF หรือ annual assessment |

### Requirements อื่นที่สำคัญ

| ข้อ | รายละเอียด |
|---|---|
| **8.1** | Policy ให้เฉพาะคนที่ได้รับอนุญาตเข้าถึงระบบ + MFA สำหรับ CDE |
| **10.1** | Audit trail เชื่อมโยงการเข้าถึงทุกครั้งกับ user แต่ละคน |
| **11.3** | Pen test หลังการเปลี่ยนแปลงสำคัญของแอป |
| **12.6** | Security awareness program สำหรับพนักงานทุกคน |

> **PCI DSS v4.0 — สิ่งใหม่:**
> - **Defined Approach** (วิธีดั้งเดิม) vs **Customized Approach** (วิธียืดหยุ่นสำหรับองค์กร risk-mature)
> - บาง requirement เป็น best practice จนถึง **31 March 2025** แล้วบังคับใช้

---

## Chapter 07 — Tooling: SonarQube

เครื่องมือ **Static Application Security Testing (SAST)** ที่สแกนโค้ดหาช่องโหว่ — เป็นตัวอย่างของการนำ secure coding ไปใช้จริงใน CI/CD

### 2 ประเภท Pipeline

| Build Pipeline | Quality Pipeline |
|---|---|
| Compile + test + sonar scan + build + publish package ไป INT/UAT/PROD | Compile + unit test + sonar scan โดยไม่ build — ใช้สแกน feature branch ก่อน merge |

### 3 ประเภท Issue

- **Bug** — ข้อผิดพลาดในโค้ด
- **Vulnerability** — จุดในโค้ดที่เปิดให้ถูกโจมตี (มาจากโค้ดเองหรือ external dependency)
- **Code Smell** — โค้ดไม่สวย/บำรุงรักษายาก

### Security Hotspots

จุดในโค้ดที่ **อ่อนไหวด้าน security** และต้องการ **review จากมนุษย์** ไม่ใช่ vulnerability ที่ยืนยันแล้ว แต่เป็นจุดที่ควรตรวจสอบ ทำผ่าน SonarQube UI

ตัวอย่าง: `printStackTrace()` ใน production code → Sonar เตือนว่า "Make sure this debug feature is deactivated before delivering the code in production"

### Workflow

1. Sonar scan fail → ดู Quality gate ERROR
2. เข้า SonarQube UI ดู failed conditions
3. Vulnerability → แก้โค้ด · Security Hotspot → review manually
4. รัน build ใหม่ → ตรวจให้แน่ใจว่า issue ไม่กลับมา

---

## Quiz — 5 Questions + เฉลย

### Q1 — Security Misconfiguration

**Question:** Out-of-date software, use of default passwords, and open ports are characteristic of which type of vulnerability?

- ✗ A. Cross-site scripting
- ✗ B. Insecure direct object references
- ✅ **C. Security misconfiguration**
- ✗ D. Cross-site request forgery

**คำอธิบาย:** จาก OWASP Top 10 อันดับ 5 — Security Misconfiguration ครอบคลุมการตั้งค่าผิดทุกระดับ ตัวอย่างที่ระบุไว้ตรงเป๊ะ: ซอฟต์แวร์ล้าสมัย, default password, port เปิดโดยไม่จำเป็น

**ทำไมตัวอื่นไม่ใช่:**
- XSS เป็นการฉีด script (กลุ่ม Injection)
- IDOR เกี่ยวกับการเข้าถึง object ผ่าน parameter (Broken Access Control)
- CSRF คือการหลอกผู้ใช้ที่ login แล้วทำ action

> **Key insight:** ถ้าคำถามพูดถึง patch ไม่อัปเดต / default credentials / port เปิด → คิดถึง `misconfiguration` ทันที

---

### Q2 — Server-Side Request Forgery (SSRF)

**Question:** Which of the following is a feature of 'Server-Side Request Forgeries' in terms of secure coding?

- ✗ A. Unauthorized users may access administrative functions
- ✗ B. Allows an attacker to forge digital signatures
- ✗ C. Allows an attacker to make requests on a user's behalf
- ✅ **D. Flaws occurring when a web application fetches a remote resource without validating the user-supplied URL**

**คำอธิบาย:** SSRF (OWASP #10) มีนิยามตรงตัว: "SSRF flaws occur when a web application fetches a remote resource without validating the user-supplied URL" ทำให้ผู้โจมตีบังคับ server ไปยิง request ปลายทางที่ไม่คาดคิด — แม้จะมี firewall ก็ตาม

**ตัวอย่างการโจมตี:**
- Port scan internal network
- เข้าถึง `file:///etc/passwd`
- ดึง cloud metadata จาก `http://169.254.169.254/`

> **⚠️ ระวังกับดัก:** ตัวเลือก C คือ CSRF (Cross-Site Request Forgery) ไม่ใช่ SSRF · CSRF หลอก browser ของผู้ใช้ · SSRF หลอก server ของเป้าหมาย

---

### Q3 — Secure Coding Principles (Multi-Select)

**Question:** Which of the following statements is true? (Select all that apply)

- ✅ **A. When writing secure code, input validation is your first line of defence**
- ✅ **B. Use data sanitization to remove unsafe characters from input data**
- ✅ **C. Run processes with the minimum permissions required to complete them**
- ✗ D. Add complexity to code to minimize the risk of it being compromised

**คำอธิบาย:**

- **A ถูก:** Input validation คือ first line of defence (Secure Coding Tip #1)
- **B ถูก:** Data sanitization ลบอักขระอันตรายผ่าน whitelist/blacklist/escape (Tip #3)
- **C ถูก:** Principle of Least Privilege — สิทธิ์น้อยที่สุดและให้เฉพาะเวลาที่ต้องใช้ (Tip #4)
- **D ผิด — ตรงข้ามกับความจริง!** หลักการ "Keep it Simple" (Tip #6) บอกว่า "ยิ่งซับซ้อน ยิ่งมีโอกาสพลาดและพลาดช่องโหว่" (KISS Principle)

---

### Q4 — PCI DSS Requirement (False Statement)

**Question:** Which of the following statements is false?

- ✅ **A. PCI DSS Requirement 6.2.2 requires that software development personnel receive training at least once every 2 years** ← FALSE
- ✗ B. PCI DSS Requirement 10.1 mandates the use of audit trails to link all access to system components to individual users ← True
- ✗ C. PCI DSS Requirement 3.4 requires that payment card PAN codes should be rendered unreadable anywhere they are stored ← True
- ✗ D. PCI DSS Requirement 8.1 states that policies should be defined and implemented to ensure only authorized personnel have access ← True

**คำอธิบาย:** Requirement 6.2.2 บังคับให้ training **ทุก 12 เดือน** (ปีละครั้ง) ไม่ใช่ทุก 2 ปี — เพราะภัยคุกคามเปลี่ยนแปลงเร็ว รออบรม 2 ปีถือว่านานเกินไป

> **จำง่ายๆ:** PCI DSS เกือบทุก periodic activity → `annual / 12 months` (เช่น 6.2.2, 6.5.1, 12.6)

---

### Q5 — Exploit Definition

**Question:** Which of the following best describes an "exploit"?

- ✗ A. An email-based phishing scam
- ✗ B. An inbuilt weakness in a line of code
- ✅ **C. A method by which a hacker enters the system at a vulnerable location**

**คำอธิบาย:** นิยามตรงตัวจากบทเรียน: "An exploit is a method by which the hacker enters the system at a vulnerable location" — อาจเป็น hacking tool, scanner, malware, script หรือ social engineering

**ทำไมตัวอื่นไม่ใช่:**
- A · Phishing เป็นแค่ตัวอย่างหนึ่ง ไม่ใช่นิยามทั้งหมด
- B · นี่คือนิยามของ **Vulnerability** ไม่ใช่ Exploit

> **สูตรจำ:**
> - Vulnerability = "ช่องโหว่อยู่ที่ไหน" · Exploit = "ใช้ช่องโหว่นั้นยังไง"
> - เปรียบเทียบ: Vuln = ประตูไม่ล็อก · Exploit = การเดินผ่านประตูเข้าไป

---

*End of Compendium · Secure Coding 2025*

*Build security in, don't bolt it on*
