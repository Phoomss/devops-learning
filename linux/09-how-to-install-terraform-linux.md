# 🌍 How to Install Terraform on Linux (Ubuntu)

Terraform คือ infrastructure as code tool สำหรับสร้างและจัดการ infrastructure บน cloud หรือบนระบบต่างๆ

รองรับ Ubuntu 20.04, 22.04, 24.04

---

## 🔧 Prerequisites

- Ubuntu
- `unzip`
- อินเทอร์เน็ต

---

## 🔄 Step 1: ติดตั้ง unzip

```bash
sudo apt update
sudo apt install unzip -y
```

---

## 📥 Step 2: ดาวน์โหลด Terraform

```bash
TERRAFORM_VERSION="1.6.8"
curl -LO "https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip"
unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip
sudo mv terraform /usr/local/bin/
sudo chmod +x /usr/local/bin/terraform
rm terraform_${TERRAFORM_VERSION}_linux_amd64.zip
```

---

## ✅ Step 3: ตรวจสอบการติดตั้ง

```bash
terraform version
```

---

## 🧩 Step 4: สร้างโปรเจกต์ Terraform ตัวอย่าง

สร้างไฟล์ `main.tf`:

```hcl
terraform {
  required_providers {
    random = {
      source  = "hashicorp/random"
      version = "~> 3.5"
    }
  }
}

provider "random" {}

resource "random_pet" "example" {
  length = 2
}

output "name" {
  value = random_pet.example.id
}
```

รันคำสั่ง:

```bash
terraform init
terraform plan
terraform apply -auto-approve
```

---

## 🗑️ Step 5: ลบทรัพยากร

```bash
terraform destroy -auto-approve
```

---

## 📁 คำสั่ง Terraform พื้นฐาน

| คำสั่ง | ความหมาย | ตัวอย่าง |
|--------|-----------|----------|
| `terraform init` | เตรียมโปรเจกต์ | - |
| `terraform plan` | แสดงแผนการเปลี่ยนแปลง | - |
| `terraform apply` | ปรับสภาพแวดล้อม | `terraform apply -auto-approve` |
| `terraform destroy` | ลบทรัพยากร | `terraform destroy -auto-approve` |
| `terraform fmt` | จัดรูปแบบไฟล์ HCL | `terraform fmt` |
| `terraform validate` | ตรวจสอบ syntax | `terraform validate` |

---

## 💡 Tips

- ใช้ `terraform state list` และ `terraform show` เพื่อตรวจสอบ state
- ตั้งค่าตัวแปรใน `terraform.tfvars` เพื่อแยกค่า config ออกจากไฟล์ main
