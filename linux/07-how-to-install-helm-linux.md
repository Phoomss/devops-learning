# 📦 How to Install Helm on Linux (Ubuntu)

Helm คือ package manager สำหรับ Kubernetes ช่วยติดตั้งและจัดการแอปพลิเคชันบนคลัสเตอร์

รองรับ Ubuntu 20.04, 22.04, 24.04

---

## 🔧 Prerequisites

- Kubernetes cluster (เช่น Minikube, K3s, หรือ cluster จริง)
- `kubectl` ติดตั้งแล้ว

---

## 📥 Step 1: ดาวน์โหลด Helm

```bash
curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

---

## ✅ Step 2: ตรวจสอบการติดตั้ง

```bash
helm version
```

---

## 📚 Step 3: เพิ่ม repository และอัปเดต

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

---

## 🚀 Step 4: ติดตั้งตัวอย่างแอป

```bash
helm install my-nginx bitnami/nginx
```

ตรวจสอบ release:

```bash
helm list
```

---

## 🔍 Step 5: ดูรายละเอียด release

```bash
helm status my-nginx
```

ลบ release เมื่อไม่ใช้:

```bash
helm uninstall my-nginx
```

---

## 📁 คำสั่ง Helm พื้นฐาน

| คำสั่ง | ความหมาย | ตัวอย่าง |
|--------|-----------|----------|
| `helm repo add <name> <url>` | เพิ่ม Helm repo | `helm repo add bitnami https://charts.bitnami.com/bitnami` |
| `helm repo update` | อัปเดตรายการ chart | `helm repo update` |
| `helm search repo <keyword>` | ค้นหา chart ใน repo | `helm search repo nginx` |
| `helm install <release> <chart>` | ติดตั้ง chart | `helm install my-nginx bitnami/nginx` |
| `helm list` | ดู release ที่ติดตั้ง | `helm list` |
| `helm status <release>` | ดูสถานะ release | `helm status my-nginx` |
| `helm uninstall <release>` | ลบ release | `helm uninstall my-nginx` |

---

## 💡 Tips

- หากต้องการติดตั้ง Helm แบบแมนนวล ให้ดาวน์โหลดไฟล์ tarball จาก GitHub แล้วแตกไฟล์
- ถ้าใช้งานบน cluster ที่มี RBAC ให้แน่ใจว่า `kubectl` ทำงานได้ก่อนติดตั้ง Helm
