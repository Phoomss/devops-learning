# 🧰 How to Install Ubuntu on VirtualBox

## 📌 สิ่งที่ต้องเตรียมก่อนเริ่ม

1. **VirtualBox** – ดาวน์โหลดจาก: https://www.virtualbox.org/  
2. **Ubuntu ISO** – ดาวน์โหลดจาก: https://ubuntu.com/download/desktop

---

## 🔧 ขั้นตอนการติดตั้ง Ubuntu บน VirtualBox

### 1. ติดตั้ง VirtualBox

- ดาวน์โหลดและติดตั้ง VirtualBox ตามระบบปฏิบัติการของคุณ (Windows / macOS / Linux)

---

### 2. สร้าง Virtual Machine ใหม่

1. เปิด VirtualBox แล้วคลิก **"New"**  
2. ตั้งชื่อเช่น `Ubuntu-22.04`  
3. **Type:** Linux  
4. **Version:** Ubuntu (64-bit)

---

### 3. กำหนดขนาด RAM

- แนะนำขั้นต่ำ **2048 MB (2 GB)** หรือมากกว่า  
- ถ้าคุณมี RAM มากให้ใช้ **4096 MB (4 GB)** เพื่อให้ระบบลื่นขึ้น

---

### 4. สร้าง Virtual Hard Disk

- เลือก **Create a virtual hard disk now** → Next  
- เลือก **VDI (VirtualBox Disk Image)** → Next  
- เลือก **Dynamically allocated**  
- ขนาดแนะนำ: **20 GB ขึ้นไป**

---

### 5. ติดตั้ง Ubuntu ด้วย ISO

1. ไปที่ **Settings → Storage**  
2. ใต้ “Controller: IDE” คลิก “Empty” แล้วคลิกไอคอนแผ่น CD → เลือก `Choose a disk file…`  
3. เลือกไฟล์ **Ubuntu ISO** ที่ดาวน์โหลดไว้  
4. คลิก OK

---

### 6. เริ่มการติดตั้ง

- คลิก **Start** เพื่อเริ่ม VM  
- เมื่อ Ubuntu เริ่มทำงาน ให้เลือก  
  - Language  
  - Keyboard  
  - Installation Type → ปกติเลือก “Erase disk and install Ubuntu”  
  - ตั้งค่า User และ Password ตามต้องการ

---

### 7. หลังติดตั้งเสร็จ

- รีสตาร์ตเครื่อง Virtual Machine  
- ถ้าระบบยังบูตจาก ISO อยู่ → ไปที่ **Settings → Storage** แล้วนำ ISO ออก

---

## 💡 เคล็ดลับเพิ่มเติม

- ติดตั้ง **Guest Additions** เพื่อให้ VM ใช้งานได้เต็มประสิทธิภาพ:
  - ไปที่เมนู VM → Devices → Insert Guest Additions CD Image...
  - เปิด Terminal แล้วรันคำสั่งเพื่อติดตั้ง

---

> ✅ พร้อมใช้งาน Ubuntu บน VirtualBox แล้ว!
