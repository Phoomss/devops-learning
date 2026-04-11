# Kubernetes Installation / การติดตั้ง Kubernetes

## Installation Methods Comparison / เปรียบเทียบวิธีการติดตั้ง

| Method / วิธีการ | Complexity / ความยากง่าย | Use Case / กรณีใช้งาน | Features / คุณสมบัติ | Best For / เหมาะสำหรับ |
|--------|-----------|----------|----------|----------|
| **kubeadm** | Medium / ปานกลาง | Learning, custom setups / การเรียนรู้, ตั้งค่าแบบกำหนดเอง | Minimal, official tool / เครื่องมือทางการแบบมินิมอล | Understanding K8s internals / ทำความเข้าใจภายในของ K8s |
| **Kubespray** | Medium-Hard / ปานกลาง-ยาก | Production, automation / การผลิต, ระบบอัตโนมัติ | Ansible-based, flexible / ใช้ Ansible, ยืดหยุ่น | Automated production deploys / การติดตั้งระบบผลิตอัตโนมัติ |
| **Rancher** | Easy / ง่าย | Enterprise, multi-cluster / องค์กร, หลายคลัสเตอร์ | GUI, full platform / มี GUI, แพลตฟอร์มครบ | Managing multiple clusters / จัดการหลายคลัสเตอร์ |
| **k3s** | Easy / ง่าย | Edge, IoT, dev / เอจ, IoT, พัฒนา | Lightweight, single binary / น้ำหนักเบา, ไฟล์เดียว | Resource-constrained envs / สภาพแวดล้อมที่มีทรัพยากรจำกัด |
| **kind** | Easy / ง่าย | Local development / พัฒนาในเครื่อง | Docker-based / ใช้ Docker | Testing, CI/CD / ทดสอบ, CI/CD |
| **minikube** | Easy / ง่าย | Local learning / เรียนรู้ในเครื่อง | VM-based / ใช้ VM | Learning K8s locally / เรียนรู้ K8s ในเครื่อง |

---

## Method 1: kubeadm / วิธีที่ 1: kubeadm

### What is it? / kubeadm คืออะไร?
Official Kubernetes tool for bootstrapping a minimum viable cluster.
เครื่องมือทางการของ Kubernetes สำหรับการเริ่มต้นสร้างคลัสเตอร์ขั้นต่ำที่ใช้งานได้

### Prerequisites / ข้อกำหนดเบื้องต้น
- Ubuntu 20.04+ (2+ nodes) / Ubuntu 20.04 ขึ้นไป (2 โหนดขึ้นไป)
- 2+ CPU, 2GB RAM per node / ซีพียู 2 คอร์ขึ้นไป, แรม 2GB ขึ้นไปต่อโหนด
- Network connectivity between nodes / โหนดทุกตัวเชื่อมต่อเครือข่ายถึงกัน
- Disabled swap / ปิดการใช้งาน swap
- Unique hostname for each node / แต่ละโหนดต้องมี hostname ที่ไม่ซ้ำกัน

### Installation Steps / ขั้นตอนการติดตั้ง

#### ON ALL NODES / บนทุกโหนด

**1. Disable swap / ปิดการใช้งาน swap**
```bash
# ปิด swap ชั่วคราว (จนกว่าจะ reboot)
swapoff -a
# ปิด swap ถาวร โดยคอมเมนต์บรรทัด swap ใน /etc/fstab
sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```

**2. Install container runtime (containerd) / ติดตั้ง container runtime (containerd)**
```bash
# อัปเดตรายการแพ็กเกจ
apt update
# ติดตั้ง containerd
apt install -y containerd
# สร้างไดเรกทอรีสำหรับไฟล์ตั้งค่า
mkdir -p /etc/containerd
# สร้างไฟล์ตั้งค่าเริ่มต้น
containerd config default > /etc/containerd/config.toml

# เปิดใช้งาน systemd cgroup driver (จำเป็นสำหรับ Kubernetes)
sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml
# รีสตาร์ท containerd เพื่อให้การตั้งค่ามีผล
systemctl restart containerd
```

