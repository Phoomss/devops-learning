# Kubernetes Components / ส่วนประกอบของคูเบอร์เนทีส

## What is Kubernetes? / คูเบอร์เนทีสคืออะไร?

Kubernetes (often abbreviated as K8s) is an open-source container orchestration platform for automating deployment, scaling, and management of containerized applications. It was originally designed by Google and is now maintained by the Cloud Native Computing Foundation (CNCF).

> คูเบอร์เนทีส (มักย่อว่า K8s) คือแพลตฟอร์มโอเพนซอร์สสำหรับจัดการคอนเทนเนอร์อัตโนมัติ ใช้สำหรับการติดตั้ง ขยายขนาด และจัดการแอปพลิเคชันที่ทำงานในคอนเทนเนอร์ เดิมออกแบบโดยกูเกิล และปัจจุบันดูแลโดย Cloud Native Computing Foundation (CNCF)

### Key Features / คุณสมบัติหลัก

- **Automated Rollouts and Rollbacks** / การปรับปรุงและย้อนกลับอัตโนมัติ
  - Kubernetes gradually rolls out changes and can rollback if something goes wrong.
  - คูเบอร์เนทีสค่อยๆ ปรับปรุงเปลี่ยนแปลงและสามารถย้อนกลับได้หากเกิดปัญหา

- **Self-Healing** / การซ่อมตัวเอง
  - Restarts failed containers, replaces and reschedules pods when nodes die.
  - เริ่มคอนเทนเนอร์ที่ล้มเหลวใหม่ แทนที่และจัดตารางพอดใหม่เมื่อโหนดขัดข้อง

- **Service Discovery and Load Balancing** / การค้นหาบริการและสมดุลโหลด
  - Exposes containers using DNS name or IP address and distributes traffic.
  - เปิดเผยคอนเทนเนอร์โดยใช้ชื่อ DNS หรือที่อยู่ IP และแจกจ่ายการจราจร

- **Storage Orchestration** / การจัดการที่เก็บข้อมูล
  - Mounts storage systems automatically (local, cloud, network storage).
  - เมาท์ระบบที่เก็บข้อมูลอัตโนมัติ (ท้องถิ่น คลาวด์ ที่เก็บข้อมูลเครือข่าย)

- **Secret and Configuration Management** / การจัดการความลับและการกำหนดค่า
  - Stores sensitive information (passwords, OAuth tokens) securely.
  - จัดเก็บข้อมูลสำคัญ (รหัสโทเค็น OAuth) อย่างปลอดภัย

---

## Architecture / สถาปัตยกรรม

A Kubernetes cluster consists of two main parts: the **Control Plane** (manages the cluster) and **Worker Nodes** (run the applications).

> คลัสเตอร์คูเบอร์เนทีสประกอบด้วยสองส่วนหลัก: **Control Plane** (จัดการคลัสเตอร์) และ **Worker Nodes** (รันแอปพลิเคชัน)

```
┌─────────────────────────────────────────────────────┐
│                   Control Plane                     │
│  ┌─────────────┐  ┌──────────┐  ┌─────────────────┐ │
│  │  API Server │  │   etcd   │  │    Scheduler    │ │
│  └──────┬──────┘  └──────────┘  └────────┬────────┘ │
│         │                                │          │
│  ┌──────┴────────────────────────────────┴────────┐ │
│  │          Controller Manager                    │ │
│  │     (Cloud Controller Manager)                 │ │
│  └─────────────────────┬──────────────────────────┘ │
└────────────────────────┼────────────────────────────┘
                         │
          ┌──────────────┴──────────────┐
          │                             │
┌─────────▼────────┐         ┌─────────▼────────┐
│    Worker Node 1 │         │    Worker Node 2 │
│ ┌──────────────┐ │         │ ┌──────────────┐ │
│ │   kubelet    │ │         │ │   kubelet    │ │
│ │ kube-proxy   │ │         │ │ kube-proxy   │ │
│ │  Containers  │ │         │ │  Containers  │ │
│ └──────────────┘ │         │ └──────────────┘ │
└──────────────────┘         └──────────────────┘
```

---

### Control Plane Components / ส่วนประกอบของระนาบควบคุม

The Control Plane is the brain of the Kubernetes cluster. It makes global decisions about the cluster (e.g., scheduling), detects and responds to cluster events.

> Control Plane เป็นสมองของคลัสเตอร์คูเบอร์เนทีส ตัดสินใจระดับคลัสเตอร์ (เช่น การจัดตาราง) ตรวจจับและตอบสนองต่อเหตุการณ์ในคลัสเตอร์

#### 1. API Server (kube-apiserver) / เซิร์ฟเวอร์ API

- The front-end of the Kubernetes control plane / ส่วนหน้าของระนาบควบคุมคูเบอร์เนทีส
- Handles all REST requests and validates them / จัดการคำขอ REST ทั้งหมดและตรวจสอบความถูกต้อง
- Only component that communicates directly with etcd / ส่วนประกอบเดียวที่สื่อสารกับ etcd โดยตรง
- Serves the Kubernetes API and is designed to scale horizontally / ให้บริการ API คูเบอร์เนทีสและออกแบบให้ขยายขนาดในแนวนอน
- All communication between components goes through the API Server / การสื่อสารทั้งหมดระหว่างส่วนประกอบผ่าน API Server

> **Key Concept / แนวคิดหลัก:** The API Server is the central management point. All operations (create, update, delete, read) go through it.
> API Server เป็นจุดจัดการกลาง การดำเนินการทั้งหมด (สร้าง อัพเดท ลบ อ่าน) ผ่านมัน

#### 2. etcd

- Consistent and highly available distributed key-value store / ที่เก็บคีย์-ค่าแบบกระจายที่สอดคล้องและมีความพร้อมใช้งานสูง
- Stores ALL cluster state and configuration data / จัดเก็บสถานะและการกำหนดค่าคลัสเตอร์ทั้งหมด
- Uses the Raft consensus algorithm for leader election / ใช้อัลกอริทึมฉันทามติ Raft สำหรับการเลือกผู้นำ
- Must be backed up regularly for disaster recovery / ต้องสำรองข้อมูลเป็นประจำสำหรับการกู้คืนจากภัยพิบัติ
- Runs as a separate service or can be managed by kubeadm / ทำงานเป็นบริการแยกหรือจัดการโดย kubeadm

