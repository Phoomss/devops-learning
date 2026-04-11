# DevOps Learning Guide / คู่มือการเรียนรู้ DevOps

Welcome to the comprehensive DevOps learning guide! This collection covers essential topics for DevOps engineers, from fundamentals to advanced topics.

ยินดีต้อนรับสู่คู่มือการเรียนรู้ DevOps อย่างครอบคลุม! ชุดนี้ครอบคลุมหัวข้อสำคัญสำหรับวิศวกร DevOps ตั้งแต่พื้นฐานจนถึงขั้นสูง

---

## 📚 Table of Contents / สารบัญ

### Fundamentals / พื้นฐาน

| Topic / หัวข้อ | File / ไฟล์ | Description / คำอธิบาย |
|-------|------|-------------|
| 🐧 Linux / ลินุกซ์ | [Linux Fundamentals](linux/00-linux-fundamentals.md) | OS basics, commands, services / พื้นฐานระบบปฏิบัติการ, คำสั่ง, บริการ |
| 🌐 OSI Model / โมเดล OSI | [OSI Model](osi-model/00-osi-model.md) | 7-layer networking model / โมเดลเครือข่าย 7 ชั้น |
| 🐳 Docker / ด็อกเกอร์ | [Docker](docker/00-docker.md) | Container fundamentals / พื้นฐานคอนเทนเนอร์ |

### Kubernetes / คูเบอร์เนทีส

| Topic / หัวข้อ | File / ไฟล์ | Description / คำอธิบาย |
|-------|------|-------------|
| 📦 K8s Components / ส่วนประกอบ K8s | [K8s Components](k8s/00-k8s-components.md) | Architecture and objects / สถาปัตยกรรมและออบเจกต์ |
| 🔌 K8s CNI / เครือข่าย K8s | [K8s CNI](k8s/01-k8s-cni.md) | Networking and OSI model relation / เครือข่ายและความสัมพันธ์กับ OSI |
| ⚙️ K8s Installation / ติดตั้ง K8s | [K8s Installation](k8s/02-k8s-installation.md) | kubeadm, Kubespray, Rancher |

### Security & Reliability / ความปลอดภัยและความน่าเชื่อถือ

| Topic / หัวข้อ | File / ไฟล์ | Description / คำอธิบาย |
|-------|------|-------------|
| 🔒 TLS / ทีแอลเอส | [TLS](tls/00-tls.md) | Certificates, ACME, Let's Encrypt / ใบรับรอง, ACME, Let's Encrypt |
| 📊 SRE / เอสอาร์อี | [SRE](sre/00-sre.md) | Site Reliability Engineering / วิศวกรรมความน่าเชื่อถือของไซต์ |

### Workflow & Tools / เวิร์กโฟลว์และเครื่องมือ

| Topic / หัวข้อ | File / ไฟล์ | Description / คำอธิบาย |
|-------|------|-------------|
| 🔀 Git Workflow / เวิร์กโฟลว์ Git | [Git Workflow](git-workflow/00-git-workflow.md) | Trunk-based vs Git Flow |
| ⎈ Helm / เฮล์ม | [Helm](helm/00-helm.md) | Kubernetes package manager / ตัวจัดการแพ็กเกจคูเบอร์เนทีส |

---

## 🎯 Tasks Checklist / รายการสิ่งที่ต้องทำ

### Task 1: TLS / ใบรับรอง TLS
- [ ] Issue self domain certificate / ออกใบรับรองสำหรับโดเมนของคุณ
- [ ] Issue certificate with ACME (HTTP-01, DNS-01) / ออกใบรับรองด้วย ACME
- [ ] Capture screen when issued and when expire / จับภาพหน้าจอเมื่อออกและเมื่อหมดอายุ
- 📖 [Learn more / เรียนรู้เพิ่มเติม](tls/00-tls.md)

### Task 2: Git Workflow / เวิร์กโฟลว์ Git
- [ ] Understand Trunk-Based Development / เข้าใจ Trunk-Based Development
- [ ] Understand Git Flow / เข้าใจ Git Flow
- [ ] Compare and document differences / เปรียบเทียบและบันทึกความแตกต่าง
- 📖 [Learn more / เรียนรู้เพิ่มเติม](git-workflow/00-git-workflow.md)

### Task 3: Helm / เฮล์ม
- [ ] Understand what Helm is / เข้าใจว่า Helm คืออะไร
- [ ] Deploy application with Helm / แอปพลิเคชันที่ติดตั้งด้วย Helm
- [ ] Create custom chart / สร้าง chart แบบกำหนดเอง
- 📖 [Learn more / เรียนรู้เพิ่มเติม](helm/00-helm.md)

