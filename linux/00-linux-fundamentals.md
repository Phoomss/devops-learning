# Linux Fundamentals / พื้นฐาน Linux

## What is Linux? / Linux คืออะไร?
Linux is an open-source, Unix-like operating system kernel that powers most of the internet's servers, cloud infrastructure, and DevOps tools.

Linux เป็น kernel ของระบบปฏิบัติการแบบ open-source ที่คล้าย Unix ซึ่งขับเคลื่อนเซิร์ฟเวอร์ส่วนใหญ่บนอินเทอร์เน็ต, cloud infrastructure และเครื่องมือ DevOps

## Key Concepts / แนวคิดหลัก

### File System Hierarchy / ลำดับชั้นระบบไฟล์
```
/         - Root directory (ไดเรกทอรีราก)
/bin      - Essential binaries (ไบนารีพื้นฐาน เช่น ls, cp, cat)
/etc      - Configuration files (ไฟล์ตั้งค่า)
/home     - User home directories (ไดเรกทอรีบ้านของผู้ใช้)
/var      - Variable data (ข้อมูลที่เปลี่ยนแปลง เช่น logs, databases)
/usr      - User programs and libraries (โปรแกรมและไลบรารีของผู้ใช้)
/tmp      - Temporary files (ไฟล์ชั่วคราว)
```

### Essential Commands / คำสั่งสำคัญ

#### File Operations / การจัดการไฟล์
```bash
ls -la          # List all files with details (แสดงไฟล์ทั้งหมดพร้อมรายละเอียด)
cd /path        # Change directory (เปลี่ยนไดเรกทอรี)
pwd             # Print working directory (แสดงไดเรกทอรีปัจจุบัน)
cp src dest     # Copy files (คัดลอกไฟล์)
mv src dest     # Move/rename files (ย้าย/เปลี่ยนชื่อไฟล์)
rm -rf dir      # Remove directory recursively (ลบไดเรกทอรีแบบวนซ้ำ)
```

#### System Information / ข้อมูลระบบ
```bash
top             # Process monitor (ตรวจสอบกระบวนการ)
htop            # Interactive process viewer (แสดงกระบวนการแบบโต้ตอบ)
df -h           # Disk space (พื้นที่ดิสก์)
free -m         # Memory usage (การใช้หน่วยความจำ)
uname -a        # System information (ข้อมูลระบบ)
```

#### Permissions / สิทธิ์
```bash
chmod 755 file  # Change permissions (เปลี่ยนสิทธิ์)
chown user:group file  # Change ownership (เปลี่ยนเจ้าของ)
```

#### Networking / เครือข่าย
```bash
ip addr         # Show IP addresses (แสดงที่อยู่ IP)
ss -tuln        # Show listening ports (แสดงพอร์ตที่กำลังรับฟัง)
ping host       # Test connectivity (ทดสอบการเชื่อมต่อ)
curl URL        # HTTP requests (ส่งคำขอ HTTP)
```

### Package Management (Ubuntu/Debian) / การจัดการแพ็กเกจ
```bash
apt update              # Update package list (อัปเดตรายการแพ็กเกจ)
apt install package     # Install package (ติดตั้งแพ็กเกจ)
apt remove package      # Remove package (ลบแพ็กเกจ)
apt upgrade             # Upgrade all packages (อัปเกรดแพ็กเกจทั้งหมด)
```

### Service Management (systemd) / การจัดการบริการ
```bash
systemctl start service     # Start service (เริ่มบริการ)
systemctl stop service      # Stop service (หยุดบริการ)
systemctl enable service    # Enable on boot (เปิดใช้งานเมื่อเริ่มระบบ)
systemctl status service    # Check status (ตรวจสอบสถานะ)
journalctl -u service       # View logs (ดูล็อก)
```

### User Management / การจัดการผู้ใช้
```bash
useradd username        # Create user (สร้างผู้ใช้)
passwd username         # Set password (ตั้งรหัสผ่าน)
usermod -aG sudo user   # Add to sudo group (เพิ่มในกลุ่ม sudo)
id username             # Show user info (แสดงข้อมูลผู้ใช้)
```

### File Permissions Explained / อธิบายสิทธิ์ไฟล์
```
-rwxr-xr--
│  │  │  └─ Others: read (ผู้อื่น: อ่าน)
│  │  └──── Group: read, execute (กลุ่ม: อ่าน, รัน)
│  └─────── Owner: read, write, execute (เจ้าของ: อ่าน, เขียน, รัน)
└────────── File type (- = file, d = directory) (ประเภทไฟล์)
```

### Useful Shell Scripts / สคริปต์ Shell ที่มีประโยชน์
```bash
#!/bin/bash
# Example: Backup script (ตัวอย่าง: สคริปต์สำรองข้อมูล)
echo "Starting backup..."
tar -czf /backup/home.tar.gz /home
echo "Backup completed!"
```

## Practice Exercises / แบบฝึกหัด
1. Create a user and assign sudo privileges (สร้างผู้ใช้และกำหนดสิทธิ์ sudo)
2. Set up a web server (nginx) and manage the service (ติดตั้งเว็บเซิร์ฟเวอร์และจัดการบริการ)
3. Write a shell script to monitor disk usage (เขียนสคริปต์ตรวจสอบการใช้ดิสก์)
4. Configure SSH key-based authentication (ตั้งค่าการยืนยันตัวตนด้วย SSH key)
5. Find all files modified in the last 7 days (ค้นหาไฟล์ที่แก้ไขใน 7 วันที่ผ่านมา)

## Resources / แหล่งข้อมูล
- [Linux Journey](https://linuxjourney.com/)
- [Linux Command Library](https://linuxcommandlibrary.com/)
