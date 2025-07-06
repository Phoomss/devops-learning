# 🌐 How to Install NGINX on Ubuntu

NGINX เป็นเว็บเซิร์ฟเวอร์ที่มีประสิทธิภาพสูง ใช้ได้ทั้งเป็น web server, reverse proxy และ load balancer

---

## 🛠️ ขั้นตอนการติดตั้ง NGINX

### 1. อัปเดตระบบ
```bash
sudo apt update
sudo apt upgrade
sudo apt install nginx
```
### 2.  ตรวจสอบสถานะ NGINX
```bash
sudo systemctl status nginx
```
## 🔎 ทดสอบการทำงาน

เปิดเว็บเบราว์เซอร์แล้วไปที่  
`http://localhost` หรือ `http://<your-server-ip>`  
จะเห็นหน้า **Welcome to nginx!**

---

## ⚙️ คำสั่งพื้นฐานของ NGINX

| คำสั่ง                         | คำอธิบาย                                 |
|--------------------------------|------------------------------------------|
| `sudo systemctl start nginx`   | เริ่มต้น nginx                           |
| `sudo systemctl stop nginx`    | หยุด nginx                              |
| `sudo systemctl restart nginx` | รีสตาร์ต nginx                         |
| `sudo systemctl reload nginx`  | โหลด config ใหม่โดยไม่หยุด service     |
| `sudo systemctl enable nginx`  | ให้ nginx ทำงานตอนเปิดเครื่อง          |
| `sudo systemctl disable nginx` | ปิดไม่ให้ nginx ทำงานอัตโนมัติ          |

---

## 📁 ไฟล์และโฟลเดอร์สำคัญ

| Path                         | รายละเอียด                     |
|------------------------------|-------------------------------|
| `/etc/nginx/nginx.conf`       | ไฟล์ config หลัก              |
| `/etc/nginx/sites-available/` | เก็บ config ของเว็บไซต์        |
| `/etc/nginx/sites-enabled/`   | เก็บ symlink จาก sites-available |
| `/var/www/html/`               | โฟลเดอร์เก็บไฟล์เว็บไซต์เริ่มต้น |
| `/var/log/nginx/`             | ไฟล์ log ของ nginx            |

## 🔍 ทดสอบและ debug

### ตรวจสอบ syntax ของ config
```bash
sudo nginx -t
```
### ดู log error
```bash
tail -f /var/log/nginx/error.log
```