**3. Install kubeadm, kubelet, kubectl / ติดตั้ง kubeadm, kubelet, kubectl**
```bash
# ติดตั้งแพ็กเกจที่จำเป็นสำหรับการเพิ่ม repository
apt install -y apt-transport-https ca-certificates curl

# เพิ่ม Kubernetes apt repository
# ดาวน์โหลคคีย์ GPG และบันทึกไว้ใน keyring
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | \
  gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# เพิ่มแหล่งแพ็กเกจ Kubernetes ลงใน sources list
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] \
  https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | \
  tee /etc/apt/sources.list.d/kubernetes.list

# อัปเดตและติดตั้งเครื่องมือ Kubernetes
apt update
apt install -y kubelet kubeadm kubectl
# ป้องกันการอัปเดตอัตโนมัติสำหรับแพ็กเกจเหล่านี้
apt-mark hold kubelet kubeadm kubectl
```

**4. Enable kubelet / เปิดใช้งาน kubelet**
```bash
# เปิดใช้งาน kubelet ให้เริ่มทำงานทันทีและเริ่มอัตโนมัติเมื่อ boot
systemctl enable --now kubelet
```

#### ON CONTROL PLANE NODE / บนโหนด Control Plane

**5. Initialize cluster / เริ่มต้นสร้างคลัสเตอร์**
```bash
# เริ่มต้นคลัสเตอร์โดยกำหนดช่วง IP สำหรับ Pod network (ใช้ 10.244.0.0/16 สำหรับ Calico)
kubeadm init --pod-network-cidr=10.244.0.0/16
```

**6. Setup kubeconfig / ตั้งค่า kubeconfig**
```bash
# สร้างไดเรกทอรี .kube ใน home
mkdir -p $HOME/.kube
# คัดลอกไฟล์ admin.conf มาใช้เป็น config
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
# ตั้งค่าเจ้าของไฟล์ให้เป็น user ปัจจุบัน
chown $(id -u):$(id -g) $HOME/.kube/config
```

**7. Verify cluster / ตรวจสอบคลัสเตอร์**
```bash
# แสดงรายการโหนดทั้งหมดในคลัสเตอร์
kubectl get nodes
# แสดง Pod ทั้งหมดในระบบ (namespace kube-system)
kubectl get pods -n kube-system
```

**8. Install CNI (Calico) / ติดตั้ง CNI (Calico)**
```bash
# ติดตั้ง Calico เป็น Container Network Interface (CNI) สำหรับจัดการเครือข่ายของ Pod
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml
```

**9. Save join command / บันทึกคำสั่ง join**
```bash
# คำสั่ง kubeadm init จะแสดงคำสั่ง join สำหรับโหนด worker - ให้บันทึกไว้!
# ตัวอย่าง:
# kubeadm join <control-plane-ip>:6443 \
#   --token abc123 \
#   --discovery-token-ca-cert-hash sha256:hash
```

#### ON WORKER NODES / บนโหนด Worker

**10. Join cluster / เข้าร่วมคลัสเตอร์**
```bash
# ใช้คำสั่ง join ที่ได้จาก kubeadm init เพื่อเพิ่มโหนด worker เข้าคลัสเตอร์
kubeadm join <control-plane-ip>:6443 \
  --token <token> \
  --discovery-token-ca-cert-hash <hash>
```

**11. Verify on control plane / ตรวจสอบบน control plane**
```bash
# ตรวจสอบว่าโหนดทั้งหมดแสดงสถานะ Ready
kubectl get nodes
# ควรแสดงโหนดทั้งหมดเป็น Ready
```

### Generate New Join Token (if expired) / สร้าง Join Token ใหม่ (หากหมดอายุ)
```bash
# รันบน control plane
# สร้าง token ใหม่และแสดงคำสั่ง join ที่สมบูรณ์
kubeadm token create --print-join-command
```

---

## Method 2: Kubespray / วิธีที่ 2: Kubespray

### What is it? / Kubespray คืออะไร?
Ansible-based Kubernetes deployment tool for production-grade clusters.
เครื่องมือติดตั้ง Kubernetes แบบใช้ Ansible สำหรับคลัสเตอร์ระดับการผลิต

### Prerequisites / ข้อกำหนดเบื้องต้น
- Ansible installed on deployment machine / ติดตั้ง Ansible บนเครื่องที่จะใช้ deploy
- SSH access to all nodes / สามารถ SSH ไปยังทุกโหนดได้
- Python on all nodes / มี Python ติดตั้งบนทุกโหนด
- SSH key-based authentication / ใช้การยืนยันตัวตนด้วย SSH key

### Installation Steps / ขั้นตอนการติดตั้ง