> **Key Concept / แนวคิดหลัก:** etcd is the single source of truth for the cluster. If etcd data is lost, the cluster is lost.
> etcd เป็นแหล่งความจริงเดียวของคลัสเตอร์ หากข้อมูล etcd หาย คลัสเตอร์จะหาย

#### 3. Scheduler (kube-scheduler) / ตัวจัดตาราง

- Watches for newly created Pods with no assigned node / เฝ้าดูพอดที่สร้างใหม่ที่ยังไม่มีโหนดที่กำหนด
- Selects the best node for each Pod based on: / เลือกโหนดที่ดีที่สุดสำหรับแต่ละพอดโดยพิจารณาจาก:
  - **Resource requirements** (CPU, memory) / ความต้องการทรัพยากร (CPU, หน่วยความจำ)
  - **Affinity and anti-affinity rules** / กฎความใกล้ชิดและความไม่ใกล้ชิด
  - **Taints and tolerations** / จุดตำหนิและการยอมรับ
  - **Data locality** / ตำแหน่งที่ตั้งของข้อมูล
  - **Deadlines** / เส้นตาย

> **Key Concept / แนวคิดหลัก:** The Scheduler decides WHERE a Pod should run, but it does NOT actually run the Pod. The kubelet handles that.
> Scheduler ตัดสินใจว่าพอดควรรันที่ไหน แต่ไม่ได้รันพอดจริง kubelet จัดการเรื่องนั้น

#### 4. Controller Manager (kube-controller-manager) / ตัวจัดการคอนโทรลเลอร์

- Runs multiple controller processes as a single binary / รันกระบวนการคอนโทรลเลอร์หลายตัวเป็นไบนารีเดียว
- Each controller watches the cluster state and makes changes to reach the desired state / คอนโทรลเลอร์แต่ละตัวเฝ้าดูสถานะคลัสเตอร์และทำการเปลี่ยนแปลงเพื่อให้ถึงสถานะที่ต้องการ
- Key controllers include: / คอนโทรลเลอร์หลักรวมถึง:
  - **Node Controller**: Monitors node health and responds when nodes go down / ตรวจสอบสุขภาพโหนดและตอบสนองเมื่อโหนดล้มเหลว
  - **Replication Controller**: Maintains the correct number of pod replicas / รักษาจำนวนพอดจำลองที่ถูกต้อง
  - **Endpoint Controller**: Joins Services and Pods / เชื่อมต่อ Services และ Pods
  - **Service Account & Token Controllers**: Creates default accounts and API access tokens for new namespaces / สร้างบัญชีเริ่มต้นและโทเค็นเข้าถึง API สำหรับเนมสเปซใหม่

> **Key Concept / แนวคิดหลัก:** Controllers use a reconciliation loop - they continuously compare the current state with the desired state and act to minimize the difference.
> คอนโทรลเลอร์ใช้ลูปการปรองดอง - เปรียบเทียบสถานะปัจจุบันกับสถานะที่ต้องการอย่างต่อเนื่องและดำเนินการเพื่อลดความแตกต่าง

#### 5. Cloud Controller Manager / ตัวจัดการคอนโทรลเลอร์คลาวด์

- Embeds cloud-specific control logic / ฝังตรรกะควบคุมเฉพาะคลาวด์
- Interfaces with cloud providers (AWS, GCP, Azure, etc.) / เชื่อมต่อกับผู้ให้บริการคลาวด์
- Manages cloud-specific resources: / จัดการทรัพยากรเฉพาะคลาวด์:
  - **Route Controller**: Sets up routes in the cloud infrastructure / ตั้งเส้นทางในโครงสร้างพื้นฐานคลาวด์
  - **Service Controller**: Creates, updates, and deletes cloud load balancers / สร้าง อัพเดท และลบโหลดบาลานเซอร์คลาวด์
  - **Volume Controller**: Manages cloud storage volumes / จัดการปริมาณที่เก็บข้อมูลคลาวด์
  - **Node Controller**: Communicates with cloud provider to check node status / สื่อสารกับผู้ให้บริการคลาวด์เพื่อตรวจสอบสถานะโหนด

> **Key Concept / แนวคิดหลัก:** This allows Kubernetes to work with different cloud providers without changing the core Kubernetes code.
> สิ่งนี้ทำให้คูเบอร์เนทีสทำงานกับผู้ให้บริการคลาวด์ต่างๆ ได้โดยไม่ต้องเปลี่ยนโค้ดหลัก

---

### Node Components / ส่วนประกอบของโหนด

Worker Nodes are the machines (physical or virtual) that run the actual application workloads. Each node contains the services necessary to run Pods.

> Worker Nodes เป็นเครื่อง (กายภาพหรือเสมือน) ที่รันเวิร์กโหลดแอปพลิเคชันจริง แต่ละโหนดมีบริการที่จำเป็นสำหรับการรันพอด

#### 1. kubelet / คูบีเลต

- An agent that runs on EVERY node in the cluster / เอเจนต์ที่รันทุกโหนดในคลัสเตอร์
- Receives Pod specifications from the API Server / รับข้อกำหนดพอดจาก API Server
- Ensures that containers described in PodSpec are running and healthy / ตรวจสอบว่าคอนเทนเนอร์ที่อธิบายใน PodSpec ทำงานและมีสุขภาพดี
- Reports the node and pod status back to the control plane / รายงานสถานะโหนดและพอดกลับไปยังระนาบควบคุม
- Does NOT manage containers not created by Kubernetes / ไม่จัดการคอนเทนเนอร์ที่ไม่ได้สร้างโดยคูเบอร์เนทีส
- Communicates with the container runtime via CRI (Container Runtime Interface) / สื่อสารกับรันไทม์คอนเทนเนอร์ผ่าน CRI

