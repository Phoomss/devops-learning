# Kubernetes CNI & OSI Model / คิวเบอร์เนติส CNI และโมเดล OSI

---

## Table of Contents / สารบัญ

1. [What is CNI? / CNI คืออะไร?](#what-is-cni--cni-คืออะไร)
2. [Why CNI is Needed / ทำไมต้องใช้ CNI](#why-cni-is-needed--ทำไมต้องใช้-cni)
3. [Kubernetes Networking Requirements / ข้อกำหนดเครือข่ายของคิวเบอร์เนติส](#kubernetes-networking-requirements--ข้อกำหนดเครือข่ายของคิวเบอร์เนติส)
4. [Popular CNI Plugins / ปลั๊กอิน CNI ยอดนิยม](#popular-cni-plugins--ปลั๊กอิน-cni-ยอดนิยม)
5. [How CNI Relates to OSI Model / CNI สัมพันธ์กับโมเดล OSI อย่างไร](#how-cni-relates-to-osi-model--cni-สัมพันธ์กับโมเดล-osi-อย่างไร)
6. [CNI in Action: Calico / CNI ในการปฏิบัติ: Calico](#cni-in-action-calico--cni-ในการปฏิบัติ-calico)
7. [Network Policies / นโยบายเครือข่าย](#network-policies--นโยบายเครือข่าย)
8. [CNI Comparison / เปรียบเทียบ CNI](#cni-comparison--เปรียบเทียบ-cni)
9. [Practical Exercise: Test Pod Communication / แบบฝึกหัด: ทดสอบการสื่อสารของ Pod](#practical-exercise-test-pod-communication--แบบฝึกหัดทดสอบการสื่อสารของ-pod)
10. [Troubleshooting CNI / การแก้ไขปัญหา CNI](#troubleshooting-cni--การแก้ไขปัญหา-cni)
11. [CNI Configuration / การตั้งค่า CNI](#cni-configuration--การตั้งค่า-cni)
12. [Why CNI and OSI Matter Together / ทำไม CNI และ OSI สำคัญร่วมกัน](#why-cni-and-osi-matter-together--ทำไม-cni-และ-osi-สำคัญร่วมกัน)
13. [Network Flow Example / ตัวอย่างการไหลของเครือข่าย](#network-flow-example--ตัวอย่างการไหลของเครือข่าย)
14. [Practice Exercises / แบบฝึกหัด](#practice-exercises--แบบฝึกหัด)
15. [Resources / แหล่งข้อมูล](#resources--แหล่งข้อมูล)

---

## What is CNI? / CNI คืออะไร?

**English:**
Container Network Interface (CNI) is a specification and set of libraries for writing plugins that configure network interfaces in Linux containers. It defines how network resources should be allocated and deallocated for containers, providing a standardized way to connect containers to networks.

**Thai:**
Container Network Interface (CNI) คือข้อกำหนด (specification) และชุดไลบรารีสำหรับเขียนปลั๊กอินที่จัดการอินเทอร์เฟซเครือข่ายในคอนเทนเนอร์ Linux CNI กำหนดวิธีการจัดสรรและยกเลิกการจัดสรรทรัพยากรเครือข่ายสำหรับคอนเทนเนอร์ โดยเป็นวิธีการมาตรฐานในการเชื่อมต่อคอนเทนเนอร์เข้ากับเครือข่าย

**Key Points / ประเด็นสำคัญ:**

- **CNI is a Cloud Native Computing Foundation (CNCF) project** / CNI เป็นโปรเจกต์ของ Cloud Native Computing Foundation (CNCF)
- **It provides a plugin-based architecture** / มีสถาปัตยกรรมแบบใช้ปลั๊กอิน
- **Container runtimes (like containerd, CRI-O) call CNI plugins to set up networking** / รันไทม์ของคอนเทนเนอร์ (เช่น containerd, CRI-O) เรียกปลั๊กอิน CNI เพื่อตั้งค่าเครือข่าย
- **CNI is NOT Kubernetes-specific** — it works with any container runtime / CNI ไม่ได้จำเพาะกับคิวเบอร์เนติส — ใช้งานได้กับรันไทม์คอนเทนเนอร์ใดๆ

---

## Why CNI is Needed / ทำไมต้องใช้ CNI

**English:**
Without CNI, containers on different hosts cannot communicate with each other. Kubernetes requires a robust networking solution to enable seamless communication between pods, services, and external systems. CNI provides this abstraction layer.

**Thai:**
หากไม่มี CNI คอนเทนเนอร์บนโฮสต์ต่างกันจะไม่สามารถสื่อสารกันได้ คิวเบอร์เนติสต้องการโซลูชันเครือข่ายที่แข็งแกร่งเพื่อเปิดใช้งานการสื่อสารแบบไร้รอยต่อระหว่างพอด บริการ และระบบภายนอก CNI ให้ชั้นนามธรรม (abstraction layer) นี้

### Reasons CNI is Essential / เหตุผลที่ CNI จำเป็น:

| Reason / เหตุผล | English Description / คำอธิบาย (EN) | Thai Description / คำอธิบาย (TH) |
|---|---|---|
| **Pod Networking** | Assigns unique IP to each pod | กำหนด IP เฉพาะให้กับแต่ละพอด |
| **Cross-Node Communication** | Enables pods on different nodes to talk | เปิดใช้งานพอดบนโหนดต่างกันให้สื่อสารกันได้ |
| **No NAT** | Direct pod-to-pod communication without address translation | การสื่อสารพอดถึงพอดโดยตรงโดยไม่ต้องแปลที่อยู่ |
| **Service Discovery** | Works with kube-proxy for service routing | ทำงานร่วมกับ kube-proxy สำหรับการเราต์บริการ |
| **Network Policies** | Implements security rules for traffic control | นำกฎความปลอดภัยไปใช้เพื่อควบคุมทราฟฟิก |
| **Plugin Flexibility** | Swap CNI plugins without changing Kubernetes | สลับปลั๊กอิน CNI ได้โดยไม่ต้องเปลี่ยนคิวเบอร์เนติส |

---

## Kubernetes Networking Requirements / ข้อกำหนดเครือข่ายของคิวเบอร์เนติส

**English:**
Kubernetes defines a set of fundamental networking requirements that every CNI plugin must satisfy. These requirements ensure predictable and consistent networking behavior across all Kubernetes deployments.

**Thai:**
คิวเบอร์เนติสกำหนดข้อกำหนดเครือข่ายพื้นฐานที่ปลั๊กอิน CNI ทุกตัวต้องปฏิบัติตาม ข้อกำหนดเหล่านี้ช่วยให้พฤติการณ์เครือข่ายสามารถคาดเดาได้และสม่ำเสมอในทุกการติดตั้งคิวเบอร์เนติส

### Fundamental Requirements / ข้อกำหนดพื้นฐาน:

1. **Pod-to-Pod Communication (without NAT)** / การสื่อสารพอดถึงพอด (โดยไม่ใช้ NAT)
   - **EN:** All pods can communicate directly with all other pods without network address translation.
   - **TH:** พอดทุกตัวสามารถสื่อสารโดยตรงกับพอดอื่นทั้งหมดได้โดยไม่ต้องใช้การแปลที่อยู่เครือข่าย (NAT)

2. **Node-to-Pod Communication** / การสื่อสารโหนดถึงพอด
   - **EN:** Nodes (and their agents like kubelet) can reach all pods on that node and across nodes.
   - **TH:** โหนด (และเอเจนต์เช่น kubelet) สามารถเข้าถึงพอดทั้งหมดบนโหนดนั้นและข้ามโหนดได้

3. **Unique IP per Pod** / IP เฉพาะต่อพอด
   - **EN:** Every pod gets its own unique IP address, eliminating port conflicts.
   - **TH:** พอดทุกตัวได้รับ IP เฉพาะของตัวเอง กำจัดความขัดแย้งของพอร์ต

4. **Service Networking** / การใช้บริการเครือข่าย
   - **EN:** ClusterIP services provide stable virtual IPs for pod groups; external access via NodePort/LoadBalancer/Ingress.
   - **TH:** บริการ ClusterIP ให้ IP เสมือนที่เสถียรสำหรับกลุ่มพอด; การเข้าถึงภายนอกผ่าน NodePort/LoadBalancer/Ingress

5. **Network Policies** / นโยบายเครือข่าย
   - **EN:** Fine-grained traffic control at pod level (allow/deny based on labels, ports, namespaces).
   - **TH:** การควบคุมทราฟฟิกอย่างละเอียดในระดับพอด (อนุญาต/ปฏิเสธตามป้ายกำกับ พอร์ต เนมสเปซ)

---

## Popular CNI Plugins / ปลั๊กอิน CNI ยอดนิยม

**English:**
Several CNI plugins are available for Kubernetes, each with different features, performance characteristics, and use cases. The table below compares the most popular options.

**Thai:**
มีปลั๊กอิน CNI หลายตัวสำหรับคิวเบอร์เนติส แต่ละตัวมีคุณสมบัติ ประสิทธิภาพ และกรณีการใช้งานต่างกัน ตารางด้านล่างเปรียบเทียบตัวเลือกยอดนิยม

| Plugin / ปลั๊กอิน | Type / ประเภท | OSI Layer / ชั้น OSI | Features / คุณสมบัติ | Use Case / กรณีการใช้งาน |
|---|---|---|---|---|
| **Calico** | L3 Routing / การเราต์ L3 | L3/L4/L7 | BGP routing, network policies, eBPF support / การเราต์ BGP, นโยบายเครือข่าย, รองรับ eBPF | Production environments, security-focused / สิ่งแวดล้อมโพรดักชัน มุ่งเน้นความปลอดภัย |
| **Flannel** | Overlay Network / เครือข่ายโอเวอร์เลย์ | L2/L3 | Simple setup, VXLAN, host-gateway / ตั้งค่าง่าย, VXLAN, host-gateway | Development, learning, small clusters / การพัฒนา, การเรียนรู้, คลัสเตอร์เล็ก |
| **Weave Net** | Mesh Network / เครือข่ายเมช | L2/L3 | Automatic encryption, mesh topology, DNS / การเข้ารหัสอัตโนมัติ, โทโพโลยีเมช, DNS | Multi-host setups, secure communication / ตั้งค่าหลายโฮสต์ การสื่อสารปลอดภัย |
| **Cilium** | eBPF-Based / ใช้ eBPF | L3/L4/L7 | Advanced observability, identity-aware, L7 filtering / การสังเกตขั้นสูง, รู้จักตัวตน, กรอง L7 | Cloud-native, advanced security, microservices / คลาวด์เนทีฟ ความปลอดภัยขั้นสูง ไมโครเซอร์วิส |
| **Canal** | Hybrid / ผสม | L2/L3/L4 | Combines Flannel networking + Calico policies / รวมเครือข่าย Flannel + นโยบาย Calico | Simple setup + security policies / ตั้งค่าง่าย + นโยบายความปลอดภัย |

### Choosing a CNI Plugin / การเลือกปลั๊กอิน CNI

**English:**
- **For production with security needs:** Choose Calico or Cilium
- **For development/learning:** Choose Flannel (simplest)
- **For advanced L7 visibility:** Choose Cilium
- **For simplicity + policies:** Choose Canal

**Thai:**
- **สำหรับโพรดักชันที่มีความต้องการด้านความปลอดภัย:** เลือก Calico หรือ Cilium
- **สำหรับการพัฒนา/การเรียนรู้:** เลือก Flannel (ง่ายที่สุด)
- **สำหรับการมองเห็น L7 ขั้นสูง:** เลือก Cilium
- **สำหรับความง่าย + นโยบาย:** เลือก Canal

---

## How CNI Relates to OSI Model / CNI สัมพันธ์กับโมเดล OSI อย่างไร

**English:**
The OSI (Open Systems Interconnection) model is a 7-layer framework for understanding network communication. CNI plugins operate primarily at Layers 2, 3, and increasingly at Layers 4 and 7. Understanding these layers helps in selecting, configuring, and troubleshooting CNI plugins.

**Thai:**
โมเดล OSI (Open Systems Interconnection) เป็นกรอบ 7 ชั้นสำหรับทำความเข้าใจการสื่อสารเครือข่าย ปลั๊กอิน CNI ทำงานหลักที่ชั้น 2, 3 และเพิ่มขึ้นที่ชั้น 4 และ 7 การทำความเข้าใจชั้นเหล่านี้ช่วยในการเลือก ตั้งค่า และแก้ไขปัญหาปลั๊กอิน CNI

### Layer 2 — Data Link Layer / ชั้นที่ 2 — ชั้นเชื่อมโยงข้อมูล

**English:**
- Handles MAC (Media Access Control) addressing
- Manages Ethernet frames
- Provides bridge/switch functionality
- **CNI Example:** Flannel in VXLAN mode creates an L2 overlay network, making pods on different nodes appear to be on the same broadcast domain.

**Thai:**
- จัดการที่อยู่ MAC (Media Access Control)
- จัดการเฟรม Ethernet
- ให้ฟังก์ชันบริดจ์/สวิตช์
- **ตัวอย่าง CNI:** Flannel ในโหมด VXLAN สร้างเครือข่ายโอเวอร์เลย์ L2 ทำให้พอดบนโหนดต่างกันดูเหมือนอยู่ในโดเมน broadcast เดียวกัน

### Layer 3 — Network Layer / ชั้นที่ 3 — ชั้นเครือข่าย

**English:**
- Handles IP addressing and routing
- Manages subnet allocation
- Uses routing protocols like BGP (Border Gateway Protocol)
- **CNI Example:** Calico operates at L3 using native IP routing and BGP peering between nodes to advertise pod routes.

**Thai:**
- จัดการที่อยู่ IP และการเราต์
- จัดการการจัดสรรซับเน็ต
- ใช้โปรโตคอลการเราต์เช่น BGP (Border Gateway Protocol)
- **ตัวอย่าง CNI:** Calico ทำงานที่ L3 โดยใช้การเราต์ IP ดั้งเดิมและ BGP peering ระหว่างโหนดเพื่อประกาศเส้นทางพอด

### Layer 4 — Transport Layer / ชั้นที่ 4 — ชั้นขนส่ง

**English:**
- Handles TCP/UDP port management
- Enables connection tracking and stateful filtering
- Supports network policies based on ports and protocols
- **CNI Example:** Cilium provides L4 connection tracking and can enforce policies based on TCP/UDP ports and connection states.

**Thai:**
- จัดการพอร์ต TCP/UDP
- เปิดใช้งานการติดตามการเชื่อมต่อและการกรองแบบมีสถานะ
- รองรับนโยบายเครือข่ายตามพอร์ตและโปรโตคอล
- **ตัวอย่าง CNI:** Cilium ให้การติดตามการเชื่อมต่อ L4 และสามารถบังคับใช้นโยบายตามพอร์ต TCP/UDP และสถานะการเชื่อมต่อ

### Layer 7 — Application Layer / ชั้นที่ 7 — ชั้นแอปพลิเคชัน

**English:**
- Handles application-level protocols (HTTP, gRPC, Kafka, etc.)
- Enables deep packet inspection and content-aware filtering
- Supports policies based on HTTP paths, methods, headers
- **CNI Example:** Cilium can inspect L7 traffic and enforce policies like "allow only GET requests to /api/v1" — something traditional CNIs cannot do.

**Thai:**
- จัดการโปรโตคอลระดับแอปพลิเคชัน (HTTP, gRPC, Kafka ฯลฯ)
- เปิดใช้งานการตรวจสอบแพ็กเก็ตเชิงลึกและการกรองตามเนื้อหา
- รองรับนโยบายตามเส้นทาง HTTP เมธอด เฮดเดอร์
- **ตัวอย่าง CNI:** Cilium สามารถตรวจสอบทราฟฟิก L7 และบังคับใช้นโยบายเช่น "อนุญาตเฉพาะคำขอ GET ไปยัง /api/v1" — สิ่งที่ CNI แบบดั้งเดิมทำไม่ได้

### OSI Layer Summary Table / ตารางสรุปชั้น OSI

| OSI Layer / ชั้น OSI | Name / ชื่อ | CNI Relevance / ความเกี่ยวข้องกับ CNI | Example Plugin / ปลั๊กอินตัวอย่าง |
|---|---|---|---|
| Layer 1 / ชั้น 1 | Physical / ทางกายภาพ | Not handled by CNI / CNI ไม่จัดการ | N/A |
| Layer 2 / ชั้น 2 | Data Link / เชื่อมโยงข้อมูล | MAC, Ethernet, VXLAN overlay | Flannel (VXLAN) |
| Layer 3 / ชั้น 3 | Network / เครือข่าย | IP routing, BGP, subnet | Calico, Flannel |
| Layer 4 / ชั้น 4 | Transport / ขนส่ง | TCP/UDP, connection tracking, port policies | Cilium, Calico |
| Layer 5 / ชั้น 5 | Session / เซสชัน | Not directly managed / ไม่จัดการโดยตรง | N/A |
| Layer 6 / ชั้น 6 | Presentation / นำเสนอ | Not directly managed / ไม่จัดการโดยตรง | N/A |
| Layer 7 / ชั้น 7 | Application / แอปพลิเคชัน | HTTP, gRPC filtering, deep inspection | Cilium |

---

## CNI in Action: Calico / CNI ในการปฏิบัติ: Calico

**English:**
Calico is one of the most widely used CNI plugins in production Kubernetes clusters. It provides high-performance networking with native L3 routing and comprehensive network policy enforcement.

**Thai:**
Calico เป็นหนึ่งในปลั๊กอิน CNI ที่ใช้กันอย่างแพร่หลายในคลัสเตอร์คิวเบอร์เนติสโพรดักชัน ให้เครือข่ายประสิทธิภาพสูงด้วยการเราต์ L3 ดั้งเดิมและการบังคับใช้นโยบายเครือข่ายอย่างครอบคลุม

### Installation / การติดตั้ง

```bash
# Install Calico / ติดตั้ง Calico
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml

# Verify Calico pods are running / ตรวจสอบพอด Calico ว่าทำงานอยู่
kubectl get pods -n kube-system | grep calico
# Expected output: calico-node-xxxxx, calico-kube-controllers-xxxxx
# ผลลัพธ์ที่คาด: calico-node-xxxxx, calico-kube-controllers-xxxxx

# Check node network status / ตรวจสอบสถานะเครือข่ายโหนด
kubectl get nodes -o wide

# View IP pools (Calico-specific) / ดูพูล IP (เฉพาะ Calico)
kubectl get ippool -A
# This shows the IP ranges allocated for pods
# แสดงช่วง IP ที่จัดสรรสำหรับพอด
```

### How Calico Works / Calico ทำงานอย่างไร

**English:**
1. **IP Allocation:** Calico assigns IP addresses to pods from configured IP pools (IPPools).
2. **Route Programming:** It programs routing tables on each node so that traffic destined for specific pod IPs is routed correctly.
3. **BGP Advertisement:** Calico uses BGP (Border Gateway Protocol) to advertise pod routes between nodes, enabling cross-node communication without overlay networking.
4. **Policy Enforcement:** Network policies are enforced using iptables (legacy) or eBPF (modern mode) for high-performance packet filtering.
5. **No Overlay Overhead:** Unlike Flannel's VXLAN, Calico's native L3 routing has lower latency and higher throughput.

**Thai:**
1. **การจัดสรร IP:** Calico กำหนดที่อยู่ IP ให้พอดจากพูล IP ที่กำหนดค่าไว้ (IPPools)
2. **การโปรแกรมเส้นทาง:** ตั้งค่าตารางการเราต์บนแต่ละโหนดเพื่อให้ทราฟฟิกที่ไปยัง IP พอดเฉพาะถูกเราต์อย่างถูกต้อง
3. **การประกาศ BGP:** Calico ใช้ BGP (Border Gateway Protocol) เพื่อประกาศเส้นทางพอดระหว่างโหนด เปิดใช้งานการสื่อสารข้ามโหนดโดยไม่ต้องการเครือข่ายโอเวอร์เลย์
4. **การบังคับใช้นโยบาย:** นโยบายเครือข่ายถูกบังคับใช้ผ่าน iptables (แบบเก่า) หรือ eBPF (โหมดทันสมัย) สำหรับการกรองแพ็กเก็ตประสิทธิภาพสูง
5. **ไม่มีโอเวอร์เฮดโอเวอร์เลย์:** ต่างจาก VXLAN ของ Flannel การเราต์ L3 ดั้งเดิมของ Calico มีความหน่วงแฝงต่ำกว่าและปริมาณงานสูงกว่า

### Calico Architecture Diagram / แผนผังสถาปัตยกรรม Calico

```
+-----------+                    +-----------+
|  Node 1   |                    |  Node 2   |
|           |    BGP Peering     |           |
| +-------+ |  <=============>   | +-------+ |
| | Pod A |--|--- routes -------|->| Pod B | |
| |10.0.1.5| |                  |  |10.0.2.8| |
| +-------+ |                    | +-------+ |
|           |                    |           |
| calico-node|                   | calico-node|
+-----------+                    +-----------+
       |                              |
       +---------- etcd/API ----------+
                     |
              Calico Controller
              (kube-system namespace)
              /เนมสเปซ kube-system
```

---

## Network Policies / นโยบายเครือข่าย

**English:**
Network Policies are Kubernetes resources that control the flow of traffic between pods and external endpoints. They act like virtual firewalls, allowing you to define which pods can communicate with which other pods, namespaces, or external IPs. **Important:** Network Policies require CNI support — not all CNIs enforce them (Flannel does NOT support Network Policies).

**Thai:**
นโยบายเครือข่ายเป็นทรัพยากรคิวเบอร์เนติสที่ควบคุมการไหลของทราฟฟิกระหว่างพอดและปลายทางภายนอก ทำหน้าที่เหมือนไฟร์วอลล์เสมือน ช่วยให้คุณกำหนดได้ว่าพอดใดสามารถสื่อสารกับพอด เนมสเปซ หรือ IP ภายนอกใด **สำคัญ:** นโยบายเครือข่ายต้องการการรองรับจาก CNI — ไม่ใช่ทุก CNI ที่บังคับใช้ (Flannel ไม่รองรับนโยบายเครือข่าย)

### Default Deny All Ingress / ปฏิเสธการเข้าทั้งหมดเป็นค่าเริ่มต้น

**English:**
This policy blocks all incoming traffic to pods in the specified namespace. It's a security best practice to start with deny-all and then explicitly allow required traffic.

**Thai:**
นโยบายนี้บล็อกทราฟฟิกขาเข้าทั้งหมดไปยังพอดในเนมสเปซที่ระบุ เป็นวิธีปฏิบัติด้านความปลอดภัยที่ดีที่สุดที่จะเริ่มด้วยปฏิเสธทั้งหมดแล้วอนุญาตเฉพาะทราฟฟิกที่ต้องการ

```yaml
# deny-all-ingress.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy          # ประเภท: นโยบายเครือข่าย
metadata:
  name: deny-all-ingress     # ชื่อ: ปฏิเสธการเข้าทั้งหมด
  namespace: default         # เนมสเปซ: ค่าเริ่มต้น
spec:
  podSelector: {}            # เลือกพอดทั้งหมด (セレกเตอร์ว่าง = ทั้งหมด)
  policyTypes:
  - Ingress                  # ประเภทนโยบาย: การเข้า (Ingress)
  # No ingress rules = deny all incoming traffic
  # ไม่มีกฎการเข้า = ปฏิเสธทราฟฟิกขาเข้าทั้งหมด
```

```bash
# Apply the deny-all policy / ใช้นโยบายปฏิเสธทั้งหมด
kubectl apply -f deny-all-ingress.yaml

# Verify the policy / ตรวจสอบนโยบาย
kubectl get networkpolicy -n default
kubectl describe networkpolicy deny-all-ingress -n default
```

### Allow Specific Traffic / อนุญาตทราฟฟิกเฉพาะ

**English:**
After applying deny-all, you need explicit policies to allow specific traffic. This example allows traffic only from pods labeled `app: frontend` to pods labeled `app: nginx` on port 80.

**Thai:**
หลังใช้นโยบายปฏิเสธทั้งหมด คุณต้องมีนโยบายชัดเจนเพื่ออนุญาตทราฟฟิกเฉพาะ ตัวอย่างนี้อนุญาตทราฟฟิกจากพอดที่มีป้าย `app: frontend` ไปยังพอดที่มีป้าย `app: nginx` บนพอร์ต 80 เท่านั้น

```yaml
# allow-frontend-to-nginx.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy                # ประเภท: นโยบายเครือข่าย
metadata:
  name: allow-frontend-to-nginx    # ชื่อ: อนุญาตฟรอนต์เอนด์ไป nginx
  namespace: default               # เนมสเปซ: ค่าเริ่มต้น
spec:
  podSelector:                     # เลือกพอดเป้าหมาย
    matchLabels:
      app: nginx                   # พอดที่มีป้าย app: nginx
  policyTypes:
  - Ingress                        # ประเภท: การเข้า
  ingress:                         # กฎการเข้า
  - from:                          # จากแหล่งที่มา
    - podSelector:                 # เลือกพอดต้นทาง
        matchLabels:
          app: frontend            # พอดที่มีป้าย app: frontend เท่านั้น
    ports:                         # พอร์ตที่อนุญาต
    - protocol: TCP                # โปรโตคอล: TCP
      port: 80                     # พอร์ต: 80 (HTTP)
```

```bash
# Apply the policy / ใช้นโยบาย
kubectl apply -f allow-frontend-to-nginx.yaml

# Test: traffic from frontend pod should SUCCEED
# ทดสอบ: ทราฟฟิกจากพอดฟรอนต์เอนด์ควรสำเร็จ
kubectl exec frontend-pod -- curl -s http://<nginx-pod-ip>

# Test: traffic from other pods should FAIL
# ทดสอบ: ทราฟฟิกจากพอดอื่นควรล้มเหลว
kubectl exec other-pod -- curl -s --connect-timeout 3 http://<nginx-pod-ip>
```

### Allow from Specific Namespace / อนุญาตจากเนมสเปซเฉพาะ

**English:**
This policy allows traffic only from pods in the `monitoring` namespace to access the `api` pods in the current namespace on port 8080. This is useful for allowing monitoring tools (like Prometheus) to scrape metrics from your application.

**Thai:**
นโยบายนี้อนุญาตทราฟฟิกจากพอดในเนมสเปซ `monitoring` เท่านั้นเพื่อเข้าถึงพอด `api` ในเนมสเปซปัจจุบันบนพอร์ต 8080 มีประโยชน์สำหรับอนุญาตเครื่องมือตรวจสอบ (เช่น Prometheus) ดึงเมตริกจากแอปพลิเคชัน

```yaml
# allow-monitoring-namespace.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy                    # ประเภท: นโยบายเครือข่าย
metadata:
  name: allow-monitoring-access        # ชื่อ: อนุญาตการเข้าถึงการตรวจสอบ
  namespace: production                # เนมสเปซ: โพรดักชัน
spec:
  podSelector:                         # เลือกพอดเป้าหมาย
    matchLabels:
      app: api                         # พอด API
  policyTypes:
  - Ingress                            # ประเภท: การเข้า
  ingress:                             # กฎการเข้า
  - from:                              # จากแหล่งที่มา
    - namespaceSelector:               # เลือกเนมสเปซต้นทาง
        matchLabels:
          name: monitoring             # เนมสเปซที่มีป้าย name: monitoring
    ports:                             # พอร์ตที่อนุญาต
    - protocol: TCP                    # โปรโตคอล: TCP
      port: 8080                       # พอร์ต: 8080
```

### Egress Policy Example / ตัวอย่างนโยบายขาออก

**English:**
Control outbound traffic from pods. This example allows pods to only reach external DNS servers and specific APIs.

**Thai:**
ควบคุมทราฟฟิกขาออกจากพอด ตัวอย่างนี้อนุญาตพอดเข้าถึงเฉพาะเซิร์ฟเวอร์ DNS ภายนอก และ API เฉพาะ

```yaml
# egress-policy.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy                # ประเภท: นโยบายเครือข่าย
metadata:
  name: restrict-egress            # ชื่อ: จำกัดทราฟฟิกขาออก
  namespace: production            # เนมสเปซ: โพรดักชัน
spec:
  podSelector:                     # เลือกพอด
    matchLabels:
      app: backend                 # พอดแบ็กเอนด์
  policyTypes:
  - Egress                         # ประเภท: การออก (ขาออก)
  egress:                          # กฎการออก
  - to:                            # ไปยังปลายทาง
    - ipBlock:                     # บล็อก IP
        cidr: 8.8.8.8/32           # Google DNS
    ports:
    - protocol: UDP                # โปรโตคอล: UDP
      port: 53                     # พอร์ต: DNS
  - to:                            # ไปยังปลายทาง
    - ipBlock:                     # บล็อก IP
        cidr: 10.0.0.0/8           # Internal network / เครือข่ายภายใน
    ports:
    - protocol: TCP                # โปรโตคอล: TCP
      port: 443                    # พอร์ต: HTTPS
```

---

## CNI Comparison / เปรียบเทียบ CNI

### Flannel

**English:**
Flannel is the simplest CNI plugin, designed for basic Kubernetes networking. It creates an overlay network using VXLAN or host-gateway mode.

**Thai:**
Flannel เป็นปลั๊กอิน CNI ที่ง่ายที่สุด ออกแบบสำหรับเครือข่ายคิวเบอร์เนติสพื้นฐาน สร้างเครือข่ายโอเวอร์เลย์โดยใช้โหมด VXLAN หรือ host-gateway

```bash
# Install Flannel / ติดตั้ง Flannel
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

# Pros / จุดแข็ง:
# - Simple and easy to understand / เรียบง่าย เข้าใจง่าย
# - Quick to deploy / ติดตั้งเร็ว
# - Low resource usage / ใช้ทรัพยากรน้อย

# Cons / จุดอ่อน:
# - No Network Policy support / ไม่รองรับนโยบายเครือข่าย
# - Limited advanced features / คุณสมบัติขั้นสูงจำกัด
# - VXLAN overhead reduces performance / โอเวอร์เฮด VXLAN ลดประสิทธิภาพ

# OSI Layer / ชั้น OSI: L2/L3 (VXLAN overlay)
```

### Calico

**English:**
Calico is a production-grade CNI that provides high-performance L3 routing with full network policy support. It uses BGP for route distribution instead of overlay networking.

**Thai:**
Calico เป็น CNI ระดับโพรดักชันให้การเราต์ L3 ประสิทธิภาพสูงพร้อมรองรับนโยบายเครือข่ายเต็มรูปแบบ ใช้ BGP สำหรับการแจกจ่ายเส้นทางแทนเครือข่ายโอเวอร์เลย์

```bash
# Install Calico / ติดตั้ง Calico
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

# Pros / จุดแข็ง:
# - Full-featured with network policies / คุณสมบัติครบครันพร้อมนโยบายเครือข่าย
# - BGP routing for high performance / การเราต์ BGP เพื่อประสิทธิภาพสูง
# - No overlay overhead (native L3) / ไม่มีโอเวอร์เฮดโอเวอร์เลย์ (L3 ดั้งเดิม)
# - Well-documented and widely adopted / เอกสารดีและใช้งานแพร่หลาย
# - eBPF mode for even better performance / โหมด eBPF เพื่อประสิทธิภาพดียิ่งขึ้น

# Cons / จุดอ่อน:
# - More complex configuration / การตั้งค่าซับซ้อนกว่า
# - Requires BGP understanding for advanced setups / ต้องการความเข้าใจ BGP สำหรับตั้งค่าขั้นสูง
# - Higher resource usage than Flannel / ใช้ทรัพยากรมากกว่า Flannel

# OSI Layer / ชั้น OSI: L3 (native IP routing), L4/L7 with policies
```

### Cilium

**English:**
Cilium is a next-generation CNI built on eBPF (extended Berkeley Packet Filter). It provides advanced networking, security, and observability at the kernel level without changing application code.

**Thai:**
Cilium เป็น CNI รุ่นใหม่สร้างบน eBPF (extended Berkeley Packet Filter) ให้เครือข่าย ความปลอดภัย และการสังเกตขั้นสูงที่ระดับเคอร์เนลโดยไม่ต้องเปลี่ยนโค้ดแอปพลิเคชัน

```bash
# Install Cilium via Helm / ติดตั้ง Cilium ผ่าน Helm
helm repo add cilium https://helm.cilium.io/
helm install cilium cilium/cilium --namespace kube-system

# Pros / จุดแข็ง:
# - eBPF-based for kernel-level performance / ใช้ eBPF เพื่อประสิทธิภาพระดับเคอร์เนล
# - L7 visibility and filtering / การมองเห็นและกรอง L7
# - Identity-aware security policies / นโยบายความปลอดภัยตามตัวตน
# - Advanced observability (metrics, tracing) / การสังเกตขั้นสูง (เมตริก การติดตาม)
# - Service mesh capabilities (without sidecars) / ความสามารถเซอร์วิสเมช (ไม่มี sidecar)

# Cons / จุดอ่อน:
# - Newer technology, steeper learning curve / เทคโนโลยีใหม่ เส้นการเรียนรู้ชันกว่า
# - Requires newer kernel versions (4.9+) / ต้องการเคอร์เนลเวอร์ชันใหม่กว่า (4.9+)
# - More complex to troubleshoot / แก้ไขปัญหาซับซ้อนกว่า

# OSI Layer / ชั้น OSI: L3/L4/L7 (full stack)
```

### Comparison Matrix / ตารางเปรียบเทียบ

| Feature / คุณสมบัติ | Flannel | Calico | Cilium |
|---|---|---|---|
| **Primary Use / การใช้หลัก** | Simple overlay / โอเวอร์เลย์ง่าย | L3 routing / การเราต์ L3 | eBPF platform / แพลตฟอร์ม eBPF |
| **Network Policies / นโยบายเครือข่าย** | NO / ไม่รองรับ | YES / รองรับ | YES / รองรับ |
| **OSI Layers / ชั้น OSI** | L2/L3 | L3/L4/L7 | L3/L4/L7 |
| **Performance / ประสิทธิภาพ** | Moderate (VXLAN overhead) / ปานกลาง | High (native L3) / สูง | Very High (eBPF) / สูงมาก |
| **Encryption / การเข้ารหัส** | IPsec (optional) | IPsec / WireGuard | IPsec / WireGuard |
| **L7 Filtering / การกรอง L7** | NO / ไม่รองรับ | NO / ไม่รองรับ | YES / รองรับ |
| **Observability / การสังเกต** | Basic / พื้นฐาน | Moderate / ปานกลาง | Advanced / ขั้นสูง |
| **Ease of Setup / ความง่ายตั้งค่า** | Easiest / ง่ายสุด | Moderate / ปานกลาง | Complex / ซับซ้อน |
| **Best For / เหมาะสำหรับ** | Dev/Learning / พัฒนา/เรียน | Production / โพรดักชัน | Cloud-native advanced / คลาวด์เนทีฟขั้นสูง |

---

## Practical Exercise: Test Pod Communication / แบบฝึกหัด: ทดสอบการสื่อสารของ Pod

**English:**
This exercise demonstrates pod-to-pod communication within a Kubernetes cluster. You'll create test pods, verify their IP assignments, and test connectivity.

**Thai:**
แบบฝึกหัดนี้สาธิตการสื่อสารพอดถึงพอดภายในคลัสเตอร์คิวเบอร์เนติส คุณจะสร้างพอดทดสอบ ตรวจสอบการกำหนด IP และทดสอบการเชื่อมต่อ

### Step 1: Create Test Pods / ขั้นที่ 1: สร้างพอดทดสอบ

```bash
# Create two test pods using busybox (lightweight Linux image)
# สร้างพอดทดสอบสองตัวใช้ busybox (อิมเมจ Linux ขนาดเล็ก)

kubectl run test1 --image=busybox --restart=Never -- sleep 3600
# test1 pod created / สร้างพอด test1 แล้ว

kubectl run test2 --image=busybox --restart=Never -- sleep 3600
# test2 pod created / สร้างพอด test2 แล้ว

# Wait a moment, then verify both pods are running
# รอสักครู่ แล้วตรวจสอบว่าพอดทั้งสองทำงานอยู่
kubectl get pods -o wide
# Output shows: NAME, READY, STATUS, IP, NODE
# ผลลัพธ์แสดง: ชื่อ, ความพร้อม, สถานะ, IP, โหนด

# Note the IP addresses of both pods
# บันทึกที่อยู่ IP ของพอดทั้งสอง
TEST1_IP=$(kubectl get pod test1 -o jsonpath='{.status.podIP}')
TEST2_IP=$(kubectl get pod test2 -o jsonpath='{.status.podIP}')

echo "test1 IP: $TEST1_IP"   # แสดง IP ของ test1
echo "test2 IP: $TEST2_IP"   # แสดง IP ของ test2
```

### Step 2: Test Connectivity / ขั้นที่ 2: ทดสอบการเชื่อมต่อ

```bash
# From test1, ping test2 (ICMP test)
# จาก test1 ปิง test2 (ทดสอบ ICMP)
kubectl exec test1 -- ping -c 4 $TEST2_IP

# Expected output / ผลลัพธ์ที่คาดหวัง:
# 4 packets transmitted, 4 received, 0% packet loss
# ส่ง 4 แพ็กเก็ต ได้รับ 4 สูญเสีย 0%

# From test1, make HTTP request to test2 (if running web server)
# จาก test1 สร้างคำขอ HTTP ไป test2 (ถ้ารันเว็บเซิร์ฟเวอร์)
kubectl exec test1 -- wget -qO- --timeout=5 http://$TEST2_IP:80 2>&1 || echo "No web server on test2 / ไม่มีเว็บเซิร์ฟเวอร์บน test2"

# Test DNS resolution from within a pod
# ทดสอบการแก้ DNS จากภายในพอด
kubectl run dns-test --image=busybox:1.28 --restart=Never -- sleep 3600
kubectl exec dns-test -- nslookup kubernetes.default
# Should resolve to the ClusterIP of the kubernetes service
# ควรแก้เป็นที่อยู่ ClusterIP ของบริการ kubernetes
```

### Step 3: Cross-Node Test / ขั้นที่ 3: ทดสอบข้ามโหนด

```bash
# Check if pods are on different nodes
# ตรวจสอบว่าพอดอยู่บนโหนดต่างกันหรือไม่
kubectl get pods -o wide

# If test1 and test2 are on different nodes, cross-node communication is working
# ถ้า test1 และ test2 อยู่บนโหนดต่างกัน การสื่อสารข้ามโหนดทำงานอยู่

# Verify routing table on nodes (requires node access)
# ตรวจสอบตารางการเราต์บนโหนด (ต้องการการเข้าถึงโหนด)
# On node 1:
ip route | grep $TEST2_IP
# Should show a route to test2's IP via Calico/Flannel
# ควรแสดงเส้นทางไปยัง IP ของ test2 ผ่าน Calico/Flannel
```

---

## Troubleshooting CNI / การแก้ไขปัญหา CNI

**English:**
When networking issues arise in Kubernetes, systematic troubleshooting is essential. Below are common troubleshooting steps organized by category.

**Thai:**
เมื่อมีปัญหาเครือข่ายในคิวเบอร์เนติส การแก้ไขอย่างเป็นระบบเป็นสิ่งจำเป็น ด้านล่างเป็นขั้นตอนการแก้ไขปัญหาทั่วไปจัดตามหมวดหมู่

### 1. Check CNI Plugin Pods / ตรวจสอบพอดปลั๊กอิน CNI

```bash
# Check CNI plugin pods status / ตรวจสอบสถานะพอดปลั๊กอิน CNI
kubectl get pods -n kube-system | grep -E 'calico|flannel|cilium'

# Look for Running status — any CrashLoopBackOff or Error indicates issues
# มองหาสถานะ Running — CrashLoopBackOff หรือ Error บ่งชี้ปัญหา

# Detailed view of CNI pods / ดูรายละเอียดพอด CNI
kubectl describe pod -n kube-system -l k8s-app=calico-node
# Check events for errors / ตรวจสอบเหตุการณ์สำหรับข้อผิดพลาด

# View logs for each CNI / ดูบันทึกสำหรับแต่ละ CNI

# Calico logs / บันทึก Calico
kubectl logs -n kube-system -l k8s-app=calico-node --tail=100

# Flannel logs / บันทึก Flannel
kubectl logs -n kube-system -l app=flannel --tail=100

# Cilium logs / บันทึก Cilium
kubectl logs -n kube-system -l k8s-app=cilium --tail=100
```

### 2. Check Node Network Status / ตรวจสอบสถานะเครือข่ายโหนด

```bash
# Describe node to check network conditions
# อธิบายโหนดเพื่อตรวจสอบสภาวะเครือข่าย
kubectl describe node <node-name>

# Look for these conditions / มองหาสภาวะเหล่านี้:
# - NetworkReady: True (should be True)
#   เครือข่ายพร้อม: True (ควรเป็น True)
# - RouteCreated: True (for CNIs that manage routes)
#   สร้างเส้นทางแล้ว: True (สำหรับ CNI ที่จัดการเส้นทาง)

# Check node annotations for CNI-specific info
# ตรวจสอบคำอธิบายโหนดสำหรับข้อมูลเฉพาะ CNI
kubectl get node <node-name> -o json | grep -A 5 "annotations"
```

### 3. Test DNS Resolution / ทดสอบการแก้ DNS

```bash
# Create a test pod for DNS testing
# สร้างพอดทดสอบสำหรับการทดสอบ DNS
kubectl run dns-test --image=busybox:1.28 --restart=Never -- sleep 3600

# Test DNS resolution / ทดสอบการแก้ DNS
kubectl exec dns-test -- nslookup kubernetes.default
kubectl exec dns-test -- nslookup google.com

# Expected output / ผลลัพธ์ที่คาดหวัง:
# Server:    10.96.0.10        (CoreDNS ClusterIP / CoreDNS ClusterIP)
# Address 1: 10.96.0.10        (CoreDNS ClusterIP / CoreDNS ClusterIP)
# Name:      kubernetes.default
# Address 1: 10.96.0.1         (kubernetes service IP / IP บริการ kubernetes)

# If DNS fails, check CoreDNS pods / ถ้า DNS ล้มเหลว ตรวจสอบพอด CoreDNS
kubectl get pods -n kube-system -l k8s-app=kube-dns
kubectl logs -n kube-system -l k8s-app=kube-dns
```

### 4. Check Network Policies / ตรวจสอบนโยบายเครือข่าย

```bash
# List all network policies in all namespaces
# แสดงนโยบายเครือข่ายทั้งหมดในทุกเนมสเปซ
kubectl get networkpolicies --all-namespaces

# Describe a specific policy / อธิบายนโยบายเฉพาะ
kubectl describe networkpolicy <policy-name> -n <namespace>

# Check if your CNI supports and enforces policies
# ตรวจสอบว่า CNI ของคุณรองรับและบังคับใช้นโยบายหรือไม่
# Flannel does NOT enforce network policies!
# Flannel ไม่บังคับใช้นโยบายเครือข่าย!

# Test policy enforcement / ทดสอบการบังคับใช้นโยบาย
kubectl exec <source-pod> -- curl -s --connect-timeout 3 http://<target-pod-ip>
# If policy denies: connection should timeout or be refused
# ถ้านโยบายปฏิเสธ: การเชื่อมต่อควรหมดเวลาหรือถูกปฏิเสธ
```

### 5. Check Pod Networking / ตรวจสอบเครือข่ายพอด

```bash
# Enter the pod's network namespace
# เข้าสู่เนมสเปซเครือข่ายของพอด
kubectl exec -it <pod-name> -- sh

# Inside the pod / ภายในพอด:

# Check IP address assigned to the pod
# ตรวจสอบ IP ที่กำหนดให้พอด
ip addr
# Look for eth0 interface with pod's IP
# มองหาอินเทอร์เฟซ eth0 พร้อม IP ของพอด

# Check routing table
# ตรวจสอบตารางการเราต์
ip route
# Should have default route via node's gateway
# ควรมีเส้นทางเริ่มต้นผ่านเกตเวย์ของโหนด

# Test external connectivity
# ทดสอบการเชื่อมต่อภายนอก
ping 8.8.8.8 -c 4
# If this fails, check node networking
# ถ้าล้มเหลว ตรวจสอบเครือข่ายโหนด

# Check DNS configuration
# ตรวจสอบการตั้งค่า DNS
cat /etc/resolv.conf
# Should point to cluster DNS (CoreDNS)
# ควรชี้ไปยัง DNS ของคลัสเตอร์ (CoreDNS)

# Exit pod / ออกจากพอด
exit
```

### 6. Check CNI Configuration Files / ตรวจสอบไฟล์ตั้งค่า CNI

```bash
# On each node, check CNI configuration
# บนแต่ละโหนด ตรวจสอบการตั้งค่า CNI

# List CNI config files / แสดงไฟล์ตั้งค่า CNI
ls -la /etc/cni/net.d/
# Should contain 10-calico.conflist or similar
# ควรมี 10-calico.conflist หรือคล้ายกัน

# List CNI plugin binaries / แสดงไบนารีปลั๊กอิน CNI
ls -la /opt/cni/bin/
# Should contain bridge, portmap, bandwidth, etc.
# ควรมี bridge, portmap, bandwidth ฯลฯ

# View CNI config content / ดูเนื้อหาการตั้งค่า CNI
cat /etc/cni/net.d/10-calico.conflist
```

### 7. Common CNI Issues and Solutions / ปัญหา CNI ทั่วไปและวิธีแก้ไข

| Issue / ปัญหา | Possible Cause / สาเหตุที่เป็นไปได้ | Solution / วิธีแก้ไข |
|---|---|---|
| Pods cannot communicate / พอดสื่อสารไม่ได้ | CNI not installed / ไม่ได้ติดตั้ง CNI | Install CNI plugin / ติดตั้งปลั๊กอิน CNI |
| DNS not resolving / DNS ไม่แก้ชื่อ | CoreDNS pods not running / พอด CoreDNS ไม่ทำงาน | Check CoreDNS status / ตรวจสอบสถานะ CoreDNS |
| Cross-node communication fails / การสื่อสารข้ามโหนดล้มเหลว | BGP/VXLAN misconfiguration / BGP/VXLAN ตั้งค่าผิด | Check CNI logs, verify routing / ตรวจสอบบันทึก CNI ยืนยันการเราต์ |
| Network policies not working / นโยบายเครือข่ายไม่ทำงาน | CNI doesn support policies (e.g., Flannel) / CNI ไม่รองรับนโยบาย (เช่น Flannel) | Switch to Calico or Cilium / เปลี่ยนเป็น Calico หรือ Cilium |
| Pod stuck in ContainerCreating / พอดค้างใน ContainerCreating | CNI plugin error / ข้อผิดพลาดปลั๊กอิน CNI | Check CNI logs, reinstall / ตรวจสอบบันทึก CNI ติดตั้งใหม่ |
| High latency / ความหน่วงแฝงสูง | Overlay overhead (VXLAN) / โอเวอร์เฮดโอเวอร์เลย์ | Switch to native L3 (Calico) / เปลี่ยนเป็น L3 ดั้งเดิม (Calico) |

---

## CNI Configuration / การตั้งค่า CNI

**English:**
CNI configuration involves two main components: configuration files that define the network setup, and binary plugins that implement the networking logic. Understanding these files helps with troubleshooting and customization.

**Thai:**
การตั้งค่า CNI ประกอบด้วยองค์ประกอบหลักสองอย่าง: ไฟล์ตั้งค่าที่กำหนดการตั้งค่าเครือข่าย และไบนารีปลั๊กอินที่นำตรรกะเครือข่ายไปใช้ การเข้าใจไฟล์เหล่านี้ช่วยในการแก้ไขปัญหาและการปรับแต่ง

### File Locations / ตำแหน่งไฟล์

**English:**
- **CNI Config Directory:** `/etc/cni/net.d/` — Contains `.conflist` or `.conf` files that define network configuration
- **CNI Plugin Binaries:** `/opt/cni/bin/` — Contains executable CNI plugins (bridge, portmap, bandwidth, etc.)

**Thai:**
- **ไดเรกทอรีตั้งค่า CNI:** `/etc/cni/net.d/` — มีไฟล์ `.conflist` หรือ `.conf` ที่กำหนดการตั้งค่าเครือข่าย
- **ไบนารีปลั๊กอิน CNI:** `/opt/cni/bin/` — มีปลั๊กอิน CNI แบบดำเนินการ (bridge, portmap, bandwidth ฯลฯ)

```bash
# Check CNI configuration directory
# ตรวจสอบไดเรกทอรีตั้งค่า CNI
ls -la /etc/cni/net.d/
# Example output / ตัวอย่างผลลัพธ์:
# -rw-r--r-- 1 root root  345 Mar 10 10:00 10-calico.conflist
# -rwxr-xr-x 1 root root  100 Mar 10 10:00 calico-kubeconfig

# Check CNI plugin binaries
# ตรวจสอบไบนารีปลั๊กอิน CNI
ls -la /opt/cni/bin/
# Example output / ตัวอย่างผลลัพธ์:
# -rwxr-xr-x 1 root root 4234240 Mar 10 10:00 bridge
# -rwxr-xr-x 1 root root 9793536 Mar 10 10:00 calico
# -rwxr-xr-x 1 root root 3686400 Mar 10 10:00 portmap
# -rwxr-xr-x 1 root root 3551232 Mar 10 10:00 bandwidth
```

### Kubelet CNI Configuration / การตั้งค่า Kubelet CNI

**English:**
Kubelet is responsible for calling CNI plugins when pods are created. It reads configuration from `/etc/cni/net.d/` and executes plugins from `/opt/cni/bin/`.

**Thai:**
Kubelet รับผิดชอบในการเรียกปลั๊กอิน CNI เมื่อพอดถูกสร้าง อ่านการตั้งค่าจาก `/etc/cni/net.d/` และดำเนินการปลั๊กอินจาก `/opt/cni/bin/`

```bash
# Verify kubelet is configured to use CNI
# ตรวจสอบ kubelet ตั้งค่าใช้ CNI

# Check kubelet flags / ตรวจสอบแฟล็ก kubelet
ps aux | grep kubelet | grep cni
# Should show --cni-bin-dir=/opt/cni/bin --cni-conf-dir=/etc/cni/net.d
# ควรแสดง --cni-bin-dir=/opt/cni/bin --cni-conf-dir=/etc/cni/net.d

# Check kubelet config file (if using config file instead of flags)
# ตรวจสอบไฟล์ตั้งค่า kubelet (ถ้าใช้ไฟล์ตั้งค่าแทนแฟล็ก)
cat /var/lib/kubelet/config.yaml | grep -A 5 cni
```

### Sample CNI Configuration File / ตัวอย่างไฟล์ตั้งค่า CNI

**English:**
Below is an example Calico CNI configuration file:

**Thai:**
ด้านล่างเป็นตัวอย่างไฟล์ตั้งค่า Calico CNI:

```json
// /etc/cni/net.d/10-calico.conflist
{
  "name": "k8s-pod-network",      // ชื่อ: เครือข่ายพอด k8s
  "cniVersion": "0.3.1",          // เวอร์ชัน CNI
  "plugins": [
    {
      "type": "calico",           // ประเภทปลั๊กอิน: calico
      "log_level": "info",        // ระดับบันทึก: info
      "datastore_type": "kubernetes",  // ประเภทที่เก็บข้อมูล: คิวเบอร์เนติส
      "nodename": "node-01",      // ชื่อโหนด
      "ipam": {                   // การจัดการ IP (IPAM)
          "type": "calico-ipam"   // ประเภท: calico-ipam
      },
      "policy": {                 // นโยบาย
          "type": "k8s"           // ประเภท: k8s (ใช้ Kubernetes NetworkPolicy)
      },
      "kubernetes": {             // การตั้งค่าคิวเบอร์เนติส
          "kubeconfig": "/etc/cni/net.d/calico-kubeconfig"
      }
    },
    {
      "type": "portmap",          // ประเภท: portmap (จัดการการแมปพอร์ต)
      "snat": true,               // เปิดใช้ SNAT
      "capabilities": {"portMappings": true}  // ความสามารถ: การแมปพอร์ต
    },
    {
      "type": "bandwidth",        // ประเภท: bandwidth (จัดการแบนด์วิดท์)
      "capabilities": {"bandwidth": true}  // ความสามารถ: แบนด์วิดท์
    }
  ]
}
```

### Verify CNI Installation / ตรวจสอบการติดตั้ง CNI

```bash
# Step 1: Check CNI config files exist
# ขั้นที่ 1: ตรวจสอบไฟล์ตั้งค่า CNI มีอยู่
ls -l /etc/cni/net.d/
# Expected: at least one .conf or .conflist file
# ที่คาดหวัง: มีไฟล์ .conf หรือ .conflist อย่างน้อยหนึ่งไฟล์

# Step 2: Check CNI plugin binaries exist
# ขั้นที่ 2: ตรวจสอบไบนารีปลั๊กอิน CNI มีอยู่
ls -l /opt/cni/bin/
# Expected: bridge, portmap, and CNI-specific plugins
# ที่คาดหวัง: bridge, portmap และปลั๊กอินเฉพาะ CNI

# Step 3: Verify CNI pods are running
# ขั้นที่ 3: ตรวจสอบพอด CNI ทำงานอยู่
kubectl get pods -n kube-system | grep -E 'calico|flannel|cilium'
# Expected: All CNI pods in Running status
# ที่คาดหวัง: พอด CNI ทั้งหมดในสถานะ Running

# Step 4: Test pod-to-pod communication
# ขั้นที่ 4: ทดสอบการสื่อสารพอดถึงพอด
kubectl run test-pod --image=busybox --restart=Never -- sleep 60
# Wait for pod to be running / รอพอดทำงาน
kubectl get pods test-pod
# Then test connectivity / แล้วทดสอบการเชื่อมต่อ
kubectl exec test-pod -- ping -c 2 8.8.8.8
```

---

## Why CNI and OSI Matter Together / ทำไม CNI และ OSI สำคัญร่วมกัน

**English:**
Understanding both CNI and the OSI model together provides a powerful framework for designing, implementing, and troubleshooting Kubernetes networking. The OSI model gives you a layered mental model, while CNI implements specific layers of that model.

**Thai:**
การเข้าใจทั้ง CNI และโมเดล OSI ร่วมกันให้กรอบอันทรงพลังสำหรับการออกแบบ นำไปใช้ และแก้ไขปัญหาเครือข่ายคิวเบอร์เนติส โมเดล OSI ให้แบบจำลองทางจิตแบบชั้น ส่วน CNI นำชั้นเฉพาะของแบบจำลองนั้นไปใช้

### Benefits of Understanding Both / ประโยชน์ของการเข้าใจทั้งสองอย่าง:

| # | Benefit / ประโยชน์ | English Explanation / คำอธิบาย (EN) | Thai Explanation / คำอธิบาย (TH) |
|---|---|---|---|
| 1 | **Choose the Right CNI** | Knowing OSI layers helps you select a CNI that operates at the layers you need (L3 for routing, L7 for application filtering). | การรู้จักชั้น OSI ช่วยเลือก CNI ที่ทำงานในชั้นที่คุณต้องการ (L3 สำหรับการเราต์, L7 สำหรับการกรองแอปพลิเคชัน) |
| 2 | **Debug Network Issues** | When communication fails, you can systematically check each layer: L1 (cables/hardware), L2 (MAC/bridge), L3 (IP/routes), L4 (ports/firewall), L7 (application). | เมื่อการสื่อสารล้มเหลว คุณสามารถตรวจสอบแต่ละชั้นอย่างเป็นระบบ: L1 (สาย/ฮาร์ดแวร์), L2 (MAC/บริดจ์), L3 (IP/เส้นทาง), L4 (พอร์ต/ไฟร์วอลล์), L7 (แอปพลิเคชัน) |
| 3 | **Design Network Policies** | Understanding which layer each policy operates at helps you create effective security rules (L3/L4 for basic, L7 for advanced). | การเข้าใจว่านโยบายแต่ละอันทำงานที่ชั้นไหนช่วยสร้างกฎความปลอดภัยที่มีประสิทธิภาพ (L3/L4 สำหรับพื้นฐาน, L7 สำหรับขั้นสูง) |
| 4 | **Optimize Performance** | Different layers have different overheads. Native L3 (Calico) is faster than L2 overlay (Flannel VXLAN). | ชั้นต่างกันมีโอเวอร์เฮดต่างกัน L3 ดั้งเดิม (Calico) เร็วกว่าโอเวอร์เลย์ L2 (Flannel VXLAN) |
| 5 | **Implement Security** | Encryption can be applied at different layers: L2 (MACsec), L3 (IPsec/WireGuard), L4 (mTLS), L7 (TLS). | การเข้ารหัสสามารถใส่ในชั้นต่างกัน: L2 (MACsec), L3 (IPsec/WireGuard), L4 (mTLS), L7 (TLS) |
| 6 | **Troubleshoot Effectively** | A systematic OSI-layer approach prevents missing the real cause — always start from L1 and work up. | วิธีการ OSI อย่างเป็นระบบป้องกันไม่พลาดสาเหตุจริง — เริ่มจาก L1 แล้วขึ้นบนเสมอ |

---

## Network Flow Example / ตัวอย่างการไหลของเครือข่าย

**English:**
Let's trace a complete network flow from Pod A on Node 1 to Pod B on Node 2, showing how each OSI layer is involved and which CNI components handle each layer.

**Thai:**
มาติดตามการไหลของเครือข่ายสมบูรณ์จากพอด A บนโหนด 1 ไปพอด B บนโหนด 2 แสดงว่าแต่ละชั้น OSI เกี่ยวข้องอย่างไรและองค์ประกอบ CNI ใดจัดการแต่ละชั้น

### Scenario / สถานการณ์

```
Pod A (Node 1, IP: 10.244.1.5)  ----->  Pod B (Node 2, IP: 10.244.2.8)
พอด A (โหนด 1, IP: 10.244.1.5)  ----->  พอด B (โหนด 2, IP: 10.244.2.8)

Application: Pod A sends HTTP GET request to Pod B's web server
แอปพลิเคชัน: พอด A ส่งคำขอ HTTP GET ไปเว็บเซิร์ฟเวอร์ของพอด B
```

### Step-by-Step Flow / การไหลทีละขั้นตอน

#### Layer 7 — Application Layer / ชั้นที่ 7 — ชั้นแอปพลิเคชัน

```
English:
Pod A's application creates an HTTP GET request to http://10.244.2.8:80
The application (e.g., curl, wget, or a web app) generates the HTTP request.

Thai:
แอปพลิเคชันของพอด A สร้างคำขอ HTTP GET ไป http://10.244.2.8:80
แอปพลิเคชัน (เช่น curl, wget หรือเว็บแอป) สร้างคำขอ HTTP

Handled by: Application code (not CNI)
จัดการโดย: โค้ดแอปพลิเคชัน (ไม่ใช่ CNI)
```

#### Layer 4 — Transport Layer / ชั้นที่ 4 — ชั้นขนส่ง

```
English:
- TCP handshake initiated: source port (e.g., 45678) → destination port 80
- TCP segment created with SYN, ACK flags
- Connection tracking established (Cilium tracks this at L4)

Thai:
- เริ่ม TCP handshake: พอร์ตต้นทาง (เช่น 45678) → พอร์ตปลายทาง 80
- สร้างเซ็กเมนต์ TCP พร้อมแฟล็ก SYN, ACK
- สร้างการติดตามการเชื่อมต่อ (Cilium ติดตามที่ L4)

Handled by: Kernel TCP/IP stack, CNI (Cilium/Calico for L4 policies)
จัดการโดย: สแตก TCP/IP ของเคอร์เนล, CNI (Cilium/Calico สำหรับนโยบาย L4)
```

#### Layer 3 — Network Layer / ชั้นที่ 3 — ชั้นเครือข่าย

```
English:
- IP packet created: source 10.244.1.5 → destination 10.244.2.8
- Calico looks up routing table: destination 10.244.2.0/24 is on Node 2
- BGP advertises this route from Node 2 to Node 1
- Packet is routed to Node 2's IP

Thai:
- สร้างแพ็กเก็ต IP: ต้นทาง 10.244.1.5 → ปลายทาง 10.244.2.8
- Calico มองตารางการเราต์: ปลายทาง 10.244.2.0/24 อยู่บนโหนด 2
- BGP ประกาศเส้นทางนี้จากโหนด 2 ไปโหนด 1
- แพ็กเก็ตถูกเราต์ไปยัง IP ของโหนด 2

Handled by: Calico (BGP routing, IP forwarding)
จัดการโดย: Calico (การเราต์ BGP, การส่งต่อ IP)
```

#### Layer 2 — Data Link Layer / ชั้นที่ 2 — ชั้นเชื่อมโยงข้อมูล

```
English:
- Ethernet frame created with source MAC (Node 1) → destination MAC (Node 2 or gateway)
- If using Flannel VXLAN: the packet is encapsulated in a VXLAN header
- If using Calico native routing: direct Ethernet frame to next hop

Thai:
- สร้างเฟรม Ethernet พร้อม MAC ต้นทาง (โหนด 1) → MAC ปลายทาง (โหนด 2 หรือเกตเวย์)
- ถ้าใช้ Flannel VXLAN: แพ็กเก็ตถูกห่อส่วนหัว VXLAN
- ถ้าใช้ Calico native routing: เฟรม Ethernet ตรงไปยังฮ็อปถัดไป

Handled by: Kernel networking, CNI (Flannel for VXLAN encapsulation)
จัดการโดย: การเราต์เคอร์เนล, CNI (Flannel สำหรับการห่อ VXLAN)
```

#### Layer 1 — Physical Layer / ชั้นที่ 1 — ชั้นกายภาพ

```
English:
- Bits transmitted over physical medium (Ethernet cable, fiber, wireless)
- Switches/routers forward frames between nodes
- This layer is NOT managed by CNI

Thai:
- บิตถูกส่งผ่านสื่อทางกายภาพ (สาย Ethernet, ไฟเบอร์, ไร้สาย)
- สวิตช์/เราเตอร์ส่งต่อเฟรมระหว่างโหนด
- ชั้นนี้ CNI ไม่จัดการ

Handled by: Physical infrastructure (switches, cables, routers)
จัดการโดย: โครงสร้างพื้นฐานทางกายภาพ (สวิตช์ สาย สายเราเตอร์)
```

### Complete Flow Diagram / แผนภาพการไหลสมบูรณ์

```
+------------------------------------------------------------------+
|  Pod A (Node 1)                           Pod B (Node 2)          |
|  พอด A (โหนด 1)                            พอด B (โหนด 2)          |
|                                                                  |
|  L7: HTTP GET /                            L7: รับ HTTP GET /    |
|      (Application code)                         (เว็บเซิร์ฟเวอร์)   |
|       |                                          ^                |
|       v                                          |                |
|  L4: TCP :45678 → :80                        L4: TCP :80 ← :45678|
|      (Connection tracking)                        (ตอบกลับ)         |
|       |                                          ^                |
|       v                                          |                |
|  L3: IP 10.244.1.5 → 10.244.2.8              L3: รับแพ็กเก็ต IP   |
|      (Calico BGP routing)                         (Calico เราต์)    |
|       |                                          ^                |
|       v                                          |                |
|  L2: Ethernet frame → Node 2                 L2: รับเฟรม Ethernet  |
|      (MAC addressing / VXLAN)                     (ถอดห่อถ้ามี)      |
|       |                                          ^                |
|       v                                          |                |
|  L1: Physical transmission ─── Ethernet ──────> L1: รับสัญญาณ      |
|      (สาย/สวิตช์/เราเตอร์)                         (สาย/สวิตช์)       |
+------------------------------------------------------------------+

CNI manages L2-L3 (sometimes L4, L7 with Cilium)
CNI จัดการ L2-L3 (บางครั้ง L4, L7 กับ Cilium)
Application manages L4-L7
แอปพลิเคชันจัดการ L4-L7
```

---

## Practice Exercises / แบบฝึกหัด

### Exercise 1: Install CNI Plugin / แบบฝึกหัดที่ 1: ติดตั้งปลั๊กอิน CNI

**Tasks / งาน:**

1. **Deploy a cluster with Calico CNI**
   - **EN:** Install Calico on your Kubernetes cluster and verify all pods are running.
   - **TH:** ติดตั้ง Calico บนคลัสเตอร์คิวเบอร์เนติสและตรวจสอบพอดทั้งหมดทำงานอยู่

2. **Verify pod-to-pod communication**
   - **EN:** Create two test pods on different nodes and confirm they can ping and communicate.
   - **TH:** สร้างพอดทดสอบสองตัวบนโหนดต่างกันและยืนยันว่าปิงและสื่อสารกันได้

3. **Document your findings**
   - **EN:** Record the installation steps, pod statuses, and communication test results.
   - **TH:** บันทึกขั้นตอนการติดตั้ง สถานะพอด และผลลัพธ์การทดสอบการสื่อสาร

```bash
# Commands to run / คำสั่งที่ต้องรัน:
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml
kubectl get pods -n kube-system | grep calico
kubectl run test-a --image=busybox --restart=Never -- sleep 3600
kubectl run test-b --image=busybox --restart=Never -- sleep 3600
kubectl get pods -o wide
kubectl exec test-a -- ping -c 3 <test-b-ip>
```

### Exercise 2: Create Network Policies / แบบฝึกหัดที่ 2: สร้างนโยบายเครือข่าย

**Tasks / งาน:**

1. **Deny all ingress traffic**
   - **EN:** Apply a default deny-all ingress policy and verify all incoming traffic is blocked.
   - **TH:** ใช้นโยบายปฏิเสธการเข้าทั้งหมดและยืนยันทราฟฟิกขาเข้าทั้งหมดถูกบล็อก

2. **Allow specific pod-to-pod communication**
   - **EN:** Create a policy that allows only specific pods (by label) to communicate.
   - **TH:** สร้างนโยบายที่อนุญาตเฉพาะพอดเฉพาะ (ตามป้ายกำกับ) ให้สื่อสารได้

3. **Test policy enforcement**
   - **EN:** Verify allowed traffic succeeds and denied traffic fails with timeout.
   - **TH:** ยืนยันทราฟฟิกที่อนุญาตสำเร็จและทราฟฟิกที่ปฏิเสธล้มเหลวด้วยการหมดเวลา

```bash
# Step 1: Apply deny-all / ขั้นที่ 1: ใช้นโยบายปฏิเสธทั้งหมด
kubectl apply -f - <<EOF
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all-ingress
  namespace: default
spec:
  podSelector: {}
  policyTypes:
  - Ingress
EOF

# Step 2: Allow specific traffic / ขั้นที่ 2: อนุญาตทราฟฟิกเฉพาะ
kubectl apply -f - <<EOF
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-nginx-from-frontend
  namespace: default
spec:
  podSelector:
    matchLabels:
      app: nginx
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
    ports:
    - protocol: TCP
      port: 80
EOF

# Step 3: Test / ขั้นที่ 3: ทดสอบ
kubectl run nginx --image=nginx --labels=app=nginx --restart=Never
kubectl run frontend --image=busybox --labels=app=frontend --restart=Never -- sleep 3600
kubectl run other --image=busybox --restart=Never -- sleep 3600
# frontend → nginx should WORK (frontend → nginx ควรทำงาน)
# other → nginx should FAIL (other → nginx ควรล้มเหลว)
```

### Exercise 3: Understand OSI Layers / แบบฝึกหัดที่ 3: ทำความเข้าใจชั้น OSI

**Tasks / งาน:**

1. **Document which OSI layers your CNI operates at**
   - **EN:** Identify and document the OSI layers (L2, L3, L4, L7) that your installed CNI plugin handles.
   - **TH:** ระบุและบันทึกชั้น OSI (L2, L3, L4, L7) ที่ปลั๊กอิน CNI ที่ติดตั้งอยู่จัดการ

2. **Compare with a different CNI plugin**
   - **EN:** Research another CNI plugin and compare which layers they operate at.
   - **TH:** วิจัยปลั๊กอิน CNI อื่นและเปรียบเทียบชั้นที่ทำงาน

3. **Explain how network policies map to OSI layers**
   - **EN:** Describe how Kubernetes Network Policies map to specific OSI layers (L3 for IP, L4 for port, L7 for HTTP with Cilium).
   - **TH:** อธิบายว่านโยบายเครือข่ายคิวเบอร์เนติสแมปกับชั้น OSI เฉพาะอย่างไร (L3 สำหรับ IP, L4 สำหรับพอร์ต, L7 สำหรับ HTTP กับ Cilium)

### Exercise 4: Troubleshooting / แบบฝึกหัดที่ 4: การแก้ไขปัญหา

**Tasks / งาน:**

1. **Simulate a network issue**
   - **EN:** Stop a CNI pod or misconfigure a network policy to create a controlled failure.
   - **TH:** หยุดพอด CNI หรือตั้งค่าผิดพลาดนโยบายเครือข่ายเพื่อสร้างความล้มเหลวควบคุม

2. **Use systematic OSI-layer approach to debug**
   - **EN:** Start from L1 and work up through L7, checking each layer for issues.
   - **TH:** เริ่มจาก L1 แล้วขึ้นผ่าน L7 ตรวจสอบแต่ละชั้นสำหรับปัญหา

3. **Document findings and solution**
   - **EN:** Write down what layer failed, why it failed, and how you fixed it.
   - **TH:** เขียนว่าชั้นไหนล้มเหลว ทำไมล้มเหลว และแก้ไขอย่างไร

```bash
# Simulate CNI pod failure / จำลองความล้มเหลวพอด CNI
kubectl delete pod -n kube-system -l k8s-app=calico-node --field-selector=status.phase=Running

# Debug using OSI approach / แก้ไขปัญหาใช้วิธีการ OSI
# L1: Check physical connectivity (ping node IPs)
#     ตรวจสอบการเชื่อมต่อทางกายภาพ (ปิง IP โหนด)
ping <node2-ip>

# L2: Check MAC addresses and bridge interfaces
#     ตรวจสอบที่อยู่ MAC และอินเทอร์เฟซบริดจ์
ip link show

# L3: Check IP routes and BGP status
#     ตรวจสอบเส้นทาง IP และสถานะ BGP
ip route show
kubectl get ippool -A

# L4: Check port accessibility
#     ตรวจสอบการเข้าถึงพอร์ต
kubectl exec <pod> -- nc -zv <target-ip> <port>

# L7: Check application-level connectivity
#     ตรวจสอบการเชื่อมต่อระดับแอปพลิเคชัน
kubectl exec <pod> -- curl -v http://<target-ip>:<port>
```

### Exercise 5: Bonus — Advanced CNI Features / แบบฝึกหัดที่ 5: โบนัส — คุณสมบัติ CNI ขั้นสูง

**Tasks / งาน:**

1. **Test different CNI plugins**
   - **EN:** Install and compare Flannel, Calico, and Cilium on separate test clusters.
   - **TH:** ติดตั้งและเปรียบเทียบ Flannel, Calico และ Cilium บนคลัสเตอร์ทดสอบแยก

2. **Benchmark network performance**
   - **EN:** Use tools like `iperf3` or `netperf` to measure throughput and latency between pods.
   - **TH:** ใช้เครื่องมือเช่น `iperf3` หรือ `netperf` วัดปริมาณงานและความหน่วงแฝงระหว่างพอด

3. **Implement L7 network policy with Cilium**
   - **EN:** Create a CiliumNetworkPolicy that filters HTTP traffic based on path, method, or headers.
   - **TH:** สร้าง CiliumNetworkPolicy ที่กรองทราฟฟิก HTTP ตามเส้นทาง เมธอด หรือเฮดเดอร์

```yaml
# Example: Cilium L7 Policy / ตัวอย่าง: นโยบาย L7 ของ Cilium
# Allow only GET requests to /api/v1/health, deny everything else
# อนุญาตเฉพาะคำขอ GET ไป /api/v1/health ปฏิเสธอย่างอื่นทั้งหมด
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy      # ประเภท: นโยบายเครือข่าย Cilium
metadata:
  name: l7-http-policy         # ชื่อ: นโยบาย HTTP L7
spec:
  endpointSelector:            # เลือกจุดปลายทาง
    matchLabels:
      app: api-server          # แอป: api-server
  ingress:                     # การเข้า
  - toPorts:                   # ไปยังพอร์ต
    - ports:                   # พอร์ต
      - port: "8080"           # พอร์ต: 8080
        protocol: TCP          # โปรโตคอล: TCP
      rules:                   # กฎ
        http:                  # กฎ HTTP
        - method: GET          # เมธอด: GET
          path: "/api/v1/health"  # เส้นทาง: /api/v1/health
```

```bash
# Apply Cilium L7 policy / ใช้นโยบาย L7 ของ Cilium
kubectl apply -f cilium-l7-policy.yaml

# Test: GET /api/v1/health should SUCCEED
# ทดสอบ: GET /api/v1/health ควรสำเร็จ
kubectl exec client -- curl -s http://api-server:8080/api/v1/health

# Test: GET /api/v1/secret should FAIL
# ทดสอบ: GET /api/v1/secret ควรล้มเหลว
kubectl exec client -- curl -s --connect-timeout 3 http://api-server:8080/api/v1/secret

# Test: POST /api/v1/health should FAIL (wrong method)
# ทดสอบ: POST /api/v1/health ควรล้มเหลว (เมธอดผิด)
kubectl exec client -- curl -s -X POST --connect-timeout 3 http://api-server:8080/api/v1/health
```

---

## Quick Reference Card / การ์ดอ้างอิงด่วน

### CNI Plugin Quick Comparison / เปรียบเทียบด่วนปลั๊กอิน CNI

| Question / คำถาม | Recommendation / คำแนะนำ |
|---|---|
| I need the simplest setup / ฉันต้องการตั้งค่าที่ง่ายที่สุด | **Flannel** |
| I need network policies / ฉันต้องการนโยบายเครือข่าย | **Calico** or **Cilium** |
| I need L7 filtering / ฉันต้องการกรอง L7 | **Cilium** |
| I need production-grade performance / ฉันต้องการประสิทธิภาพระดับโพรดักชัน | **Calico** or **Cilium** |
| I'm learning Kubernetes / ฉันกำลังเรียนคิวเบอร์เนติส | **Flannel** (start) → **Calico** (next) |

### Common Commands / คำสั่งทั่วไป

```bash
# Install CNI / ติดตั้ง CNI
kubectl apply -f <cni-manifest.yaml>

# Check CNI status / ตรวจสอบสถานะ CNI
kubectl get pods -n kube-system | grep -E 'calico|flannel|cilium'

# View network policies / แสดงนโยบายเครือข่าย
kubectl get networkpolicies --all-namespaces

# Test pod connectivity / ทดสอบการเชื่อมต่อพอด
kubectl exec <pod> -- ping <target-ip>
kubectl exec <pod> -- curl -s <target-ip>:<port>

# View CNI logs / ดูปฏิทิน CNI
kubectl logs -n kube-system -l k8s-app=<cni-name>

# Check pod IP / ตรวจสอบ IP พอด
kubectl get pod <pod-name> -o jsonpath='{.status.podIP}'

# Check node routes / ตรวจสอบเส้นทางโหนด
kubectl get nodes -o wide

# Debug from inside pod / แก้ไขปัญหาจากภายในพอด
kubectl exec -it <pod> -- sh
ip addr      # Check IP / ตรวจสอบ IP
ip route     # Check routes / ตรวจสอบเส้นทาง
ping 8.8.8.8 # Test external / ทดสอบภายนอก
nslookup kubernetes.default  # Test DNS / ทดสอบ DNS
```

---

## Resources / แหล่งข้อมูล

### Official Documentation / เอกสารทางการ

| Resource / แหล่งข้อมูล | Description / คำอธิบาย | Link / ลิงก์ |
|---|---|---|
| **CNI Specification** / ข้อกำหนด CNI | Official CNI specification and reference implementation / ข้อกำหนด CNI ทางการและการอ้างอิงการใช้งาน | [GitHub - containernetworking/cni](https://github.com/containernetworking/cni) |
| **Calico Documentation** / เอกสาร Calico | Full Calico user guide, installation, and troubleshooting / คู่มือผู้ใช้ Calico เต็มรูปแบบ การติดตั้ง และการแก้ไขปัญหา | [Project Calico Docs](https://projectcalico.docs.tigera.io/) |
| **Flannel Documentation** / เอกสาร Flannel | Flannel setup, configuration, and architecture / การตั้งค่า Flacket การตั้งค่า และสถาปัตยกรรม | [Flannel GitHub](https://github.com/flannel-io/flannel) |
| **Cilium Documentation** / เอกสาร Cilium | Cilium guides for eBPF, networking, security, and observability / คู่มือ Cilium สำหรับ eBPF เครือข่าย ความปลอดภัย และการสังเกต | [Cilium Docs](https://docs.cilium.io/) |
| **Kubernetes Networking** / เครือข่ายคิวเบอร์เนติส | Official Kubernetes networking concepts and cluster administration / แนวคิดเครือข่ายคิวเบอร์เนติสทางการและการบริหารคลัสเตอร์ | [K8s Networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/) |

### Additional Learning Resources / แหล่งข้อมูลการเรียนรู้เพิ่มเติม

| Resource / แหล่งข้อมูล | Description / คำอธิบาย | Link / ลิงก์ |
|---|---|---|
| **Cilium Workshop** / เวิร์กช็อป Cilium | Hands-on tutorials for Cilium and eBPF / บทสอนปฏิบัติสำหรับ Cilium และ eBPF | [Cilium Workshop](https://isovalent.com/resources/workshops/) |
| **Calico Network Policy Examples** / ตัวอย่างนโยบายเครือข่าย Calico | Collection of NetworkPolicy YAML examples / คอลเลกชันตัวอย่าง NetworkPolicy YAML | [Calico Policy Examples](https://projectcalico.docs.tigera.io/security/calico-network-policy/) |
| **Kubernetes Network Policy Recipes** / สูตรนโยบายเครือข่ายคิวเบอร์เนติส | Community-maintained network policy examples / ตัวอย่างนโยบายที่ชุมชนดูแล | [Ahmetb Network Policy Recipes](https://github.com/ahmetb/kubernetes-network-policy-recipes) |
| **eBPF.io** | Learn about eBPF technology behind Cilium / เรียนรู้เทคโนโลยี eBPF เบื้องหลัง Cilium | [eBPF.io](https://ebpf.io/) |

---

## Summary / สรุป

**English:**
CNI (Container Network Interface) is the backbone of Kubernetes networking. It provides the plugins that enable pod-to-pod communication, network policy enforcement, and cross-node connectivity. Understanding how CNI relates to the OSI model helps you choose the right plugin, design effective network policies, and troubleshoot issues systematically. Calico, Flannel, and Cilium are the most popular choices, each operating at different OSI layers with varying capabilities.

**Thai:**
CNI (Container Network Interface) เป็นกระดูกสันหลังของเครือข่ายคิวเบอร์เนติส ให้ปลั๊กอินที่เปิดใช้งานการสื่อสารพอดถึงพอด การบังคับใช้นโยบายเครือข่าย และการเชื่อมต่อข้ามโหนด การเข้าใจว่า CNI สัมพันธ์กับโมเดล OSI อย่างไรช่วยให้คุณเลือกปลั๊กอินที่ถูกต้อง ออกแบบนโยบายเครือข่ายที่มีประสิทธิภาพ และแก้ไขปัญหาอย่างเป็นระบบ Calico, Flannel และ Cilium เป็นตัวเลือกยอดนิยม แต่ละตัวทำงานที่ชั้น OSI ต่างกันมีความสามารถต่างกัน

**Key Takeaways / ข้อสำคัญที่ต้องจำ:**

1. **CNI enables Kubernetes networking** / CNI เปิดใช้งานเครือข่ายคิวเบอร์เนติส
2. **OSI model provides troubleshooting framework** / โมเดล OSI ให้กรอบการแก้ไขปัญหา
3. **Choose CNI based on your needs** / เลือก CNI ตามความต้องการของคุณ
4. **Network Policies require CNI support** / นโยบายเครือข่ายต้องการการรองรับจาก CNI
5. **Calico = L3 routing, Cilium = eBPF/L7, Flannel = simple** / Calico = การเราต์ L3, Cilium = eBPF/L7, Flannel = ง่าย
6. **Always test pod-to-pod communication after CNI install** / ทดสอบการสื่อสารพอดถึงพอดเสมอหลังติดตั้ง CNI

---

*Document Version / เวอร์ชันเอกสาร: 2.0 — Bilingual Edition (English/Thai)*
*Last Updated / อัปเดตล่าสุด: April 2026 / เมษายน 2026*