### Task 4: Install Kubernetes / ติดตั้งคูเบอร์เนทีส
- [ ] Install with kubeadm / ติดตั้งด้วย kubeadm
- [ ] Install with Kubespray / ติดตั้งด้วย Kubespray
- [ ] Install with Rancher / ติดตั้งด้วย Rancher
- 📖 [Learn more / เรียนรู้เพิ่มเติม](k8s/02-k8s-installation.md)

### Task 5: Kubernetes CNI / เครือข่ายคูเบอร์เนทีส
- [ ] Understand what CNI is / เข้าใจว่า CNI คืออะไร
- [ ] Learn relationship with OSI model / เรียนรู้ความสัมพันธ์กับโมเดล OSI
- [ ] Deploy and test network policies / ติดตั้งและทดสอบนโยบายเครือข่าย
- 📖 [Learn more / เรียนรู้เพิ่มเติม](k8s/01-k8s-cni.md)

---

## 🚀 Learning Path / เส้นทางการเรียนรู้

### Beginner / ผู้เริ่มต้น
1. Start with [Linux Fundamentals](linux/00-linux-fundamentals.md) / เริ่มจากพื้นฐาน Linux
2. Learn [OSI Model](osi-model/00-osi-model.md) / เรียนรู้โมเดล OSI
3. Master [Docker](docker/00-docker.md) / เชี่ยวชาญ Docker

### Intermediate / ระดับกลาง
4. Study [K8s Components](k8s/00-k8s-components.md) / ศึกษาส่วนประกอบ K8s
5. Practice [K8s Installation](k8s/02-k8s-installation.md) / ฝึกติดตั้ง K8s
6. Understand [TLS](tls/00-tls.md) / เข้าใจ TLS

### Advanced / ระดับสูง
7. Explore [K8s CNI](k8s/01-k8s-cni.md) / สำรวจ CNI ของ K8s
8. Learn [Helm](helm/00-helm.md) / เรียนรู้ Helm
9. Adopt [SRE](sre/00-sre.md) practices / นำแนวทาง SRE ไปใช้
10. Master [Git Workflow](git-workflow/00-git-workflow.md) / เชี่ยวชาญ Git Workflow

---

## 📂 Directory Structure / โครงสร้างไดเรกทอรี

```
devops-learning/
├── README.md                          # Main table of contents / สารบัญหลัก
├── linux/
│   └── 00-linux-fundamentals.md       # Linux fundamentals / พื้นฐาน Linux
├── osi-model/
│   └── 00-osi-model.md                # OSI 7-layer model / โมเดล OSI 7 ชั้น
├── docker/
│   └── 00-docker.md                   # Docker containerization / คอนเทนเนอร์ Docker
├── k8s/
│   ├── 00-k8s-components.md           # Kubernetes architecture / สถาปัตยกรรม K8s
│   ├── 01-k8s-cni.md                  # Container networking / เครือข่ายคอนเทนเนอร์
│   └── 02-k8s-installation.md         # Installation guides / คู่มือการติดตั้ง
├── tls/
│   └── 00-tls.md                      # TLS certificates / ใบรับรอง TLS
├── sre/
│   └── 00-sre.md                      # Site Reliability Engineering / SRE
├── git-workflow/
│   └── 00-git-workflow.md             # Git workflows / เวิร์กโฟลว์ Git
└── helm/
    └── 00-helm.md                     # Kubernetes package manager / ตัวจัดการแพ็กเกจ
```

---

## 💡 Tips for Learning / เคล็ดลับการเรียนรู้

1. **Hands-on practice** / ฝึกปฏิบัติจริง: Follow examples and exercises in each guide / ทำตามตัวอย่างและแบบฝึกหัดในแต่ละคู่มือ
2. **Take notes** / จดบันทึก: Document your own experiences and screenshots / บันทึกประสบการณ์และภาพหน้าจอของคุณ
3. **Complete tasks** / ทำให้ครบ tareas: Work through Tasks 1-5 systematically / ทำงานผ่าน Task 1-5 อย่างเป็นระบบ
4. **Build projects** / สร้างโปรเจกต์: Apply learning to real projects / นำการเรียนรู้ไปใช้กับโปรเจกต์จริง
5. **Stay updated** / อัปเดตเสมอ: Technologies evolve, check for latest versions / เทคโนโลยีเปลี่ยนแปลงเสมอ ตรวจสอบเวอร์ชันล่าสุด

---

## 📖 Additional Resources / แหล่งข้อมูลเพิ่มเติม

- [Kubernetes Official Docs](https://kubernetes.io/docs/) / เอกสารทางการของ Kubernetes
- [Docker Docs](https://docs.docker.com/) / เอกสารทางการของ Docker
- [Linux Journey](https://linuxjourney.com/) / เรียนรู้ Linux ทีละขั้นตอน
- [Google SRE Book](https://sre.google/sre-book/) / หนังสือ SRE ของ Google (ฟรี!)

---

Happy Learning! 🚀 / เรียนรู้ให้สนุก! 🚀