> **Key Concept / แนวคิดหลัก:** kubelet is the "captain" of each node. It ensures all Pods assigned to its node are running as expected.
> kubelet คือ "กัปตัน" ของแต่ละโหนด ตรวจสอบว่าพอดทั้งหมดที่กำหนดให้โหนดทำงานตามคาด

#### 2. kube-proxy / คูบ์พร็อกซี

- A network proxy that runs on each node in the cluster / พร็อกซีเครือข่ายที่รันทุกโหนดในคลัสเตอร์
- Maintains network rules (iptables or IPVS) on nodes / รักษากฎเครือข่าย (iptables หรือ IPVS) บนโหนด
- Enables communication between Pods inside and outside the cluster / เปิดใช้งานการสื่อสารระหว่างพอดภายในและภายนอกคลัสเตอร์
- Implements the concept of Kubernetes Services / นำแนวคิด Services ของคูเบอร์เนทีสไปใช้
- Handles load balancing traffic to backend Pods / จัดการสมดุลโหลดการจราจรไปยังพอดแบ็กเอนด์

> **Key Concept / แนวคิดหลัก:** kube-proxy ensures that when you send traffic to a Service IP, it gets forwarded to the correct Pod.
> kube-proxy ตรวจสอบว่าเมื่อส่งการจราจรไปยัง Service IP มันจะถูกส่งต่อไปยังพอดที่ถูกต้อง

#### 3. Container Runtime / รันไทม์คอนเทนเนอร์

- The software responsible for running containers / ซอฟต์แวร์ที่รับผิดชอบในการรันคอนเทนเนอร์
- Must implement the CRI (Container Runtime Interface) / ต้องใช้งาน CRI (Container Runtime Interface)
- Common options: / ตัวเลือกทั่วไป:
  - **containerd**: Default runtime, industry standard / รันไทม์เริ่มต้น มาตรฐานอุตสาหกรรม
  - **CRI-O**: Lightweight runtime specifically for Kubernetes / รันไทม์น้ำหนักเบาสำหรับคูเบอร์เนทีสโดยเฉพาะ
  - **Docker Engine**: Deprecated (requires dockershim) / เลิกใช้แล้ว (ต้องมี dockershim)

> **Key Concept / แนวคิดหลัก:** Kubernetes does NOT run containers directly. It delegates this to a container runtime via CRI.
> คูเบอร์เนทีสไม่รันคอนเทนเนอร์โดยตรง แต่มอบหมายให้รันไทม์คอนเทนเนอร์ผ่าน CRI

---

## Key Objects / ออบเจกต์หลัก

Kubernetes objects are persistent entities in the Kubernetes system. They describe the desired state of your cluster.

> ออบเจกต์คูเบอร์เนทีสคือเอนทิตีถาวรในระบบคูเบอร์เนทีส อธิบายสถานะที่ต้องการของคลัสเตอร์

### Pod / พอด

The smallest deployable unit in Kubernetes. A Pod represents a single instance of a running process and can contain one or more containers.

> หน่วยที่เล็กที่สุดที่ติดตั้งได้ในคูเบอร์เนทีส พอดแสดงตัวอย่างเดียวของกระบวนการที่ทำงานและสามารถมีคอนเทนเนอร์หนึ่งตัวหรือมากกว่า

```yaml
apiVersion: v1             # API version / เวอร์ชัน API
kind: Pod                  # Object type / ประเภทออบเจกต์
metadata:
  name: nginx-pod          # Pod name / ชื่อพอด
  labels:                  # Labels for identification / ฉลากสำหรับระบุตัวตน
    app: nginx             #   - Key: app, Value: nginx / คู่คีย์-ค่า
spec:
  containers:
  - name: nginx            # Container name / ชื่อคอนเทนเนอร์
    image: nginx:latest    # Container image / อิมเมจคอนเทนเนอร์
    ports:
    - containerPort: 80    # Port the container listens on / พอร์ตที่คอนเทนเนอร์ฟัง
    resources:
      requests:            # Minimum resources requested / ทรัพยากรขั้นต่ำที่ขอ
        memory: "64Mi"     #   - Memory: 64 Mebibytes / หน่วยความจำ
        cpu: "250m"        #   - CPU: 250 millicores (0.25 core) / ซีพียู
      limits:              # Maximum resources allowed / ทรัพยากรสูงสุดที่อนุญาต
        memory: "128Mi"    #   - Memory: 128 Mebibytes / หน่วยความจำ
        cpu: "500m"        #   - CPU: 500 millicores (0.5 core) / ซีพียู
```

> **Key Concept / แนวคิดหลัก:** Containers in the same Pod share the same network namespace (same IP and port space) and can communicate via localhost.
> คอนเทนเนอร์ในพอดเดียวกันใช้เนมสเปซเครือข่ายเดียวกัน (IP และพอร์ตเดียวกัน) และสื่อสารผ่าน localhost ได้

---

### ReplicaSet / เรพลิกาเซต

Ensures a specified number of pod replicas are running at any given time. It is rarely used directly; Deployments manage ReplicaSets.

> ตรวจสอบว่ามีพอดจำลองจำนวนที่กำหนดรันอยู่ตลอดเวลา ไม่ค่อยใช้โดยตรง Deployment จัดการ ReplicaSet

```yaml
apiVersion: apps/v1        # API version / เวอร์ชัน API
kind: ReplicaSet           # Object type / ประเภทออบเจกต์
metadata:
  name: nginx-rs           # ReplicaSet name / ชื่อเรพลิกาเซต
spec:
  replicas: 3              # Desired number of pod replicas / จำนวนพอดจำลองที่ต้องการ
  selector:
    matchLabels:           # Select pods with these labels / เลือกพอดที่มีฉลากเหล่านี้
      app: nginx           #   - Must match pod template labels / ต้องตรงกับฉลากเทมเพลตพอด
  template:                # Pod template / เทมเพลตพอด
    metadata:
      labels:
        app: nginx         # Label applied to each pod / ฉลากที่ใช้กับแต่ละพอด
    spec:
      containers:
      - name: nginx        # Container name / ชื่อคอนเทนเนอร์
        image: nginx:1.21  # Container image / อิมเมจคอนเทนเนอร์
```

