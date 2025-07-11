
# ☸️ คู่มือ Kubernetes ฉบับเข้าใจง่าย (พร้อมเปรียบเทียบกับฟุตบอลไทยลีก)

---

## 📌 Kubernetes คืออะไร?

Kubernetes (K8s) คือระบบจัดการ container แบบอัตโนมัติ เช่น:

- จัดการการ deploy แอป
- ขยายจำนวน pod (scale)
- จัดการ load balancing
- ฟื้นตัวเองได้เมื่อมีข้อผิดพลาด

---

## ⚽ เปรียบเทียบ Kubernetes กับฟุตบอลไทยลีก

| องค์ประกอบ Kubernetes | เปรียบกับในไทยลีก |
|------------------------|----------------------|
| Cluster (คลัสเตอร์)     | ระบบสนามแข่งทั้งหมด |
| Node                   | สนามแต่ละแห่ง        |
| Pod                    | นักฟุตบอล             |
| Container              | รองเท้าสตั๊ดแต่ละคู่   |
| Deployment             | โค้ชวางแผนผู้เล่น     |
| Service                | ทางเข้า/ตั๋วเข้าชม    |
| Namespace              | ลีกย่อย เช่น T1, T2  |
| ConfigMap/Secret       | สมุดแผนเกม / รหัสลับ  |
| Volume                 | ห้องเก็บของในสนาม     |
| Ingress                | ทางเข้าให้คนภายนอกเข้าถึงเว็บ |

---

## 🛠️ วิธีสร้าง Kubernetes Cluster (ใช้ Minikube)

### 1. ติดตั้ง Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

### 2. สร้าง Cluster ด้วย Docker

```bash
minikube start --driver=docker
```

> หรือถ้าคุณใช้ VirtualBox:
```bash
minikube start --driver=virtualbox
```

---

## 📦 พื้นฐาน Kubernetes ที่ควรรู้

| สิ่งที่ควรรู้ | รายละเอียด |
|---------------|-------------|
| Node | เครื่องที่รัน workload (อาจเป็นเครื่องจริงหรือ VM) |
| Pod | หน่วยที่เล็กที่สุดใน Kubernetes (รัน 1 หรือมากกว่า container) |
| Deployment | คำสั่งที่บอกให้ K8s ควบคุมจำนวน pod และ rolling update |
| Service | ช่องทางเข้า pod แบบเสถียร |
| Namespace | แบ่งกลุ่มการใช้งาน เช่น dev, prod |

---

## 🔧 ตัวอย่างคำสั่งพื้นฐาน

```bash
# ดู node
kubectl get nodes

# สร้าง deployment ชื่อ nginx
kubectl create deployment nginx --image=nginx

# เปิด port 80 ให้เข้าถึงได้จากภายนอก
kubectl expose deployment nginx --type=NodePort --port=80

# เชื่อมต่อ service ผ่าน localhost
kubectl port-forward svc/nginx 8080:80
```

เปิดเบราว์เซอร์ไปที่:
```
http://localhost:8080
```

---

## 📁 ตัวอย่าง deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
            - containerPort: 80
```

---

## 🔐 ฟีเจอร์ระดับกลาง

| ฟีเจอร์ | ใช้ทำอะไร |
|---------|------------|
| ConfigMap | เก็บข้อมูล config ทั่วไป (ไม่ลับ) |
| Secret | เก็บข้อมูลลับ (เช่น password) |
| Volume | แชร์ข้อมูลให้ container |
| Ingress | route เข้าหลาย service ผ่าน domain/path เดียว |

---

## 🧼 การลบ resource

```bash
kubectl delete deployment nginx
kubectl delete service nginx
```

---

## 🧠 เรียนรู้ต่อยอด

- ใช้ `Helm` ติดตั้ง package
- ทำ CI/CD บน Jenkins หรือ GitHub Actions
- Deploy บน Cloud เช่น GKE, EKS, AKS
- ติดตั้ง metrics server, Grafana, Prometheus

---

> 📘 เอกสารเพิ่มเติม:  
> - https://kubernetes.io/docs/home/  
> - https://minikube.sigs.k8s.io  
> - https://kind.sigs.k8s.io  
