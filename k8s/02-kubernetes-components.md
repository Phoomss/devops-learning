
# ☸️ อธิบายส่วนประกอบต่าง ๆ ของ Kubernetes (K8s) อย่างละเอียด

---

## 🧭 ภาพรวม Kubernetes

**Kubernetes** (K8s) คือระบบจัดการ container แบบอัตโนมัติ เช่น:

- จัดการการ deploy แอป
- ขยายจำนวน pod
- จัดการ load balancing
- ฟื้นตัวเองอัตโนมัติเมื่อ service ล่ม

---

## 🔧 ส่วนประกอบหลักของ Kubernetes

### 1. Cluster (คลัสเตอร์)
กลุ่มของเครื่องที่ทำงานร่วมกัน โดยมี Control Plane และ Worker Node

---

### 2. Node (โหนด)
เครื่องที่ใช้รันแอปพลิเคชัน (จริงหรือ VM)

- Master Node (Control Plane): บริหารจัดการ
- Worker Node: รัน workload (Pod)

---

### 3. Pod
หน่วย deploy ที่เล็กที่สุด รัน 1 หรือหลาย container ที่แชร์ network/storage

---

### 4. Container
โปรแกรมที่รันใน Pod เช่น nginx, mysql (ใช้ image จาก Docker)

---

## 🚦 ส่วนควบคุมการทำงาน

### 5. Deployment
กำหนดให้ Kubernetes สร้าง pod ตามที่ต้องการ เช่น replicas, image, update

### 6. ReplicaSet
รักษาจำนวน pod ให้อยู่ในจำนวนที่ระบุ

### 7. Service
เปิดให้ pod ถูกเข้าถึงได้

- ClusterIP: ใช้ภายใน
- NodePort: เปิด port
- LoadBalancer: ใช้กับ cloud
- ExternalName: DNS ภายนอก

### 8. Ingress
Route traffic จากภายนอกมายัง Pod แบบกำหนด path/domain ได้

---

## 🗃️ จัดการ config และ storage

### 9. ConfigMap
เก็บ config ที่ไม่เป็นความลับ เช่น ENV

### 10. Secret
เก็บข้อมูลลับ เช่น password (เข้ารหัส base64)

### 11. Volume
เก็บข้อมูลถาวรให้ container ใช้ร่วมกัน

- emptyDir, hostPath, configMap
- PersistentVolume (PV)
- PersistentVolumeClaim (PVC)

---

## 🧠 อื่น ๆ

### 12. Namespace
แบ่ง cluster ออกเป็น environment เช่น dev, prod

### 13. Label & Selector
ติด tag ให้ resource แล้วใช้ selector หา resource ที่ต้องการ

### 14. Helm
ระบบจัดการแพ็กเกจบน K8s เหมือน apt/yum

---

## 📦 คำสั่งควบคุมพื้นฐาน

```bash
kubectl get nodes
kubectl get pods
kubectl describe pod <name>
kubectl apply -f deployment.yaml
kubectl delete pod <name>
kubectl logs <pod>
kubectl exec -it <pod> -- bash
```

---

## ⚙️ ส่วนของ Control Plane

| Component | หน้าที่ |
|-----------|---------|
| kube-apiserver | รับคำสั่งทั้งหมดจากผู้ใช้ |
| etcd | ฐานข้อมูลของ Cluster |
| kube-scheduler | กำหนดว่า Pod ไปอยู่ Node ใด |
| kube-controller-manager | ตรวจสอบว่า Resource ทำงานตาม spec |

---

## 🧠 สรุป
Kubernetes ช่วยให้เราควบคุมระบบขนาดใหญ่ได้ง่าย มีความยืดหยุ่นสูง และเหมาะกับระบบ production

> ศึกษาเพิ่มเติม: https://kubernetes.io/docs