> **Key Concept / แนวคิดหลัก:** If a Pod dies, the ReplicaSet creates a new one to maintain the desired count.
> หากพอดตาย ReplicaSet จะสร้างพอดใหม่เพื่อรักษาจำนวนที่ต้องการ

---

### Deployment / ดีพลอยเมนต์

Manages ReplicaSets and provides declarative updates for Pods and ReplicaSets. This is the recommended way to manage stateless applications.

> จัดการ ReplicaSet และให้การอัพเดทแบบประกาศสำหรับพอดและ ReplicaSet นี่คือวิธีที่แนะนำสำหรับจัดการแอปพลิเคชันแบบไม่มีสถานะ

```yaml
apiVersion: apps/v1        # API version / เวอร์ชัน API
kind: Deployment           # Object type / ประเภทออบเจกต์
metadata:
  name: nginx-deployment   # Deployment name / ชื่อดีพลอยเมนต์
spec:
  replicas: 3              # Number of pod replicas / จำนวนพอดจำลอง
  strategy:
    type: RollingUpdate    # Update strategy / กลยุทธ์การอัพเดท
    rollingUpdate:
      maxSurge: 1          # Max extra pods during update / พอดเพิ่มเติมสูงสุดระหว่างอัพเดท
      maxUnavailable: 0    # Max unavailable pods / พอดที่ใช้การไม่ได้สูงสุด
  selector:
    matchLabels:
      app: nginx           # Select pods to manage / เลือกพอดที่จะจัดการ
  template:
    metadata:
      labels:
        app: nginx         # Labels for pods / ฉลากสำหรับพอด
    spec:
      containers:
      - name: nginx        # Container name / ชื่อคอนเทนเนอร์
        image: nginx:1.21  # Container image / อิมเมจคอนเทนเนอร์
        ports:
        - containerPort: 80 # Container port / พอร์ตคอนเทนเนอร์
```

> **Key Concept / แนวคิดหลัก:** Deployments handle rolling updates automatically - they replace old pods with new ones gradually, ensuring zero downtime.
> Deployment จัดการการอัพเดทแบบ rolling อัตโนมัติ - แทนที่พอดเก่าด้วยพอดใหม่อย่างค่อยเป็นค่อยไป รับประกันเวลาหยุดทำงานเป็นศูนย์

---

### Service / เซอร์วิส

A Service provides a stable network endpoint (IP address and DNS name) for a set of Pods. It solves the problem of Pods being ephemeral (they get new IPs when recreated).

> Service ให้เอนด์พอยต์เครือข่ายที่เสถียร (ที่อยู่ IP และชื่อ DNS) สำหรับชุดพอด แก้ปัญหาพอดที่มีอายุสั้น (ได้ IP ใหม่เมื่อสร้างใหม่)

```yaml
apiVersion: v1             # API version / เวอร์ชัน API
kind: Service              # Object type / ประเภทออบเจกต์
metadata:
  name: nginx-service      # Service name / ชื่อเซอร์วิส
spec:
  selector:
    app: nginx             # Select pods to expose / เลือกพอดที่จะเปิดเผย
  ports:
  - port: 80               # Service port (the port other services use) / พอร์ตเซอร์วิส (พอร์ตที่เซอร์วิสอื่นใช้)
    targetPort: 80         # Container port (the port the pod listens on) / พอร์ตคอนเทนเนอร์ (พอร์ตที่พอดฟัง)
  type: ClusterIP          # Service type / ประเภทเซอร์วิส
```

#### Service Types / ประเภทของเซอร์วิส

| Type / ประเภท | Description / คำอธิบาย | Use Case / กรณีใช้ |
|---|---|---|
| `ClusterIP` | Internal only (default) / ภายในเท่านั้น (ค่าเริ่มต้น) | Service-to-service communication / การสื่อสารระหว่างเซอร์วิส |
| `NodePort` | Exposes on each node's IP at a static port / เปิดเผยที่ IP แต่ละโหนดที่พอร์ตคงที่ | External access for testing / การเข้าถึงภายนอกสำหรับการทดสอบ |
| `LoadBalancer` | Cloud provider load balancer / โหลดบาลานเซอร์จากผู้ให้บริการคลาวด์ | Production external access / การเข้าถึงภายนอกสำหรับโปรดักชัน |
| `ExternalName` | Maps to a DNS name / แมปกับชื่อ DNS | External service reference / อ้างอิงเซอร์วิสภายนอก |

> **Key Concept / แนวคิดหลัก:** Services decouple clients from Pods. Clients talk to the Service, and the Service routes traffic to healthy Pods.
> Services แยกไคลเอนต์ออกจากพอด ไคลเอนต์คุยกับ Service และ Service เส้นทางการจราจรไปยังพอดที่สุขภาพดี

---

### Namespace / เนมสเปซ

A Namespace provides a mechanism for isolating groups of resources within a single cluster. Think of it as a "virtual cluster."

> เนมสเปซให้กลไกสำหรับการแยกกลุ่มทรัพยากรภายในคลัสเตอร์เดียว คิดว่าเป็น "คลัสเตอร์เสมือน"

```bash
# Create a new namespace / สร้างเนมสเปซใหม่
kubectl create namespace dev

# List resources in a specific namespace / แสดงทรัพยากรในเนมสเปซเฉพาะ
kubectl get pods -n dev

# Delete a namespace (and all resources inside it) / ลบเนมสเปซ (และทรัพยากรทั้งหมดภายใน)
kubectl delete namespace dev

# List all namespaces / แสดงเนมสเปซทั้งหมด
kubectl get namespaces

# Set default namespace for current context / ตั้งเนมสเปซเริ่มต้นสำหรับคอนเท็กซ์ปัจจุบัน
kubectl config set-context --current --namespace=dev
```

> **Key Concept / แนวคิดหลัก:** Namespaces are useful for multi-tenant environments (e.g., dev, staging, production) to avoid naming conflicts and provide resource isolation.
> เนมสเปซมีประโยชน์สำหรับสภาพแวดล้อมหลายผู้เช่า (เช่น dev, staging, production) เพื่อหลีกเลี่ยงความขัดแย้งของชื่อและการแยกทรัพยากร

