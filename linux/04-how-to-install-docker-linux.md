
# 🐳 How to Install Docker on Linux (Ubuntu)

Docker คือ container platform ที่ช่วยให้เรารันแอปแบบ isolated environment ได้ง่ายและรวดเร็ว

รองรับ Ubuntu 20.04, 22.04, 24.04

---

## 🔧 Step 1: อัปเดตระบบ และติดตั้ง dependencies

```bash
sudo apt update
sudo apt install ca-certificates curl gnupg lsb-release -y
```

---

## 🔐 Step 2: เพิ่ม Docker GPG Key

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

---

## 📦 Step 3: เพิ่ม Docker repository

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) \
  signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

---

## 🔄 Step 4: อัปเดต apt และติดตั้ง Docker

```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

---

## ✅ Step 5: ทดสอบการทำงานของ Docker

```bash
sudo docker run hello-world
```

ควรเห็นข้อความ:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

---

## 🔓 (Optional) ใช้งาน Docker โดยไม่ต้องใช้ sudo

```bash
sudo usermod -aG docker $USER
newgrp docker
```

ลองรันใหม่โดยไม่ใช้ `sudo`:

```bash
docker run hello-world
```

---

## 📁 Docker Command พื้นฐาน

| คำสั่ง | ความหมาย | ตัวอย่าง | หมายเหตุเพิ่มเติม |
|--------|-----------|----------|------------------|
| `docker ps` | แสดง container ที่กำลังรันอยู่ในระบบ | `docker ps` | ใช้ดู container ที่ยังไม่ถูก stop |
| `docker ps -a` | แสดง container ทั้งหมด (ทั้งที่รันอยู่และหยุดแล้ว) | `docker ps -a` | มีประโยชน์เวลาต้องการดูประวัติ container |
| `docker images` | แสดงรายการ image ที่ดึงมาไว้ในเครื่อง | `docker images` | ใช้เช็คว่าเรามี image ไหนใน local บ้าง |
| `docker pull <image>` | ดาวน์โหลด image จาก Docker Hub | `docker pull nginx` | ต้องเชื่อมเน็ต ใช้ดึง image ครั้งแรก |
| `docker run <image>` | สร้างและรัน container จาก image ที่กำหนด | `docker run nginx` | ถ้าไม่มี image จะถูก pull อัตโนมัติ |
| `docker run -it <image> bash` | รัน container แบบ interactive พร้อม shell | `docker run -it ubuntu bash` | ใช้เข้าไปทำงานภายใน container |
| `docker stop <container>` | หยุด container ที่กำลังรัน | `docker stop webserver` | ใช้ชื่อหรือ ID ของ container ได้ |
| `docker rm <container>` | ลบ container ที่หยุดแล้ว | `docker rm webserver` | ต้อง stop ก่อนถึงจะลบได้ |
| `docker rmi <image>` | ลบ image ที่ไม่ใช้งาน | `docker rmi nginx` | ต้องลบ container ที่ใช้ image นั้นก่อน |
| `docker exec -it <container> bash` | เข้า shell ของ container ที่กำลังรัน | `docker exec -it mycontainer bash` | เหมือน SSH เข้าไปใน container |
| `docker logs <container>` | ดู log ที่แสดงจาก container | `docker logs mycontainer` | ใช้ debug ได้ดี |
| `docker build -t <name> .` | สร้าง image จาก Dockerfile | `docker build -t myapp .` | ใช้กับโปรเจกต์ที่มี Dockerfile |
| `docker-compose up` | สร้างและรันหลาย container จากไฟล์ compose | `docker-compose up -d` | ใช้เมื่อมี `docker-compose.yml` |

---