**1. Clone Kubespray / ดึงโค้ด Kubespray**
```bash
# คัดลอก repository ของ Kubespray
git clone https://github.com/kubernetes-sigs/kubespray.git
cd kubespray

# Checkout เวอร์ชันที่เสถียร
git checkout release-2.24
```

**2. Install dependencies / ติดตั้ง dependencies**
```bash
# ติดตั้ง Python packages ที่จำเป็นผ่าน pip
pip install -r requirements.txt
```

**3. Create inventory / สร้าง inventory**
```bash
# คัดลอกตัวอย่าง inventory มาใช้
cp -rfp inventory/sample inventory/mycluster

# กำหนด IP ของโหนดทั้งหมด
declare -a IPS=(192.168.1.10 192.168.1.11 192.168.1.12)
# ใช้สคริปต์ inventory builder สร้างไฟล์ hosts.yaml อัตโนมัติ
CONFIG_FILE=inventory/mycluster/hosts.yaml \
  python3 contrib/inventory_builder/inventory.py ${IPS[@]}
```

**4. Review and customize inventory / ตรวจสอบและปรับแต่ง inventory**
```bash
# ดูเนื้อหาของไฟล์ inventory ที่สร้างขึ้นมา
cat inventory/mycluster/hosts.yaml
```

**Sample Inventory / ตัวอย่าง Inventory:**
```yaml
all:
  hosts:
    node1:
      ansible_host: 192.168.1.10  # IP ที่ใช้เชื่อมต่อ
      ip: 192.168.1.10            # IP ของโหนด
    node2:
      ansible_host: 192.168.1.11
      ip: 192.168.1.11
    node3:
      ansible_host: 192.168.1.12
      ip: 192.168.1.12

  children:
    kube_control_plane:  # กลุ่มโหนด control plane
      hosts:
        node1:
    kube_node:           # กลุ่มโหนด worker ทั้งหมด (รวม control plane)
      hosts:
        node1:
        node2:
        node3:
    k8s_cluster:         # กลุ่มคลัสเตอร์ทั้งหมด
      children:
        kube_control_plane:
        kube_node:
```

**5. Configure variables (optional) / กำหนดตัวแปร (ไม่บังคับ)**
```bash
# ดูไฟล์ตั้งค่าตัวแปรของคลัสเตอร์
cat inventory/mycluster/group_vars/k8s_cluster/k8s-cluster.yml
# สามารถปรับแต่งเวอร์ชัน Kubernetes, network plugin, และอื่นๆ
```

**6. Deploy cluster / Deploy คลัสเตอร์**
```bash
# รัน Ansible playbook เพื่อติดตั้งคลัสเตอร์
# --user=ubuntu: ใช้ user ubuntu ในการเชื่อมต่อ
# --become: ยกระดับสิทธิ์เป็น root
# --become-user=root: ระบุ user ที่จะใช้เป็น root
ansible-playbook -i inventory/mycluster/hosts.yaml \
  --user=ubuntu \
  --become \
  --become-user=root \
  cluster.yml
```

**7. Verify cluster / ตรวจสอบคลัสเตอร์**
```bash
# คัดลอก kubeconfig จาก control plane node
scp node1:/etc/kubernetes/admin.conf ~/.kube/config

# ตรวจสอบคลัสเตอร์
kubectl get nodes
```

---

## Method 3: Rancher / วิธีที่ 3: Rancher

### What is it? / Rancher คืออะไร?
Complete Kubernetes management platform with GUI for managing multiple clusters.
แพลตฟอร์มจัดการ Kubernetes แบบครบวงจรพร้อม GUI สำหรับจัดการหลายคลัสเตอร์

### Installation Steps / ขั้นตอนการติดตั้ง

**1. Install Docker (on Rancher server) / ติดตั้ง Docker (บนเซิร์ฟเวอร์ Rancher)**
```bash
# อัปเดตรายการแพ็กเกจ
apt update
# ติดตั้ง Docker
apt install -y docker.io
```

**2. Run Rancher container / รัน Rancher container**
```bash
# รัน Rancher ในโหมด detached
# --restart=unless-stopped: รีสตาร์ทอัตโนมัติเสมอ
# -p 80:80 -p 443:443: เปิดพอร์ต HTTP และ HTTPS
# --privileged: ให้สิทธิ์เต็มที่แก่ container
docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  --privileged \
  --name rancher \
  rancher/rancher:latest
```