---

### ConfigMap / คอนฟิกแมป

Store non-confidential configuration data in key-value pairs. Keeps configuration separate from container images.

> จัดเก็บข้อมูลการกำหนดค่าที่ไม่เป็นความลับในรูปแบบคีย์-ค่า แยกการกำหนดค่าออกจากอิมเมจคอนเทนเนอร์

```yaml
apiVersion: v1             # API version / เวอร์ชัน API
kind: ConfigMap            # Object type / ประเภทออบเจกต์
metadata:
  name: app-config         # ConfigMap name / ชื่อคอนฟิกแมป
data:
  APP_ENV: production      #   - Application environment / สภาพแวดล้อมแอปพลิเคชัน
  LOG_LEVEL: info          #   - Logging level / ระดับการบันทึก
  DATABASE_HOST: db.local  #   - Database hostname / ชื่อโฮสต์ฐานข้อมูล
```

#### Using ConfigMap in a Pod / การใช้ ConfigMap ในพอด

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: configmap-pod
spec:
  containers:
  - name: app
    image: myapp:latest
    envFrom:
    - configMapRef:
        name: app-config   # Reference the ConfigMap / อ้างอิงคอนฟิกแมป
```

> **Key Concept / แนวคิดหลัก:** ConfigMaps allow you to change application configuration without rebuilding container images.
> ConfigMaps อนุญาตให้เปลี่ยนการกำหนดค่าแอปพลิเคชันโดยไม่ต้องสร้างอิมเมจคอนเทนเนอร์ใหม่

---

### Secret / ซีเคร็ต

Store sensitive information such as passwords, OAuth tokens, and SSH keys. Data is stored as base64-encoded strings (note: NOT encrypted by default).

> จัดเก็บข้อมูลสำคัญ เช่น รหัสผ่าน โทเค็น OAuth และคีย์ SSH ข้อมูลจัดเก็บเป็นสตริงที่เข้ารหัส base64 (หมายเหตุ: ไม่ได้เข้ารหัสลับโดยค่าเริ่มต้น)

```yaml
apiVersion: v1             # API version / เวอร์ชัน API
kind: Secret               # Object type / ประเภทออบเจกต์
metadata:
  name: db-credentials     # Secret name / ชื่อซีเคร็ต
type: Opaque              # Generic secret type / ประเภทซีเคร็ตทั่วไป
data:
  username: YWRtaW4=       # base64 encoded "admin" / เข้ารหัส base64 ของ "admin"
  password: cGFzc3dvcmQ=   # base64 encoded "password" / เข้ารหัส base64 ของ "password"
```

#### Using Secret in a Pod / การใช้ Secret ในพอด

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-pod
spec:
  containers:
  - name: app
    image: myapp:latest
    env:
    - name: DB_USERNAME    # Environment variable name / ชื่อตัวแปรสภาพแวดล้อม
      valueFrom:
        secretKeyRef:
          name: db-credentials   # Secret name / ชื่อซีเคร็ต
          key: username          # Key in the Secret / คีย์ในซีเคร็ต
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: password
```

> **Key Concept / แนวคิดหลัก:** Secrets are NOT encrypted by default in etcd. For production, enable encryption at rest or use external secret managers (e.g., HashiCorp Vault, AWS Secrets Manager).
> Secrets ไม่ได้เข้ารหัสลับโดยค่าเริ่มต้นใน etcd สำหรับโปรดักชัน เปิดใช้งานการเข้ารหัสข้อมูลที่เหลือหรือใช้ตัวจัดการซีเคร็ตภายนอก

---

## Essential Commands / คำสั่งสำคัญ

### Cluster Information / ข้อมูลคลัสเตอร์

```bash
# Show cluster information / แสดงข้อมูลคลัสเตอร์
kubectl cluster-info

# List all nodes in the cluster / แสดงโหนดทั้งหมดในคลัสเตอร์
kubectl get nodes

# List all namespaces / แสดงเนมสเปซทั้งหมด
kubectl get namespaces

# Get detailed cluster information / รับข้อมูลคลัสเตอร์แบบละเอียด
kubectl cluster-info dump
```

---

### Resource Management / การจัดการทรัพยากร

```bash
# List all pods in the current namespace / แสดงพอดทั้งหมดในเนมสเปซปัจจุบัน
kubectl get pods

# List all services / แสดงเซอร์วิสทั้งหมด
kubectl get services

# List all deployments / แสดงดีพลอยเมนต์ทั้งหมด
kubectl get deployments

# List all resources / แสดงทรัพยากรทั้งหมด
kubectl get all

# Show pods with extended info (node, IP, etc.) / แสดงพอดพร้อมข้อมูลขยาย (โหนด, IP, ฯลฯ)
kubectl get pods -o wide

# Show detailed information about a specific pod / แสดงข้อมูลละเอียดเกี่ยวกับพอดเฉพาะ
kubectl describe pod <pod-name>

# List resources in a specific namespace / แสดงทรัพยากรในเนมสเปซเฉพาะ
kubectl get pods -n <namespace>

# Watch resources in real-time / ติดตามทรัพยากรแบบเรียลไทม์
kubectl get pods -w

# Output in YAML format / เอาต์พุตในรูปแบบ YAML
kubectl get pod <pod-name> -o yaml

# Output in JSON format / เอาต์พุตในรูปแบบ JSON
kubectl get pod <pod-name> -o json
```

---

### Apply and Delete / การติดตั้งและลบ

