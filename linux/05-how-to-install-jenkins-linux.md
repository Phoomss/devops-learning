# 🧩 How to Install Jenkins on Linux (Ubuntu)

Jenkins คือ automation server สำหรับสร้าง CI/CD pipeline และรันงานอัตโนมัติ

รองรับ Ubuntu 20.04, 22.04, 24.04

---

## 🔧 Prerequisites

- Ubuntu
- Java 11 หรือ Java 17
- อินเทอร์เน็ตเพื่อดาวน์โหลด package

---

## 🔄 Step 1: ติดตั้ง Java

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
```

ตรวจสอบเวอร์ชัน Java:

```bash
java -version
```

---

## 🔐 Step 2: เพิ่ม Jenkins repository

```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
sudo sh -c 'echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```

---

## 📦 Step 3: ติดตั้ง Jenkins

```bash
sudo apt update
sudo apt install jenkins -y
```

---

## ▶️ Step 4: เริ่ม Jenkins และตรวจสอบสถานะ

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins
```

---

## 🌐 Step 5: เปิดใช้งาน Jenkins

Jenkins จะรันที่พอร์ต `8080`

เปิดเว็บเบราว์เซอร์ที่:

```
http://localhost:8080
```

---

## 🔑 Step 6: ดูรหัสผ่านเริ่มต้น

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

ใช้รหัสผ่านนี้เพื่อปลดล็อกหน้า setup ครั้งแรก

---

## 📌 Step 7: คำสั่งจัดการ Jenkins

```bash
sudo systemctl restart jenkins
sudo systemctl stop jenkins
sudo journalctl -u jenkins -f
```

---

## 📁 คำสั่งพื้นฐาน

| คำสั่ง | ความหมาย | ตัวอย่าง |
|--------|-----------|----------|
| `sudo systemctl start jenkins` | เริ่ม Jenkins service | - |
| `sudo systemctl stop jenkins` | หยุด Jenkins service | - |
| `sudo systemctl restart jenkins` | รีสตาร์ท Jenkins | - |
| `sudo systemctl status jenkins` | ดูสถานะ Jenkins | - |
| `sudo journalctl -u jenkins -f` | ดู log แบบ real-time | - |
| `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` | ดูรหัสผ่านเริ่มต้น | - |

---

## 💡 Tips

- หากต้องการปลดล็อก firewall ให้ใช้:

```bash
sudo ufw allow 8080
```

- ถ้าจะติดตั้ง Jenkins plugin เพิ่มเติม ให้ใช้ UI ในหน้า Dashboard