**3. Access Rancher UI / เข้าใช้งาน Rancher UI**
```bash
# เปิดเบราว์เซอร์ไปที่ https://<server-ip>

# ดึงรหัสผ่านเริ่มต้น (Bootstrap Password)
docker logs rancher 2>&1 | grep "Bootstrap Password"
```

**4. Create cluster via UI / สร้างคลัสเตอร์ผ่าน UI**

1. Login and set admin password / เข้าสู่ระบบและตั้งรหัสผ่าน admin
2. Click "Create Cluster" -> "Custom" / คลิก "Create Cluster" -> "Custom"
3. Select Kubernetes version / เลือกเวอร์ชัน Kubernetes
4. Configure node roles / กำหนดบทบาทโหนด:
   - Control Plane / โหนดควบคุม
   - etcd / ฐานข้อมูล etcd
   - Worker / โหนดทำงาน
5. Copy the generated registration command / คัดลอกคำสั่งลงทะเบียนที่สร้างขึ้นมา

**5. Register nodes / ลงทะเบียนโหนด**
```bash
# รันคำสั่งนี้บนแต่ละ worker node (คัดลอกจาก Rancher UI)
# คำสั่งนี้จะติดตั้ง Rancher agent เพื่อเชื่อมต่อโหนดกับคลัสเตอร์
docker run -d --privileged --restart=unless-stopped \
  --net=host --pid=host --uts=host \
  -v /:/host \
  -v /var/run:/var/run \
  --privileged \
  rancher/rancher-agent:v2.x.x \
  --server https://rancher.example.com \
  --token abc123 \
  --ca-checksum sha256:hash \
  --etcd --controlplane --worker
```

**6. Download kubeconfig / ดาวน์โหลค kubeconfig**
```bash
# ไปที่ Rancher UI -> Cluster -> Kubeconfig File
# คัดลอกและบันทึกเป็น ~/.kube/config
```

---

## Method 4: k3s (Lightweight) / วิธีที่ 4: k3s (แบบน้ำหนักเบา)

### What is it? / k3s คืออะไร?
Lightweight Kubernetes distribution for edge, IoT, and development.
Kubernetes แบบน้ำหนักเบาสำหรับ edge, IoT, และการพัฒนา

### Installation / การติดตั้ง

```bash
# ติดตั้ง server (ทำหน้าที่ control plane)
# สคริปต์นี้จะดาวน์โหลดและติดตั้ง k3s อัตโนมัติ
curl -sfL https://get.k3s.io | sh -

# ดึง token สำหรับใช้เชื่อมต่อ worker node
cat /var/lib/rancher/k3s/server/node-token

# ติดตั้ง agent (worker node)
# K3S_URL: URL ของ server
# K3S_TOKEN: token ที่ได้จากขั้นตอนก่อนหน้า
curl -sfL https://get.k3s.io | K3S_URL=https://server:6443 \
  K3S_TOKEN=token sh -

# ตรวจสอบว่าคลัสเตอร์ทำงาน
kubectl get nodes
```

---

## Method 5: kind (Kubernetes in Docker) / วิธีที่ 5: kind (Kubernetes ใน Docker)

### What is it? / kind คืออะไร?
Run Kubernetes locally using Docker containers as nodes.
รัน Kubernetes ในเครื่องโดยใช้ Docker containers เป็นโหนด

### Installation / การติดตั้ง

```bash
# ติดตั้ง kind ผ่าน Go
# ต้องมี Go ติดตั้งไว้ในเครื่องก่อน
go install sigs.k8s.io/kind@v0.20.0

# สร้างคลัสเตอร์ชื่อ "dev"
kind create cluster --name dev

# ตรวจสอบข้อมูลคลัสเตอร์
kubectl cluster-info

# ลบคลัสเตอร์เมื่อไม่ต้องการใช้งาน
kind delete cluster --name dev
```

---

## Post-Installation Tasks / งานหลังการติดตั้ง

### 1. Verify Cluster / ตรวจสอบคลัสเตอร์
```bash
# แสดงรายการโหนดทั้งหมด (ควรเห็นสถานะ Ready)
kubectl get nodes
# แสดง Pod ทั้งหมดในระบบ
kubectl get pods -n kube-system
# แสดงบริการทั้งหมด
kubectl get svc
```

### 2. Install Metrics Server / ติดตั้ง Metrics Server
```bash
# ติดตั้ง Metrics Server สำหรับดูการใช้ทรัพยากร
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
# แสดงการใช้ CPU/RAM ของโหนด
kubectl top nodes
# แสดงการใช้ CPU/RAM ของ Pod
kubectl top pods
```