```bash
# Create or update resources from a YAML file / สร้างหรืออัพเดททรัพยากรจากไฟล์ YAML
kubectl apply -f manifest.yaml

# Create or update all resources in a directory / สร้างหรืออัพเดททรัพยากรทั้งหมดในไดเรกทอรี
kubectl apply -f ./directory/

# Delete resources from a YAML file / ลบทรัพยากรจากไฟล์ YAML
kubectl delete -f manifest.yaml

# Delete a specific pod / ลบพอดเฉพาะ
kubectl delete pod <pod-name>

# Force delete a pod (use with caution) / ลบพอดแบบบังคับ (ใช้ด้วยความระมัดระวัง)
kubectl delete pod <pod-name> --force --grace-period=0

# Delete all pods in a namespace / ลบพอดทั้งหมดในเนมสเปซ
kubectl delete pods --all -n <namespace>

# Dry run to see what would happen without making changes / รันทดสอบเพื่อดูว่าจะเกิดอะไรขึ้นโดยไม่ทำการเปลี่ยนแปลง
kubectl apply -f manifest.yaml --dry-run=client
```

---

### Debugging / การดีบัก

```bash
# View pod logs / ดูบันทึกพอด
kubectl logs <pod-name>

# Follow logs in real-time (like tail -f) / ติดตามบันทึกแบบเรียลไทม์ (เหมือน tail -f)
kubectl logs -f <pod-name>

# View logs from a specific container in a multi-container pod / ดูบันทึกจากคอนเทนเนอร์เฉพาะในพอดหลายคอนเทนเนอร์
kubectl logs <pod-name> -c <container-name>

# View previous container logs (after restart) / ดูก่อนหน้าบันทึกคอนเทนเนอร์ (หลังรีสตาร์ท)
kubectl logs <pod-name> --previous

# Execute a command inside a running container / รันคำสั่งในคอนเทนเนอร์ที่ทำงานอยู่
kubectl exec -it <pod-name> -- bash

# Execute a command in a specific container / รันคำสั่งในคอนเทนเนอร์เฉพาะ
kubectl exec -it <pod-name> -c <container-name> -- /bin/sh

# Show resource usage (CPU/memory) of pods / แสดงการใช้ทรัพยากร (CPU/หน่วยความจำ) ของพอด
kubectl top pods

# Show resource usage of nodes / แสดงการใช้ทรัพยากรของโหนด
kubectl top nodes

# Get events in the cluster / รับเหตุการณ์ในคลัสเตอร์
kubectl get events

# Sort events by creation time / เรียงลำดับเหตุการณ์ตามเวลาสร้าง
kubectl get events --sort-by='.metadata.creationTimestamp'

# Copy files to/from a pod / คัดลอกไฟล์ไป/กลับจากพอด
kubectl cp <pod-name>:/path/to/file ./local-file
kubectl cp ./local-file <pod-name>:/path/to/destination
```

---

### Scaling / การขยายขนาด

```bash
# Scale a deployment to a specific number of replicas / ขยายดีพลอยเมนต์เป็นจำนวนจำลองที่กำหนด
kubectl scale deployment nginx --replicas=5

# Autoscale based on CPU usage / ขยายขนาดอัตโนมัติตามการใช้ CPU
kubectl autoscale deployment nginx --min=2 --max=10 --cpu-percent=80

# View Horizontal Pod Autoscaler (HPA) / ดู Horizontal Pod Autoscaler
kubectl get hpa

# Delete an autoscaler / ลบตัวขยายขนาดอัตโนมัติ
kubectl delete hpa nginx
```

> **Note / หมายเหตุ:** Autoscaling requires metrics-server to be installed in the cluster.
> การขยายขนาดอัตโนมัติต้องมี metrics-server ติดตั้งในคลัสเตอร์

---

### Rolling Updates / การอัพเดทแบบ Rolling

```bash
# Update the container image / อัพเดทอิมเมจคอนเทนเนอร์
kubectl set image deployment/nginx nginx=nginx:1.22

# Check the status of the rollout / ตรวจสอบสถานะการ rollout
kubectl rollout status deployment/nginx

# View rollout history / ดูประวัติการ rollout
kubectl rollout history deployment/nginx

# View details of a specific revision / ดูรายละเอียดการแก้ไขเฉพาะ
kubectl rollout history deployment/nginx --revision=2

# Undo the last rollout (rollback) / ย้อนกลับการ rollout ล่าสุด (rollback)
kubectl rollout undo deployment/nginx

# Undo to a specific revision / ย้อนกลับถึงการแก้ไขเฉพาะ
kubectl rollout undo deployment/nginx --to-revision=2

# Pause a rollout (to make multiple changes) / หยุด rollout ชั่วคราว (เพื่อทำการเปลี่ยนแปลงหลายอย่าง)
kubectl rollout pause deployment/nginx

# Resume a paused rollout /_resume_rollout ที่หยุดไว้
kubectl rollout resume deployment/nginx
```

---

## Storage / ที่เก็บข้อมูล

Kubernetes provides several storage abstractions to manage persistent data for Pods.

> คูเบอร์เนทีสให้การแยกส่วนที่เก็บข้อมูลหลายอย่างเพื่อจัดการข้อมูลถาวรสำหรับพอด

### Storage Hierarchy / ลำดับชั้นที่เก็บข้อมูล

```
PersistentVolume (PV)
        ↑
        |  binds to / ผูกมัดกับ
PersistentVolumeClaim (PVC)
        ↑
        |  used by / ใช้โดย
      Pod
```

---

### PersistentVolume (PV) / ปริมาณถาวร

A cluster-wide piece of storage provisioned by an administrator or dynamically provisioned using StorageClasses.

> ที่เก็บข้อมูลระดับคลัสเตอร์ที่จัดเตรียมโดยผู้ดูแลระบบหรือจัดเตรียมแบบไดนามิกโดยใช้ StorageClasses

```yaml
apiVersion: v1             # API version / เวอร์ชัน API
kind: PersistentVolume     # Object type / ประเภทออบเจกต์
metadata:
  name: pv-data            # PV name / ชื่อ PV
spec:
  capacity:
    storage: 10Gi          # Total storage capacity / ความจุที่เก็บข้อมูลทั้งหมด
  accessModes:
    - ReadWriteOnce        # RWO: Read/write by a single node / อ่าน/เขียนโดยโหนดเดียว
    # ReadWriteMany (RWX): Read/write by multiple nodes / อ่าน/เขียนโดยหลายโหนด
    # ReadOnlyMany (ROX): Read-only by multiple nodes / อ่านอย่างเดียวโดยหลายโหนด
  hostPath:                # Storage type (local path for testing) / ประเภทที่เก็บ (พาธท้องถิ่นสำหรับทดสอบ)
    path: "/data"          # Path on the host node / พาธบนโหนดโฮสต์
```

