# 🔐 How to use OpenSSH on Ubuntu

## ✅ SSH คืออะไร?

SSH (Secure Shell) เป็นโปรโตคอลสำหรับการเชื่อมต่อเข้าสู่ระบบระยะไกลอย่างปลอดภัย โดยใช้การเข้ารหัส

---

## 📦 1. ติดตั้ง OpenSSH

### ติดตั้ง SSH Server (ใช้กับเครื่องที่เราต้องการ remote เข้าไป)
```bash
sudo apt update
sudo apt install openssh-server
```

### ตรวจสอบสถานะ ssh service
```bash
sudo systemctl status ssh
```

ถ้าแสดงว่า `active (running)` แปลว่าใช้งานได้แล้ว

### หากต้องการเปิด/ปิด/รีสตาร์ต SSH
```bash
sudo systemctl start ssh      # เริ่ม service
sudo systemctl stop ssh       # หยุด service
sudo systemctl restart ssh    # รีสตาร์ต service
```

---

## 💻 2. การเชื่อมต่อ SSH Client

### เชื่อมต่อจากเครื่อง client เข้าสู่ server
```bash
ssh username@ip_address
```

ตัวอย่าง:
```bash
ssh ubuntu@192.168.1.100
```

หากเชื่อมต่อครั้งแรกจะมีข้อความถามให้กดยืนยัน `yes`

---

## 🗝️ 3. การสร้าง SSH Key Pair

### สร้าง key (บนเครื่อง client)
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

- กด Enter เพื่อใช้ path ปกติ: `~/.ssh/id_rsa`
- ระบบจะได้ไฟล์:
  - `id_rsa` (private key)
  - `id_rsa.pub` (public key)

### ส่ง public key ไปยัง server

```bash
ssh-copy-id username@ip_address
```

ตัวอย่าง:
```bash
ssh-copy-id ubuntu@192.168.1.100
```

จากนี้สามารถ ssh ได้โดยไม่ต้องพิมพ์รหัสผ่าน

---

## 🔒 4. ปรับแต่ง SSH Server (ไฟล์ config)

เปิดไฟล์:
```bash
sudo nano /etc/ssh/sshd_config
```

ตัวเลือกแนะนำ:
```bash
PermitRootLogin no            # ห้าม root login
PasswordAuthentication no     # ปิด login ด้วย password (ใช้ key เท่านั้น)
Port 2222                     # เปลี่ยน port เพิ่มความปลอดภัย
```

หลังจากแก้แล้ว ต้อง restart ssh:
```bash
sudo systemctl restart ssh
```

---

## 🔧 5. ใช้ SSH ทำ port forwarding

### Local forwarding
```bash
ssh -L 8080:localhost:3000 user@server
```
เปิดพอร์ต `3000` บน server ให้ใช้งานผ่าน `localhost:8080` ที่เครื่องเรา

---

## 🧪 ตรวจสอบ SSH ทำงานได้หรือไม่

```bash
telnet ip_address 22
```

หรือ:
```bash
nc -zv ip_address 22
```

---

## 📁 ไฟล์สำคัญเกี่ยวกับ SSH

| ไฟล์ | ความหมาย |
|------|-----------|
| `~/.ssh/id_rsa` | Private key (ห้ามแจก) |
| `~/.ssh/id_rsa.pub` | Public key (ไว้แจกให้ server) |
| `/etc/ssh/sshd_config` | config ของ SSH Server |
| `/var/log/auth.log` | Log การ login |

---

> 🔐 **Tips**: ใช้ Fail2Ban หรือ UFW เพื่อเพิ่มความปลอดภัยในการใช้งาน SSH บน public server
