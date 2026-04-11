# OSI Model / โมเดล OSI

> **Open Systems Interconnection Model / โมเดลการเชื่อมต่อระบบเปิด**
>
> This guide covers the OSI Model in detail, with English explanations followed by Thai translations for every section.
> คู่มือนี้ครอบคลุมโมเดล OSI อย่างละเอียด พร้อมคำอธิบายภาษาอังกฤษตามด้วยการแปลภาษาไทยในทุกส่วน

---

## Table of Contents / สารบัญ

1. [What is the OSI Model? / โมเดล OSI คืออะไร?](#what-is-the-osi-model--โมเดล-osi-คืออะไร)
2. [Why DevOps Engineers Need to Know OSI / ทำไมวิศวกร DevOps ต้องรู้จัก OSI](#why-devops-engineers-need-to-know-osi--ทำไมวิศวกร-devops-ต้องรู้จัก-osi)
3. [The 7 Layers Explained / อธิบายทั้ง 7 เลเยอร์](#the-7-layers-explained--อธิบายทั้ง-7-เลเยอร์)
4. [Real-World Example: Accessing a Website / ตัวอย่างจริง: การเข้าถึงเว็บไซต์](#real-world-example-accessing-a-website--ตัวอย่างจริง-การเข้าถึงเว็บไซต์)
5. [TCP vs UDP Comparison / เปรียบเทียบ TCP กับ UDP](#tcp-vs-udp-comparison--เปรียบเทียบ-tcp-กับ-udp)
6. [TCP 3-Way Handshake / การจับมือสามทางของ TCP](#tcp-3-way-handshake--การจับมือสามทางของ-tcp)
7. [Network Devices by Layer / อุปกรณ์เครือข่ายตามเลเยอร์](#network-devices-by-layer--อุปกรณ์เครือข่ายตามเลเยอร์)
8. [Troubleshooting Commands / คำสั่งแก้ปัญหาเครือข่าย](#troubleshooting-commands--คำสั่งแก้ปัญหาเครือข่าย)
9. [Practice Exercises / แบบฝึกหัด](#practice-exercises--แบบฝึกหัด)
10. [Resources / แหล่งข้อมูลเพิ่มเติม](#resources--แหล่งข้อมูลเพิ่มเติม)

---

## What is the OSI Model? / โมเดล OSI คืออะไร?

### Definition / นิยาม

**English:**
The Open Systems Interconnection (OSI) model is a conceptual framework created by the International Organization for Standardization (ISO) that standardizes network communications into **7 distinct layers**. Each layer has a specific role and communicates with the layers directly above and below it. The OSI model helps engineers understand, design, and troubleshoot networks by breaking down complex communication into manageable components.

**ภาษาไทย:**
โมเดล OSI (Open Systems Interconnection) เป็นกรอบแนวคิดที่สร้างโดยองค์การระหว่างประเทศว่าด้วยการมาตรฐาน (ISO) ซึ่งแบ่งการสื่อสารเครือข่ายออกเป็น **7 เลเยอร์ที่แตกต่างกัน** แต่ละเลเยอร์มีบทบาทเฉพาะและสื่อสารกับเลเยอร์ที่อยู่ด้านบนและด้านล่างโดยตรง โมเดล OSI ช่วยให้นักวิศวกรเข้าใจ ออกแบบ และแก้ปัญหาเครือข่ายโดยแบ่งการสื่อสารที่ซับซ้อนออกเป็นส่วนๆ ที่จัดการได้

---

### Purpose of the OSI Model / จุดประสงค์ของโมเดล OSI

**English:**
- **Standardization**: Allows different systems and vendors to communicate with each other regardless of their underlying technology.
- **Modularity**: Each layer can be developed, updated, or replaced independently without affecting other layers.
- **Troubleshooting**: Helps isolate problems to specific layers, making debugging faster and more efficient.
- **Education**: Provides a common language and mental model for discussing network concepts.

**ภาษาไทย:**
- **การมาตรฐาน**: ทำให้ระบบและผู้จำหน่ายที่แตกต่างกันสามารถสื่อสารกันได้โดยไม่คำนึงถึงเทคโนโลยีพื้นฐาน
- **ความเป็นโมดูล**: แต่ละเลเยอร์สามารถพัฒนา อัปเดต หรือแทนที่ได้โดยไม่กระทบเลเยอร์อื่น
- **การแก้ปัญหา**: ช่วยแยกปัญหาไปยังเลเยอร์เฉพาะ ทำให้การดีบักเร็วและมีประสิทธิภาพมากขึ้น
- **การศึกษา**: ให้ภาษาทั่วไปและแบบจำลองความคิดสำหรับการพูดถึงแนวคิดเครือข่าย

---

### Historical Context / บริบททางประวัติศาสตร์

**English:**
The OSI model was first introduced in **1984** by ISO. While the TCP/IP model became the practical implementation used on the internet today, the OSI model remains the primary **teaching and troubleshooting framework** for understanding how networks operate. Many networking concepts, protocols, and devices are still described using OSI layer terminology.

**ภาษาไทย:**
โมเดล OSI เปิดตัวครั้งแรกในปี **ค.ศ. 1984** โดย ISO ในขณะที่โมเดล TCP/IP กลายเป็นการนำไปใช้จริงบนอินเทอร์เน็ตในปัจจุบัน แต่โมเดล OSI ยังคงเป็น **กรอบการสอนและการแก้ปัญหาหลัก** สำหรับเข้าใจการทำงานของเครือข่าย แนวคิด โปรโตคอล และอุปกรณ์เครือข่ายจำนวนมากยังคงอธิบายโดยใช้ศัพท์ของเลเยอร์ OSI

---

## Why DevOps Engineers Need to Know OSI / ทำไมวิศวกร DevOps ต้องรู้จัก OSI

**English:**
Understanding the OSI model is essential for DevOps engineers because networking is at the core of everything we do. Here's why:

1. **Troubleshooting Network Issues**: When a service is unreachable, knowing whether the problem is at the physical layer (cable unplugged), network layer (routing issue), transport layer (port blocked), or application layer (HTTP 500 error) dramatically reduces debugging time.

2. **Configuring Firewalls and Security Groups**: Cloud providers (AWS, GCP, Azure) use security groups and network ACLs that operate at different layers. Understanding L3/L4 vs L7 rules is critical for proper security configuration.

3. **Load Balancing Decisions**: Choosing between Layer 4 (transport) and Layer 7 (application) load balancers affects performance, features, and cost. L4 LBs route based on IP/port; L7 LBs can inspect HTTP headers and route based on URL paths.

4. **Kubernetes Networking (CNI)**: Container Network Interface plugins operate at L2-L4. Understanding how pods communicate, how services work (iptables/IPVS), and how network policies function requires OSI knowledge.

5. **Security Implementation**: TLS/SSL encryption happens at Layer 6 (Presentation). Understanding where encryption occurs helps design proper security architectures.

6. **Monitoring and Observability**: Different monitoring tools operate at different layers. Ping works at L3, port checks at L4, and health endpoints at L7.

**ภาษาไทย:**
การเข้าใจโมเดล OSI เป็นสิ่งสำคัญสำหรับวิศวกร DevOps เนื่องจากเครือข่ายเป็นแกนกลางของทุกสิ่งที่เราทำ นี่คือเหตุผล:

1. **การแก้ปัญหาเครือข่าย**: เมื่อบริการเข้าถึงไม่ได้ การรู้ว่าปัญหาอยู่ที่เลเยอร์ใด (สายเคเบิลหลุด, ปัญหารouting, พอร์ตถูกบล็อก, หรือข้อผิดพลาด HTTP 500) ลดเวลาดีบักได้อย่างมาก

2. **การกำหนดค่าไฟร์วอลล์และ Security Groups**: ผู้ให้บริการคลาวด์ (AWS, GCP, Azure) ใช้ security groups และ network ACLs ที่ทำงานในเลเยอร์ต่างกัน การเข้าใจกฎ L3/L4 เทียบกับ L7 สำคัญมากสำหรับการกำหนดค่าความปลอดภัยที่เหมาะสม

3. **การตัดสินใจเลือก Load Balancer**: การเลือกระหว่าง Layer 4 (transport) และ Layer 7 (application) load balancers ส่งผลต่อประสิทธิภาพ คุณสมบัติ และค่าใช้จ่าย L4 LBs route ตาม IP/port; L7 LBs สามารถตรวจสอบ HTTP headers และ route ตาม URL paths

4. **เครือข่าย Kubernetes (CNI)**: Container Network Interface plugins ทำงานที่ L2-L4 การเข้าใจว่า pods สื่อสารกันอย่างไร services ทำงานอย่างไร (iptables/IPVS) และ network policies ทำงานอย่างไรต้องมีความรู้ OSI

5. **การนำไปใช้ด้านความปลอดภัย**: การเข้ารหัส TLS/SSL เกิดขึ้นที่เลเยอร์ 6 (Presentation) การเข้าใจว่าการเข้ารหัสเกิดขึ้นที่ไหนช่วยออกแบบสถาปัตยกรรมความปลอดภัยที่เหมาะสม

6. **การตรวจสอบและ Observability**: เครื่องมือตรวจสอบต่างกันทำงานที่เลเยอร์ต่างกัน Ping ทำงานที่ L3, การตรวจสอบพอร์ตที่ L4, และ health endpoints ที่ L7

---

## The 7 Layers Explained / อธิบายทั้ง 7 เลเยอร์

**English:**
The OSI model is typically viewed from **Layer 7 (top) to Layer 1 (bottom)**, but data flows **down** through the layers on the sender's side and **up** through the layers on the receiver's side. Each layer adds its own header (encapsulation) before passing data to the layer below.

**ภาษาไทย:**
โมเดล OSI มักมองจาก **เลเยอร์ 7 (บนสุด) ไปยังเลเยอร์ 1 (ล่างสุด)** แต่ข้อมูลไหล **ลง** ผ่านเลเยอร์ด้านผู้ส่ง และ **ขึ้น** ผ่านเลเยอร์ด้านผู้รับ แต่ละเลเยอร์เพิ่ม header ของตัวเอง (encapsulation) ก่อนส่งข้อมูลไปยังเลเยอร์ด้านล่าง

---

### Layer 7: Application Layer / เลเยอร์ 7: เลเยอร์แอปพลิเคชัน

**English:**

- **Function**: The Application layer is the closest layer to the end user. It provides network services directly to applications like web browsers, email clients, and file transfer programs. This is where users interact with the network.

- **Key Protocols**:
  - **HTTP/HTTPS**: Web browsing (ports 80, 443)
  - **FTP/SFTP**: File transfer (ports 20, 21, 22)
  - **SMTP**: Email sending (port 25)
  - **DNS**: Domain name resolution (port 53)
  - **SSH**: Secure shell access (port 22)

- **Common Devices**: Application-layer firewalls, API gateways, Layer 7 load balancers

- **DevOps Relevance**: API gateways, ingress controllers, and service meshes (like Istio) operate at this layer. Understanding HTTP methods, status codes, and headers is essential.

- **Real-World Example**: When you type `https://example.com` in your browser, the browser creates an HTTP GET request at Layer 7.

**ภาษาไทย:**

- **หน้าที่**: เลเยอร์ Application เป็นเลเยอร์ที่อยู่ใกล้ผู้ใช้ที่สุด ให้บริการเครือข่ายโดยตรงแก่แอปพลิเคชัน เช่น เว็บเบราว์เซอร์ ไคลเอนต์อีเมล และโปรแกรมถ่ายโอนไฟล์ นี่คือจุดที่ผู้ใช้โต้ตอบกับเครือข่าย

- **โปรโตคอลหลัก**:
  - **HTTP/HTTPS**: การเรียกดูเว็บ (พอร์ต 80, 443)
  - **FTP/SFTP**: การถ่ายโอนไฟล์ (พอร์ต 20, 21, 22)
  - **SMTP**: การส่งอีเมล (พอร์ต 25)
  - **DNS**: การแก้ไขชื่อโดเมน (พอร์ต 53)
  - **SSH**: การเข้าถึง shell อย่างปลอดภัย (พอร์ต 22)

- **อุปกรณ์ทั่วไป**: ไฟร์วอลล์ระดับแอปพลิเคชัน, API gateways, Layer 7 load balancers

- **ความเกี่ยวข้องกับ DevOps**: API gateways, ingress controllers, และ service meshes (เช่น Istio) ทำงานที่เลเยอร์นี้ การเข้าใจ HTTP methods, status codes, และ headers เป็นสิ่งจำเป็น

- **ตัวอย่างจริง**: เมื่อคุณพิมพ์ `https://example.com` ในเบราว์เซอร์ เบราว์เซอร์จะสร้าง HTTP GET request ที่เลเยอร์ 7

---

### Layer 6: Presentation Layer / เลเยอร์ 6: เลเยอร์การนำเสนอ

**English:**

- **Function**: The Presentation layer is responsible for data translation, encryption/decryption, and compression. It ensures that data sent from one system's application layer is readable by another system's application layer. Think of it as the "translator" of the network stack.

- **Key Responsibilities**:
  - **Data Translation**: Converting between different data formats (e.g., EBCDIC to ASCII)
  - **Encryption/Decryption**: SSL/TLS encryption for HTTPS, SSH encryption
  - **Compression**: Reducing data size for transmission (gzip, deflate)
  - **Serialization**: Converting objects to bytes for transmission (JSON, XML, Protocol Buffers)

- **Common Protocols/Formats**: SSL/TLS, JPEG, MPEG, GIF, ASCII, EBCDIC

- **DevOps Relevance**: TLS termination (where HTTPS is decrypted to HTTP) is a critical concept. Load balancers and ingress controllers often handle TLS at this layer. Understanding encryption is vital for security compliance.

- **Real-World Example**: When you visit an HTTPS website, the Presentation layer encrypts your data using TLS before it leaves your machine and decrypts incoming data.

**ภาษาไทย:**

- **หน้าที่**: เลเยอร์ Presentation รับผิดชอบการแปลข้อมูล การเข้ารหัส/ถอดรหัส และการบีบอัดข้อมูล รับรองว่าข้อมูลที่ส่งจาก application layer ของระบบหนึ่งสามารถอ่านได้อีกโดย system อีก Think of it as the "translator" of the network stack.

- **หน้าที่หลัก**:
  - **การแปลข้อมูล**: การแปลงระหว่างรูปแบบข้อมูลต่างๆ (เช่น EBCDIC เป็น ASCII)
  - **การเข้ารหัส/ถอดรหัส**: การเข้ารหัส SSL/TLS สำหรับ HTTPS, การเข้ารหัส SSH
  - **การบีบอัด**: ลดขนาดข้อมูลสำหรับการส่ง (gzip, deflate)
  - **การ serialize**: การแปลง objects เป็น bytes สำหรับการส่ง (JSON, XML, Protocol Buffers)

- **โปรโตคอล/รูปแบบทั่วไป**: SSL/TLS, JPEG, MPEG, GIF, ASCII, EBCDIC

- **ความเกี่ยวข้องกับ DevOps**: TLS termination (ที่ที่ HTTPS ถูกถอดรหัสเป็น HTTP) เป็นแนวคิดสำคัญ Load balancers และ ingress controllers มักจัดการ TLS ที่เลเยอร์นี้ การเข้าใจการเข้ารหัสสำคัญมากสำหรับการปฏิบัติตามความปลอดภัย

- **ตัวอย่างจริง**: เมื่อคุณเยี่ยมชมเว็บไซต์ HTTPS เลเยอร์ Presentation จะเข้ารหัสข้อมูลของคุณโดยใช้ TLS ก่อนออกจากเครื่องของคุณและถอดรหัสข้อมูลขาเข้า

---

### Layer 5: Session Layer / เลเยอร์ 5: เลเยอร์เซสชัน

**English:**

- **Function**: The Session layer establishes, manages, and terminates communication sessions between applications. A "session" is a semi-permanent dialogue between two devices. This layer handles session checkpoints, recovery, and synchronization.

- **Key Responsibilities**:
  - **Session Establishment**: Creating a communication channel between two applications
  - **Session Maintenance**: Keeping the connection alive during communication
  - **Session Termination**: Gracefully closing the communication channel
  - **Checkpointing**: Saving session state so it can resume after interruption
  - **Dialog Control**: Managing who can transmit (half-duplex vs full-duplex)

- **Common Protocols**: NetBIOS, RPC (Remote Procedure Call), PPTP, SIP (Session Initiation Protocol)

- **DevOps Relevance**: Understanding sessions is crucial for stateful applications, WebSocket connections, and long-lived API connections. Session persistence (sticky sessions) in load balancing is a Layer 5 concept.

- **Real-World Example**: When you log into a web application, the session layer maintains your login state. If your connection drops briefly, session checkpointing may allow you to resume without re-authenticating.

**ภาษาไทย:**

- **หน้าที่**: เลเยอร์ Session สร้าง จัดการ และยุติเซสชันการสื่อสารระหว่างแอปพลิเคชัน "เซสชัน" คือการสนทนากึ่งถาวรระหว่างสองอุปกรณ์ เลเยอร์นี้จัดการ checkpoints การกู้คืน และการซิงโครไนซ์ของเซสชัน

- **หน้าที่หลัก**:
  - **การสร้างเซสชัน**: สร้างช่องทางการสื่อสารระหว่างสองแอปพลิเคชัน
  - **การบำรุงรักษาเซสชัน**: รักษาการเชื่อมต่อให้คงอยู่ระหว่างการสื่อสาร
  - **การยุติเซสชัน**: ปิดช่องทางการสื่อสารอย่างเรียบร้อย
  - **Checkpointing**: บันทึกสถานะเซสชันเพื่อให้สามารถกลับมาหลังจากถูกขัดจังหวะ
  - **การควบคุมการสนทนา**: จัดการว่าใครสามารถส่งได้ (half-duplex เทียบกับ full-duplex)

- **โปรโตคอลทั่วไป**: NetBIOS, RPC (Remote Procedure Call), PPTP, SIP (Session Initiation Protocol)

- **ความเกี่ยวข้องกับ DevOps**: การเข้าใจเซสชันสำคัญสำหรับแอปพลิเคชันที่มีสถานะ การเชื่อมต่อ WebSocket และการเชื่อมต่อ API ที่ใช้งานนาน Session persistence (sticky sessions) ใน load balancing เป็นแนวคิดเลเยอร์ 5

- **ตัวอย่างจริง**: เมื่อคุณเข้าสู่เว็บแอปพลิเคชัน เลเยอร์ session จะรักษาสถานะการเข้าสู่ระบบของคุณ หากการเชื่อมต่อขาดหายไปชั่วครู่ การ checkpoint ของเซสชันอาจทำให้คุณสามารถกลับมาได้โดยไม่ต้องเข้าสู่ระบบใหม่

---

### Layer 4: Transport Layer / เลเยอร์ 4: เลเยอร์การขนส่ง

**English:**

- **Function**: The Transport layer provides end-to-end communication services for applications. It is responsible for reliable data transfer, flow control, error correction, and segmentation of data. This layer introduces the concept of **ports**, which allow multiple services to run on a single IP address.

- **Key Protocols**:
  - **TCP (Transmission Control Protocol)**: Connection-oriented, reliable, guaranteed delivery with error recovery. Used for web traffic, email, file transfers.
  - **UDP (User Datagram Protocol)**: Connectionless, faster, no delivery guarantee. Used for streaming, DNS queries, VoIP.

- **Key Concepts**:
  - **Ports**: Numbered endpoints (0-65535) that identify specific services (e.g., port 80 = HTTP, port 443 = HTTPS)
  - **Segmentation**: Breaking large data into smaller segments for transmission
  - **Flow Control**: Managing data rate to prevent overwhelming the receiver
  - **Error Recovery**: Retransmitting lost or corrupted data (TCP only)
  - **Multiplexing**: Allowing multiple simultaneous connections on one host

- **Common Devices**: Layer 4 firewalls, Layer 4 load balancers

- **DevOps Relevance**: Kubernetes Services operate at Layer 4. Understanding TCP vs UDP is critical for choosing the right protocol for your services. Port mapping, NAT, and connection tracking all happen at this layer.

- **Real-World Example**: When your browser connects to a web server, TCP establishes a reliable connection on port 443. The Transport layer ensures all data arrives correctly and in order.

**ภาษาไทย:**

- **หน้าที่**: เลเยอร์ Transport ให้บริการสื่อสารปลายต่อปลายสำหรับแอปพลิเคชัน รับผิดชอบการถ่ายโอนข้อมูลที่เชื่อถือได้ การควบคุมการไหล การแก้ไขข้อผิดพลาด และการแบ่งข้อมูล เลเยอร์นี้แนะนำแนวคิดของ **พอร์ต** ซึ่งทำให้บริการหลายรายการสามารถทำงานบนที่อยู่ IP เดียวได้

- **โปรโตคอลหลัก**:
  - **TCP (Transmission Control Protocol)**: เชื่อมต่อได้ เชื่อถือได้ รับรองการส่งพร้อมการกู้คืนข้อผิดพลาด ใช้สำหรับการรับส่งข้อมูลเว็บ อีเมล การถ่ายโอนไฟล์
  - **UDP (User Datagram Protocol)**: ไม่มีการเชื่อมต่อ เร็วกว่า ไม่มีการรับรองการส่ง ใช้สำหรับการสตรีม คำค้นหา DNS VoIP

- **แนวคิดหลัก**:
  - **พอร์ต**: จุดปลายทางที่มีหมายเลข (0-65535) ที่ระบุบริการเฉพาะ (เช่น พอร์ต 80 = HTTP, พอร์ต 443 = HTTPS)
  - **การแบ่งส่วน**: แบ่งข้อมูลขนาดใหญ่เป็นส่วนย่อยๆ สำหรับการส่ง
  - **การควบคุมการไหล**: จัดการอัตราการข้อมูลเพื่อป้องกันไม่ให้ผู้รับรับข้อมูลมากเกินไป
  - **การกู้คืนข้อผิดพลาด**: ส่งข้อมูล丢失หรือเสียหายซ้ำ (เฉพาะ TCP)
  - **Multiplexing**: อนุญาตการเชื่อมต่อพร้อมกันหลายรายการบนโฮสต์เดียว

- **อุปกรณ์ทั่วไป**: ไฟร์วอลล์เลเยอร์ 4, Load balancers เลเยอร์ 4

- **ความเกี่ยวข้องกับ DevOps**: Kubernetes Services ทำงานที่เลเยอร์ 4 การเข้าใจ TCP เทียบกับ UDP สำคัญสำหรับการเลือกโปรโตคอลที่เหมาะสมสำหรับบริการของคุณ การทำแผนที่พอร์ต NAT และการติดตามการเชื่อมต่อทั้งหมดเกิดขึ้นที่เลเยอร์นี้

- **ตัวอย่างจริง**: เมื่อเบราว์เซอร์ของคุณเชื่อมต่อกับเว็บเซิร์ฟเวอร์ TCP จะสร้างการเชื่อมต่อที่เชื่อถือได้บนพอร์ต 443 เลเยอร์ Transport รับรองว่าข้อมูลทั้งหมดมาถึงอย่างถูกต้องและตามลำดับ

---

### Layer 3: Network Layer / เลเยอร์ 3: เลเยอร์เครือข่าย

**English:**

- **Function**: The Network layer handles logical addressing and routing of data packets across multiple networks. It determines the best path for data to travel from source to destination, potentially through many intermediate routers. This layer introduces **IP addresses**.

- **Key Protocols**:
  - **IP (Internet Protocol)**: IPv4 (e.g., 192.168.1.1) and IPv6 (e.g., 2001:db8::1)
  - **ICMP (Internet Control Message Protocol)**: Used for diagnostics (ping, traceroute)
  - **IPSec**: Secure IP communication (VPN tunnels)
  - **ARP (Address Resolution Protocol)**: Maps IP addresses to MAC addresses
  - **OSPF, BGP**: Routing protocols for dynamic path selection

- **Key Concepts**:
  - **IP Addressing**: Logical addresses assigned to network interfaces
  - **Subnetting**: Dividing networks into smaller segments
  - **Routing**: Determining the path packets take across networks
  - **Fragmentation**: Splitting packets that are too large for the underlying network (MTU)
  - **TTL (Time To Live)**: Prevents packets from circulating forever

- **Common Devices**: Routers, Layer 3 switches

- **DevOps Relevance**: VPCs, subnets, route tables, NAT gateways, and VPC peering in cloud providers all operate at Layer 3. Kubernetes pod networking and CNI plugins deal with IP allocation and routing at this layer.

- **Real-World Example**: When you send data from your laptop to a server in another country, Layer 3 determines which routers the packets pass through to reach their destination.

**ภาษาไทย:**

- **หน้าที่**: เลเยอร์ Network จัดการที่อยู่เชิงตรรกะและการ routing แพ็กเก็ตข้อมูลข้ามเครือข่ายหลายแห่ง กำหนดเส้นทางที่ดีที่สุดให้ข้อมูลเดินทางจากต้นทางไปยังปลายทาง อาจผ่านเราเตอร์กลางหลายตัว เลเยอร์นี้แนะนำ **ที่อยู่ IP**

- **โปรโตคอลหลัก**:
  - **IP (Internet Protocol)**: IPv4 (เช่น 192.168.1.1) และ IPv6 (เช่น 2001:db8::1)
  - **ICMP (Internet Control Message Protocol)**: ใช้สำหรับการวินิจฉัย (ping, traceroute)
  - **IPSec**: การสื่อสาร IP อย่างปลอดภัย (อุโมงค์ VPN)
  - **ARP (Address Resolution Protocol)**: แผนที่ที่อยู่ IP เป็นที่อยู่ MAC
  - **OSPF, BGP**: โปรโตคอล routing สำหรับการเลือกเส้นทางแบบไดนามิก

- **แนวคิดหลัก**:
  - **การกำหนดที่อยู่ IP**: ที่อยู่เชิงตรรกะที่กำหนดให้กับอินเทอร์เฟซเครือข่าย
  - **Subnetting**: การแบ่งเครือข่ายออกเป็นส่วนย่อยๆ
  - **Routing**: กำหนดเส้นทางที่แพ็กเก็ตใช้ข้ามเครือข่าย
  - **Fragmentation**: การแบ่งแพ็กเก็ตที่ใหญ่เกินไปสำหรับเครือข่ายพื้นฐาน (MTU)
  - **TTL (Time To Live)**: ป้องกันแพ็กเก็ตวนเวียนตลอดไป

- **อุปกรณ์ทั่วไป**: เราเตอร์, สวิตช์เลเยอร์ 3

- **ความเกี่ยวข้องกับ DevOps**: VPCs, subnets, route tables, NAT gateways, และ VPC peering ในผู้ให้บริการคลาวด์ทั้งหมดทำงานที่เลเยอร์ 3 การเครือข่าย Kubernetes pod และ CNI plugins จัดการกับการจัดสรร IP และ routing ที่เลเยอร์นี้

- **ตัวอย่างจริง**: เมื่อคุณส่งข้อมูลจากแล็ปท็อปของคุณไปยังเซิร์ฟเวอร์ในประเทศอื่น เลเยอร์ 3 กำหนดว่าเราเตอร์ใดที่แพ็กเก็ตจะผ่านเพื่อไปถึงปลายทาง

---

### Layer 2: Data Link Layer / เลเยอร์ 2: เลเยอร์เชื่อมโยงข้อมูล

**English:**

- **Function**: The Data Link layer handles node-to-node data transfer within the same network segment (LAN). It provides error detection and correction from the physical layer and manages access to the physical medium. This layer introduces **MAC addresses**.

- **Key Sublayers**:
  - **LLC (Logical Link Control)**: Manages flow control and error checking
  - **MAC (Media Access Control)**: Controls how devices gain access to the medium and permission to transmit

- **Key Protocols**:
  - **Ethernet (IEEE 802.3)**: Most common LAN technology
  - **VLAN (Virtual LAN)**: Logically segmenting a physical network
  - **STP (Spanning Tree Protocol)**: Prevents network loops
  - **PPP (Point-to-Point Protocol)**: Direct connection between two nodes

- **Key Concepts**:
  - **MAC Address**: Unique 48-bit hardware address (e.g., AA:BB:CC:DD:EE:FF)
  - **Frames**: Data units at Layer 2 (packets are Layer 3)
  - **Switching**: Forwarding frames based on MAC address tables
  - **VLAN Tagging**: Adding VLAN IDs to frames (802.1Q)
  - **CSMA/CD**: Carrier Sense Multiple Access with Collision Detection (Ethernet)

- **Common Devices**: Switches, bridges, network interface cards (NICs)

- **DevOps Relevance**: Container networking, bridge networks, and VLAN configurations operate at Layer 2. Understanding MAC addresses and switching is important for bare-metal deployments and network troubleshooting.

- **Real-World Example**: Within your office LAN, when Computer A sends data to Computer B, the switch uses MAC addresses to deliver frames directly to the correct port.

**ภาษาไทย:**

- **หน้าที่**: เลเยอร์ Data Link จัดการการถ่ายโอนข้อมูลระหว่างโหนดภายในส่วนเครือข่ายเดียวกัน (LAN) ให้การตรวจจับและแก้ไขข้อผิดพลาดจากเลเยอร์กายภาพและจัดการการเข้าถึงสื่อทางกายภาพ เลเยอร์นี้แนะนำ **ที่อยู่ MAC**

- **Sublayers หลัก**:
  - **LLC (Logical Link Control)**: จัดการการควบคุมการไหลและการตรวจสอบข้อผิดพลาด
  - **MAC (Media Access Control)**: ควบคุมวิธีการที่อุปกรณ์เข้าถึงสื่อและอนุญาตให้ส่ง

- **โปรโตคอลหลัก**:
  - **Ethernet (IEEE 802.3)**: เทคโนโลยี LAN ที่พบบ่อยที่สุด
  - **VLAN (Virtual LAN)**: การแบ่งส่วนเครือข่ายทางกายภาพอย่างมีตรรกะ
  - **STP (Spanning Tree Protocol)**: ป้องกันลูปเครือข่าย
  - **PPP (Point-to-Point Protocol)**: การเชื่อมต่อโดยตรงระหว่างสองโหนด

- **แนวคิดหลัก**:
  - **ที่อยู่ MAC**: ที่อยู่ฮาร์ดแวร์ 48-bit ที่ไม่ซ้ำกัน (เช่น AA:BB:CC:DD:EE:FF)
  - **เฟรม**: หน่วยข้อมูลที่เลเยอร์ 2 (แพ็กเก็ตคือเลเยอร์ 3)
  - **Switching**: การส่งต่อเฟรมตามตารางที่อยู่ MAC
  - **VLAN Tagging**: การเพิ่ม VLAN IDs ให้กับเฟรม (802.1Q)
  - **CSMA/CD**: Carrier Sense Multiple Access with Collision Detection (Ethernet)

- **อุปกรณ์ทั่วไป**: สวิตช์, บริดจ์, การ์ดอินเทอร์เฟซเครือข่าย (NICs)

- **ความเกี่ยวข้องกับ DevOps**: การเครือข่ายคอนเทนเนอร์ เครือข่ายบริดจ์ และการกำหนดค่า VLAN ทำงานที่เลเยอร์ 2 การเข้าใจที่อยู่ MAC และ switching สำคัญสำหรับการ deploy แบบ bare-metal และการแก้ปัญหาเครือข่าย

- **ตัวอย่างจริง**: ภายใน LAN สำนักงานของคุณ เมื่อคอมพิวเตอร์ A ส่งข้อมูลไปยังคอมพิวเตอร์ B สวิตช์ใช้ที่อยู่ MAC เพื่อส่งเฟรมไปยังพอร์ตที่ถูกต้องโดยตรง

---

### Layer 1: Physical Layer / เลเยอร์ 1: เลเยอร์กายภาพ

**English:**

- **Function**: The Physical layer deals with the actual physical transmission of raw bit streams (0s and 1s) over a communication medium. It defines electrical, mechanical, procedural, and functional specifications for activating, maintaining, and deactivating physical links.

- **Key Specifications**:
  - **Voltage Levels**: What voltage represents a 0 vs a 1
  - **Timing**: How fast bits are transmitted (bandwidth, data rate)
  - **Physical Connectors**: RJ-45, fiber optic connectors, USB
  - **Transmission Media**: Copper cables, fiber optics, radio waves
  - **Topologies**: Star, bus, ring, mesh physical layouts

- **Key Standards**:
  - **Ethernet Cables**: Cat5e, Cat6, Cat6a, Cat7 (twisted pair)
  - **Fiber Optics**: Single-mode, multi-mode fiber
  - **Wireless**: IEEE 802.11 (Wi-Fi) standards
  - **Serial**: RS-232, USB

- **Key Concepts**:
  - **Bandwidth**: Maximum data transfer rate of the medium
  - **Duplex**: Half-duplex (one direction at a time) vs Full-duplex (both directions simultaneously)
  - **Signal Encoding**: How bits are represented as electrical/optical signals
  - **Attenuation**: Signal degradation over distance
  - **Crosstalk**: Interference between adjacent cables

- **Common Devices**: Hubs, repeaters, cables, transceivers, patch panels

- **DevOps Relevance**: While DevOps engineers rarely deal with physical cables directly, understanding Layer 1 is crucial for troubleshooting connectivity issues in data centers, bare-metal servers, and on-premises infrastructure.

- **Real-World Example**: The electrical pulses traveling through a Cat6 Ethernet cable from your laptop to the wall jack operate at Layer 1.

**ภาษาไทย:**

- **หน้าที่**: เลเยอร์ Physical เกี่ยวข้องกับการส่งสัญญาณบิตดิบ (0 และ 1) จริงผ่านสื่อการสื่อสาร กำหนดข้อกำหนดทางไฟฟ้า กลไก ขั้นตอน และหน้าที่สำหรับการเปิดใช้งาน บำรุงรักษา และปิดการใช้งานลิงก์ทางกายภาพ

- **ข้อกำหนดหลัก**:
  - **ระดับแรงดันไฟฟ้า**: แรงดันไฟฟ้าใดแทน 0 เทียบกับ 1
  - **เวลา**: บิตถูกส่งเร็วแค่ไหน (แบนด์วิธ อัตราข้อมูล)
  - **ขั้วต่อทางกายภาพ**: RJ-45, ขั้วต่อใยแก้วนำแสง, USB
  - **สื่อส่งสัญญาณ**: สายทองแดง ใยแก้วนำแสง คลื่นวิทยุ
  - **โทโพโลยี**: การจัดวางทางกายภาพแบบ Star, bus, ring, mesh

- **มาตรฐานหลัก**:
  - **สาย Ethernet**: Cat5e, Cat6, Cat6a, Cat7 (twisted pair)
  - **ใยแก้วนำแสง**: Single-mode, multi-mode fiber
  - **ไร้สาย**: มาตรฐาน IEEE 802.11 (Wi-Fi)
  - **Serial**: RS-232, USB

- **แนวคิดหลัก**:
  - **แบนด์วิธ**: อัตราการถ่ายโอนข้อมูลสูงสุดของสื่อ
  - **Duplex**: Half-duplex (ครั้งละทิศทางเดียว) เทียบกับ Full-duplex (ทั้งสองทิศทางพร้อมกัน)
  - **การเข้ารหัสสัญญาณ**: บิตถูกแทนเป็นสัญญาณไฟฟ้า/แสงอย่างไร
  - **การลดทอน**: การลดทอนสัญญาณตามระยะทาง
  - **Crosstalk**: การรบกวนระหว่างสายที่อยู่ติดกัน

- **อุปกรณ์ทั่วไป**: ฮับ, รีพีตเตอร์, สายเคเบิล, ทรานซีฟเวอร์, แผงแพทช์

- **ความเกี่ยวข้องกับ DevOps**: แม้วิศวกร DevOps จะไม่ค่อยจัดการสายเคเบิลโดยตรง แต่การเข้าใจเลเยอร์ 1 สำคัญสำหรับการแก้ปัญหาการเชื่อมต่อในศูนย์ข้อมูล เซิร์ฟเวอร์ bare-metal และโครงสร้างพื้นฐานภายในองค์กร

- **ตัวอย่างจริง**: พัลส์ไฟฟ้าที่เดินทางผ่านสาย Cat6 Ethernet จากแล็ปท็อปของคุณไปยังแจ็คผนังทำงานที่เลเยอร์ 1

---

### Complete Layer Summary Table / ตารางสรุปเลเยอร์ทั้งหมด

| Layer / เลเยอร์ | Name / ชื่อ | PDU / หน่วยข้อมูล | Addressing / การอยู่ | Protocols / โปรโตคอล | Devices / อุปกรณ์ |
|:---:|:---|:---|:---|:---|:---|
| 7 | Application / แอปพลิเคชัน | Data / ข้อมูล | URLs, URIs | HTTP, FTP, DNS, SMTP | Firewalls (L7), API Gateways |
| 6 | Presentation / การนำเสนอ | Data / ข้อมูล | N/A | SSL/TLS, JPEG, MPEG | Encryption devices |
| 5 | Session / เซสชัน | Data / ข้อมูล | Session IDs | NetBIOS, RPC, SIP | Session controllers |
| 4 | Transport / การขนส่ง | Segment / เซกเมนต์ | Port Numbers | TCP, UDP | L4 Firewalls, L4 LBs |
| 3 | Network / เครือข่าย | Packet / แพ็กเก็ต | IP Addresses | IP, ICMP, IPSec, BGP | Routers, L3 Switches |
| 2 | Data Link / เชื่อมโยงข้อมูล | Frame / เฟรม | MAC Addresses | Ethernet, VLAN, PPP | Switches, Bridges |
| 1 | Physical / กายภาพ | Bits / บิต | N/A | N/A (signals) | Hubs, Repeaters, Cables |

---

## Data Encapsulation and Decapsulation / การห่อหุ้มข้อมูลและการแยกห่อหุ้ม

**English:**
When data is sent from one computer to another, it travels **down** the OSI layers on the sender's side (encapsulation) and **up** the layers on the receiver's side (decapsulation). At each layer, a header is added containing control information.

**Encapsulation Process (Sender) / กระบวนการ Encapsulation (ผู้ส่ง):**
```
1. L7: User data (e.g., HTTP request)
       ↓ + L7 header
2. L6: Encrypted/compressed data
       ↓ + L6 header
3. L5: Session-managed data
       ↓ + L5 header
4. L4: TCP segment (+ port numbers)
       ↓ + L4 header
5. L3: IP packet (+ source & destination IP)
       ↓ + L3 header
6. L2: Ethernet frame (+ MAC addresses)
       ↓ + L2 header
7. L1: Raw bits transmitted over the medium
```

**Decapsulation Process (Receiver) / กระบวนการ Decapsulation (ผู้รับ):**
```
1. L1: Receives raw bits
2. L2: Strips Ethernet frame header, checks MAC
3. L3: Strips IP header, checks destination IP
4. L4: Strips TCP/UDP header, checks port
5. L5-L6: Session/Presentation processing
6. L7: Application receives the original data
```

**ภาษาไทย:**
เมื่อข้อมูลถูกส่งจากคอมพิวเตอร์เครื่องหนึ่งไปยังอีกเครื่องหนึ่ง ข้อมูลจะเดินทาง **ลง** เลเยอร์ OSI ด้านผู้ส่ง (encapsulation) และ **ขึ้น** ผ่านเลเยอร์ด้านผู้รับ (decapsulation) ที่แต่ละเลเยอร์ จะมีการเพิ่ม header ที่มีข้อมูลควบคุม

**กระบวนการ Encapsulation (ผู้ส่ง):**
```
1. L7: ข้อมูลผู้ใช้ (เช่น HTTP request)
       ↓ + header L7
2. L6: ข้อมูลเข้ารหัส/บีบอัด
       ↓ + header L6
3. L5: ข้อมูลจัดการเซสชัน
       ↓ + header L5
4. L4: TCP segment (+ หมายเลขพอร์ต)
       ↓ + header L4
5. L3: IP packet (+ IP ต้นทางและปลายทาง)
       ↓ + header L3
6. L2: Ethernet frame (+ ที่อยู่ MAC)
       ↓ + header L2
7. L1: บิตดิบถูกส่งผ่านสื่อ
```

**กระบวนการ Decapsulation (ผู้รับ):**
```
1. L1: รับบิตดิบ
2. L2: ลบ header Ethernet frame, ตรวจสอบ MAC
3. L3: ลบ header IP, ตรวจสอบ IP ปลายทาง
4. L4: ลบ header TCP/UDP, ตรวจสอบพอร์ต
5. L5-L6: การประมวลผล Session/Presentation
6. L7: แอปพลิเคชันรับข้อมูลดั้งเดิม
```

---

## Real-World Example: Accessing a Website / ตัวอย่างจริง: การเข้าถึงเว็บไซต์

**English:**
Let's trace what happens when you type `https://example.com` in your browser and press Enter. Every single step maps to a specific OSI layer.

**ภาษาไทย:**
มาดูกันว่าเกิดอะไรขึ้นเมื่อคุณพิมพ์ `https://example.com` ในเบราว์เซอร์แล้วกด Enter ทุกขั้นตอนเชื่อมโยงกับเลเยอร์ OSI เฉพาะ

---

### Step-by-Step Breakdown / การแบ่งตามขั้นตอน

**Step 1: DNS Resolution (Layer 7 & Layer 3) / ขั้นตอนที่ 1: การแก้ไข DNS (เลเยอร์ 7 และ เลเยอร์ 3)**

**English:**
Your browser needs to convert `example.com` into an IP address. It sends a DNS query (Layer 7 application) via UDP port 53 (Layer 4) to a DNS server. The DNS server's IP is reached through IP routing (Layer 3).

**ภาษาไทย:**
เบราว์เซอร์ของคุณต้องแปลง `example.com` เป็นที่อยู่ IP ส่งคำค้นหา DNS (แอปพลิเคชันเลเยอร์ 7) ผ่าน UDP พอร์ต 53 (เลเยอร์ 4) ไปยังเซิร์ฟเวอร์ DNS IP ของเซิร์ฟเวอร์ DNS ถูกเข้าถึงผ่านการ routing IP (เลเยอร์ 3)

---

**Step 2: TCP Connection Setup (Layer 4) / ขั้นตอนที่ 2: การตั้งค่าการเชื่อมต่อ TCP (เลเยอร์ 4)**

**English:**
Once the IP address is known, your browser initiates a TCP 3-way handshake with the server on port 443 (HTTPS). This creates a reliable connection at the Transport layer.

**ภาษาไทย:**
เมื่อทราบที่อยู่ IP แล้ว เบราว์เซอร์ของคุณจะเริ่ม TCP 3-way handshake กับเซิร์ฟเวอร์บนพอร์ต 443 (HTTPS) นี่สร้างการเชื่อมต่อที่เชื่อถือได้ที่เลเยอร์ Transport

---

**Step 3: TLS Handshake (Layer 6) / ขั้นตอนที่ 3: การจับมือ TLS (เลเยอร์ 6)**

**English:**
Before sending application data, a TLS handshake occurs. The client and server negotiate encryption algorithms, exchange certificates, and establish encryption keys. This happens at the Presentation layer.

**ภาษาไทย:**
ก่อนส่งข้อมูลแอปพลิเคชัน เกิดการจับมือ TLS ไคลเอนต์และเซิร์ฟเวอร์เจรจาอัลกอริทึมการเข้ารหัส แลกเปลี่ยนใบรับรอง และสร้างคีย์การเข้ารหัส นี่เกิดขึ้นที่เลเยอร์ Presentation

---

**Step 4: HTTP Request (Layer 7) / ขั้นตอนที่ 4: คำขอ HTTP (เลเยอร์ 7)**

**English:**
Your browser sends an HTTP GET request over the encrypted TLS connection. The request includes headers (User-Agent, Accept, Cookies) and targets a specific URL path.

**ภาษาไทย:**
เบราว์เซอร์ของคุณส่งคำขอ HTTP GET ผ่านการเชื่อมต่อ TLS ที่เข้ารหัสแล้ว คำขอรวมถึง headers (User-Agent, Accept, Cookies) และกำหนดเป้าหมาย URL path เฉพาะ

---

**Step 5: IP Routing (Layer 3) / ขั้นตอนที่ 5: การ Routing IP (เลเยอร์ 3)**

**English:**
The data is broken into packets, each with source and destination IP addresses. Routers along the path examine the destination IP and forward packets toward the server using routing tables.

**ภาษาไทย:**
ข้อมูลถูกแบ่งเป็นแพ็กเก็ต แต่ละแพ็กเก็ตมีที่อยู่ IP ต้นทางและปลายทาง เราเตอร์ตามเส้นทางตรวจสอบ IP ปลายทางและส่งต่อแพ็กเก็ตไปยังเซิร์ฟเวอร์โดยใช้ตาราง routing

---

**Step 6: Ethernet Delivery (Layer 2) / ขั้นตอนที่ 6: การส่งผ่าน Ethernet (เลเยอร์ 2)**

**English:**
At each hop (router-to-router, router-to-server), the packets are encapsulated in Ethernet frames with source and destination MAC addresses. Switches forward these frames within each LAN segment.

**ภาษาไทย:**
ที่แต่ละ hop (เราเตอร์ถึงเราเตอร์, เราเตอร์ถึงเซิร์ฟเวอร์) แพ็กเก็ตถูกห่อหุ้มในเฟรม Ethernet พร้อมที่อยู่ MAC ต้นทางและปลายทาง สวิตช์ส่งต่อเฟรมเหล่านี้ภายในแต่ละส่วน LAN

---

**Step 7: Physical Transmission (Layer 1) / ขั้นตอนที่ 7: การส่งสัญญาณทางกายภาพ (เลเยอร์ 1)**

**English:**
Finally, the frames are converted to electrical signals (copper), light pulses (fiber), or radio waves (Wi-Fi) and transmitted across the physical medium to the next device.

**ภาษาไทย:**
สุดท้าย เฟรมถูกแปลงเป็นสัญญาณไฟฟ้า (ทองแดง) พัลส์แสง (ใยแก้ว) หรือคลื่นวิทยุ (Wi-Fi) และส่งผ่านสื่อทางกายภาพไปยังอุปกรณ์ถัดไป

---

### Visual Flow Plan / แผนภาพการไหล

```
Your Browser                              example.com Server
=============                              ==================
   |                                              |
   |  L7: HTTP GET /                              |
   |  L6: TLS Encrypt                             |
   |  L5: Session ID                              |
   |  L4: TCP Segment (src_port:54321 → dst:443)  |
   |  L3: IP Packet (src:192.168.1.10 → dst:93.184.216.34)
   |  L2: Ethernet Frame (src:AA:BB:... → dst:Router MAC)
   |  L1: Electrical Signals =======================> |
   |                                              |
   |  <============================================= |
   |  L1-L2-L3-L4: Reverse process                  |
   |  L6: TLS Decrypt                               |
   |  L7: HTTP 200 OK + HTML Content                |
   |                                              |
```

---

## TCP vs UDP Comparison / เปรียบเทียบ TCP กับ UDP

**English:**
TCP and UDP are the two primary protocols at the Transport Layer (Layer 4). Understanding their differences is crucial for DevOps engineers designing and troubleshooting networked applications.

**ภาษาไทย:**
TCP และ UDP เป็นโปรโตคอลหลักสองตัวที่เลเยอร์ Transport (เลเยอร์ 4) การเข้าใจความแตกต่างเป็นสิ่งสำคัญสำหรับวิศวกร DevOps ที่ออกแบบและแก้ปัญหาแอปพลิเคชันเครือข่าย

---

### Detailed Comparison Table / ตารางเปรียบเทียบโดยละเอียด

| Feature / คุณสมบัติ | TCP | UDP |
|:---|:---|:---|
| **Full Name / ชื่อเต็ม** | Transmission Control Protocol | User Datagram Protocol |
| **Connection Type / ประเภทการเชื่อมต่อ** | Connection-oriented (requires handshake) / เชื่อมต่อได้ (ต้องจับมือ) | Connectionless (fire and forget) / ไม่มีการเชื่อมต่อ (ส่งแล้วลืม) |
| **Reliability / ความน่าเชื่อถือ** | Guaranteed delivery with acknowledgments / รับรองการส่งพร้อมการตอบรับ | No delivery guarantee / ไม่มีการรับรองการส่ง |
| **Ordering / การจัดลำดับ** | Packets reassembled in correct order / แพ็กเก็ตประกอบใหม่ตามลำดับที่ถูกต้อง | No ordering guarantee / ไม่มีการจัดลำดับ |
| **Error Recovery / การกู้คืนข้อผิดพลาด** | Retransmits lost/corrupted packets / ส่งแพ็กเก็ต丢失หรือเสียหายซ้ำ | No error recovery / ไม่มีการกู้คืนข้อผิดพลาด |
| **Flow Control / การควบคุมการไหล** | Yes (sliding window) / ใช่ (sliding window) | No / ไม่ใช่ |
| **Congestion Control / การควบคุมความแออัด** | Yes (slows down when network is congested) / ใช่ (ช้าลงเมื่อเครือข่ายแออัด) | No / ไม่ใช่ |
| **Header Size / ขนาด Header** | 20-60 bytes / 20-60 ไบต์ | 8 bytes / 8 ไบต์ |
| **Speed / ความเร็ว** | Slower due to overhead / ช้ากว่าเนื่องจาก overhead | Faster, minimal overhead / เร็วกว่า, overhead น้อย |
| **Use Cases / กรณีใช้** | Web (HTTP/HTTPS), Email (SMTP), File Transfer (FTP, SFTP), Database connections / เว็บ อีเมล ถ่ายโอนไฟล์ การเชื่อมต่อฐานข้อมูล | Streaming (video/audio), DNS queries, VoIP, Online gaming, Monitoring (metrics) / สตรีมมิ่ง คำค้นหา DNS VoIP เกมออนไลน์ การตรวจสอบ |
| **Examples in DevOps / ตัวอย่างใน DevOps** | API calls, database replication, SSH, container registry pulls / การเรียก API การจำลองฐานข้อมูล SSH การดึง container registry | Service mesh telemetry, DNS resolution, video streaming to CDN, syslog / การ telemetry ของ service mesh การแก้ไข DNS สตรีมมิ่งวิดีโอไปยัง CDN syslog |

---

### When to Use TCP / เมื่อใดควรใช้ TCP

**English:**
- When data integrity is critical (financial transactions, file transfers)
- When the complete message must arrive correctly (web pages, emails)
- When you need built-in flow and congestion control
- **Rule of thumb**: If you're unsure, use TCP.

**ภาษาไทย:**
- เมื่อความสมบูรณ์ของข้อมูลสำคัญ (ธุรกรรมทางการเงิน การถ่ายโอนไฟล์)
- เมื่อข้อความทั้งหมดต้องมาถึงอย่างถูกต้อง (เว็บเพจ อีเมล)
- เมื่อต้องการการควบคุมการไหลและความแออัดในตัว
- **กฎทั่วไป**: หากคุณไม่แน่ใจ ให้ใช้ TCP

---

### When to Use UDP / เมื่อใดควรใช้ UDP

**English:**
- When speed matters more than perfect delivery (live video, voice calls)
- When occasional data loss is acceptable (streaming can skip frames)
- For simple request/response patterns where the overhead of TCP is unnecessary (DNS)
- For broadcasting/multicasting (one-to-many communication)
- **Rule of thumb**: Use UDP when you can tolerate some loss and need maximum speed.

**ภาษาไทย:**
- เมื่อความเร็วมสำคัญกว่าการส่งที่สมบูรณ์แบบ (วิดีโอสด การโทรด้วยเสียง)
- เมื่อการสูญเสียข้อมูลบางครั้งยอมรับได้ (สตรีมมิ่งสามารถข้ามเฟรมได้)
- สำหรับรูปแบบคำขอ/ตอบกลับอย่างง่ายที่ overhead ของ TCP ไม่จำเป็น (DNS)
- สำหรับการ broadcasting/multicasting (การสื่อสารหนึ่งถึงหลาย)
- **กฎทั่วไป**: ใช้ UDP เมื่อคุณสามารถทนต่อการสูญเสียบางส่วนและต้องการความเร็วสูงสุด

---

## TCP 3-Way Handshake / การจับมือสามทางของ TCP

**English:**
The TCP 3-way handshake is the process used to establish a reliable TCP connection between a client and a server. It ensures both parties are ready to communicate and agree on initial sequence numbers.

**ภาษาไทย:**
การจับมือสามทางของ TCP เป็นกระบวนการที่ใช้สร้างการเชื่อมต่อ TCP ที่เชื่อถือได้ระหว่างไคลเอนต์และเซิร์ฟเวอร์ รับรองว่าทั้งสองฝ่ายพร้อมสื่อสารและตกลงหมายเลขลำดับเริ่มต้น

---

### The Three Steps / สามขั้นตอน

**Step 1: SYN (Synchronize) / ขั้นตอนที่ 1: SYN (ซิงโครไนซ์)**

**English:**
The client sends a TCP packet with the SYN (synchronize) flag set to the server. This packet includes the client's initial sequence number (ISN). It's like saying: "Hello server, I want to connect. My starting number is X."

**ภาษาไทย:**
ไคลเอนต์ส่งแพ็กเก็ต TCP ที่มีแฟลก SYN (synchronize) ไปยังเซิร์ฟเวอร์ แพ็กเก็ตนี้รวมถึงหมายเลขลำดับเริ่มต้น (ISN) ของไคลเอนต์ เหมือนกับการพูดว่า: "สวัสดีเซิร์ฟเวอร์ ฉันต้องการเชื่อมต่อ หมายเลขเริ่มต้นของฉันคือ X"

---

**Step 2: SYN-ACK (Synchronize-Acknowledge) / ขั้นตอนที่ 2: SYN-ACK (ซิงโครไนซ์-ตอบรับ)**

**English:**
The server responds with a TCP packet that has both SYN and ACK flags set. It acknowledges the client's sequence number (ACK = X + 1) and sends its own initial sequence number (ISN). It's like saying: "I hear you, client. I'm ready. My starting number is Y, and I acknowledge your number X."

**ภาษาไทย:**
เซิร์ฟเวอร์ตอบกลับด้วยแพ็กเก็ต TCP ที่มีทั้งแฟลก SYN และ ACK ตอบรับหมายเลขลำดับของไคลเอนต์ (ACK = X + 1) และส่งหมายเลขลำดับเริ่มต้นของตัวเอง (ISN) เหมือนกับการพูดว่า: "ฉันได้ยินแล้วไคลเอนต์ ฉันพร้อมแล้ว หมายเลขเริ่มต้นของฉันคือ Y และฉันรับทราบหมายเลข X ของคุณ"

---

**Step 3: ACK (Acknowledge) / ขั้นตอนที่ 3: ACK (ตอบรับ)**

**English:**
The client sends a final TCP packet with the ACK flag set, acknowledging the server's sequence number (ACK = Y + 1). At this point, the connection is established, and data transfer can begin. It's like saying: "Got it, server. Let's start communicating!"

**ภาษาไทย:**
ไคลเอนต์ส่งแพ็กเก็ต TCP สุดท้ายที่มีแฟลก ACK ตั้งค่าไว้ ตอบรับหมายเลขลำดับของเซิร์ฟเวอร์ (ACK = Y + 1) ที่จุดนี้ การเชื่อมต่อถูกสร้างขึ้น และการถ่ายโอนข้อมูลสามารถเริ่มต้นได้ เหมือนกับการพูดว่า: "เข้าใจแล้วเซิร์ฟเวอร์ เริ่มสื่อสารกันเลย!"

---

### Visual Diagram / แผนภาพ

```
        TCP 3-Way Handshake / การจับมือสามทางของ TCP
        ============================================

Client / ไคลเอนต์                           Server / เซิร์ฟเวอร์
      |                                          |
      |  SYN (seq=100)                          |
      |  "I want to connect"                    |
      |  "ฉันต้องการเชื่อมต่อ"                     |
      | ---------------------------------------> |
      |                                          |
      |  SYN-ACK (seq=300, ack=101)             |
      |  "I'm ready, here's my number"          |
      |  "ฉันพร้อมแล้ว นี่คือหมายเลขของฉัน"        |
      | <--------------------------------------- |
      |                                          |
      |  ACK (ack=301)                          |
      |  "Connection established!"              |
      |  "การเชื่อมต่อสร้างขึ้นแล้ว!"              |
      | ---------------------------------------> |
      |                                          |
      |  === DATA TRANSFER / ถ่ายโอนข้อมูล ===  |
      |                                          |
```

---

### Connection Termination (4-Way Handshake) / การยุติการเชื่อมต่อ (4-Way Handshake)

**English:**
Closing a TCP connection requires 4 steps (FIN, ACK, FIN, ACK) because TCP is full-duplex -- each direction must be closed independently.

**Step 1**: Client sends FIN (finish) to close its side of the connection.
**Step 2**: Server sends ACK acknowledging the FIN.
**Step 3**: Server sends FIN to close its side of the connection.
**Step 4**: Client sends ACK acknowledging the server's FIN.

**ภาษาไทย:**
การปิดการเชื่อมต่อ TCP ต้องการ 4 ขั้นตอน (FIN, ACK, FIN, ACK) เพราะ TCP เป็น full-duplex -- แต่ละทิศทางต้องถูกปิดแยกกัน

**ขั้นตอนที่ 1**: ไคลเอนต์ส่ง FIN (finish) เพื่อปิดด้านของตัวเอง
**ขั้นตอนที่ 2**: เซิร์ฟเวอร์ส่ง ACK ตอบรับ FIN
**ขั้นตอนที่ 3**: เซิร์ฟเวอร์ส่ง FIN เพื่อปิดด้านของตัวเอง
**ขั้นตอนที่ 4**: ไคลเอนต์ส่ง ACK ตอบรับ FIN ของเซิร์ฟเวอร์

---

## Network Devices by Layer / อุปกรณ์เครือข่ายตามเลเยอร์

**English:**
Different networking devices operate at different OSI layers. Understanding which layer a device works at helps you choose the right equipment and troubleshoot effectively.

**ภาษาไทย:**
อุปกรณ์เครือข่ายต่างกันทำงานที่เลเยอร์ OSI ต่างกัน การเข้าใจว่าอุปกรณ์ทำงานที่เลเยอร์ใดช่วยคุณเลือกอุปกรณ์ที่เหมาะสมและแก้ปัญหาได้อย่างมีประสิทธิภาพ

---

### Layer 1: Physical Layer Devices / อุปกรณ์เลเยอร์ 1: เลเยอร์กายภาพ

| Device / อุปกรณ์ | Description / คำอธิบาย |
|:---|:---|
| **Hub / ฮับ** | Broadcasts all incoming signals to all ports. No intelligence. Deprecated. / ส่งสัญญาณขาเข้าทั้งหมดไปยังทุกพอร์ต ไม่มี intelligence ล้าสมัยแล้ว |
| **Repeater / รีพีตเตอร์** | Amplifies and regenerates signals to extend cable length. / ขยายสัญญาณและสร้างใหม่เพื่อขยายความยาวสาย |
| **Cables / สายเคเบิล** | Cat5e, Cat6, fiber optic cables for physical transmission. / Cat5e, Cat6, สายใยแก้วนำแสงสำหรับการส่งสัญญาณทางกายภาพ |
| **Transceivers / ทรานซีฟเวอร์** | Convert electrical signals to optical and vice versa (SFP, SFP+). / แปลงสัญญาณไฟฟ้าเป็นแสงและกลับกัน (SFP, SFP+) |

---

### Layer 2: Data Link Layer Devices / อุปกรณ์เลเยอร์ 2: เลเยอร์เชื่อมโยงข้อมูล

| Device / อุปกรณ์ | Description / คำอธิบาย |
|:---|:---|
| **Switch / สวิตช์** | Forwards frames based on MAC address tables. Intelligent device that learns which MAC addresses are on which ports. / ส่งต่อเฟรมตามตารางที่อยู่ MAC อุปกรณ์อัจฉริยะที่เรียนรู้ว่าที่อยู่ MAC ใดอยู่บนพอร์ตใด |
| **Bridge / บริดจ์** | Connects two LAN segments and forwards frames between them. Similar to a switch but with fewer ports. / เชื่อมต่อสองส่วน LAN และส่งต่อเฟรมระหว่างพวกเขา คล้ายสวิตช์แต่มีพอร์ตน้อยกว่า |
| **NIC (Network Interface Card) / การ์ดอินเทอร์เฟซเครือข่าย** | Hardware that connects a computer to a network. Has a unique MAC address burned in. / ฮาร์ดแวร์ที่เชื่อมต่อคอมพิวเตอร์กับเครือข่าย มีที่อยู่ MAC ที่ไม่ซ้ำกันฝังอยู่ |
| **Access Point (Wireless) / จุดเข้าใช้งาน (ไร้สาย)** | Connects wireless devices to a wired LAN at Layer 2. / เชื่อมต่ออุปกรณ์ไร้สายกับ LAN แบบมีสายที่เลเยอร์ 2 |

---

### Layer 3: Network Layer Devices / อุปกรณ์เลเยอร์ 3: เลเยอร์เครือข่าย

| Device / อุปกรณ์ | Description / คำอธิบาย |
|:---|:---|
| **Router / เราเตอร์** | Routes packets between different networks using IP addresses. Makes decisions based on routing tables. / route แพ็กเก็ตระหว่างเครือข่ายต่างกันโดยใช้ที่อยู่ IP ตัดสินใจตามตาราง routing |
| **Layer 3 Switch / สวิตช์เลเยอร์ 3** | A switch that can also perform routing functions. Combines speed of switching with routing intelligence. / สวิตช์ที่สามารถทำหน้าที่ routing ได้ รวมความเร็วของการ switching กับ intelligence การ routing |
| **Firewall (Traditional) / ไฟร์วอลล์ (ดั้งเดิม)** | Filters packets based on IP addresses, ports, and protocols. Makes allow/deny decisions. / กรองแพ็กเก็ตตามที่อยู่ IP พอร์ต และโปรโตคอล ตัดสินใจอนุญาต/ปฏิเสธ |

---

### Layer 4-7: Upper Layer Devices / อุปกรณ์เลเยอร์ 4-7: เลเยอร์บน

| Device / อุปกรณ์ | Layer / เลเยอร์ | Description / คำอธิบาย |
|:---|:---:|:---|
| **L4 Load Balancer / L4 Load Balancer** | 4 | Distributes traffic based on IP and port only. Fast but limited intelligence. / แจกจ่ายการกระจายตาม IP และพอร์ตเท่านั้น เร็วแต่มี intelligence จำกัด |
| **L7 Load Balancer / L7 Load Balancer** | 7 | Inspects HTTP headers, URLs, cookies. Can route based on content. / ตรวจสอบ HTTP headers, URLs, cookies สามารถ route ตามเนื้อหา |
| **WAF (Web Application Firewall) / WAF** | 7 | Inspects application-layer traffic for attacks (SQL injection, XSS). / ตรวจสอบการจราจรเลเยอร์แอปพลิเคชันสำหรับการโจมตี (SQL injection, XSS) |
| **API Gateway / API Gateway** | 7 | Routes API requests, handles authentication, rate limiting, and transformation. / route คำขอ API จัดการการตรวจสอบสิทธิ์ จำกัดอัตรา และแปลง |
| **Proxy Server / Proxy Server** | 7 | Acts as an intermediary between clients and servers. Can cache content, filter requests. / ทำตัวเป็นสื่อกลางระหว่างไคลเอนต์และเซิร์ฟเวอร์ สามารถแคชเนื้อหา กรองคำขอ |

---

## Troubleshooting Commands / คำสั่งแก้ปัญหาเครือข่าย

**English:**
Here is a comprehensive list of network troubleshooting commands organized by OSI layer. Each command includes an explanation of what it does and when to use it.

**ภาษาไทย:**
นี่คือรายการคำสั่งแก้ปัญหาเครือข่ายที่ครอบคลุมจัดเรียงตามเลเยอร์ OSI แต่ละคำสั่งรวมถึงคำอธิบายว่าทำอะไรและเมื่อใดควรใช้

---

### Layer 1-2: Physical and Data Link / เลเยอร์ 1-2: กายภาพและเชื่อมโยงข้อมูล

```bash
# Check physical link status / ตรวจสอบสถานะลิงก์ทางกายภาพ
ip link show
# Shows all network interfaces with their state (UP/DOWN).
# แสดงอินเทอร์เฟซเครือข่ายทั้งหมดพร้อมสถานะ (UP/DOWN)
# Look for "state UP" or "state DOWN" to see if the cable is connected.
# มองหา "state UP" หรือ "state DOWN" เพื่อดูว่าสายเชื่อมต่อหรือไม่

# Check detailed interface information / ตรวจสอบข้อมูลอินเทอร์เฟซโดยละเอียด
ethtool eth0
# Shows link speed, duplex mode, auto-negotiation, and physical link status.
# แสดงความเร็วลิงก์ โหมด duplex การต่อรองอัตโนมัติ และสถานะลิงก์ทางกายภาพ
# Useful for verifying if a 1Gbps link is actually running at 1Gbps.
# มีประโยชน์สำหรับการตรวจสอบว่าลิงก์ 1Gbps ทำงานที่ 1Gbps จริงหรือไม่

# View MAC address and interface statistics / ดูที่อยู่ MAC และสถิติอินเทอร์เฟซ
ip link show eth0
# Displays the MAC address (link/ether), MTU, and TX/RX packet counts.
# แสดงที่อยู่ MAC (link/ether), MTU, และจำนวนแพ็กเก็ต TX/RX
# High error counts indicate physical layer problems.
# จำนวนข้อผิดพลาดสูงบ่งชี้ปัญหาเลเยอร์กายภาพ

# Check ARP table (IP to MAC mapping) / ตรวจสอบตาราง ARP (การแมป IP เป็น MAC)
ip neigh show
# or
arp -a
# Shows the mapping between IP addresses and MAC addresses on your local network.
# แสดงการแมประหว่างที่อยู่ IP และที่อยู่ MAC บนเครือข่ายท้องถิ่นของคุณ
# "INCOMPLETE" entries indicate ARP resolution failures.
# รายการ "INCOMPLETE" บ่งชี้ความล้มเหลวในการแก้ไข ARP

# Monitor traffic at Layer 2 / ตรวจสอบการจราจรที่เลเยอร์ 2
tcpdump -i eth0 -e
# The -e flag shows Ethernet (MAC) addresses in captured packets.
# แฟลก -e แสดงที่อยู่ Ethernet (MAC) ในแพ็กเก็ตที่จับได้
```

---

### Layer 3: Network / เลเยอร์ 3: เครือข่าย

```bash
# Show IP addresses on interfaces / แสดงที่อยู่ IP บนอินเทอร์เฟซ
ip addr show
# or
ifconfig
# Lists all IP addresses assigned to network interfaces.
# แสดงที่อยู่ IP ทั้งหมดที่กำหนดให้กับอินเทอร์เฟซเครือข่าย

# Show routing table / แสดงตาราง routing
ip route show
# or
route -n
# Displays how packets are routed. The "default" route is your gateway.
# แสดงวิธีการ route แพ็กเก็ต route "default" คือ gateway ของคุณ

# Test connectivity to a host / ทดสอบการเชื่อมต่อกับโฮสต์
ping 8.8.8.8
# Sends ICMP echo requests. Measures round-trip time and packet loss.
# ส่งคำขอ ICMP echo วัดเวลาไป-กลับและการสูญเสียแพ็กเก็ต
# Works at Layer 3 (ICMP over IP).
# ทำงานที่เลเยอร์ 3 (ICMP เหนือ IP)

# Test DNS resolution (also Layer 7) / ทดสอบการแก้ไข DNS (เลเยอร์ 7 ด้วย)
ping example.com
# If ping by IP works but ping by domain fails, DNS is the problem.
# หาก ping ตาม IP ทำงานแต่ ping ตามโดเมนล้มเหลว DNS คือปัญหา

# Trace the route to a destination / ติดตามเส้นทางไปยังปลายทาง
traceroute example.com
# or
tracepath example.com
# Shows each router (hop) between you and the destination.
# แสดงเราเตอร์แต่ละตัว (hop) ระหว่างคุณและปลายทาง
# Helps identify where packets are being dropped.
# ช่วยระบุว่าแพ็กเก็ตถูกละทิ้งที่ไหน

# Trace with specific port (combines L3 and L4) / ติดตามด้วยพอร์ตเฉพาะ (รวม L3 และ L4)
mtr example.com
# Combines ping and traceroute into one continuous diagnostic tool.
# รวม ping และ traceroute เป็นเครื่องมือวินิจฉัยต่อเนื่องเดียว
# Shows packet loss and latency at each hop.
# แสดงการสูญเสียแพ็กเก็ตและ latency ที่แต่ละ hop

# Check MTU (Maximum Transmission Unit) / ตรวจสอบ MTU (Maximum Transmission Unit)
ping -M do -s 1472 example.com
# Tests if packets of a specific size can reach the destination without fragmentation.
# ทดสอบว่าแพ็กเก็ตขนาดเฉพาะสามารถไปถึงปลายทางได้โดยไม่แบ่งส่วน
# 1472 + 28 (ICMP+IP header) = 1500 (standard Ethernet MTU)
```

---

### Layer 4: Transport / เลเยอร์ 4: การขนส่ง

```bash
# List all listening ports and connections / แสดงพอร์ตที่เปิดอยู่และการเชื่อมต่อทั้งหมด
ss -tuln
# -t: TCP, -u: UDP, -l: listening, -n: numeric (no DNS resolution)
# -t: TCP, -u: UDP, -l: listening, -n: numeric (ไม่แก้ไข DNS)
# Faster than netstat, shows current socket statistics.
# เร็วกว่า netstat แสดงสถิติ socket ปัจจุบัน

# Show all connections with process info / แสดงการเชื่อมต่อทั้งหมดพร้อมข้อมูลกระบวนการ
ss -tulnp
# The -p flag shows which process is using each port.
# แฟลก -p แสดงกระบวนการที่ใช้แต่ละพอร์ต
# Useful for finding which application is listening on a port.
# มีประโยชน์สำหรับการค้นหาว่าแอปพลิเคชันใดกำลังฟังพอร์ต

# Alternative (older command) / ทางเลือก (คำสั่งเก่ากว่า)
netstat -tuln
# Similar to ss but slower. Still widely used.
# คล้าย ss แต่ช้ากว่า ยังใช้กันอย่างแพร่หลาย

# Test connectivity to a specific port / ทดสอบการเชื่อมต่อกับพอร์ตเฉพาะ
telnet example.com 443
# or (preferred)
nc -zv example.com 443
# Tests if a TCP port is open and reachable.
# ทดสอบว่าพอร์ต TCP เปิดและเข้าถึงได้หรือไม่
# nc (netcat) is more versatile and safer than telnet.
# nc (netcat) หลากหลายกว่าและปลอดภัยกว่า telnet

# Test TCP connection with detailed output / ทดสอบการเชื่อมต่อ TCP พร้อมผลลัพธ์โดยละเอียด
curl -v telnet://example.com:443
# Shows the TCP handshake and connection status.
# แสดงการจับมือ TCP และสถานะการเชื่อมต่อ

# Capture packets on a specific port / จับแพ็กเก็ตบนพอร์ตเฉพาะ
tcpdump -i eth0 port 443
# Captures all traffic on port 443 (HTTPS).
# จับการจราจรทั้งหมดบนพอร์ต 443 (HTTPS)
# Essential for debugging connection issues.
# สำคัญสำหรับการดีบักปัญหาการเชื่อมต่อ

# Check firewall rules (iptables) / ตรวจสอบกฎไฟร์วอลล์ (iptables)
sudo iptables -L -n -v
# Lists all firewall rules with packet/byte counters.
# แสดงกฎไฟร์วอลล์ทั้งหมดพร้อมตัวนับแพ็กเก็ต/ไบต์
# -n: numeric, -v: verbose
```

---

### Layer 7: Application / เลเยอร์ 7: แอปพลิเคชัน

```bash
# Make an HTTP request with verbose output / สร้างคำขอ HTTP พร้อมผลลัพธ์ verbose
curl -v https://example.com
# -v shows: DNS resolution, TCP handshake, TLS negotiation, HTTP headers.
# -v แสดง: การแก้ไข DNS การจับมือ TCP การเจรจา TLS HTTP headers
# Essential for debugging API and web issues.
# สำคัญสำหรับการดีบักปัญหา API และเว็บ

# Fetch only HTTP headers (HEAD request) / ดึงเฉพาะ HTTP headers (คำขอ HEAD)
curl -I https://example.com
# Useful for checking response codes without downloading the full response.
# มีประโยชน์สำหรับการตรวจสอบรหัสตอบกลับโดยไม่ต้องดาวน์โหลดการตอบกลับทั้งหมด

# Make a request with specific headers / สร้างคำขอพร้อม headers เฉพาะ
curl -H "Authorization: Bearer TOKEN" https://api.example.com
# Tests API authentication and authorization.
# ทดสอบการตรวจสอบสิทธิ์และอนุญาต API

# DNS lookup / การค้นหา DNS
dig example.com
# Shows detailed DNS response including A records, TTL, and nameservers.
# แสดงการตอบกลับ DNS โดยละเอียดรวมถึงเรคคอร์ด A, TTL, และ nameservers
# Look for "ANSWER SECTION" to see the resolved IP.
# มองหา "ANSWER SECTION" เพื่อดู IP ที่แก้ไข

# Simple DNS lookup / การค้นหา DNS อย่างง่าย
nslookup example.com
# Simpler alternative to dig, shows IP address for a domain.
# ทางเลือกที่ง่ายกว่า dig แสดงที่อยู่ IP สำหรับโดเมน

# Query specific DNS record types / ค้นหาประเภทเรคคอร์ด DNS เฉพาะ
dig example.com MX        # Mail server records / เรคคอร์ดเซิร์ฟเวอร์อีเมล
dig example.com TXT       # Text records (SPF, DKIM) / เรคคอร์ดข้อความ
dig example.com NS        # Nameserver records / เรคคอร์ด nameserver

# Test TLS/SSL certificate / ทดสอบใบรับรอง TLS/SSL
openssl s_client -connect example.com:443
# Shows the TLS certificate, chain, and negotiation details.
# แสดงใบรับรอง TLS chain และรายละเอียดการเจรจา
# Look for "Verify return code: 0 (ok)" to confirm valid certificate.
# มองหา "Verify return code: 0 (ok)" เพื่อยืนยันใบรับรองที่ถูกต้อง

# Check certificate expiration / ตรวจสอบการหมดอายุใบรับรอง
echo | openssl s_client -connect example.com:443 2>/dev/null | openssl x509 -noout -dates
# Shows the "notBefore" and "notAfter" dates of the SSL certificate.
# แสดงวันที่ "notBefore" และ "notAfter" ของใบรับรอง SSL

# Test HTTP/2 support / ทดสอบการรองรับ HTTP/2
curl -v --http2 https://example.com
# Checks if the server supports HTTP/2 protocol.
# ตรวจสอบว่าเซิร์ฟเวอร์รองรับโปรโตคอล HTTP/2 หรือไม่
```

---

### Comprehensive Network Diagnostic Script / สคริปต์วินิจฉัยเครือข่ายแบบครอบคลุม

**English:**
Here's a handy diagnostic script that checks connectivity across all layers:

**ภาษาไทย:**
นี่คือสคริปต์วินิจฉัยที่ตรวจสอบการเชื่อมต่อทุกเลเยอร์:

```bash
#!/bin/bash
# network-diagnostic.sh - Multi-layer network diagnostic
# network-diagnostic.sh - การวินิจฉัยเครือข่ายหลายเลเยอร์

TARGET="${1:-example.com}"

echo "=== Network Diagnostic for: $TARGET ==="
echo "=== การวินิจฉัยเครือข่ายสำหรับ: $TARGET ==="

# Layer 3: Check IP connectivity / เลเยอร์ 3: ตรวจสอบการเชื่อมต่อ IP
echo -e "\n[Layer 3] Ping test / ทดสอบ Ping:"
ping -c 3 $TARGET

# Layer 3: Show route / เลเยอร์ 3: แสดงเส้นทาง
echo -e "\n[Layer 3] Route to target / เส้นทางไปยังเป้าหมาย:"
traceroute -m 10 $TARGET

# Layer 4: Check port connectivity / เลเยอร์ 4: ตรวจสอบการเชื่อมต่อพอร์ต
echo -e "\n[Layer 4] Port 80 (HTTP) / พอร์ต 80 (HTTP):"
nc -zv -w 3 $TARGET 80
echo -e "\n[Layer 4] Port 443 (HTTPS) / พอร์ต 443 (HTTPS):"
nc -zv -w 3 $TARGET 443

# Layer 7: HTTP request / เลเยอร์ 7: คำขอ HTTP
echo -e "\n[Layer 7] HTTP request / คำขอ HTTP:"
curl -s -o /dev/null -w "HTTP Status: %{http_code}\nTime: %{time_total}s\n" https://$TARGET

# DNS resolution / การแก้ไข DNS
echo -e "\n[DNS] Resolution / การแก้ไข:"
dig +short $TARGET
```

---

## Common Network Issues and OSI Layer Mapping / ปัญหาเครือข่ายทั่วไปและการแมปเลเยอร์ OSI

| Issue / ปัญหา | Likely Layer / เลเยอร์ที่น่าจะเป็น | Symptoms / อาการ | Solution / วิธีแก้ |
|:---|:---:|:---|:---|
| Cable unplugged / สายหลุด | L1 | Interface shows DOWN / อินเทอร์เฟซแสดง DOWN | Plug in cable / เสียบสาย |
| Wrong VLAN / VLAN ผิด | L2 | Can't reach gateway / ไม่สามารถเข้าถึง gateway | Check switch port VLAN config / ตรวจสอบการกำหนดค่าพอร์ตสวิตช์ |
| Wrong IP or subnet / IP หรือ subnet ผิด | L3 | Can ping locally but not remotely / ping ท้องถิ่นได้แต่ไม่ไกล | Check IP config and routing / ตรวจสอบการกำหนดค่า IP และ routing |
| Port blocked by firewall / พอร์ตถูกบล็อกโดยไฟร์วอลล์ | L4 | Connection refused/timeout / การเชื่อมต่อถูกปฏิเสธ/หมดเวลา | Check firewall rules / ตรวจสอบกฎไฟร์วอลล์ |
| Service not running / บริการไม่ทำงาน | L7 | Port open but no response / พอร์ตเปิดแต่ไม่ตอบกลับ | Check application logs / ตรวจสอบบันทึกแอปพลิเคชัน |
| DNS not resolving / DNS ไม่แก้ไข | L7 | Can ping IP but not domain / ping IP ได้แต่ไม่ใช่โดเมน | Check DNS config / ตรวจสอบการกำหนดค่า DNS |
| TLS certificate expired / ใบรับรอง TLS หมดอายุ | L6 | SSL handshake fails / การจับมือ SSL ล้มเหลว | Renew certificate / ต่ออายุใบรับรอง |
| Session timeout / เซสชันหมดอายุ | L5 | Logged out unexpectedly / ออกจากระบบโดยไม่คาดคิด | Check session config / ตรวจสอบการกำหนดค่าเซสชัน |

---

## Practice Exercises / แบบฝึกหัด

### Exercise 1: Trace a Web Request / แบบฝึกหัดที่ 1: ติดตามคำขอเว็บ

**English:**
Trace a complete HTTP request from your browser to a web server through all 7 OSI layers. Document:
1. What happens at each layer?
2. What headers are added?
3. What protocols are used?
4. What devices process the data?

**ภาษาไทย:**
ติดตามคำขอ HTTP สมบูรณ์จากเบราว์เซอร์ของคุณไปยังเว็บเซิร์ฟเวอร์ผ่านทั้ง 7 เลเยอร์ OSI บันทึก:
1. เกิดอะไรขึ้นที่แต่ละเลเยอร์?
2. Headers ใดถูกเพิ่ม?
3. โปรโตคอลใดถูกใช้?
4. อุปกรณ์ใดประมวลผลข้อมูล?

---

### Exercise 2: Packet Capture Analysis / แบบฝึกหัดที่ 2: การวิเคราะห์การจับแพ็กเก็ต

**English:**
Use `tcpdump` to capture the TCP 3-way handshake when connecting to a website:
```bash
sudo tcpdump -i any -n host example.com and port 443 -w capture.pcap
curl -s https://example.com > /dev/null
```
Then analyze the capture file with `tcpdump -r capture.pcap` or Wireshark. Identify:
1. The SYN, SYN-ACK, and ACK packets
2. The sequence numbers
3. The TLS Client Hello message
4. The HTTP request

**ภาษาไทย:**
ใช้ `tcpdump` เพื่อจับการจับมือสามทางของ TCP เมื่อเชื่อมต่อกับเว็บไซต์:
```bash
sudo tcpdump -i any -n host example.com and port 443 -w capture.pcap
curl -s https://example.com > /dev/null
```
จากนั้นวิเคราะห์ไฟล์ที่จับได้ด้วย `tcpdump -r capture.pcap` หรือ Wireshark ระบุ:
1. แพ็กเก็ต SYN, SYN-ACK, และ ACK
2. หมายเลขลำดับ
3. ข้อความ TLS Client Hello
4. คำขอ HTTP

---

### Exercise 3: Firewall Rule Analysis / แบบฝึกหัดที่ 3: การวิเคราะห์กฎไฟร์วอลล์

**English:**
Create an iptables rule and identify which OSI layer it operates at:
```bash
# Block incoming traffic on port 8080
sudo iptables -A INPUT -p tcp --dport 8080 -j DROP

# List rules
sudo iptables -L -n -v

# Clean up
sudo iptables -D INPUT -p tcp --dport 8080 -j DROP
```
Question: Is this an L3, L4, or L7 rule? Why?

**ภาษาไทย:**
สร้างกฎ iptables และระบุว่าทำงานที่เลเยอร์ OSI ใด:
```bash
# บล็อกการจราจรขาเข้าบนพอร์ต 8080
sudo iptables -A INPUT -p tcp --dport 8080 -j DROP

# แสดงกฎ
sudo iptables -L -n -v

# ล้าง
sudo iptables -D INPUT -p tcp --dport 8080 -j DROP
```
คำถาม: นี่เป็นกฎ L3, L4, หรือ L7? ทำไม?

---

### Exercise 4: L4 vs L7 Load Balancing / แบบฝึกหัดที่ 4: Load Balancing แบบ L4 เทียบกับ L7

**English:**
Research and explain the differences between Layer 4 and Layer 7 load balancing:
1. What information does each use to make routing decisions?
2. What are the performance trade-offs?
3. When would you choose one over the other?
4. How does Kubernetes implement both (Services vs Ingress)?

**ภาษาไทย:**
วิจัยและอธิบายความแตกต่างระหว่าง load balancing เลเยอร์ 4 และเลเยอร์ 7:
1. แต่ละประเภทใช้ข้อมูลอะไรในการตัดสินใจ routing?
2. การแลกเปลี่ยนประสิทธิภาพคืออะไร?
3. เมื่อใดคุณควรเลือกอย่างใดอย่างหนึ่ง?
4. Kubernetes นำไปใช้ทั้งสองแบบอย่างไร (Services เทียบกับ Ingress)?

---

### Exercise 5: Network Troubleshooting Challenge / แบบฝึกหัดที่ 5: ความท้าทายในการแก้ปัญหาเครือข่าย

**English:**
Given the following scenario, use the OSI model approach to troubleshoot:
- A web application at `https://app.example.com` is unreachable
- The server is running and the application process is healthy
- Other services on the same server work fine

Use a bottom-up approach (L1 to L7) to identify the problem. Document your steps and findings.

**ภาษาไทย:**
จากสถานการณ์ต่อไปนี้ ใช้แนวทางการแก้ปัญหาแบบ OSI model:
- เว็บแอปพลิเคชันที่ `https://app.example.com` เข้าถึงไม่ได้
- เซิร์ฟเวอร์กำลังทำงานและกระบวนการแอปพลิเคชันปกติ
- บริการอื่นบนเซิร์ฟเวอร์เดียวกันทำงานปกติ

ใช้แนวทางจากล่างขึ้นบน (L1 ถึง L7) เพื่อระบุปัญหา บันทึกขั้นตอนและผลการค้นพบ

---

### Exercise 6: Build a Network Diagram / แบบฝึกหัดที่ 6: สร้างแผนภาพเครือข่าย

**English:**
Draw a network diagram for a typical web application stack showing:
1. Client browser
2. CDN
3. L7 Load Balancer / WAF
4. Application servers
5. Database servers
6. Label which OSI layer each component operates at

**ภาษาไทย:**
วาดแผนภาพเครือข่ายสำหรับสแต็กแอปพลิเคชันเว็บทั่วไปแสดง:
1. เบราว์เซอร์ไคลเอนต์
2. CDN
3. L7 Load Balancer / WAF
4. เซิร์ฟเวอร์แอปพลิเคชัน
5. เซิร์ฟเวอร์ฐานข้อมูล
6. ติดป้ายกำกับว่าองค์ประกอบแต่ละตัวทำงานที่เลเยอร์ OSI ใด

---

## Quick Reference: Port Numbers / การอ้างอิงด่วน: หมายเลขพอร์ต

**English:**
Here are the most common port numbers that DevOps engineers encounter:

**ภาษาไทย:**
นี่คือหมายเลขพอร์ตที่พบบ่อยที่สุดที่วิศวกร DevOps พบ:

| Port / พอร์ต | Protocol / โปรโตคอล | Service / บริการ | Layer / เลเยอร์ |
|:---:|:---|:---|:---:|
| 20-21 | TCP | FTP (File Transfer) / ถ่ายโอนไฟล์ | 7 |
| 22 | TCP | SSH (Secure Shell) / เชลล์ปลอดภัย | 7 |
| 23 | TCP | Telnet (unencrypted) / ไม่เข้ารหัส | 7 |
| 25 | TCP | SMTP (Email sending) / ส่งอีเมล | 7 |
| 53 | UDP/TCP | DNS (Domain Resolution) / แก้ไขโดเมน | 7 |
| 67-68 | UDP | DHCP (IP assignment) / กำหนด IP | 7 |
| 80 | TCP | HTTP (Web) / เว็บ | 7 |
| 110 | TCP | POP3 (Email retrieval) / ดึงอีเมล | 7 |
| 143 | TCP | IMAP (Email retrieval) / ดึงอีเมล | 7 |
| 443 | TCP | HTTPS (Secure Web) / เว็บปลอดภัย | 7 |
| 445 | TCP | SMB (File sharing) / แบ่งปันไฟล์ | 7 |
| 993 | TCP | IMAPS (Secure IMAP) / IMAP ปลอดภัย | 7 |
| 3306 | TCP | MySQL (Database) / ฐานข้อมูล | 7 |
| 5432 | TCP | PostgreSQL (Database) / ฐานข้อมูล | 7 |
| 6379 | TCP | Redis (Cache) / แคช | 7 |
| 8080 | TCP | HTTP Alternate / HTTP ทางเลือก | 7 |
| 8443 | TCP | HTTPS Alternate / HTTPS ทางเลือก | 7 |
| 27017 | TCP | MongoDB (Database) / ฐานข้อมูล | 7 |

---

## Mnemonics for Remembering OSI Layers / เทคนิคช่วยจำเลเยอร์ OSI

**English:**
Use these mnemonics to remember the 7 layers from Layer 7 to Layer 1:

**"All People Seem To Need Data Processing"**
- **A**ll = **A**pplication (7)
- **P**eople = **P**resentation (6)
- **S**eem = **S**ession (5)
- **T**o = **T**ransport (4)
- **N**eed = **N**etwork (3)
- **D**ata = **D**ata Link (2)
- **P**rocessing = **P**hysical (1)

**ภาษาไทย:**
ใช้เทคนิคช่วยจำเหล่านี้เพื่อจำ 7 เลเยอร์จากเลเยอร์ 7 ถึงเลเยอร์ 1:

**"All People Seem To Need Data Processing"**
- **A**ll = **A**pplication (7) / แอปพลิเคชัน
- **P**eople = **P**resentation (6) / การนำเสนอ
- **S**eem = **S**ession (5) / เซสชัน
- **T**o = **T**ransport (4) / การขนส่ง
- **N**eed = **N**etwork (3) / เครือข่าย
- **D**ata = **D**ata Link (2) / เชื่อมโยงข้อมูล
- **P**rocessing = **P**hysical (1) / กายภาพ

---

## Summary / สรุป

**English:**
The OSI Model is the foundation for understanding how networks operate. As a DevOps engineer, you don't need to memorize every detail, but you should:

1. **Understand the purpose of each layer** and what happens there.
2. **Know the key protocols** (HTTP, TCP, IP, Ethernet) and which layer they belong to.
3. **Use the OSI model as a troubleshooting framework** -- start from L1 and work up, or start from L7 and work down.
4. **Recognize which layer your tools operate at** -- ping is L3, telnet is L4, curl is L7.
5. **Apply this knowledge daily** -- every time you configure a firewall, debug a connection, or design a network architecture, you're using OSI concepts.

**ภาษาไทย:**
โมเดล OSI เป็นพื้นฐานสำหรับเข้าใจการทำงานของเครือข่าย ในฐานะวิศวกร DevOps คุณไม่จำเป็นต้องจำทุกรายละเอียด แต่คุณควร:

1. **เข้าใจจุดประสงค์ของแต่ละเลเยอร์** และสิ่งที่เกิดขึ้นที่นั่น
2. **รู้จักโปรโตคอลหลัก** (HTTP, TCP, IP, Ethernet) และเลเยอร์ที่พวกเขาอยู่
3. **ใช้โมเดล OSI เป็นกรอบการแก้ปัญหา** -- เริ่มจาก L1 แล้วขึ้นบน หรือเริ่มจาก L7 แล้วลงล่าง
4. **ระบุว่าเครื่องมือของคุณทำงานที่เลเยอร์ใด** -- ping คือ L3, telnet คือ L4, curl คือ L7
5. **ใช้ความรู้นี้ทุกวัน** -- ทุกครั้งที่คุณกำหนดค่าไฟร์วอลล์ ดีบักการเชื่อมต่อ หรือออกแบบสถาปัตยกรรมเครือข่าย คุณกำลังใช้แนวคิด OSI

---

## Resources / แหล่งข้อมูลเพิ่มเติม

### Documentation and Tutorials / เอกสารและบทช่วยสอน

- **[Cloudflare OSI Model Guide](https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/)** -- Comprehensive overview with real-world examples / ภาพรวมโดยละเอียดพร้อมตัวอย่างจริง
- **[Network Direction YouTube](https://networkdirection.com/)** -- Excellent video tutorials on networking concepts / บทเรียนวิดีโอยอดเยี่ยมเกี่ยวกับแนวคิดเครือข่าย
- **[Cisco Networking Academy](https://www.netacad.com/)** -- Free networking courses including OSI model / หลักสูตรเครือข่ายฟรีรวมถึงโมเดล OSI
- **[GeeksforGeeks OSI Model](https://www.geeksforgeeks.org/layers-of-osi-model/)** -- Detailed layer-by-layer explanation / คำอธิบายทีละเลเยอร์โดยละเอียด

### Interactive Tools / เครื่องมือโต้ตอบ

- **[Wireshark](https://www.wireshark.org/)** -- Industry-standard packet analyzer / เครื่องมือวิเคราะห์แพ็กเก็ตมาตรฐานอุตสาหกรรม
- **[TCP Dump Tutorial](https://danielmiessler.com/study/tcpdump/)** -- Practical tcpdump guide / คู่มือ tcpdump เชิงปฏิบัติ
- **[Scapy](https://scapy.net/)** -- Python library for packet manipulation / ไลบรารี Python สำหรับการจัดการแพ็กเก็ต

### Books / หนังสือ

- **"Computer Networking: A Top-Down Approach"** by Kurose & Ross -- Excellent textbook / ตำรายอดเยี่ยม
- **"TCP/IP Illustrated"** by W. Richard Stevens -- Deep dive into TCP/IP / เจาะลึก TCP/IP
- **"Network Warrior"** by Gary A. Donahue -- Practical networking / เครือข่ายเชิงปฏิบัติ

---

> **Next Steps / ขั้นตอนถัดไป:**
> After mastering the OSI model, proceed to learn about:
> หลังจากเชี่ยวชาญโมเดล OSI แล้ว Proceed to เรียนรู้เกี่ยวกับ:
> - **TCP/IP Model** -- The practical 4-layer implementation / การนำไปใช้จริง 4 เลเยอร์
> - **Subnetting and CIDR** -- IP address management / การจัดการที่อยู่ IP
> - **DNS in Depth** -- How domain resolution works / การแก้ไขโดเมนทำงานอย่างไร
> - **HTTP/HTTPS Protocols** -- Application layer details / รายละเอียดเลเยอร์แอปพลิเคชัน
> - **TLS/SSL Handshake** -- Encryption negotiation / การเจรจาการเข้ารหัส

---

*Last updated: April 2026 / อัปเดตล่าสุด: เมษายน 2026*