#### Access Modes / โหมดการเข้าถึง

| Mode / โหมด | Abbreviation / อักษรย่อ | Description / คำอธิบาย |
|---|---|---|
| ReadWriteOnce | RWO | Volume can be mounted as read-write by a single node / โวลุ่มสามารถเมาท์แบบอ่าน-เขียนโดยโหนดเดียว |
| ReadWriteMany | RWX | Volume can be mounted as read-write by many nodes / โวลุ่มสามารถเมาท์แบบอ่าน-เขียนโดยหลายโหนด |
| ReadOnlyMany | ROX | Volume can be mounted as read-only by many nodes / โวลุ่มสามารถเมาท์แบบอ่านอย่างเดียวโดยหลายโหนด |

---

### PersistentVolumeClaim (PVC) / คำขอปริมาณถาวร

A request for storage by a user. PVCs consume PV resources. Kubernetes automatically binds a PVC to a matching PV.

> คำขอที่เก็บข้อมูลโดยผู้ใช้ PVC ใช้ทรัพยากร PV คูเบอร์เนทีสผูก PVC กับ PV ที่ตรงกันอัตโนมัติ

```yaml
apiVersion: v1             # API version / เวอร์ชัน API
kind: PersistentVolumeClaim # Object type / ประเภทออบเจกต์
metadata:
  name: pvc-data           # PVC name / ชื่อ PVC
spec:
  accessModes:
    - ReadWriteOnce        # Must match PV access mode / ต้องตรงกับโหมดการเข้าถึง PV
  resources:
    requests:
      storage: 5Gi         # Requested storage size / ขนาดที่เก็บข้อมูลที่ขอ
  # Optional: specify a storage class / ไม่บังคับ: ระบุ storage class
  # storageClassName: standard
```

#### Using PVC in a Pod / การใช้ PVC ในพอด

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: storage-pod
spec:
  containers:
  - name: app
    image: myapp:latest
    volumeMounts:
    - name: data-volume    # Volume name / ชื่อโวลุ่ม
      mountPath: /data     # Mount path inside container / พาธเมาท์ในคอนเทนเนอร์
  volumes:
  - name: data-volume      # Volume name / ชื่อโวลุ่ม
    persistentVolumeClaim:
      claimName: pvc-data  # Reference the PVC / อ้างอิง PVC
```

> **Key Concept / แนวคิดหลัก:** When a Pod is deleted, the data in the PVC persists. This is crucial for databases and stateful applications.
> เมื่อพอดถูกลบ ข้อมูลใน PVC ยังคงอยู่ สิ่งนี้สำคัญสำหรับฐานข้อมูลและแอปพลิเคชันที่มีสถานะ

---

### StorageClass (Optional) / คลาสที่เก็บ (ตัวเลือก)

StorageClasses enable dynamic provisioning of PVs. Instead of manually creating PVs, you create a StorageClass and PVCs automatically trigger PV creation.

> StorageClasses เปิดใช้งานการจัดเตรียม PV แบบไดนามิก แทนที่จะสร้าง PV ด้วยตนเอง คุณสร้าง StorageClass และ PVC จะทริกเกอร์การสร้าง PV อัตโนมัติ

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard           # StorageClass name / ชื่อ StorageClass
provisioner: kubernetes.io/no-provisioner   # Provisioner / ผู้จัดเตรียม
volumeBindingMode: WaitForFirstConsumer     # Wait for PVC before creating PV / รอ PVC ก่อนสร้าง PV
```

---

## Practice Exercises / แบบฝึกหัดปฏิบัติ

Complete these exercises to build hands-on Kubernetes experience:
> ทำแบบฝึกหัดเหล่านี้เพื่อสร้างประสบการณ์คูเบอร์เนทีสจริง:

1. **Deploy a 3-Replica Web Application / ติดตั้งแอปเว็บ 3 ตัวจำลอง**
   - Create a Deployment with 3 replicas of an nginx web server.
   - สร้าง Deployment พร้อมพอดจำลอง 3 ตัวของเซิร์ฟเวอร์เว็บ nginx
   - Verify all pods are running.
   - ตรวจสอบว่าพอดทั้งหมดทำงาน

2. **Create a Service to Expose the Application / สร้างเซอร์วิสเพื่อเปิดเผยแอปพลิเคชัน**
   - Expose the deployment using a ClusterIP service.
   - เผยแพร่ดีพลอยเมนต์โดยใช้เซอร์วิส ClusterIP
   - Test internal connectivity between pods.
   - ทดสอบการเชื่อมต่อภายในระหว่างพอด

3. **Configure ConfigMap for Application Settings / กำหนดค่า ConfigMap สำหรับการตั้งค่าแอปพลิเคชัน**
   - Create a ConfigMap with environment variables.
   - สร้าง ConfigMap พร้อมตัวแปรสภาพแวดล้อม
   - Mount it into your pods and verify the values.
   - เมาท์ลงในพอดและตรวจสอบค่า

4. **Set Up PersistentVolume for Data Storage / ตั้ง PersistentVolume สำหรับที่เก็บข้อมูล**
   - Create a PV and PVC (5Gi).
   - สร้าง PV และ PVC (5Gi)
   - Mount the PVC into a pod and write data.
   - เมาท์ PVC เข้าพอดและเขียนข้อมูล
   - Delete the pod and verify data persistence after recreation.
   - ลบพอดและตรวจสอบความถาวรของข้อมูลหลังสร้างใหม่

5. **Perform a Rolling Update and Rollback / ทำการอัพเดทแบบ Rolling และ Rollback**
   - Update the nginx image version.
   - อัพเดทเวอร์ชันอิมเมจ nginx
   - Monitor the rollout status.
   - ติดตามสถานะ rollout
   - Rollback to the previous version.
   - Rollback กลับเป็นเวอร์ชันก่อนหน้า

