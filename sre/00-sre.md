# SRE (Site Reliability Engineering) / ความน่าเชื่อถือของไซต์วิศวกรรม

> SRE is a discipline that combines software engineering and operations to create scalable and highly reliable systems. It was pioneered by Google.
>
> SRE คือวินัยที่รวมวิศวกรรมซอฟต์แวร์และการดำเนินงานเข้าด้วยกันเพื่อสร้างระบบที่มีความน่าเชื่อถือสูงและสามารถขยายขนาดได้ เริ่มต้นโดย Google

---

## Table of Contents / สารบัญ

1. [What is SRE? / SRE คืออะไร?](#what-is-sre--sre-คืออะไร)
2. [SRE vs DevOps / SRE เทียบกับ DevOps](#sre-vs-devops--sre-เทียบกับ-devops)
3. [Key Concepts / แนวคิดหลัก](#key-concepts--แนวคิดหลัก)
   - [SLI (Service Level Indicator)](#sli-service-level-indicator--ตัวชี้วัดระดับบริการ)
   - [SLO (Service Level Objective)](#slo-service-level-objective--วัตถุประสงค์ระดับบริการ)
   - [SLA (Service Level Agreement)](#sla-service-level-agreement--ข้อตกลงระดับบริการ)
4. [Error Budget / งบประมาณข้อผิดพลาด](#error-budget--งบประมาณข้อผิดพลาด)
5. [The Four Golden Signals / สัญญาณทองสี่ประการ](#the-four-golden-signals--สัญญาณทองสี่ประการ)
6. [Additional Signals / สัญญาณเพิ่มเติม](#additional-signals--สัญญาณเพิ่มเติม)
7. [SRE Practices / แนวปฏิบัติ SRE](#sre-practices--แนวปฏิบัติ-sre)
8. [Tools / เครื่องมือ](#tools--เครื่องมือ)
9. [Example: Prometheus Configuration / ตัวอย่าง: การกำหนดค่า Prometheus](#example-prometheus-configuration--ตัวอย่างการกำหนดค่า-prometheus)
10. [Example: Alert Rules / ตัวอย่าง: กฎการแจ้งเตือน](#example-alert-rules--ตัวอย่างกฎการแจ้งเตือน)
11. [SRE Mindset / แนวคิด SRE](#sre-mindset--แนวคิด-sre)
12. [Practice Exercises / แบบฝึกหัด](#practice-exercises--แบบฝึกหัด)
13. [Resources / แหล่งข้อมูล](#resources--แหล่งข้อมูล)

---

## What is SRE? / SRE คืออะไร?

SRE (Site Reliability Engineering) is a discipline that combines software engineering and IT operations to create scalable and highly reliable systems. It was pioneered by Google around 2003 to solve the problem of managing massive-scale systems while maintaining rapid innovation.

SRE (Site Reliability Engineering) คือวินัยที่รวมวิศวกรรมซอฟต์แวร์และการดำเนินงานด้านไอทีเข้าด้วยกันเพื่อสร้างระบบที่มีความน่าเชื่อถือสูงและสามารถขยายขนาดได้ เริ่มต้นโดย Google เมื่อประมาณปี 2003 เพื่อแก้ไขปัญหาการจัดการระบบขนาดใหญ่ในขณะที่รักษาการนวัตกรรมอย่างรวดเร็ว

### Core Principles / หลักการพื้นฐาน

- **Reliability is the primary goal** / ความน่าเชื่อถือคือเป้าหมายหลัก
- **Accept that failure will happen** / ยอมรับว่าความล้มเหลวจะเกิดขึ้น
- **Embrace change carefully** / ยอมรับการเปลี่ยนแปลงอย่างระมัดระวัง
- **Measure everything** / วัดทุกอย่าง
- **Automate relentlessly** / ทำระบบอัตโนมัติอย่างไม่หยุดยั้ง
- **Share ownership between development and operations** / แบ่งปันความรับผิดชอบระหว่างทีมพัฒนาและทีมดำเนินการ

### The SRE Team / ทีม SRE

SRE teams are composed of software engineers who specialize in operations. They typically spend:

ทีม SRE ประกอบด้วยวิศวกรซอฟต์แวร์ที่เชี่ยวชาญด้านการดำเนินงาน โดยทั่วไปพวกเขาใช้เวลา:

- **50% on development** (automation, tooling, system improvements) / 50% กับการพัฒนา (ระบบอัตโนมัติ, เครื่องมือ, การปรับปรุงระบบ)
- **50% on operations** (on-call, incidents, manual tasks) / 50% กับการดำเนินงาน (เวรเฝ้าระวัง, เหตุการณ์, งานด้วยมือ)

> **Note / หมายเหตุ:** If operational work exceeds 50%, the SRE team should push back and require development teams to fix reliability issues before taking on more operational work.
>
> **หมายเหตุ:** หากงานดำเนินการเกิน 50% ทีม SRE ควรผลักดันและต้องการให้ทีมพัฒนาแก้ไขปัญหาความน่าเชื่อถือก่อนที่จะรับงานดำเนินการเพิ่มเติม

---

## SRE vs DevOps / SRE เทียบกับ DevOps

While SRE and DevOps share the same goals (faster, more reliable software delivery), they approach the problem differently:

แม้ว่า SRE และ DevOps จะมีเป้าหมายเดียวกัน (การส่งมอบซอฟต์แวร์ที่รวดเร็วและน่าเชื่อถือมากขึ้น) แต่พวกเขามีวิธีการที่แตกต่างกัน:

| Aspect / ด้าน | DevOps / เดฟออปส์ | SRE / เอสอาร์อี |
|---------------|-------------------|-----------------|
| **Focus / จุดเน้น** | Culture & processes / วัฒนธรรมและกระบวนการ | Engineering approach / วิธีการทางวิศวกรรม |
| **Implementation / การนำไปใช้** | Organizational change / การเปลี่ยนแปลงองค์กร | Prescriptive practices / แนวปฏิบัติที่กำหนดไว้ |
| **Team / ทีม** | Cross-functional / ทีมข้ามหน้าที่ | Specialized engineers / วิศวกรผู้เชี่ยวชาญ |
| **Metric / ตัวชี้วัด** | Deployment frequency / ความถี่ในการปรับใช้ | Error budget consumption / การใช้งบประมาณข้อผิดพลาด |
| **Philosophy / ปรัชญา** | "Break down silos" / "ทำลายกำแพงระหว่างทีม" | "Treat operations as a software problem" / "จัดการการดำเนินงานเป็นปัญหาซอฟต์แวร์" |
| **Scope / ขอบเขต** | Entire SDLC / วงจรการพัฒนาซอฟต์แวร์ทั้งหมด | Production systems / ระบบการผลิต |

### Relationship / ความสัมพันธ์

- **DevOps** tells you *what* to do (cultural shift, collaboration) / DevOps บอกคุณว่า *ต้องทำอะไร* (การเปลี่ยนแปลงวัฒนธรรม, การทำงานร่วมกัน)
- **SRE** tells you *how* to do it (specific practices, tools, metrics) / SRE บอกคุณว่า *ทำอย่างไร* (แนวปฏิบัติเฉพาะ, เครื่องมือ, ตัวชี้วัด)

> Think of DevOps as the philosophy and SRE as the implementation.
>
> คิดว่า DevOps เป็นปรัชญาและ SRE เป็นการนำไปใช้จริง

---

## Key Concepts / แนวคิดหลัก

### SLI (Service Level Indicator) / ตัวชี้วัดระดับบริการ

**What you measure** / **สิ่งที่คุณวัด**

An SLI is a quantitative measure of some aspect of service behavior. It answers the question: "How is the service performing right now?"

SLI คือตัววัดเชิงปริมาณของบางแง่มุมของพฤติกรรมบริการ มันตอบคำถามว่า: "บริการมีผลงานอย่างไรตอนนี้?"

#### Characteristics of Good SLIs / ลักษณะของ SLI ที่ดี

- **User-centric** / มุ่งเน้นผู้ใช้: Reflect what users actually experience / สะท้อนสิ่งที่ผู้ใช้สัมผัสจริง
- **Actionable** / สามารถดำเนินการได้: You can influence the metric / คุณสามารถส่งผลต่อตัวชี้วัดได้
- **Measurable** / สามารถวัดได้: You can collect the data accurately / คุณสามารถรวบรวมข้อมูลได้อย่างแม่นยำ

#### Examples of SLIs / ตัวอย่างของ SLIs

| SLI / ตัวชี้วัด | Description / คำอธิบาย | Example / ตัวอย่าง |
|-----------------|------------------------|---------------------|
| Latency / ความล่าช้า | Response time for requests / เวลาตอบสนองสำหรับคำขอ | 99% of requests respond in <200ms / 99% ของคำขอตอบสนองใน <200ms |
| Availability / ความพร้อมใช้งาน | Service is reachable and functional / บริการเข้าถึงได้และใช้งานได้ | 99.95% availability / ความพร้อมใช้งาน 99.95% |
| Error Rate / อัตราข้อผิดพลาด | Percentage of failed requests / เปอร์เซ็นต์ของคำขอที่ล้มเหลว | Error rate < 0.1% / อัตราข้อผิดพลาด < 0.1% |
| Throughput / ปริมาณงาน | Requests processed per second / คำขอที่ประมวลผลต่อวินาที | 10,000 req/s sustained / รองรับ 10,000 คำขอ/วินาที |
| Freshness / ความสดใหม่ | How up-to-date data is / ข้อมูลทันสมัยแค่ไหน | Data is < 5 seconds old / ข้อมูลมีอายุ < 5 วินาที |
| Correctness / ความถูกต้อง | Results are accurate / ผลลัพธ์ถูกต้อง | 99.99% of results correct / 99.99% ของผลลัพธ์ถูกต้อง |

---

### SLO (Service Level Objective) / วัตถุประสงค์ระดับบริการ

**Your target for an SLI** / **เป้าหมายของคุณสำหรับ SLI**

An SLO is a target value or range for an SLI. It represents an internal reliability goal that the team commits to achieving.

SLO คือเป้าหมายหรือช่วงเป้าหมายสำหรับ SLI มันแสดงถึงเป้าหมายความน่าเชื่อถือภายในที่ทีมมุ่งมั่นที่จะบรรลุ

#### Principles of Good SLOs / หลักการของ SLO ที่ดี

- **Not too strict, not too loose** / ไม่เข้มงวดเกินไป ไม่หลวมเกินไป: Leave room for innovation / เว้นที่สำหรับนวัตกรรม
- **Based on user expectations** / ขึ้นอยู่กับความคาดหวังของผู้ใช้: What users notice matters / สิ่งที่ผู้ใช้สังเกตเห็นคือสิ่งสำคัญ
- **Agreed upon by stakeholders** / ตกลงโดยผู้มีส่วนได้ส่วนเสีย: Everyone must buy in / ทุกคนต้องเห็นด้วย
- **Reviewed regularly** / ทบทวนเป็นประจำ: Adjust as needs change / ปรับเปลี่ยนตามความต้องการที่เปลี่ยนไป

#### Examples of SLOs / ตัวอย่างของ SLOs

| SLO / วัตถุประสงค์ | Description / คำอธิบาย | Time Window / ช่วงเวลา |
|--------------------|------------------------|------------------------|
| Uptime / เวลาทำงาน | 99.9% uptime per month / ความพร้อมใช้งาน 99.9% ต่อเดือน | Monthly / รายเดือน |
| Latency / ความล่าช้า | p99 latency < 500ms / ความล่าช้า p99 < 500ms | Rolling 30 days / 30 วันแบบกลิ้ง |
| Success Rate / อัตราความสำเร็จ | Success rate > 99.95% / อัตราความสำเร็จ > 99.95% | Quarterly / รายไตรมาส |
| Durability / ความทนทาน | 99.999999999% (11 nines) data durability / ความทนทานข้อมูล 11 ตัวเก้า | Yearly / รายปี |

#### Choosing SLO Targets / การเลือกเป้าหมาย SLO

```
SLO Target Selection Guide / คู่มือการเลือกเป้าหมาย SLO

100%    = Impossible, don't try / เป็นไปไม่ได้ อย่าพยายาม
99.99%  = Critical services (payments, auth) / บริการสำคัญ (การชำระเงิน, การยืนยันตัวตน)
99.95%  = Important services (APIs, core features) / บริการสำคัญ (APIs, คุณสมบัติหลัก)
99.9%   = Standard services (most web apps) / บริการมาตรฐาน (เว็บแอปส่วนใหญ่)
99.5%   = Internal services, batch jobs / บริการภายใน, งานแบทช์
99%     = Non-critical, best-effort services / ไม่สำคัญ, บริการความพยายามที่ดีที่สุด
```

---

### SLA (Service Level Agreement) / ข้อตกลงระดับบริการ

**Contract with users** / **สัญญากับผู้ใช้**

An SLA is a formal contract between the service provider and users. It defines the consequences (usually financial) if the service fails to meet the agreed-upon level.

SLA คือสัญญาระหว่างผู้ให้บริการและผู้ใช้ กำหนดผลที่ตามมา (โดยทั่วไปเป็นการเงิน) หากบริการไม่บรรลุระดับที่ตกลงไว้

#### Key Differences from SLO / ความแตกต่างหลักจาก SLO

| Aspect / ด้าน | SLO / วัตถุประสงค์ | SLA / ข้อตกลง |
|---------------|-------------------|---------------|
| **Audience / ผู้ชม** | Internal team / ทีมภายใน | External customers / ลูกค้าภายนอก |
| **Consequences / ผลที่ตามมา** | Process changes / การเปลี่ยนแปลงกระบวนการ | Financial penalties / ค่าปรับทางการเงิน |
| **Strictness / ความเข้มงวด** | More aggressive (harder target) / เข้มงวดกว่า (เป้าหมายยากกว่า) | Less strict (easier to meet) / เข้มน้อยกว่า (บรรลุได้ง่ายกว่า) |
| **Flexibility / ความยืดหยุ่น** | Can be adjusted easily / ปรับเปลี่ยนได้ง่าย | Requires legal review / ต้องการตรวจสอบทางกฎหมาย |

#### Example SLA / ตัวอย่าง SLA

```
SLA Example / ตัวอย่าง SLA
==========================

Guaranteed Uptime / ความพร้อมใช้งานที่รับประกัน: 99.5% per month / 99.5% ต่อเดือน

Consequences / ผลที่ตามมา:
  - 99.5% - 99.9%: No penalty / ไม่มีค่าปรับ
  - 99.0% - 99.5%: 10% credit / เครดิต 10%
  - 95.0% - 99.0%: 25% credit / เครดิต 25%
  - Below 95%: 50% credit + right to terminate / เครดิต 50% + สิทธิ์ในการยกเลิก
```

### Relationship Diagram / แผนภาพความสัมพันธ์

```
SLI → Measure (What we measure) / ตัววัด (สิ่งที่เราวัด)
  ↓
SLO → Target (Internal goal) / เป้าหมาย (เป้าหมายภายใน)
  ↓
SLA → Contract (External, with consequences) / สัญญา (ภายนอก, มีผลที่ตามมา)

Example / ตัวอย่าง:
  SLI: Current uptime is 99.95% / ความพร้อมใช้งานปัจจุบันคือ 99.95%
  SLO: Target 99.9% uptime / เป้าหมายความพร้อมใช้งาน 99.9%
  SLA: Guarantee 99.5% uptime (or refund) / รับประกัน 99.5% (หรือคืนเงิน)
```

> **Important / สำคัญ:** Your SLO should always be stricter than your SLA. This creates a buffer zone where you can take action before facing penalties.
>
> **สำคัญ:** SLO ของคุณควรเข้มงวดกว่า SLA เสมอ สิ่งนี้จะสร้างเขตกันชนที่คุณสามารถดำเนินการก่อนที่จะเผชิญกับค่าปรับ

---

## Error Budget / งบประมาณข้อผิดพลาด

The Error Budget is the amount of unreliability you are allowed before violating your SLO. It is the foundation of SRE decision-making.

งบประมาณข้อผิดพลาดคือจำนวนความไม่น่าเชื่อถือที่คุณได้รับอนุญาตก่อนที่จะละเมิด SLO มันเป็นพื้นฐานของการตัดสินใจ SRE

### Calculation / การคำนวณ

```
Error Budget = 100% - SLO

Example / ตัวอย่าง:
  SLO = 99.9%
  Error Budget = 100% - 99.9% = 0.1%

  In a 30-day month:
    Total minutes = 30 × 24 × 60 = 43,200 minutes / นาทีทั้งหมด
    Allowed downtime = 43,200 × 0.001 = 43.2 minutes / เวลาล้มเหลวที่อนุญาต
```

### Error Budget Table / ตารางงบประมาณข้อผิดพลาด

| SLO / SLO | Error Budget / งบประมาณข้อผิดพลาด | Allowed Downtime / เวลาล้มเหลวที่อนุญาต |
|-----------|-----------------------------------|---------------------------------------|
| 99.99% | 0.01% | 4.32 minutes/month / 4.32 นาที/เดือน |
| 99.95% | 0.05% | 21.6 minutes/month / 21.6 นาที/เดือน |
| 99.9% | 0.1% | 43.2 minutes/month / 43.2 นาที/เดือน |
| 99.5% | 0.5% | 3.6 hours/month / 3.6 ชั่วโมง/เดือน |
| 99% | 1% | 7.2 hours/month / 7.2 ชั่วโมง/เดือน |

### Error Budget Policy / นโยบายงบประมาณข้อผิดพลาด

The error budget policy determines how teams should act based on remaining budget:

นโยบายงบประมาณข้อผิดพลาดกำหนดว่าทีมควรดำเนินการอย่างไรตามงบประมาณที่เหลืออยู่:

```
Error Budget Decision Flow / ขั้นตอนการตัดสินใจงบประมาณข้อผิดพลาด
==================================================================

Is error budget remaining? / ยังมีงบประมาณข้อผิดพลาดเหลืออยู่หรือไม่?
  │
  ├── YES / ใช่
  │    ├── Ship new features / ปล่อยคุณสมบัติใหม่
  │    ├── Take calculated risks / รับความเสี่ยงที่คำนวณได้
  │    └── Innovate and experiment / นวัตกรรมและทดลอง
  │
  └── NO / ไม่
       ├── Halt new feature releases / หยุดการปล่อยคุณสมบัติใหม่
       ├── Focus entirely on reliability / มุ่งเน้นความน่าเชื่อถือเท่านั้น
       ├── Fix existing issues / แก้ไขปัญหาที่มีอยู่
       └── Resume only when reliability improves / เริ่มใหม่เมื่อความน่าเชื่อถือดีขึ้น
```

### Why Error Budgets Matter / ทำไมงบประมาณข้อผิดพลาดจึงสำคัญ

1. **Balances speed vs stability** / สร้างสมดุลระหว่างความเร็วและความมั่นคง
2. **Removes emotion from release decisions** / ลบอารมณ์จากการตัดสินใจปล่อย
3. **Creates shared accountability** / สร้างความรับผิดชอบร่วมกัน
4. **Enables data-driven conversations** / เปิดโอกาสให้มีการสนทนาที่ขับเคลื่อนด้วยข้อมูล
5. **Prevents both over-engineering and recklessness** / ป้องกันทั้งการวิศวกรรมมากเกินไปและความประมาท

> **Key Insight / ข้อมูลเชิงลึกหลัก:** A team with 100% uptime is probably moving too slowly. Use your error budget!
>
> **ข้อมูลเชิงลึกหลัก:** ทีมที่มีเวลาทำงาน 100% อาจกำลังเคลื่อนที่ช้าเกินไป ใช้งบประมาณข้อผิดพลาดของคุณ!

---

## The Four Golden Signals / สัญญาณทองสี่ประการ

Google's SRE team identified four critical signals that should be monitored for every service. These are called the "Four Golden Signals."

ทีม SRE ของ Google ระบุสัญญาณสำคัญสี่ประการที่ควรตรวจสอบสำหรับทุกบริการ สิ่งเหล่านี้เรียกว่า "สัญญาณทองสี่ประการ"

### 1. Latency / ความล่าช้า

**Time it takes to serve a request** / **เวลาที่ใช้ในการให้บริการคำขอ**

Latency measures how long it takes for a system to respond to a request. It is one of the most user-perceptible metrics.

ความล่าขาวัดว่าระบบใช้เวลานานเท่าใดในการตอบสนองต่อคำขอ มันเป็นหนึ่งในตัวชี้วัดที่ผู้ใช้สัมผัสได้มากที่สุด

#### How to Measure / วิธีการวัด

- **Use percentiles, not averages** / ใช้เปอร์เซ็นไทล์ ไม่ใช่ค่าเฉลี่ย: Averages hide outliers / ค่าเฉลี่ยซ่อนค่าผิดปกติ
- **Track p50, p95, p99** / ติดตาม p50, p95, p99: Different percentiles show different experiences / เปอร์เซ็นไทล์ต่างแสดงประสบการณ์ที่ต่างกัน

```
Percentile Explanation / คำอธิบายเปอร์เซ็นไทล์
==============================================

p50 (median) / p50 (มัธยฐาน):
  - 50% of requests are faster than this / 50% ของคำขอเร็วกว่านี้
  - Represents "typical" user experience / แสดงประสบการณ์ผู้ใช้ "ทั่วไป"

p95:
  - 95% of requests are faster than this / 95% ของคำขอเร็วกว่านี้
  - Represents "unlucky" user experience / แสดงประสบการณ์ผู้ใช้ "โชคไม่ดี"

p99:
  - 99% of requests are faster than this / 99% ของคำขอเร็วกว่านี้
  - Represents "very unlucky" user experience / แสดงประสบการณ์ผู้ใช้ "โชคไม่ดีมาก"
  - Critical for understanding tail latency / สำคัญสำหรับการเข้าใจความล่าช้าส่วนท้าย
```

#### Example / ตัวอย่าง

```
API Latency SLO / SLO ความล่าช้า API
=====================================

Target / เป้าหมาย: p95 latency < 200ms

Current readings / การอ่านปัจจุบัน:
  p50  = 45ms   (Good / ดี)
  p95  = 180ms  (Within SLO / อยู่ใน SLO)
  p99  = 850ms  (Investigate tail latency / สอบสวนความล่าช้าส่วนท้าย)

Alert thresholds / เกณฑ์การแจ้งเตือน:
  - Warning / คำเตือน: p95 > 150ms for 5 minutes / p95 > 150ms เป็นเวลา 5 นาที
  - Critical / วิกฤต: p95 > 300ms for 2 minutes / p95 > 300ms เป็นเวลา 2 นาที
```

---

### 2. Traffic / การจราจร

**How much demand is placed on your system** / **ความต้องการที่วางอยู่บนระบบของคุณ**

Traffic measures the volume of requests or load your system is handling. It helps you understand capacity needs and detect anomalies.

การจราดาวัดปริมาณคำขอหรือโหลดที่ระบบของคุณจัดการ ช่วยให้คุณเข้าใจความต้องการความจุและตรวจจับความผิดปกติ

#### How to Measure / วิธีการวัด

- **HTTP services** / บริการ HTTP: Requests per second / คำขอต่อวินาที
- **TCP services** / บริการ TCP: Connections per second / การเชื่อมต่อต่อวินาที
- **Queue-based systems** / ระบบคิว: Queue depth / ความลึกของคิว
- **User-facing** / ผู้ใช้-facing: Concurrent users / ผู้ใช้พร้อมกัน

#### Example / ตัวอย่าง

```
Traffic SLO / SLO การจราจร
===========================

Target / เป้าหมาย: Handle 10,000 req/s with acceptable latency
                   / รองรับ 10,000 คำขอ/วินาทีด้วยความล่าช้าที่ยอมรับได้

Current readings / การอ่านปัจจุบัน:
  - Average / เฉลี่ย: 5,000 req/s
  - Peak / สูงสุด: 8,500 req/s
  - Capacity limit / ขีดจำกัดความจุ: 12,000 req/s

Growth rate / อัตราการเติบโต: +15% month-over-month / +15% ต่อเดือน
Projected capacity breach / คาดการณ์การเกินความจุ: ~3 months / ~3 เดือน
```

#### Why Traffic Matters / ทำไมการจราจรจึงสำคัญ

- Detects **DDoS attacks** / ตรวจจับการโจมตี DDoS
- Identifies **usage patterns** / ระบุรูปแบบการใช้งาน
- Guides **capacity planning** / นำทางการวางแผนความจุ
- Triggers **auto-scaling** / เปิดใช้งานการขยายขนาดอัตโนมัติ

---

### 3. Errors / ข้อผิดพลาด

**The rate of failed requests** / **อัตราของคำขอที่ล้มเหลว**

Errors measure the rate at which requests fail. This includes explicit failures (HTTP 5xx) and implicit failures (wrong results, slow responses).

ข้อผิดพลาดวัดอัตราที่คำขอล้มเหลว รวมถึงความล้มเหลวชัดเจน (HTTP 5xx) และความล้มเหลวโดยนัย (ผลลัพธ์ผิด, การตอบสนองช้า)

#### Types of Errors / ประเภทของข้อผิดพลาด

```
Error Types / ประเภทข้อผิดพลาด
==============================

1. Explicit Errors / ข้อผิดพลาดชัดเจน
   - HTTP 5xx responses / การตอบสนอง HTTP 5xx
   - Connection refused / การเชื่อมต่อถูกปฏิเสธ
   - Timeout errors / ข้อผิดพลาดหมดเวลา

2. Implicit Errors / ข้อผิดพลาดโดยนัย
   - Wrong results returned / ผลลัพธ์ผิดถูกส่งกลับ
   - Partial data / ข้อมูลบางส่วน
   - Stale data / ข้อมูลเก่า

3. Policy Errors / ข้อผิดพลาดนโยบาย
   - Response doesn't meet SLO / การตอบสนองไม่ตรงตาม SLO
   - Violates content policy / ละเมิดนโยบายเนื้อหา
```

#### Example / ตัวอย่าง

```
Error Rate SLO / SLO อัตราข้อผิดพลาด
====================================

Target / เป้าหมาย: Error rate < 0.1% (1 in 1,000 requests)
                   / อัตราข้อผิดพลาด < 0.1% (1 ใน 1,000 คำขอ)

Current readings / การอ่านปัจจุบัน:
  - Last 5 minutes / 5 นาทีล่าสุด: 0.05% (50 errors / 100,000 requests)
  - Last 1 hour / 1 ชั่วโมงล่าสุด: 0.08%
  - Last 24 hours / 24 ชั่วโมงล่าสุด: 0.03%

Error breakdown / แบ่งแยกข้อผิดพลาด:
  - 5xx errors / ข้อผิดพลาด 5xx: 70%
  - Timeouts / หมดเวลา: 20%
  - Other errors / ข้อผิดพลาดอื่น: 10%

Alert thresholds / เกณฑ์การแจ้งเตือน:
  - Warning / คำเตือน: Error rate > 0.05% for 5 minutes / อัตราข้อผิดพลาด > 0.05% เป็นเวลา 5 นาที
  - Critical / วิกฤต: Error rate > 0.5% for 2 minutes / อัตราข้อผิดพลาด > 0.5% เป็นเวลา 2 นาที
```

---

### 4. Saturation / ความอิ่มตัว

**How "full" your system is** / **ระบบของคุณ "เต็ม" แค่ไหน**

Saturation measures how close your system is to its maximum capacity. It helps predict problems before they occur.

ความอิ่มตัววัดว่าระบบของคุณอยู่ใกล้ความจุสูงสุดแค่ไหน ช่วยคาดการณ์ปัญหาก่อนที่จะเกิดขึ้น

#### Key Resources to Monitor / ทรัพยากรสำคัญที่ต้องตรวจสอบ

```
Saturation Metrics / ตัวชี้วัดความอิ่มตัว
========================================

1. CPU / ซีพียู
   - Measure: Utilization percentage / วัด: เปอร์เซ็นต์การใช้งาน
   - Warning / คำเตือน: > 70% sustained / > 70% ต่อเนื่อง
   - Critical / วิกฤต: > 90% sustained / > 90% ต่อเนื่อง

2. Memory / หน่วยความจำ
   - Measure: Used vs available / วัด: ใช้เทียบกับที่มี
   - Warning / คำเตือน: > 80% used / ใช้ > 80%
   - Critical / วิกฤต: > 95% used (OOM risk) / ใช้ > 95% (ความเสี่ยง OOM)

3. Disk / ดิสก์
   - Measure: Space used, IOPS / วัด: พื้นที่ที่ใช้, IOPS
   - Warning / คำเตือน: > 75% used / ใช้ > 75%
   - Critical / วิกฤต: > 90% used / ใช้ > 90%

4. Network / เครือข่าย
   - Measure: Bandwidth utilization, packet loss / วัด: การใช้แบนด์วิดท์, การสูญเสียแพ็กเก็ต
   - Warning / คำเตือน: > 60% bandwidth / > 60% แบนด์วิดท์
   - Critical / วิกฤต: > 80% bandwidth / > 80% แบนด์วิดท์

5. Application-specific / เฉพาะแอปพลิเคชัน
   - Thread pools, connection pools, queue depths / กลุ่มเธรด, กลุ่มการเชื่อมต่อ, ความลึกคิว
```

#### Example / ตัวอย่าง

```
Saturation SLO / SLO ความอิ่มตัว
================================

Target / เป้าหมาย: No resource exceeds 80% utilization under normal load
                   / ไม่มีทรัพยากรเกิน 80% ภายใต้โหลดปกติ

Current readings / การอ่านปัจจุบัน:
  - CPU utilization / การใช้ CPU: 65% (OK / ใช้ได้)
  - Memory usage / การใช้หน่วยความจำ: 78% (Warning / คำเตือน)
  - Disk usage / การใช้ดิสก์: 72% (OK / ใช้ได้)
  - Network bandwidth / แบนด์วิดท์เครือข่าย: 45% (OK / ใช้ได้)
  - Connection pool saturation / ความอิ่มตัวกลุ่มการเชื่อมต่อ: 60% (OK / ใช้ได้)

Action items / รายการดำเนินการ:
  - Investigate memory usage trend / สอบทานแนวโน้มการใช้หน่วยความจำ
  - Plan memory upgrade within 30 days / วางแผนอัปเกรดหน่วยความจำภายใน 30 วัน
```

> **Important / สำคัญ:** Monitor saturation trends, not just current values. A resource at 70% that grows 5% per week will be full in 6 weeks!
>
> **สำคัญ:** ตรวจสอบแนวโน้มความอิ่มตัว ไม่ใช่แค่ค่าปัจจุบัน ทรัพยากรที่ 70% ที่เติบโต 5% ต่อสัปดาห์จะเต็มใน 6 สัปดาห์!

---

## Additional Signals / สัญญาณเพิ่มเติม

While the Four Golden Signals cover most scenarios, some services require additional monitoring:

ในขณะที่สัญญาณทองสี่ประการครอบคลุมสถานการณ์ส่วนใหญ่ แต่บางบริการต้องการการตรวจสอบเพิ่มเติม:

### Availability / ความพร้อมใช้งาน

**Is the service reachable and functional?** / **บริการเข้าถึงได้และใช้งานได้หรือไม่?**

- Measure: Uptime percentage (e.g., 99.9%) / วัด: เปอร์เซ็นต์เวลาทำงาน (เช่น 99.9%)
- Check: Can users access the service? / ตรวจสอบ: ผู้ใช้สามารถเข้าถึงบริการได้หรือไม่?
- Check: Does the service return valid responses? / ตรวจสอบ: บริการส่งกลับการตอบสนองที่ถูกต้องหรือไม่?

```
Availability Calculation / การคำนวณความพร้อมใช้งาน
==================================================

Monthly uptime calculation / การคำนวณเวลาทำงานรายเดือน:
  Total minutes in month = 30 × 24 × 60 = 43,200 minutes
                          = 30 × 24 × 60 = 43,200 นาที

  99.9% uptime = 43,200 × 0.999 = 43,156.8 minutes up
                = 43,200 × 0.999 = 43,156.8 นาทีใช้งานได้
  Allowed downtime = 43.2 minutes/month
                   = 43.2 นาที/เดือน
```

### Throughput / ปริมาณงาน

**How much work is the system completing?** / **ระบบทำงานเสร็จมากแค่ไหน?**

- Measure: Requests/second, transactions/second, bytes/second / วัด: คำขอ/วินาที, ธุรกรรม/วินาที, ไบต์/วินาที
- Useful for: Batch processing, data pipelines, message queues / มีประโยชน์สำหรับ: การประมวลผลแบทช์, ไปป์ไลน์ข้อมูล, คิวข้อความ

### Durability / ความทนทาน

**Is data preserved and not lost?** / **ข้อมูลได้รับการเก็บรักษาและไม่สูญหายหรือไม่?**

- Measure: Backup success rate, replication factor / วัด: อัตราความสำเร็จการสำรอง, ปัจจัยการทำซ้ำ
- Critical for: Databases, storage systems, file services / สำคัญสำหรับ: ฐานข้อมูล, ระบบจัดเก็บ, บริการไฟล์
- Target: Often "11 nines" (99.999999999%) / เป้าหมาย: มักเป็น "11 ตัวเก้า" (99.999999999%)

### Correctness / ความถูกต้อง

**Are the results accurate?** / **ผลลัพธ์ถูกต้องหรือไม่?**

- Measure: Percentage of correct results / วัด: เปอร์เซ็นต์ของผลลัพธ์ที่ถูกต้อง
- Hard to measure automatically, often requires validation checks / ยากที่จะวัดอัตโนมัติ มักต้องการการตรวจสอบความถูกต้อง

---

## SRE Practices / แนวปฏิบัติ SRE

### Toil Reduction / การลดงานซ้ำซ้อน

**What is Toil? / งานซ้ำซ้อนคืออะไร?**

Toil is work that is: / งานซ้ำซ้อนคืองานที่:

- **Manual** / ด้วยมือ: Requires human interaction / ต้องการการโต้ตอบของมนุษย์
- **Repetitive** / ซ้ำซ้อน: Done the same way every time / ทำวิธีเดิมทุกครั้ง
- **Automatable** / สามารถทำอัตโนมัติได้: Could be done by a script / สามารถทำด้วยสคริปต์ได้
- **Tactical** / เฉพาะหน้า: No long-term value / ไม่มีมูลค่าระยะยาว
- **Scales linearly** / ขยายเชิงเส้น: More services = more work / บริการมากขึ้น = งานมากขึ้น

#### Toil Reduction Strategies / กลยุทธ์การลดงานซ้ำซ้อน

```
Toil Reduction Framework / กรอบการลดงานซ้ำซ้อน
==============================================

1. Identify Toil / ระบุงานซ้ำซ้อน
   - Track time spent on operational tasks / ติดตามเวลาที่ใช้กับงานดำเนินการ
   - Categorize: manual, repetitive, automatable / แบ่งประเภท: ด้วยมือ, ซ้ำซ้อน, สามารถทำอัตโนมัติได้

2. Prioritize / จัดลำดับความสำคัญ
   - High frequency, low complexity first / ความถี่สูง, ความซับซ้อนต่ำก่อน
   - Calculate time savings / คำนวณการประหยัดเวลา

3. Automate / ทำให้เป็นอัตโนมัติ
   - Write scripts and tools / เขียนสคริปต์และเครื่องมือ
   - Build self-healing systems / สร้างระบบที่ซ่อมแซมตัวเองได้
   - Implement auto-scaling / ใช้การขยายขนาดอัตโนมัติ

4. Measure Impact / วัดผลกระทบ
   - Track time saved / ติดตามเวลาที่ประหยัดได้
   - Report to management / รายงานให้ผู้บริหารทราบ

Target / เป้าหมาย: < 50% of time spent on toil / < 50% ของเวลาที่ใช้กับงานซ้ำซ้อน
```

#### Examples of Toil Reduction / ตัวอย่างการลดงานซ้ำซ้อน

| Toil / งานซ้ำซ้อน | Automation / ระบบอัตโนมัติ |
|-------------------|---------------------------|
| Manual server provisioning / การเตรียมเซิร์ฟเวอร์ด้วยมือ | Infrastructure as Code (Terraform) / โค้ดโครงสร้างพื้นฐาน (Terraform) |
| Log rotation / การหมุนบันทึก | Automated log management / การจัดการบันทึกอัตโนมัติ |
| Certificate renewal / การต่ออายุใบรับรอง | Automated cert management (cert-manager) / การจัดการใบรับรองอัตโนมัติ |
| Database backups / การสำรองฐานข้อมูล | Automated backup pipelines / ไปป์ไลน์สำรองอัตโนมัติ |
| User access requests / คำขอเข้าถึงผู้ใช้ | Self-service portal / พอร์ทัลบริการตัวเอง |
| Alert triage / การคัดเลือกการแจ้งเตือน | Automated runbooks / สมุดงานอัตโนมัติ |

---

### Blameless Postmortems / การวิเคราะห์หลังเหตุการณ์โดยไม่กล่าวโทษ

**What is a Blameless Postmortem? / การวิเคราะห์หลังเหตุการณ์โดยไม่กล่าวโทษคืออะไร?**

A blameless postmortem is a written analysis of an incident that focuses on **what happened, why it happened, and how to prevent it** — without assigning blame to individuals.

การวิเคราะห์หลังเหตุการณ์โดยไม่กล่าวโทษคือการวิเคราะห์เหตุการณ์ที่เป็นลายลักษณ์อักษรที่มุ่งเน้นที่ **เกิดอะไรขึ้น, ทำไมมันเกิดขึ้น, และจะป้องกันอย่างไร** — โดยไม่กล่าวโทษบุคคล

#### Principles / หลักการ

```
Blameless Postmortem Principles / หลักการวิเคราะห์หลังเหตุการณ์โดยไม่กล่าวโทษ
============================================================================

1. Assume good intentions / สมมติความตั้งใจดี
   - Everyone was doing their best with the information they had
   - ทุกคนทำดีที่สุดด้วยข้อมูลที่มี

2. Focus on systems, not people / มุ่งเน้นระบบ ไม่ใช่คน
   - Ask "how did the system allow this?" / ถาม "ระบบอนุญาตสิ่งนี้ได้อย่างไร?"
   - Not "who made the mistake?" / ไม่ใช่ "ใครทำผิดพลาด?"

3. Learn and improve / เรียนรู้และปรับปรุง
   - Every incident is a learning opportunity / ทุกเหตุการณ์เป็นโอกาสในการเรียนรู้
   - Share findings organization-wide / แบ่งปันผลการค้นพบทั่วทั้งองค์กร

4. Document everything / บันทึกทุกอย่าง
   - Timeline of events / เส้นเวลาของเหตุการณ์
   - Root cause analysis / การวิเคราะห์สาเหตุรากเหง้า
   - Action items with owners / รายการดำเนินการพร้อมเจ้าของ
```

#### Postmortem Template / เทมเพลตการวิเคราะห์หลังเหตุการณ์

```
Postmortem Template / เทมเพลตการวิเคราะห์หลังเหตุการณ์
=====================================================

Title: [Brief description of incident]
     / [คำอธิบายสั้นๆ ของเหตุการณ์]

Date: [Date of incident]
    / [วันที่เกิดเหตุการณ์]

Severity: [SEV1/SEV2/SEV3]
        / [ระดับความรุนแรง]

Summary / สรุป:
  [2-3 sentence overview]
  [ภาพรวม 2-3 ประโยค]

Impact / ผลกระทบ:
  - Duration: [How long the issue lasted]
            / [ระยะเวลาที่เกิดปัญหา]
  - Users affected: [Number/percentage of impacted users]
                  / [จำนวน/เปอร์เซ็นต์ของผู้ใช้ที่ได้รับผลกระทบ]
  - Business impact: [Revenue, reputation, etc.]
                   / [ผลกระทบทางธุรกิจ รายได้, ชื่อเสียง, ฯลฯ]

Timeline / เส้นเวลา:
  [00:00] Issue began / ปัญหาเริ่ม
  [00:05] Alert fired / การแจ้งเตือนทำงาน
  [00:10] On-call engaged / เวรเฝ้าระวังเข้าจัดการ
  [00:30] Root cause identified / ระบุสาเหตุรากเหง้า
  [00:45] Mitigation applied / ใช้การบรรเทา
  [01:00] Service restored / บริการได้รับการกู้คืน

Root Cause / สาเหตุรากเหง้า:
  [Detailed technical explanation]
  [คำอธิบายทางเทคนิคโดยละเอียด]

Contributing Factors / ปัจจัยสนับสนุน:
  - [What conditions enabled the failure?]
  / [เงื่อนไขใดที่ทำให้เกิดความล้มเหลว?]
  - [Why wasn't it caught earlier?]
  / ทำไมไม่ถูกจับได้ก่อนหน้า?]

Action Items / รายการดำเนินการ:
  1. [ ] [Fix] [Description] - @[Owner] - [Due date]
       / [แก้ไข] [คำอธิบาย] - @[เจ้าของ] - [วันครบกำหนด]
  2. [ ] [Prevent] [Description] - @[Owner] - [Due date]
       / [ป้องกัน] [คำอธิบาย] - @[เจ้าของ] - [วันครบกำหนด]
  3. [ ] [Detect] [Description] - @[Owner] - [Due date]
       / [ตรวจจับ] [คำอธิบาย] - @[เจ้าของ] - [วันครบกำหนด]

Lessons Learned / บทเรียนที่ได้รับ:
  - [What went well?] / [อะไรที่ทำได้ดี?]
  - [What could be improved?] / [อะไรที่สามารถปรับปรุงได้?]
  - [Where did we get lucky?] / [เราโชคดีที่ไหน?]
```

---

### Monitoring & Alerting / การตรวจสอบและการแจ้งเตือน

**Monitoring vs Alerting / การตรวจสอบเทียบกับการแจ้งเตือน**

```
Monitoring / การตรวจสอบ
  - Collecting and storing metrics about system behavior
  - การรวบรวมและเก็บตัวชี้วัดเกี่ยวกับพฤติกรรมของระบบ
  - Passive: You look at it when you need to
  - แบบพาสซีฟ: คุณดูเมื่อต้องการ
  - Example: Dashboards, metric graphs
  - ตัวอย่าง: แดชบอร์ด, กราฟตัวชี้วัด

Alerting / การแจ้งเตือน
  - Actively notifying humans when something is wrong
  - การแจ้งเตือนมนุษย์เมื่อมีสิ่งผิดปกติ
  - Active: Interrupts you to demand attention
  - แบบแอคทีฟ: ขัดจังหวะคุณเพื่อเรียกร้องความสนใจ
  - Example: PagerDuty pages, Slack notifications
  - ตัวอย่าง: การแจ้งเตือน PagerDuty, การแจ้งเตือน Slack
```

#### Principles of Good Alerting / หลักการของการแจ้งเตือนที่ดี

```
Good Alerts Should Be / การแจ้งเตือนที่ดีควรเป็น:
===============================================

1. Actionable / สามารถดำเนินการได้
   - Someone can do something about it / ใครบางคนสามารถทำอะไรได้
   - No "FYI" alerts / ไม่มีการแจ้งเตือน "เพื่อทราบ"

2. Urgent / เร่งด่วน
   - Requires immediate attention / ต้องการความสนใจทันที
   - Not "check this next week" / ไม่ใช่ "ตรวจสอบสัปดาห์หน้า"

3. Real / ของจริง
   - Represents a genuine problem / แสดงถึงปัญหาจริง
   - Not a false positive / ไม่ใช่ผลบวกผิด

4. Specific / เฉพาะเจาะจง
   - Clearly states what is wrong / ระบุชัดเจนว่าอะไรผิด
   - Includes context and runbook link / รวมลิงก์บริบทและสมุดงาน
```

#### Alert Tiers / ระดับการแจ้งเตือน

```
Alert Severity Levels / ระดับความรุนแรงของการแจ้งเตือน
=====================================================

P1 - Critical / วิกฤต:
  - Service is down or severely degraded
  / บริการล่มหรือเสื่อมโทรมอย่างรุนแรง
  - Page immediately, 24/7
  / แจ้งเตือนทันที, ตลอด 24 ชั่วโมง
  - Example: Database unreachable
  / ตัวอย่าง: ฐานข้อมูลไม่สามารถเข้าถึงได้

P2 - High / สูง:
  - Significant degradation, workaround exists
  / การเสื่อมโทรมสำคัญ, มีวิธีแก้ขัดข้อง
  - Page during business hours
  / แจ้งเตือนในช่วงเวลาทำงาน
  - Example: High error rate on non-critical API
  / ตัวอย่าง: อัตราข้อผิดพลาดสูงบน API ที่ไม่สำคัญ

P3 - Medium / ปานกลาง:
  - Minor issues that need attention soon
  / ปัญหาน้อยที่ต้องให้ความสนใจเร็วๆ นี้
  - Ticket, review next business day
  / ตั๋ว, ทบทวนวันทำการถัดไป
  - Example: Disk usage approaching limit
  / ตัวอย่าง: การใช้ดิสก์เข้าใกล้ขีดจำกัด

P4 - Low / ต่ำ:
  - Informational, no immediate action needed
  / ข้อมูล, ไม่ต้องการการดำเนินการทันที
  - Log for future review
  / บันทึกเพื่อทบทวนในอนาคต
  - Example: Backup took longer than usual
  / ตัวอย่าง: การสำรองใช้เวลานานกว่าปกติ
```

---

### Capacity Planning / การวางแผนความจุ

**What is Capacity Planning? / การวางแผนความจุคืออะไร?**

Capacity planning is the practice of ensuring you have enough resources to handle current and future demand.

การวางแผนความจุคือการปฏิบัติเพื่อให้แน่ใจว่าคุณมีทรัพยากรเพียงพอที่จะจัดการความต้องการปัจจุบันและอนาคต

#### Capacity Planning Process / กระบวนการวางแผนความจุ

```
Capacity Planning Framework / กรอบการวางแผนความจุ
=================================================

1. Measure Current State / วัดสถานะปัจจุบัน
   - Monitor resource utilization (CPU, memory, disk, network)
   / ตรวจสอบการใช้ทรัพยากร (CPU, หน่วยความจำ, ดิสก์, เครือข่าย)
   - Track traffic growth trends
   / ติดตามแนวโน้มการเติบโตของการจราจร
   - Identify bottlenecks
   / ระบุจุดคอขวด

2. Forecast Future Demand / คาดการณ์ความต้องการในอนาคต
   - Analyze historical growth patterns
   / วิเคราะห์รูปแบบการเติบโตทางประวัติศาสตร์
   - Consider upcoming product launches or marketing campaigns
   / พิจารณาการเปิดตัวผลิตภัณฑ์หรือแคมเปญการตลาดที่จะมาถึง
   - Account for seasonal variations
   / บัญชีสำหรับความแปรปรวนตามฤดูกาล

3. Plan for Growth / วางแผนสำหรับการเติบโต
   - Determine when current resources will be exhausted
   / กำหนดว่าทรัพยากรปัจจุบันจะถูกใช้หมดเมื่อใด
   - Plan procurement and provisioning in advance
   / วางแผนการจัดซื้อและการเตรียมการล่วงหน้า
   - Build headroom for unexpected spikes
   / สร้างพื้นที่สำหรับการพุ่งสูงขึ้นที่ไม่คาดคิด

4. Test and Validate / ทดสอบและตรวจสอบ
   - Conduct regular load testing
   / ดำเนินการทดสอบโหลดเป็นประจำ
   - Simulate failure scenarios
   / จำลองสถานการณ์ความล้มเหลว
   - Verify auto-scaling configurations
   / ตรวจสอบการกำหนดค่าการขยายขนาดอัตโนมัติ
```

#### Load Testing / การทดสอบโหลด

```
Load Testing Strategy / กลยุทธ์การทดสอบโหลด
============================================

Types of Load Tests / ประเภทของการทดสอบโหลด:

1. Baseline Test / การทดสอบพื้นฐาน
   - Normal expected load / โหลดปกติที่คาดหวัง
   - Verify SLOs are met / ตรวจสอบว่า SLO บรรลุ
   - Run regularly / รันเป็นประจำ

2. Peak Test / การทดสอบสูงสุด
   - Maximum expected load / โหลดสูงสุดที่คาดหวัง
   - Find breaking points / หาจุดแตกหัก
   - Run before major events / รันก่อนเหตุการณ์สำคัญ

3. Stress Test / การทดสอบความเครียด
   - Beyond expected limits / เกินขีดจำกัดที่คาดหวัง
   - Understand failure modes / เข้าใจโหมดความล้มเหลว
   - Run quarterly / รันรายไตรมาส

4. Soak Test / การทดสอบแช่
   - Sustained load over long period / โหลดต่อเนื่องเป็นเวลานาน
   - Find memory leaks, resource exhaustion / หาการรั่วไหลของหน่วยความจำ, การใช้ทรัพยากรหมด
   - Run monthly / รันรายเดือน
```

---

### Release Engineering / วิศวกรรม_release

**What is Release Engineering? / วิศวกรรม_release คืออะไร?**

Release engineering encompasses the processes and tools for safely and efficiently deploying software changes to production.

วิศวกรรม_release ครอบคลุมกระบวนการและเครื่องมือสำหรับปรับใช้การเปลี่ยนแปลงซอฟต์แวร์กับการผลิตอย่างปลอดภัยและมีประสิทธิภาพ

#### Key Practices / แนวปฏิบัติหลัก

```
Release Engineering Practices / แนวปฏิบัติวิศวกรรม_release
==========================================================

1. CI/CD Pipelines / ไปป์ไลน์ CI/CD
   - Continuous Integration: Automated builds and tests
   / การรวมต่อเนื่อง: การสร้างและการทดสอบอัตโนมัติ
   - Continuous Delivery: Ready-to-deploy artifacts
   / การส่งมอบต่อเนื่อง: สิ่งที่พร้อมปรับใช้
   - Continuous Deployment: Automatic production deploys
   / การปรับใช้ต่อเนื่อง: การปรับใช้การผลิตอัตโนมัติ

2. Deployment Strategies / กลยุทธ์การปรับใช้
   - Blue-Green: Two identical environments, switch traffic
   / บลู-กรีน: สองสภาพแวดล้อมที่เหมือนกัน, สลับการจราจร
   - Canary: Gradually roll out to subset of users
   / คานารี: ปล่อยให้ผู้ใช้บางส่วนค่อยๆ
   - Rolling: Update instances one at a time
   / โรลลิง: อัปเดตอินสแตนซ์ทีละตัว
   - Feature Flags: Toggle features on/off without deploy
   / ธงคุณสมบัติ: เปิด/ปิดคุณสมบัติโดยไม่ต้องปรับใช้

3. Rollback Procedures / กระบวนการย้อนกลับ
   - Automated rollback on failure
   / ย้อนกลับอัตโนมัติเมื่อล้มเหลว
   - Tested rollback process
   / กระบวนการย้อนกลับที่ทดสอบแล้ว
   - Quick rollback (< 5 minutes target)
   / ย้อนกลับเร็ว (< 5 นาทีเป้าหมาย)

4. Change Management / การจัดการการเปลี่ยนแปลง
   - Track all changes (code, config, infra)
   / ติดตามการเปลี่ยนแปลงทั้งหมด (โค้ด, การกำหนดค่า, โครงสร้างพื้นฐาน)
   - Correlate changes with incidents
   / เชื่อมโยงการเปลี่ยนแปลงกับเหตุการณ์
   - Require code reviews and approvals
   / ต้องการการตรวจสอบโค้ดและการอนุมัติ
```

---

### Incident Management / การจัดการเหตุการณ์

**What is Incident Management? / การจัดการเหตุการณ์คืออะไร?**

Incident management is the process of responding to service disruptions, from detection through resolution and post-incident review.

การจัดการเหตุการณ์คือกระบวนการตอบสนองต่อการหยุดชะงักของบริการ ตั้งแต่การตรวจจับจนถึงการแก้ไขและการทบทวนหลังเหตุการณ์

#### Incident Response Lifecycle / วงจรการตอบสนองเหตุการณ์

```
Incident Response Lifecycle / วงจรการตอบสนองเหตุการณ์
=====================================================

1. Detection / การตรวจจับ
   - Automated alerts catch issues
   / การแจ้งเตือนอัตโนมัติจับปัญหา
   - Users report problems
   / ผู้ใช้รายงานปัญหา
   - Monitoring dashboards show anomalies
   / แดชบอร์ดการตรวจสอบแสดงความผิดปกติ

2. Triage / การคัดเลือก
   - Assess severity and impact
   / ประเมินความรุนแรงและผลกระทบ
   - Assign to appropriate team
   / มอบหมายให้ทีมที่เหมาะสม
   - Declare incident if needed
   / ประกาศเหตุการณ์หากจำเป็น

3. Response / การตอบสนอง
   - On-call engineer investigates
   / วิศวกรเวรเฝ้าระวังสอบสวน
   - Follow runbooks if available
   / ทำตามสมุดงานหากมี
   - Escalate if needed
   / ยกระดับหากจำเป็น

4. Mitigation / การบรรเทา
   - Apply fixes or workarounds
   / ใช้การแก้ไขหรือวิธีแก้ขัดข้อง
   - Prioritize restoring service over root cause
   / จัดลำดับความสำคัญในการกู้คืนบริการมากกว่าสาเหตุรากเหง้า
   - Communicate status to stakeholders
   / สื่อสารสถานะให้ผู้มีส่วนได้ส่วนเสียทราบ

5. Resolution / การแก้ไข
   - Service fully restored
   / บริการได้รับการกู้คืนเต็มที่
   - Verify all systems healthy
   / ตรวจสอบว่าทุกระบบปกติ
   - Close incident
   / ปิดเหตุการณ์

6. Post-Incident Review / การทบทวนหลังเหตุการณ์
   - Write blameless postmortem
   / เขียนการวิเคราะห์หลังเหตุการณ์โดยไม่กล่าวโทษ
   - Identify action items
   / ระบุรายการดำเนินการ
   - Share learnings
   / แบ่งปันบทเรียน
```

#### On-Call Rotation / เวรเฝ้าระวัง

```
On-Call Best Practices / แนวปฏิบัติเวรเฝ้าระวังที่ดีที่สุด
=========================================================

Rotation Schedule / ตารางเวรเฝ้าระวัง:
  - 1-week rotations are common / เวรเฝ้าระวัง 1 สัปดาห์เป็นเรื่องธรรมดา
  - Follow-the-sun for global teams / ตามดวงอาทิตย์สำหรับทีมทั่วโลก
  - Include primary + secondary on-call / รวมเวรเฝ้าระวังหลัก + รอง

Healthy On-Call / เวรเฝ้าระวังที่ดี:
  - < 2 pages per shift / < 2 การแจ้งเตือนต่อกะ
  - < 15 minutes of sleep disruption per night / < 15 นาทีของการนอนหลับต่อคืน
  - Sustainable long-term / ยั่งยืนในระยะยาว

Unhealthy On-Call / เวรเฝ้าระวังที่ไม่ดี:
  - Constant pages / การแจ้งเตือนอย่างต่อเนื่อง
  - Burnout and turnover / หมดไฟและการลาออก
  - Indicates need for reliability investment / บ่งชี้ความต้องการลงทุนในความน่าเชื่อถือ

Compensation / ค่าตอบแทน:
  - Extra pay for on-call shifts / ค่าจ้างเพิ่มสำหรับกะเวรเฝ้าระวัง
  - Comp time for sleep disruption / เวลาชดเชยสำหรับการนอนหลับถูกรบกวน
  - Recognition for good incident response / การยอมรับสำหรับการตอบสนองเหตุการณ์ที่ดี
```

---

## Tools / เครื่องมือ

### Monitoring / การตรวจสอบ

| Tool / เครื่องมือ | Description / คำอธิบาย | Use Case / กรณีใช้ |
|-------------------|------------------------|---------------------|
| **Prometheus** | Open-source metrics collection and alerting / การรวบรวมและการแจ้งเตือนตัวชี้วัดโอเพ่นซอร์ส | Kubernetes monitoring, application metrics / การตรวจสอบ Kubernetes, ตัวชี้วัดแอปพลิเคชัน |
| **Grafana** | Visualization and dashboard platform / แพลตฟอร์มการแสดงผลและแดชบอร์ด | Creating dashboards from multiple data sources / การสร้างแดชบอร์ดจากแหล่งข้อมูลหลายแห่ง |
| **Datadog** | Full-stack monitoring SaaS / SaaS การตรวจสอบสแต็กเต็ม | Enterprise monitoring, APM, infra monitoring / การตรวจสอบองค์กร, APM, การตรวจสอบโครงสร้างพื้นฐาน |
| **New Relic** | Application performance monitoring / การตรวจสอบประสิทธิภาพแอปพลิเคชัน | APM, browser monitoring, mobile monitoring / APM, การตรวจสอบเบราว์เซอร์, การตรวจสอบมือถือ |
| **Zabbix** | Enterprise-class monitoring / การตรวจสอบระดับองค์กร | Network monitoring, server monitoring / การตรวจสอบเครือข่าย, การตรวจสอบเซิร์ฟเวอร์ |

### Logging / การบันทึก

| Tool / เครื่องมือ | Description / คำอธิบาย | Use Case / กรณีใช้ |
|-------------------|------------------------|---------------------|
| **ELK Stack** | Elasticsearch, Logstash, Kibana - full logging pipeline / ไปป์ไลน์การบันทึกเต็มรูปแบบ | Centralized logging, log analysis / การบันทึกแบบรวมศูนย์, การวิเคราะห์บันทึก |
| **Loki** | Log aggregation by Grafana, optimized for cloud-native / การรวบรวมบันทึกโดย Grafana, ปรับให้เหมาะสมกับคลาวด์-เนทีฟ | Kubernetes logging, cost-effective logging / การบันทึก Kubernetes, การบันทึกประหยัดต้นทุน |
| **Fluentd** | Open-source log collector and forwarder / ตัวรวบรวมและส่งต่อบันทึกรหัสเปิด | Log collection, transformation, forwarding / การรวบรวมบันทึก, การแปลง, การส่งต่อ |
| **Splunk** | Enterprise log management and analysis / การจัดการและการวิเคราะห์บันทึกระดับองค์กร | Security analysis, compliance, troubleshooting / การวิเคราะห์ความปลอดภัย, การปฏิบัติตาม, การแก้ไขปัญหา |

### Tracing / การติดตาม

| Tool / เครื่องมือ | Description / คำอธิบาย | Use Case / กรณีใช้ |
|-------------------|------------------------|---------------------|
| **Jaeger** | Distributed tracing system by Uber / ระบบติดตามแบบกระจายโดย Uber | Microservices tracing, latency analysis / การติดตามไมโครเซอร์วิส, การวิเคราะห์ความล่าช้า |
| **Zipkin** | Distributed tracing system by Twitter / ระบบติดตามแบบกระจายโดย Twitter | Request flow tracing, performance debugging / การติดตามโฟลว์คำขอ, การแก้ไขข้อบกพร่องประสิทธิภาพ |
| **OpenTelemetry** | Observability framework (CNCF) / กรอบการสังเกตการณ์ (CNCF) | Standardized instrumentation for traces, metrics, logs / การสร้างเครื่องมือมาตรฐานสำหรับ การติดตาม, ตัวชี้วัด, บันทึก |

### Alerting / การแจ้งเตือน

| Tool / เครื่องมือ | Description / คำอธิบาย | Use Case / กรณีใช้ |
|-------------------|------------------------|---------------------|
| **PagerDuty** | Incident management and on-call platform / แพลตฟอร์มการจัดการเหตุการณ์และเวรเฝ้าระวัง | Enterprise incident response, escalation policies / การตอบสนองเหตุการณ์องค์กร, นโยบายการยกระดับ |
| **OpsGenie** | Alerting and on-call management by Atlassian / การแจ้งเตือนและการจัดการเวรเฝ้าระวังโดย Atlassian | Alert routing, on-call scheduling / การกำหนดเส้นทางการแจ้งเตือน, การจัดตารางเวรเฝ้าระวัง |
| **Alertmanager** | Prometheus alert routing and deduplication / การกำหนดเส้นทางการแจ้งเตือนและการหักลบ Prometheus | Prometheus alerting, notification routing / การแจ้งเตือน Prometheus, การกำหนดเส้นทางการแจ้งเตือน |
| **Slack/Microsoft Teams** | Communication platforms with alert integrations / แพลตฟอร์มการสื่อสารพร้อมการผสานรวมการแจ้งเตือน | Team notifications, incident war rooms / การแจ้งเตือนทีม, ห้องสงครามเหตุการณ์ |

---

## Example: Prometheus Configuration / ตัวอย่าง: การกำหนดค่า Prometheus

This is a complete Prometheus configuration file that scrapes metrics from multiple targets:

นี่คือไฟล์การกำหนดค่า Prometheus ที่สมบูรณ์ที่ดึงตัวชี้วัดจากเป้าหมายหลายแห่ง:

```yaml
# Prometheus Configuration File
# ไฟล์กำหนดค่า Prometheus

# Global settings apply to all scrape jobs
# การตั้งค่าระดับโลกใช้กับงานดึงข้อมูลทั้งหมด
global:
  # How often to scrape metrics from targets
  # ความถี่ในการดึงตัวชี้วัดจากเป้าหมาย
  scrape_interval: 15s

  # How long to wait for a scrape request before timing out
  # รอนานแค่ไหนสำหรับคำขอดึงข้อมูลก่อนที่จะหมดเวลา
  scrape_timeout: 10s

  # How often to evaluate alerting and recording rules
  # ความถี่ในการประเมินกฎการแจ้งเตือนและการบันทึก
  evaluation_interval: 15s

  # Add these labels to all metrics collected by this Prometheus
  # เพิ่มป้ายกำกับเหล่านี้ลงในตัวชี้วัดทั้งหมดที่รวบรวมโดย Prometheus นี้
  external_labels:
    # Identifies which Prometheus instance collected the metric
    # ระบุว่าอินสแตนซ์ Prometheus ใดรวบรวมตัวชี้วัด
    cluster: 'production'
    region: 'us-east-1'

# Alertmanager configuration
# การกำหนดค่า Alertmanager
alerting:
  alertmanagers:
    - static_configs:
        # Where to send alerts
        # ที่ไหนส่งการแจ้งเตือน
        - targets:
          - 'alertmanager:9093'

# Rule files to load (alerting and recording rules)
# ไฟล์กฎที่จะโหลด (กฎการแจ้งเตือนและการบันทึก)
rule_files:
  - 'alert_rules.yml'
  - 'recording_rules.yml'

# Scrape configurations - define what to monitor
# การกำหนดค่าดึงข้อมูล - กำหนดสิ่งที่ต้องตรวจสอบ
scrape_configs:
  # ============================================
  # Monitor Prometheus itself
  # ตรวจสอบ Prometheus เอง
  # ============================================
  - job_name: 'prometheus'
    # Scrape Prometheus's own /metrics endpoint
    # ดึงข้อมูล /metrics ของ Prometheus เอง
    scrape_interval: 10s  # Override global interval for this job
                          # แทนที่ช่วงเวลาสากลสำหรับงานนี้
    static_configs:
      - targets: ['localhost:9090']
        labels:
          # Custom labels for this target
          # ป้ายกำกับกำหนดเองสำหรับเป้าหมายนี้
          environment: 'production'
          team: 'infrastructure'

  # ============================================
  # Node Exporter - Linux system metrics
  # Node Exporter - ตัวชี้วัดระบบ Linux
  # ============================================
  - job_name: 'node-exporter'
    # Metrics about CPU, memory, disk, network
    # ตัวชี้วัดเกี่ยวกับ CPU, หน่วยความจำ, ดิสก์, เครือข่าย
    static_configs:
      - targets: ['node-exporter:9100']
        labels:
          instance: 'web-server-01'
          datacenter: 'dc1'
      - targets: ['node-exporter:9101']
        labels:
          instance: 'web-server-02'
          datacenter: 'dc1'
      - targets: ['node-exporter:9102']
        labels:
          instance: 'db-server-01'
          datacenter: 'dc2'

  # ============================================
  # Application Metrics - Your service
  # ตัวชี้วัดแอปพลิเคชัน - บริการของคุณ
  # ============================================
  - job_name: 'application'
    # Custom metrics path (default is /metrics)
    # เส้นตัวชี้วัดกำหนดเอง (ค่าเริ่มต้นคือ /metrics)
    metrics_path: '/metrics'
    static_configs:
      - targets: ['app:8080']
        labels:
          service: 'my-web-app'
          version: 'v2.1.0'

  # ============================================
  # Kubernetes Service Discovery
  # การค้นหาบริการ Kubernetes
  # ============================================
  - job_name: 'kubernetes-pods'
    # Automatically discover and scrape pods
    # ค้นพบและดึงข้อมูลพ็อดอัตโนมัติ
    kubernetes_sd_configs:
      - role: pod
        # Only scrape pods with this annotation
        # เฉพาะดึงข้อมูลพ็อดที่มีการอธิบายประกอบนี้
        selectors:
          - role: pod
    # Relabel configs to filter which pods to scrape
    # การกำหนดป้ายกำกับใหม่เพื่อกรองพ็อดที่จะดึงข้อมูล
    relabel_configs:
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
        action: keep
        # Only scrape pods annotated with prometheus.io/scrape=true
        # เฉพาะดึงข้อมูลพ็อดที่มีการอธิบายประกอบ prometheus.io/scrape=true
        regex: true
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_path]
        action: replace
        # Use custom metrics path if annotated
        # ใช้เส้นทางตัวชี้วัดกำหนดเองหากมีการอธิบายประกอบ
        target_label: __metrics_path__
        regex: (.+)
      - source_labels: [__address__, __meta_kubernetes_pod_annotation_prometheus_io_port]
        action: replace
        # Use custom port if annotated
        # ใช้พอร์ตกำหนดเองหากมีการอธิบายประกอบ
        regex: ([^:]+)(?::\d+)?;(\d+)
        replacement: $1:$2
        target_label: __address__

  # ============================================
  # Blackbox Exporter - HTTP/HTTPS probes
  # Blackbox Exporter - การตรวจสอบ HTTP/HTTPS
  # ============================================
  - job_name: 'blackbox-http'
    # Probe external endpoints for availability
    # ตรวจสอบจุดปลายทางภายนอกสำหรับความพร้อมใช้งาน
    metrics_path: /probe
    params:
      module: [http_2xx]  # Use http_2xx module for probing
                           # ใช้โมดูล http_2xx สำหรับการตรวจสอบ
    static_configs:
      - targets:
          # External endpoints to monitor
          # จุดปลายทางภายนอกที่ต้องตรวจสอบ
          - 'https://www.example.com'
          - 'https://api.example.com/health'
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 'blackbox-exporter:9115'
```

---

## Example: Alert Rules / ตัวอย่าง: กฎการแจ้งเตือน

These are Prometheus alerting rules that define when alerts should fire:

นี่คือกฎการแจ้งเตือน Prometheus ที่กำหนดเมื่อการแจ้งเตือนควรทำงาน:

```yaml
# Alert Rules Configuration
# ไฟล์กำหนดค่ากฎการแจ้งเตือน

# Alert rule groups
# กลุ่มกฎการแจ้งเตือน
groups:
  # ============================================
  # Group 1: HTTP/API Alerts
  # กลุ่ม 1: การแจ้งเตือน HTTP/API
  # ============================================
  - name: http-alerts
    rules:
      # ----------------------------------------
      # Alert: High Error Rate (HTTP 5xx)
      # การแจ้งเตือน: อัตราข้อผิดพลาดสูง (HTTP 5xx)
      # ----------------------------------------
      - alert: HighErrorRate
        # Trigger if > 5% of requests are 5xx over 5 minutes
        # ทำงานหาก > 5% ของคำขอเป็น 5xx ตลอด 5 นาที
        expr: |
          sum(rate(http_requests_total{status=~"5.."}[5m]))
          /
          sum(rate(http_requests_total[5m]))
          > 0.05
        for: 5m  # Must be true for 5 minutes before alerting
                # ต้องเป็นจริงเป็นเวลา 5 นาทีก่อนแจ้งเตือน
        labels:
          severity: critical  # Page immediately / แจ้งเตือนทันที
          team: backend       # Route to backend team / ส่งไปยังทีม backend
        annotations:
          # Short summary for alert notifications
          # สรุปสั้นสำหรับการแจ้งเตือน
          summary: "High HTTP error rate detected / ตรวจพบอัตราข้อผิดพลาด HTTP สูง"
          # Detailed description with dynamic values
          # คำอธิบายละเอียดพร้อมค่าไดนามิก
          description: >
            Error rate is {{ $value | humanizePercentage }}
            (threshold: 5%).
            / อัตราข้อผิดพลาดคือ {{ $value | humanizePercentage }}
            (เกณฑ์: 5%)
            Service: {{ $labels.service }}
            / บริการ: {{ $labels.service }}
            Instance: {{ $labels.instance }}
            / อินสแตนซ์: {{ $labels.instance }}

      # ----------------------------------------
      # Alert: High Latency (p99)
      # การแจ้งเตือน: ความล่าช้าสูง (p99)
      # ----------------------------------------
      - alert: HighLatencyP99
        # Trigger if p99 latency exceeds 1 second
        # ทำงานหากความล่าช้า p99 เกิน 1 วินาที
        expr: histogram_quantile(0.99, rate(http_request_duration_seconds_bucket[5m])) > 1
        for: 10m  # Wait 10 minutes to avoid alerting on brief spikes
                 # รอ 10 นาทีเพื่อหลีกเลี่ยงการแจ้งเตือนจากการพุ่งขึ้นชั่วคราว
        labels:
          severity: warning  # Review during business hours / ทบทวนในช่วงเวลาทำงาน
          team: backend
        annotations:
          summary: "High p99 latency detected / ตรวจพบความล่าช้า p99 สูง"
          description: >
            p99 latency is {{ $value }}s (threshold: 1s).
            / ความล่าช้า p99 คือ {{ $value }}วินาที (เกณฑ์: 1วินาที)
            Service: {{ $labels.service }}
            / บริการ: {{ $labels.service }}

      # ----------------------------------------
      # Alert: Service Down
      # การแจ้งเตือน: บริการล่ม
      # ----------------------------------------
      - alert: ServiceDown
        # Trigger if no metrics received from service for 2 minutes
        # ทำงานหากไม่ได้รับตัวชี้วัดจากบริการเป็นเวลา 2 นาที
        expr: up{job="application"} == 0
        for: 2m
        labels:
          severity: critical  # Immediate page / แจ้งเตือนทันที
          team: on-call       # Route to on-call engineer / ส่งไปยังวิศวกรเวรเฝ้าระวัง
        annotations:
          summary: "Service is DOWN / บริการล่ม"
          description: >
            Instance {{ $labels.instance }} of job {{ $labels.job }} has been down for more than 2 minutes.
            / อินสแตนซ์ {{ $labels.instance }} ของงาน {{ $labels.job }} ล่มนานกว่า 2 นาที
            Runbook: https://wiki.example.com/runbooks/service-down
            / สมุดงาน: https://wiki.example.com/runbooks/service-down

  # ============================================
  # Group 2: Infrastructure Alerts
  # กลุ่ม 2: การแจ้งเตือนโครงสร้างพื้นฐาน
  # ============================================
  - name: infrastructure-alerts
    rules:
      # ----------------------------------------
      # Alert: High CPU Usage
      # การแจ้งเตือน: การใช้ CPU สูง
      # ----------------------------------------
      - alert: HighCPUUsage
        # Trigger if CPU > 85% for 10 minutes
        # ทำงานหาก CPU > 85% เป็นเวลา 10 นาที
        expr: 100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 85
        for: 10m
        labels:
          severity: warning
          team: infrastructure
        annotations:
          summary: "High CPU usage detected / ตรวจพบการใช้ CPU สูง"
          description: >
            CPU usage on {{ $labels.instance }} is {{ $value }}% (threshold: 85%).
            / การใช้ CPU บน {{ $labels.instance }} คือ {{ $value }}% (เกณฑ์: 85%)

      # ----------------------------------------
      # Alert: High Memory Usage
      # การแจ้งเตือน: การใช้หน่วยความจำสูง
      # ----------------------------------------
      - alert: HighMemoryUsage
        # Trigger if memory usage > 90%
        # ทำงานหากการใช้หน่วยความจำ > 90%
        expr: |
          (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100 > 90
        for: 5m
        labels:
          severity: critical  # Risk of OOM killer / ความเสี่ยง OOM killer
          team: infrastructure
        annotations:
          summary: "High memory usage detected / ตรวจพบการใช้หน่วยความจำสูง"
          description: >
            Memory usage on {{ $labels.instance }} is {{ $value }}% (threshold: 90%).
            / การใช้หน่วยความจำบน {{ $labels.instance }} คือ {{ $value }}% (เกณฑ์: 90%)
            OOM killer may activate soon.
            / OOM killer อาจเปิดใช้งานเร็วๆ นี้

      # ----------------------------------------
      # Alert: Disk Space Running Low
      # การแจ้งเตือน: พื้นที่ดิสก์ใกล้หมด
      # ----------------------------------------
      - alert: DiskSpaceLow
        # Trigger if less than 10% disk space remaining
        # ทำงานหากพื้นที่ดิสก์เหลือน้อยกว่า 10%
        expr: |
          (node_filesystem_avail_bytes{mountpoint="/"} / node_filesystem_size_bytes{mountpoint="/"}) * 100 < 10
        for: 15m
        labels:
          severity: warning
          team: infrastructure
        annotations:
          summary: "Disk space running low / พื้นที่ดิสก์ใกล้หมด"
          description: >
            Disk space on {{ $labels.instance }} (mount: {{ $labels.mountpoint }})
            is {{ $value }}% available.
            / พื้นที่ดิสก์บน {{ $labels.instance }} (เมาท์: {{ $labels.mountpoint }})
            เหลือ {{ $value }}%

  # ============================================
  # Group 3: Business Metrics Alerts
  # กลุ่ม 3: การแจ้งเตือนตัวชี้วัดธุรกิจ
  # ============================================
  - name: business-alerts
    rules:
      # ----------------------------------------
      # Alert: Unusual Drop in Traffic
      # การแจ้งเตือน: การลดลงผิดปกติของการจราจร
      # ----------------------------------------
      - alert: UnusualTrafficDrop
        # Trigger if traffic drops by more than 50% compared to 1 hour ago
        # ทำงานหากการจราจรลดลงมากกว่า 50% เทียบกับ 1 ชั่วโมงที่แล้ว
        expr: |
          sum(rate(http_requests_total[5m])) <
          sum(rate(http_requests_total[5m] offset 1h)) * 0.5
        for: 10m
        labels:
          severity: warning
          team: backend
        annotations:
          summary: "Unusual drop in traffic detected / ตรวจพบการลดลงผิดปกติของการจราจร"
          description: >
            Current traffic is {{ $value | humanizePercentage }} of what it was 1 hour ago.
            / การจราจรปัจจุบันคือ {{ $value | humanizePercentage }} ของเมื่อ 1 ชั่วโมงที่แล้ว
            Investigate potential issues.
            / สอบสวนปัญหาที่อาจเกิดขึ้น
```

---

## SRE Mindset / แนวคิด SRE

The SRE mindset is fundamentally different from traditional operations. Here are the core principles:

แนวคิด SRE แตกต่างจากการดำเนินการแบบดั้งเดิมโดยพื้นฐาน นี่คือหลักการพื้นฐาน:

### 1. Embrace Risk / ยอมรับความเสี่ยง

```
Traditional View / มุมมองดั้งเดิม:
  "We need 100% uptime!"
  / "เราต้องการเวลาทำงาน 100%!"

SRE View / มุมมอง SRE:
  "100% reliability is too expensive. What's the right level?"
  / "ความน่าเชื่อถือ 100% แพงเกินไป ระดับที่เหมาะสมคืออะไร?"
```

**Key insight / ข้อมูลเชิงลึกหลัก:** Every additional "9" in reliability costs exponentially more. A 99.9% SLO is dramatically cheaper than 99.99%. The right SLO balances user happiness with development speed.

**ข้อมูลเชิงลึกหลัก:** ทุก "9" เพิ่มเติมในความน่าเชื่อถือมีค่าใช้จ่ายเพิ่มขึ้นอย่างมาก SLO 99.9% ถูกกว่า 99.99% อย่างมาก SLO ที่ถูกต้องสร้างสมดุลระหว่างความสุขของผู้ใช้กับความเร็วในการพัฒนา

### 2. SLIs Guide Decisions / SLI นำทางการตัดสินใจ

```
Traditional View / มุมมองดั้งเดิม:
  "I think the system is slow."
  / "ฉันคิดว่าระบบช้า"

SRE View / มุมมอง SRE:
  "p99 latency increased from 200ms to 800ms, exceeding our 500ms SLO."
  / "ความล่าช้า p99 เพิ่มขึ้นจาก 200ms เป็น 800ms เกิน SLO 500ms ของเรา"
```

**Key insight / ข้อมูลเชิงลึกหลัก:** Replace opinions with data. Decisions should be based on measurable SLIs, not gut feelings.

**ข้อมูลเชิงลึกหลัก:** แทนที่ความคิดเห็นด้วยข้อมูล การตัดสินใจควรขึ้นอยู่กับ SLI ที่วัดได้ ไม่ใช่ความรู้สึก

### 3. Reduce Toil / ลดงานซ้ำซ้อน

```
Traditional View / มุมมองดั้งเดิม:
  "We've always done it this way."
  / "เราทำแบบนี้มาตลอด"

SRE View / มุมมอง SRE:
  "If I do this task more than twice, I should automate it."
  / "ถ้าฉันทำงานนี้มากกว่าสองครั้ง ฉันควรทำให้เป็นอัตโนมัติ"
```

**Key insight / ข้อมูลเชิงลึกหลัก:** Toil is a tax on growth. Every hour spent on manual work is an hour not spent improving the system.

**ข้อมูลเชิงลึกหลัก:** งานซ้ำซ้อนเป็นภาษีของการเติบโต ทุกชั่วโมงที่ใช้งานด้วยมือคือหนึ่งชั่วโมงที่ไม่ได้ใช้ในการปรับปรุงระบบ

### 4. Learn from Failures / เรียนรู้จากความล้มเหลว

```
Traditional View / มุมมองดั้งเดิม:
  "Who caused the outage?"
  / "ใครทำให้ระบบล่ม?"

SRE View / มุมมอง SRE:
  "What system design allowed this failure to happen?"
  / "การออกแบบระบบใดที่ทำให้ความล้มเหลวนี้เกิดขึ้นได้?"
```

**Key insight / ข้อมูลเชิงลึกหลัก:** Blameless postmortems create psychological safety. When people aren't afraid to admit mistakes, the organization learns faster.

**ข้อมูลเชิงหลัก:** การวิเคราะห์หลังเหตุการณ์โดยไม่กล่าวโทษสร้างความปลอดภัยทางจิตวิทยา เมื่อคนไม่กลัวที่จะยอมรับข้อผิดพลาด องค์กรจะเรียนรู้เร็วขึ้น

### 5. SRE is Engineering / SRE คือวิศวกรรม

```
Traditional View / มุมมองดั้งเดิม:
  "Ops is about keeping servers running."
  / "การดำเนินการคือการรักษาเซิร์ฟเวอร์ให้ทำงาน"

SRE View / มุมมอง SRE:
  "Ops is a software problem. Let's solve it with code."
  / "การดำเนินการเป็นปัญหาซอฟต์แวร์ มาแก้ด้วยโค้ดกัน"
```

**Key insight / ข้อมูลเชิงหลัก:** Software engineers are better at solving operational problems than manual intervention. Build platforms, not runbooks.

**ข้อมูลเชิงหลัก:** วิศวกรซอฟต์แวร์เก่งในการแก้ปัญหาการดำเนินการมากกว่าการแทรกแซงด้วยมือ สร้างแพลตฟอร์ม ไม่ใช่สมุดงาน

---

## Practice Exercises / แบบฝึกหัด

Complete these exercises to solidify your SRE knowledge:

ทำแบบฝึกหัดเหล่านี้ให้เสร็จเพื่อเสริมสร้างความรู้ SRE ของคุณ:

### Beginner / ผู้เริ่มต้น

1. **Define SLIs, SLOs, and SLAs for a Web Application / กำหนด SLI, SLO, และ SLA สำหรับเว็บแอปพลิเคชัน**
   - Choose a web app you know well (or build a simple one) / เลือกเว็บแอปที่คุณรู้จักดี (หรือสร้างแอปง่ายๆ)
   - Define 3 SLIs (e.g., latency, availability, error rate) / กำหนด 3 SLI (เช่น ความล่าช้า, ความพร้อมใช้งาน, อัตราข้อผิดพลาด)
   - Set appropriate SLO targets for each / ตั้งเป้าหมาย SLO ที่เหมาะสมสำหรับแต่ละตัว
   - Draft an SLA with consequences / ร่าง SLA พร้อมผลที่ตามมา
   - Document your choices and justify them / บันทึกการเลือกของคุณและให้เหตุผล

2. **Calculate Error Budget for Various SLOs / คำนวณงบประมาณข้อผิดพลาดสำหรับ SLO ต่างๆ**
   - Calculate allowed downtime for: 99.9%, 99.95%, 99.99% SLOs / คำนวณเวลาที่อนุญาตสำหรับ: SLO 99.9%, 99.95%, 99.99%
   - Express in minutes per month, hours per year / แสดงเป็นนาทีต่อเดือน, ชั่วโมงต่อปี
   - Create a spreadsheet or table for reference / สร้างสเปรดชีตหรือตารางเพื่ออ้างอิง

3. **Identify Toil in Your Daily Work / ระบุงานซ้ำซ้อนในงานประจำวันของคุณ**
   - Track your time for one week / ติดตามเวลาของคุณเป็นเวลาหนึ่งสัปดาห์
   - Categorize tasks as: toil vs. engineering / แบ่งประเภทงานเป็น: งานซ้ำซ้อน vs. วิศวกรรม
   - Calculate percentage spent on toil / คำนวณเปอร์เซ็นต์ที่ใช้กับงานซ้ำซ้อน
   - Propose 3 automation projects / เสนอโครงการระบบอัตโนมัติ 3 โครงการ

### Intermediate / ระดับกลาง

4. **Set Up Prometheus and Grafana Monitoring / ตั้งค่าการตรวจสอบ Prometheus และ Grafana**
   - Install Prometheus and Grafana on a test environment / ติดตั้ง Prometheus และ Grafana บนสภาพแวดล้อมทดสอบ
   - Configure Prometheus to scrape metrics from at least 2 targets / กำหนดค่า Prometheus เพื่อดึงข้อมูลตัวชี้วัดจากอย่างน้อย 2 เป้าหมาย
   - Create a Grafana dashboard with the Four Golden Signals / สร้างแดชบอร์ด Grafana พร้อมสัญญาณทองสี่ประการ
   - Set up a test application and monitor it / ตั้งค่าแอปพลิเคชันทดสอบและตรวจสอบ

5. **Create Alert Rules for the Four Golden Signals / สร้างกฎการแจ้งเตือนสำหรับสัญญาณทองสี่ประการ**
   - Write Prometheus alert rules for: / เขียนกฎการแจ้งเตือน Prometheus สำหรับ:
     - Latency (p99 > SLO) / ความล่าช้า (p99 > SLO)
     - Traffic (sudden spike or drop) / การจราจร (พุ่งขึ้นหรือลดลงกระทันหัน)
     - Errors (error rate > threshold) / ข้อผิดพลาด (อัตราข้อผิดพลาด > เกณฑ์)
     - Saturation (CPU, memory, disk) / ความอิ่มตัว (CPU, หน่วยความจำ, ดิสก์)
   - Test alerts in a safe environment / ทดสอบการแจ้งเตือนในสภาพแวดล้อมที่ปลอดภัย

6. **Write a Blameless Postmortem / เขียนการวิเคราะห์หลังเหตุการณ์โดยไม่กล่าวโทษ**
   - Use a hypothetical incident or a real past incident / ใช้เหตุการณ์สมมติหรือเหตุการณ์จริงในอดีต
   - Follow the template provided earlier in this guide / ทำตามเทมเพลตที่กำหนดไว้ก่อนหน้าในคู่มือนี้
   - Focus on system fixes, not individual blame / มุ่งเน้นการแก้ไขระบบ ไม่ใช่การกล่าวโทษบุคคล
   - Share it with a peer for feedback / แบ่งปันกับเพื่อนเพื่อรับข้อเสนอแนะ

### Advanced / ขั้นสูง

7. **Design an On-Call Rotation Schedule / ออกแบบตารางเวรเฝ้าระวัง**
   - Assume a team of 5 engineers / สมมติทีมของวิศวกร 5 คน
   - Design a sustainable rotation (consider work-life balance) / ออกแบบเวรเฝ้าระวังที่ยั่งยืน (พิจารณาความสมดุลชีวิตการทำงาน)
   - Define escalation policies / กำหนดนโยบายการยกระดับ
   - Create alert routing rules / สร้างกฎการกำหนดเส้นทางการแจ้งเตือน

8. **Build a Capacity Planning Model / สร้างแบบจำลองการวางแผนความจุ**
   - Collect resource utilization data for a service / รวบรวมข้อมูลการใช้ทรัพยากรสำหรับบริการ
   - Analyze growth trends over time / วิเคราะห์แนวโน้มการเติบโตตลอดเวลา
   - Project when resources will be exhausted / คาดการณ์ว่าทรัพยากรจะถูกใช้หมดเมื่อใด
   - Create a capacity plan with procurement recommendations / สร้างแผนความจุพร้อมคำแนะนำการจัดซื้อ

9. **Implement Automated Remediation / ใช้การซ่อมแซมอัตโนมัติ**
   - Identify a common failure pattern / ระบุรูปแบบความล้มเหลวทั่วไป
   - Write an automated response (script, operator, or function) / เขียนการตอบสนองอัตโนมัติ (สคริปต์, โอเปอเรเตอร์, หรือฟังก์ชัน)
   - Test it in a staging environment / ทดสอบในสภาพแวดล้อมสเตจจิ้ง
   - Document the automation and add monitoring / บันทึกการตรวจสอบและเพิ่มการตรวจสอบ

10. **Design an End-to-End Observability Stack / ออกแบบสแต็กการสังเกตการณ์แบบครบวงจร**
    - Choose tools for metrics, logs, traces / เลือกเครื่องมือสำหรับตัวชี้วัด, บันทึก, การติดตาม
    - Design the data flow architecture / ออกแบบสถาปัตยกรรมโฟลว์ข้อมูล
    - Configure dashboards for different personas (dev, ops, management) / กำหนดค่าแดชบอร์ดสำหรับบุคคลต่างกัน (นักพัฒนา, ดำเนินการ, ผู้บริหาร)
    - Document cost estimates and trade-offs / บันทึกการประมาณการต้นทุนและการแลกเปลี่ยน

---

## Resources / แหล่งข้อมูล

### Books / หนังสือ

- **[Google SRE Book (Free)](https://sre.google/sre-book/table-of-contents/)** / หนังสือ Google SRE (ฟรี)
  - The original SRE book from Google. Covers all foundational concepts.
  / หนังสือ SRE ต้นฉบับจาก Google ครอบคลุมแนวคิดพื้นฐานทั้งหมด

- **[Google SRE Workbook (Free)](https://sre.google/workbook/table-of-contents/)** / สมุดงาน Google SRE (ฟรี)
  - Practical implementations and case studies from Google SREs.
  / การนำไปใช้จริงและกรณีศึกษาจาก SRE ของ Google

- **[Seeking SRE](https://www.oreilly.com/library/view/seeking-sre/9781491978856/)** / Seeking SRE
  - Conversations about running production systems at scale.
  / บทสนทนาเกี่ยวกับการใช้งานระบบการผลิตขนาดใหญ่

### Online Resources / แหล่งข้อมูลออนไลน์

- **[Awesome SRE](https://github.com/dastergon/awesome-sre)** / Awesome SRE
  - Curated list of SRE resources, tools, and practices.
  / รายการ curated ของทรัพยากร SRE, เครื่องมือ, และแนวปฏิบัติ

- **[Google Cloud SRE Docs](https://cloud.google.com/blog/products/devops-sre)** / เอกสาร SRE Google Cloud
  - Official SRE documentation and blog posts from Google.
  / เอกสาร SRE ทางการและบล็อกจาก Google

- **[NinjaSRE](https://ninjasre.com/)** / NinjaSRE
  - Practical SRE guides and tutorials.
  / คู่มือและบทเรียน SRE เชิงปฏิบัติ

- **[SRE Weekly Newsletter](https://sreweekly.com/)** / จดหมายข่าว SRE รายสัปดาห์
  - Weekly roundup of SRE news and articles.
  / สรุปข่าวและบทความ SRE รายสัปดาห์

### Communities / ชุมชน

- **[SRE Slack Community](https://sre.community/)** / ชุมชน SRE Slack
  - Active community of SRE practitioners.
  / ชุมชนที่ใช้งานของนักปฏิบัติ SRE

- **[USENIX SREcon](https://www.usenix.org/conferences/srecon)** / USENIX SREcon
  - Annual SRE conference with talks and workshops.
  / การประชุม SRE ประจำปีพร้อมการบรรยายและเวิร์คช็อป

### Tools Documentation / เอกสารเครื่องมือ

- **[Prometheus Documentation](https://prometheus.io/docs/)** / เอกสาร Prometheus
- **[Grafana Documentation](https://grafana.com/docs/)** / เอกสาร Grafana
- **[OpenTelemetry Documentation](https://opentelemetry.io/docs/)** / เอกสาร OpenTelemetry

---

## Quick Reference Card / บัตรอ้างอิงด่วน

```
SRE Quick Reference / การอ้างอิงด่วน SRE
=========================================

Key Metrics / ตัวชี้วัดสำคัญ:
  - SLI: What you measure / สิ่งที่คุณวัด
  - SLO: Your target / เป้าหมายของคุณ
  - SLA: Your contract / สัญญาของคุณ

Error Budget / งบประมาณข้อผิดพลาด:
  - Error Budget = 100% - SLO
  - Budget remaining = ship features / งบประมาณเหลือ = ปล่อยคุณสมบัติ
  - Budget exhausted = fix reliability / งบประมาณหมด = แก้ไขความน่าเชื่อถือ

Four Golden Signals / สัญญาณทองสี่ประการ:
  1. Latency: How long? / ความล่าช้า: นานแค่ไหน?
  2. Traffic: How much? / การจราจร: มากแค่ไหน?
  3. Errors: What's failing? / ข้อผิดพลาด: อะไรล้มเหลว?
  4. Saturation: How full? / ความอิ่มตัว: เต็มแค่ไหน?

Alert Principles / หลักการแจ้งเตือน:
  - Actionable / สามารถดำเนินการได้
  - Urgent / เร่งด่วน
  - Real / ของจริง
  - Specific / เฉพาะเจาะจง

Toil Target / เป้าหมายงานซ้ำซ้อน:
  - < 50% of time on manual, repetitive work
  / < 50% ของเวลากับงานด้วยมือ, ซ้ำซ้อน

SRE Ratio / อัตราส่วน SRE:
  - 50% development / 50% การพัฒนา
  - 50% operations / 50% การดำเนินการ

SRE Mindset / แนวคิด SRE:
  1. Embrace risk (100% is too expensive) / ยอมรับความเสี่ยง (100% แพงเกินไป)
  2. Let data guide decisions / ให้ข้อมูลนำทางการตัดสินใจ
  3. Automate everything possible / ทำให้เป็นอัตโนมัติทุกอย่างที่เป็นไปได้
  4. Learn from failures (blameless) / เรียนรู้จากความล้มเหลว (ไม่กล่าวโทษ)
  5. Engineering solves ops problems / วิศวกรรมแก้ปัญหาการดำเนินการ
```

---

> **Remember / จำไว้:** SRE is not a destination, it's a journey. Start small, measure everything, and continuously improve.
>
> **จำไว้:** SRE ไม่ใช่จุดหมายปลายทาง มันคือการเดินทาง เริ่มต้นเล็ก วัดทุกอย่าง และปรับปรุงอย่างต่อเนื่อง