### 3. Install Dashboard (Optional) / ติดตั้ง Dashboard (ไม่บังคับ)
```bash
# ติดตั้ง Kubernetes Dashboard UI
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
# เปิด proxy เพื่อเข้าถึง Dashboard
kubectl proxy
# เข้าใช้งานที่: http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

### 4. Test Deployment / ทดสอบ Deployment
```bash
# สร้าง deployment ชื่อ nginx ใช้ image nginx
kubectl create deployment nginx --image=nginx
# เปิดพอร์ต 80 แบบ NodePort เพื่อให้เข้าถึงจากภายนอกได้
kubectl expose deployment nginx --port=80 --type=NodePort
# ดู service เพื่อหา port ที่ถูก assign
kubectl get svc
# ทดสอบเข้าถึง nginx
curl http://<node-ip>:<node-port>
```

---

## Troubleshooting / การแก้ปัญหา

### Nodes Not Ready / โหนดสถานะไม่ Ready
```bash
# ตรวจสอบ log ของ kubelet แบบ real-time
journalctl -u kubelet -f

# ตรวจสอบเงื่อนไขของโหนด (ดูเงื่อนไขที่เป็นปัญหา)
kubectl describe node <node-name>
```

### Pods Stuck in Pending / Pod ติดค้างสถานะ Pending
```bash
# ดูรายละเอียด Pod เพื่อหาสาเหตุ
kubectl describe pod <pod-name>
# ดู events เรียงตามเวลาล่าสุด เพื่อหาข้อผิดพลาด
kubectl get events --sort-by='.lastTimestamp'
```

### Certificate Issues / ปัญหาลิขสิทธิ์ (Certificates)
```bash
# ตรวจสอบวันหมดอายุของ certificates
kubeadm certs check-expiration

# ต่ออายุ certificates ทั้งหมด
kubeadm certs renew all
```

### Reset Cluster / รีเซ็ตคลัสเตอร์
```bash
# รันบนทุกโหนดที่ต้องการลบคลัสเตอร์
# ลบการตั้งค่า kubeadm ทั้งหมด
kubeadm reset -f
# ลบไฟล์ตั้งค่า CNI
rm -rf /etc/cni/net.d
# รีสตาร์ท kubelet
systemctl restart kubelet
```

---

## Practice Exercises (Task 4) / แบบฝึกหัด (งานที่ 4)

1. **Install with kubeadm / ติดตั้งด้วย kubeadm**
   - Set up a 3-node cluster (1 control, 2 workers) / ตั้งค่าคลัสเตอร์ 3 โหนด (1 control, 2 workers)
   - Install Calico CNI / ติดตั้ง Calico CNI
   - Deploy a test application / Deploy แอปพลิเคชันทดสอบ

2. **Install with Kubespray / ติดตั้งด้วย Kubespray**
   - Automate cluster deployment / ทำระบบอัตโนมัติสำหรับ deploy คลัสเตอร์
   - Compare with manual kubeadm installation / เปรียบเทียบกับการติดตั้ง kubeadm แบบ manual

3. **Install with Rancher / ติดตั้งด้วย Rancher**
   - Set up Rancher server / ตั้งค่าเซิร์ฟเวอร์ Rancher
   - Create and manage a cluster via UI / สร้างและจัดการคลัสเตอร์ผ่าน UI

4. **Compare methods / เปรียบเทียบวิธีการ**
   - Document pros and cons of each method / บันทึกข้อดีและข้อเสียของแต่ละวิธี
   - Recommend use cases for each / แนะนำกรณีใช้งานที่เหมาะสมแต่ละวิธี

5. **Bonus / เพิ่มเติม**
   - Try k3s for lightweight setup / ลอง k3s สำหรับติดตั้งแบบน้ำหนักเบา
   - Use kind for local testing / ใช้ kind สำหรับทดสอบในเครื่อง
   - Practice cluster troubleshooting / ฝึกการแก้ปัญหาคลัสเตอร์

---

## Resources / แหล่งข้อมูล
- [kubeadm Docs / เอกสาร kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/)
- [Kubespray GitHub](https://github.com/kubernetes-sigs/kubespray)
- [Rancher Docs / เอกสาร Rancher](https://rancher.com/docs/)
- [k3s Docs / เอกสาร k3s](https://docs.k3s.io/)