6. **Configure Resource Limits and Requests / กำหนดค่าขอบเขตและความต้องการทรัพยากร**
   - Set CPU and memory requests/limits on your deployment.
   - ตั้งค่าคำขอ/ขอบเขต CPU และหน่วยความจำบนดีพลอยเมนต์
   - Observe behavior when limits are exceeded.
   - สังเกตพฤติกรรมเมื่อเกินขอบเขต

7. **Set Up Horizontal Pod Autoscaler (HPA) / ตั้ง Horizontal Pod Autoscaler**
   - Configure HPA to scale between 2 and 10 replicas based on CPU usage.
   - กำหนดค่า HPA เพื่อขยายขนาดระหว่าง 2 ถึง 10 ตัวจำลองตามการใช้ CPU
   - Generate load and observe autoscaling.
   - สร้างโหลดและสังเกตการขยายขนาดอัตโนมัติ

8. **Working with Namespaces / ทำงานกับเนมสเปซ**
   - Create `dev`, `staging`, and `production` namespaces.
   - สร้างเนมสเปซ `dev`, `staging` และ `production`
   - Deploy different versions of an app in each.
   - ติดตั้งแอปเวอร์ชันต่างๆ ในแต่ละที่
   - Practice resource isolation.
   - ฝึกการแยกทรัพยากร

9. **Working with Secrets / ทำงานกับซีเคร็ต**
   - Create a Secret with database credentials.
   - สร้างซีเคร็ตพร้อมข้อมูลประจำตัวฐานข้อมูล
   - Mount it as environment variables in a pod.
   - เมาท์เป็นตัวแปรสภาพแวดล้อมในพอด
   - Verify the values inside the container.
   - ตรวจสอบค่าภายในคอนเทนเนอร์

10. **Debugging Practice / ฝึกการดีบัก**
    - Deploy a pod that crashes intentionally.
    - ติดตั้งพอดที่ขัดข้องโดยเจตนา
    - Use `kubectl logs`, `kubectl describe`, and `kubectl exec` to diagnose the issue.
    - ใช้ `kubectl logs`, `kubectl describe` และ `kubectl exec` เพื่อวินิจฉัยปัญหา

---

## Resources / แหล่งข้อมูล

### Official Documentation / เอกสารทางการ
- [Kubernetes Official Documentation / เอกสารทางการคูเบอร์เนทีส](https://kubernetes.io/docs/)
- [Kubernetes Concepts / แนวคิดคูเบอร์เนทีส](https://kubernetes.io/docs/concepts/)
- [Kubernetes API Reference / อ้างอิง API คูเบอร์เนทีส](https://kubernetes.io/docs/reference/)

### Interactive Learning / การเรียนรู้แบบโต้ตอบ
- [Kubernetes Basics (Official Tutorial) / พื้นฐานคูเบอร์เนทีส (บทเรียนทางการ)](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
- [Katacoda Scenarios / สถานการณ์ Katacoda](https://www.katacoda.com/courses/kubernetes)
- [Play with Kubernetes / เล่นกับคูเบอร์เนทีส](https://labs.play-with-k8s.com/)

### Books / หนังสือ
- "Kubernetes Up & Running" by Kelsey Hightower et al. / "คูเบอร์เนทีสเริ่มต้นและใช้งาน" โดย Kelsey Hightower และคณะ
- "Kubernetes in Action" by Marko Lukša / "คูเบอร์เนทีสในทางปฏิบัติ" โดย Marko Lukša

### Community / ชุมชน
- [Kubernetes Slack / สแล็คคูเบอร์เนทีส](https://kubernetes.slack.com/)
- [CNCF Forums / เวที CNCF](https://discuss.cncf.io/)
- [Stack Overflow - Kubernetes Tag / สแต็กโอเวอร์โฟลว์ - แท็กคูเบอร์เนทีส](https://stackoverflow.com/questions/tagged/kubernetes)

### Cheat Sheets / แผ่นโกง
- [Official kubectl Cheat Sheet / แผ่นโกง kubectl ทางการ](https://kubernetes.io/docs/reference/kubectl/quick-reference/)
- [Kubernetes Cheat Sheet (PDF) / แผ่นโกงคูเบอร์เนทีส (PDF)](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

---

> **Quick Reference Summary / สรุปข้อมูลอ้างอิงด่วน:**
>
> | Concept / แนวคิด | Purpose / วัตถุประสงค์ |
> |---|---|
> | Pod / พอด | Smallest deployable unit / หน่วยที่เล็กที่สุดที่ติดตั้งได้ |
> | ReplicaSet / เรพลิกาเซต | Maintains pod replica count / รักษาจำนวนพอดจำลอง |
> | Deployment / ดีพลอยเมนต์ | Manages rolling updates / จัดการการอัพเดทแบบ rolling |
> | Service / เซอร์วิส | Stable network endpoint / เอนด์พอยต์เครือข่ายที่เสถียร |
> | Namespace / เนมสเปซ | Virtual cluster isolation / การแยกคลัสเตอร์เสมือน |
> | ConfigMap / คอนฟิกแมป | Non-sensitive configuration / การกำหนดค่าที่ไม่เป็นความลับ |
> | Secret / ซีเคร็ต | Sensitive data storage / ที่เก็บข้อมูลสำคัญ |
> | PersistentVolume / ปริมาณถาวร | Cluster-wide storage / ที่เก็บข้อมูลระดับคลัสเตอร์ |
> | PVC / คำขอปริมาณถาวร | User storage request / คำขอที่เก็บข้อมูลผู้ใช้ |
> | kubelet | Node agent / เอเจนต์โหนด |
> | kube-proxy | Network proxy / พร็อกซีเครือข่าย |
> | API Server | Control plane front-end / ส่วนหน้าระนาบควบคุม |
> | etcd | Cluster data store / ที่เก็บข้อมูลคลัสเตอร์ |
> | Scheduler | Pod-to-node assignment / การกำหนดพอดไปโหนด |
> | Controller Manager | State reconciliation / การปรองดองสถานะ |
