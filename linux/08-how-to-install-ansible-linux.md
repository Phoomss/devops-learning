# ⚙️ How to Install Ansible on Linux (Ubuntu)

Ansible คือ automation tool สำหรับจัดการ configuration และ deployment โดยไม่ต้องติดตั้ง agent บน target host

รองรับ Ubuntu 20.04, 22.04, 24.04

---

## 🔧 Prerequisites

- Ubuntu
- Python 3
- อินเทอร์เน็ต

---

## 🔄 Step 1: ติดตั้ง Ansible ด้วย apt

```bash
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
```

---

## ✅ Step 2: ตรวจสอบการติดตั้ง

```bash
ansible --version
```

---

## 🔍 Step 3: สร้าง inventory และทดสอบ

ตัวอย่างไฟล์ inventory:

```ini
[local]
127.0.0.1 ansible_connection=local
```

ทดสอบ connection:

```bash
ansible -i inventory local -m ping
```

---

## 📦 Step 4: รัน playbook ตัวอย่าง

สร้างไฟล์ `site.yml`:

```yaml
- hosts: local
  tasks:
    - name: แสดงข้อความ
      debug:
        msg: "Hello from Ansible"
```

รัน playbook:

```bash
ansible-playbook -i inventory site.yml
```

---

## 📁 คำสั่ง Ansible พื้นฐาน

| คำสั่ง | ความหมาย | ตัวอย่าง |
|--------|-----------|----------|
| `ansible --version` | ตรวจสอบเวอร์ชัน | - |
| `ansible -i <inventory> <host> -m ping` | ทดสอบ connection | `ansible -i inventory local -m ping` |
| `ansible-playbook -i <inventory> <playbook>` | รัน playbook | `ansible-playbook -i inventory site.yml` |
| `ansible-galaxy install <role>` | ติดตั้ง role | `ansible-galaxy install geerlingguy.apache` |
| `ansible-doc <module>` | ดู docs ของ module | `ansible-doc yum` |

---

## 💡 Tips

- ถ้าจะจัดการ remote host ให้ตั้งค่า SSH key และไฟล์ inventory ให้เรียบร้อย
- ใช้ `ansible-lint` เพื่อตรวจความถูกต้องของ playbook ก่อนรัน
