# ☸️ How to Install Kubernetes (Minikube) on Linux (Ubuntu)

Minikube คือ tool สำหรับรัน Kubernetes cluster แบบ local บนเครื่องเดียว

รองรับ Ubuntu 18.04+

---

## 🔧 Prerequisites

- Docker หรือ container runtime อื่น (เช่น containerd)

- kubectl (Kubernetes CLI)

---

## 🔄 Step 1: ติดตั้ง kubectl

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

---

## 📦 Step 2: ติดตั้ง Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

---

## ✅ Step 3: เริ่ม Minikube cluster

```bash
minikube start
```

คำสั่งนี้จะสร้าง K8s cluster แบบ local

---

## 🔍 Step 4: ทดสอบการทำงาน

```bash
kubectl get nodes
kubectl get pods -A
```

ควรเห็น node และ pods ในระบบ

---

## 🛑 Step 5: หยุด Minikube (เมื่อไม่ใช้)

```bash
minikube stop
```

---

## 📁 Kubernetes Command พื้นฐาน

| คำสั่ง | ความหมาย | ตัวอย่าง | หมายเหตุเพิ่มเติม |
|--------|-----------|----------|------------------|
| `kubectl get nodes` | แสดง nodes ใน cluster | `kubectl get nodes` | ดูสถานะของ nodes |
| `kubectl get pods` | แสดง pods | `kubectl get pods -A` | -A แสดงทุก namespace |
| `kubectl get services` | แสดง services | `kubectl get services` | ดู network services |
| `kubectl apply -f <file>` | สร้าง resources จาก YAML | `kubectl apply -f deployment.yaml` | ใช้ deploy แอป |
| `kubectl delete -f <file>` | ลบ resources จาก YAML | `kubectl delete -f deployment.yaml` | ใช้ cleanup |
| `kubectl logs <pod>` | ดู logs ของ pod | `kubectl logs mypod` | ใช้ debug |
| `kubectl exec -it <pod> -- bash` | เข้า shell ของ pod | `kubectl exec -it mypod -- bash` | เหมือน SSH เข้า pod |
| `minikube dashboard` | เปิด K8s dashboard | `minikube dashboard` | UI สำหรับจัดการ cluster |

---