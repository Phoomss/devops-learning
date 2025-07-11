# 📘 คำสั่ง Kubernetes พื้นฐาน (kubectl)

> รวมคำสั่งสำคัญสำหรับการใช้งาน Kubernetes Cluster

---

## 🧠 ข้อมูลทั่วไปของคลัสเตอร์

| คำสั่ง | รายละเอียด |
|--------|-------------|
| `kubectl version` | ดูเวอร์ชันของ client และ server |
| `kubectl cluster-info` | ดูสถานะของ cluster ที่เชื่อมอยู่ |
| `kubectl config view` | ดูค่า config ปัจจุบัน |
| `kubectl get nodes` | แสดง node ทั้งหมดใน cluster |
| `kubectl describe node <name>` | รายละเอียด node |

---

## 📦 การจัดการ Pods

| คำสั่ง | รายละเอียด |
|--------|-------------|
| `kubectl get pods` | แสดง pod ทั้งหมด |
| `kubectl get pods -A` | แสดง pod ทุก namespace |
| `kubectl describe pod <pod-name>` | รายละเอียดของ pod |
| `kubectl delete pod <pod-name>` | ลบ pod |
| `kubectl logs <pod-name>` | ดู log ของ pod |
| `kubectl exec -it <pod-name> -- bash` | เข้า shell ของ pod |

---

## ⚙️ การจัดการ Deployment

| คำสั่ง | รายละเอียด |
|--------|-------------|
| `kubectl get deployments` | แสดง deployments ทั้งหมด |
| `kubectl apply -f <file>.yaml` | สร้างหรืออัปเดต object จากไฟล์ YAML |
| `kubectl delete -f <file>.yaml` | ลบ object จาก YAML |
| `kubectl rollout restart deployment <name>` | รีสตาร์ท deployment |
| `kubectl describe deployment <name>` | ดูรายละเอียด deployment |

---

## 📡 การจัดการ Services

| คำสั่ง | รายละเอียด |
|--------|-------------|
| `kubectl get svc` | แสดง services |
| `kubectl expose pod <pod-name> --port=80 --target-port=8080 --type=NodePort` | สร้าง service จาก pod |
| `kubectl port-forward svc/<service-name> 8080:80` | Forward port ไปยัง service |

---

## 📂 Namespace

| คำสั่ง | รายละเอียด |
|--------|-------------|
| `kubectl get ns` | แสดง namespace |
| `kubectl create ns <name>` | สร้าง namespace ใหม่ |
| `kubectl delete ns <name>` | ลบ namespace |
| `kubectl config set-context --current --namespace=<name>` | เปลี่ยน namespace ปัจจุบัน |

---

## 🧪 คำสั่งช่วยดีบั๊ก

| คำสั่ง | รายละเอียด |
|--------|-------------|
| `kubectl describe <resource> <name>` | ดูรายละเอียดของ resource |
| `kubectl get events` | ดู event ล่าสุดใน cluster |
| `kubectl top pod` | ดูการใช้ resource ของ pod (ต้องติดตั้ง metrics-server) |
| `kubectl get all` | แสดงทุก object ใน namespace |

---

## 🧰 อื่น ๆ ที่สำคัญ

| คำสั่ง | รายละเอียด |
|--------|-------------|
| `kubectl apply -f https://...` | ดึง YAML จาก URL แล้ว deploy |
| `kubectl delete all --all` | ลบ resource ทั้งหมดใน namespace |
| `kubectl edit deployment <name>` | แก้ config ของ deployment ผ่าน editor |
| `kubectl scale deployment <name> --replicas=3` | เปลี่ยนจำนวน replica |

---

## 📎 Tips

- ใช้ `-n <namespace>` เพื่อสั่งใน namespace เฉพาะ
- ใช้ `-o yaml` เพื่อดู output เป็น YAML
- ใช้ `--dry-run=client -o yaml` เพื่อลองสร้าง resource โดยไม่ deploy

---

> 🧠 เหมาะสำหรับใช้บน Minikube, Kind, หรือ Cluster จริงบน Cloud
