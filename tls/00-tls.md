# TLS (Transport Layer Security) / TLS (การรักษาความปลอดภัยระดับทรานสปอร์ต)

---

## What is TLS? / TLS คืออะไร?

TLS (Transport Layer Security) is a cryptographic protocol that provides secure communication over computer networks by encrypting data in transit. It ensures that data transferred between a client (e.g., web browser) and a server (e.g., website) cannot be intercepted, read, or modified by unauthorized parties.

TLS (Transport Layer Security) คือโปรโตคอลด้านการเข้ารหัสที่ให้การสื่อสารที่ปลอดภัยผ่านเครือข่ายคอมพิวเตอร์โดยการเข้ารหัสข้อมูลระหว่างการส่งถ่าย มันช่วยให้มั่นใจได้ว่าข้อมูลที่ถ่ายโอนระหว่างไคลเอนต์ (เช่น เว็บเบราว์เซอร์) และเซิร์ฟเวอร์ (เช่น เว็บไซต์) จะไม่สามารถถูกดักจับ อ่าน หรือแก้ไขโดยผู้ที่ไม่ได้รับอนุญาต

### Why is TLS Important? / ทำไม TLS จึงสำคัญ?

- **Confidentiality / ความลับ**: Data is encrypted so only the intended recipient can read it.
  - ข้อมูลถูกเข้ารหัส sehingga เฉพาะผู้รับที่ตั้งใจไว้เท่านั้นที่สามารถอ่านได้
- **Integrity / ความสมบูรณ์**: Data cannot be altered during transit without detection.
  - ข้อมูลไม่สามารถถูกเปลี่ยนแปลงระหว่างการส่งถ่ายโดยไม่ถูกตรวจจับ
- **Authentication / การยืนยันตัวตน**: Verifies the identity of the server (and optionally the client).
  - ยืนยันตัวตนของเซิร์ฟเวอร์ (และไคลเอนต์ถ้าต้องการ)
- **Trust / ความน่าเชื่อถือ**: Browsers show a padlock icon, indicating the connection is secure.
  - เบราว์เซอร์แสดงไอคอนแม่กุญแจ แสดงว่าการเชื่อมต่อปลอดภัย

### TLS vs SSL / TLS เทียบกับ SSL

SSL (Secure Sockets Layer) is the predecessor of TLS. SSL versions 1.0, 2.0, and 3.0 are now deprecated due to security vulnerabilities. TLS 1.0 was introduced in 1999 as an upgrade to SSL 3.0.

SSL (Secure Sockets Layer) คือโปรโตคอลรุ่นก่อนหน้าของ TLS SSL เวอร์ชัน 1.0, 2.0, และ 3.0 ถูกยกเลิกใช้งานแล้วเนื่องจากช่องโหว่ด้านความปลอดภัย TLS 1.0 ถูกนำเสนอในปี 1999 เป็นการอัพเกรดจาก SSL 3.0

| Version / เวอร์ชัน | Year / ปี | Status / สถานะ |
|-------------------|-----------|----------------|
| SSL 1.0 | 1995 | Never released / ไม่ได้เผยแพร่ |
| SSL 2.0 | 1995 | Deprecated / ยกเลิกแล้ว |
| SSL 3.0 | 1996 | Deprecated / ยกเลิกแล้ว |
| TLS 1.0 | 1999 | Deprecated / ยกเลิกแล้ว |
| TLS 1.1 | 2006 | Deprecated / ยกเลิกแล้ว |
| TLS 1.2 | 2008 | Widely used / ใช้งานอย่างแพร่หลาย |
| TLS 1.3 | 2018 | Current standard / มาตรฐานปัจจุบัน |

---

## TLS Certificate Components / ส่วนประกอบของใบรับรอง TLS

A TLS certificate is a digital document that binds a public key to an identity (such as a domain name). It is issued by a trusted Certificate Authority (CA).

ใบรับรอง TLS คือเอกสารดิจิทัลที่ผูกคีย์สาธารณะกับตัวตน (เช่น ชื่อโดเมน) ออกโดยหน่วยงานที่เชื่อถือได้เรียกว่า Certificate Authority (CA)

### Key Components / ส่วนประกอบหลัก

- **Public Key / คีย์สาธารณะ**: Used to encrypt data. This key is shared openly with anyone who wants to send encrypted data to the server.
  - ใช้เพื่อเข้ารหัสข้อมูล คีย์นี้ถูกแชร์อย่างเปิดเผยกับ任何人ที่ต้องการส่งข้อมูลที่เข้ารหัสไปยังเซิร์ฟเวอร์

- **Private Key / คีย์ส่วนตัว**: Used to decrypt data. This key must be kept secret and stored securely on the server. If compromised, the certificate must be revoked.
  - ใช้เพื่อถอดรหัสข้อมูล คีย์นี้ต้องเก็บเป็นความลับและจัดเก็บอย่างปลอดภัยบนเซิร์ฟเวอร์ หากถูกเจาะ ใบรับรองต้องถูกเพิกถอน

- **Certificate / ใบรับรอง**: A digital file that binds the public key to an identity (domain name, organization). It is digitally signed by a Certificate Authority (CA).
  - ไฟล์ดิจิทัลที่ผูกคีย์สาธารณะกับตัวตน (ชื่อโดเมน, องค์กร) ถูกเซ็นดิจิทัลโดย Certificate Authority (CA)

- **CA (Certificate Authority) / ผู้มีอำนาจออกใบรับรอง**: A trusted third-party entity that verifies the identity of the certificate requester and issues the certificate. Examples: Let's Encrypt, DigiCert, GlobalSign.
  - หน่วยงานบุคคลที่สามที่เชื่อถือได้ที่ยืนยันตัวตนของผู้ขอใบรับรองและออกใบรับรอง ตัวอย่าง: Let's Encrypt, DigiCert, GlobalSign

### Certificate Structure / โครงสร้างใบรับรอง

A typical X.509 certificate contains:
ใบรับรอง X.509 โดยทั่วไปประกอบด้วย:

- **Subject / เรื่อง**: The entity the certificate is issued for (e.g., `CN=example.com`)
  - หน่วยงานที่ใบรับรองถูกออกให้ (เช่น `CN=example.com`)
- **Issuer / ผู้ออก**: The CA that issued the certificate
  - CA ที่ออกใบรับรอง
- **Validity Period / ระยะเวลาใช้งาน**: `Not Before` and `Not After` dates
  - วันที่ `เริ่มใช้งาน` และ `หมดอายุ`
- **Serial Number / หมายเลขซีเรียล**: Unique identifier for the certificate
  - ตัวระบุเฉพาะสำหรับใบรับรอง
- **Public Key / คีย์สาธารณะ**: The server's public key
  - คีย์สาธารณะของเซิร์ฟเวอร์
- **Signature Algorithm / อัลกอริทึมลายเซ็น**: Algorithm used by CA to sign the certificate
  - อัลกอริทึมที่ CA ใช้เซ็นใบรับรอง
- **Digital Signature / ลายเซ็นดิจิทัล**: CA's signature proving the certificate's authenticity
  - ลายเซ็นของ CA ที่พิสูจน์ความถูกต้องของใบรับรอง

---

## How TLS Works (Simplified) / TLS ทำงานอย่างไร (แบบง่าย)

The TLS handshake process establishes a secure, encrypted connection between a client and a server.

กระบวนการ TLS handshake สร้างการเชื่อมต่อที่ปลอดภัยและเข้ารหัสระหว่างไคลเอนต์และเซิร์ฟเวอร์

```
1. Client connects to Server
   ไคลเอนต์เชื่อมต่อกับเซิร์ฟเวอร์

2. Server sends Certificate (with public key)
   เซิร์ฟเวอร์ส่งใบรับรอง (พร้อมคีย์สาธารณะ)

3. Client verifies Certificate (trusts CA, checks expiry, validates domain)
   ไคลเอนต์ตรวจสอบใบรับรอง (เชื่อถือ CA, ตรวจสอบวันหมดอายุ, ตรวจสอบโดเมน)

4. Client and Server agree on encryption method (TLS Handshake)
   ไคลเอนต์และเซิร์ฟเวอร์ตกลงวิธีการเข้ารหัส (TLS Handshake)

5. Encrypted communication begins using symmetric encryption
   การสื่อสารที่เข้ารหัสเริ่มต้นโดยใช้การเข้ารหัสแบบสมมาตร
```

### Detailed TLS Handshake / TLS Handshake แบบละเอียด

```
Step 1: Client Hello / ไคลเอนต์ส่ง Hello
  - Client sends supported TLS versions, cipher suites, and a random number
  - ไคลเอนต์ส่งเวอร์ชัน TLS ที่รองรับ, ชุด cipher, และตัวเลขสุ่ม

Step 2: Server Hello / เซิร์ฟเวอร์ตอบ Hello
  - Server responds with chosen TLS version, cipher suite, and its certificate
  - เซิร์ฟเวอร์ตอบด้วยเวอร์ชัน TLS ที่เลือก, ชุด cipher, และใบรับรอง

Step 3: Certificate Verification / ตรวจสอบใบรับรอง
  - Client verifies the certificate chain up to a trusted root CA
  - ไคลเอนต์ตรวจสอบห่วงโซ่ใบรับรองจนถึง root CA ที่เชื่อถือ

Step 4: Key Exchange / แลกเปลี่ยนคีย์
  - Client and server generate shared session keys (using Diffie-Hellman or RSA)
  - ไคลเอนต์และเซิร์ฟเวอร์สร้างเซสชันคีย์ร่วม (ใช้ Diffie-Hellman หรือ RSA)

Step 5: Encrypted Communication / การสื่อสารเข้ารหัส
  - Both parties switch to symmetric encryption for fast, secure data transfer
  - ทั้งสองฝ่ายเปลี่ยนเป็นการเข้ารหัสแบบสมมาตรสำหรับการส่งข้อมูลที่รวดเร็วและปลอดภัย
```

---

## Types of Certificates / ประเภทของใบรับรอง

Certificates come in different types depending on the number of domains and validation level required.

ใบรับรองมีหลายประเภทขึ้นอยู่กับจำนวนโดเมนและระดับการตรวจสอบที่ต้องการ

### By Domain Coverage / ตามประเภทครอบคลุมโดเมน

| Type / ประเภท | Description / คำอธิบาย | Example / ตัวอย่าง |
|---------------|------------------------|-------------------|
| **Single Domain / โดเมนเดียว** | Covers exactly one domain or subdomain / ครอบคลุมหนึ่งโดเมนหรือซับโดเมน | `example.com` |
| **Wildcard / ไวลด์การ์ด** | Covers a domain and ALL its subdomains / ครอบคลุมโดเมนและซับโดเมนทั้งหมด | `*.example.com` (covers `api.example.com`, `mail.example.com`, etc.) |
| **Multi-Domain (SAN) / หลายโดเมน** | Covers multiple different domains in one certificate / ครอบคลุมหลายโดเมนที่แตกต่างกันในใบรับรองเดียว | `example.com`, `test.com`, `api.org` |

### By Validation Level / ตามระดับการตรวจสอบ

| Type / ประเภท | Validation / การตรวจสอบ | Use Case / กรณีใช้งาน | Issuance Time / เวลาออก |
|---------------|------------------------|----------------------|------------------------|
| **DV (Domain Validation)** | Verifies domain ownership only / ตรวจสอบความเป็นเจ้าของโดเมนเท่านั้น | Personal websites, blogs / เว็บไซต์ส่วนตัว, บล็อก | Minutes / นาที |
| **OV (Organization Validation)** | Verifies domain + organization identity / ตรวจสอบโดเมน + ตัวตนองค์กร | Business websites / เว็บไซต์ธุรกิจ | 1-3 days / วัน |
| **EV (Extended Validation)** | Thorough verification of organization / ตรวจสอบองค์กรอย่างละเอียด | Banking, e-commerce / ธนาคาร, อีคอมเมิร์ซ | 3-7 days / วัน |

> **Note / หมายเหตุ**: Modern browsers no longer show the green bar for EV certificates, but the certificate details still contain the validated organization name.
> เบราว์เซอร์สมัยใหม่ไม่แสดงแถบเขียวสำหรับใบรับรอง EV อีกต่อไป แต่รายละเอียดใบรับรองยังคงมีชื่อองค์กรที่ตรวจสอบแล้ว

---

## ACME Protocol / โปรโตคอล ACME

**ACME** (Automated Certificate Management Environment) is a protocol defined by [RFC 8555](https://datatracker.ietf.org/doc/html/rfc8555) that automates the issuance, renewal, and management of TLS certificates. Let's Encrypt is the most widely used ACME Certificate Authority.

**ACME** (Automated Certificate Management Environment) คือโปรโตคอลที่กำหนดโดย [RFC 8555](https://datatracker.ietf.org/doc/html/rfc8555) ที่จัดการการออก, ต่ออายุ, และจัดการใบรับรอง TLS อัตโนมัติ Let's Encrypt คือ Certificate Authority ที่ใช้ ACME อย่างแพร่หลายที่สุด

### ACME Workflow / ขั้นตอนการทำงานของ ACME

```
1. Client requests certificate from ACME server
   ไคลเอนต์ขอใบรับรองจากเซิร์ฟเวอร์ ACME

2. ACME server issues a "challenge" to prove domain ownership
   เซิร์ฟเวอร์ ACME ออก "ความท้าทาย" เพื่อพิสูจน์ความเป็นเจ้าของโดเมน

3. Client completes the challenge (places file, adds DNS record, etc.)
   ไคลเอนต์.complete ความท้าทาย (วางไฟล์, เพิ่ม DNS record ฯลฯ)

4. ACME server verifies the challenge
   เซิร์ฟเวอร์ ACME ตรวจสอบความท้าทาย

5. ACME server issues the certificate
   เซิร์ฟเวอร์ ACME ออกใบรับรอง
```

### Challenge Types / ประเภทความท้าทาย

#### HTTP-01 Challenge / ความท้าทาย HTTP-01

- **How it works / วิธีการทำงาน**: Proves domain control by placing a specific file at `http://<domain>/.well-known/acme-challenge/<token>`. The ACME server fetches this file to verify ownership.
  - พิสูจน์การควบคุมโดเมนโดยการวางไฟล์เฉพาะที่ `http://<domain>/.well-known/acme-challenge/<token>` เซิร์ฟเวอร์ ACME ดึงไฟล์นี้เพื่อยืนยันความเป็นเจ้าของ

- **Requirements / ข้อกำหนด**:
  - HTTP server must be running and accessible on port 80 / เซิร์ฟเวอร์ HTTP ต้องทำงานและเข้าถึงได้บนพอร์ต 80
  - The challenge file must be publicly accessible / ไฟล์ความท้าทายต้องเข้าถึงได้สาธารณะ
  - Domain must resolve to the server / โดเมนต้องชี้ไปยังเซิร์ฟเวอร์

- **Limitations / ข้อจำกัด**:
  - **Does NOT support wildcard certificates / ไม่รองรับใบรับรองไวลด์การ์ด**
  - Requires port 80 to be open / ต้องเปิดพอร์ต 80

- **Best for / เหมาะสำหรับ**: Standard web domains, single or multi-domain certificates / โดเมนเว็บมาตรฐาน, ใบรับรองโดเมนเดียวหรือหลายโดเมน

#### DNS-01 Challenge / ความท้าทาย DNS-01

- **How it works / วิธีการทำงาน**: Proves domain control by adding a specific TXT record to the domain's DNS zone. The ACME server queries DNS to verify the record exists.
  - พิสูจน์การควบคุมโดเมนโดยการเพิ่ม TXT record เฉพาะในโซน DNS ของโดเมน เซิร์ฟเวอร์ ACME ตรวจสอบ DNS เพื่อยืนยันว่า record มีอยู่จริง

- **Requirements / ข้อกำหนด**:
  - Access to DNS provider's API (or manual DNS editing) / เข้าถึง API ของผู้ให้บริการ DNS (หรือแก้ไข DNS ด้วยตนเอง)
  - Must be able to create `_acme-challenge.<domain>` TXT record / ต้องสร้าง `_acme-challenge.<domain>` TXT record ได้

- **Advantages / ข้อดี**:
  - **Supports wildcard certificates (`*.example.com`) / รองรับใบรับรองไวลด์การ์ด**
  - More secure (no HTTP exposure, no need for port 80) / ปลอดภัยกว่า (ไม่เปิด HTTP, ไม่ต้องใช้พอร์ต 80)
  - Works for internal domains / ใช้ได้กับโดเมนภายใน

- **Limitations / ข้อจำกัด**:
  - Requires DNS API credentials or manual intervention / ต้องใช้ข้อมูลรับรอง DNS API หรือการแทรกแซงด้วยตนเอง
  - DNS propagation delay may slow issuance / การดีเลย์ของการแพร่กระจาย DNS อาจทำให้การออกช้าลง

#### TLS-ALPN-01 Challenge / ความท้าทาย TLS-ALPN-01

- **How it works / วิธีการทำงาน**: Proves domain control via a special TLS handshake using the Application-Layer Protocol Negotiation (ALPN) extension. The server responds with a self-signed certificate containing the challenge token during the TLS handshake.
  - พิสูจน์การควบคุมโดเมนผ่าน TLS handshake พิเศษโดยใช้ส่วนขยาย Application-Layer Protocol Negotiation (ALPN) เซิร์ฟเวอร์ตอบกลับด้วยใบรับรองที่เซ็นตัวเองที่มีโทเค็นความท้าทายระหว่าง TLS handshake

- **Requirements / ข้อกำหนด**:
  - Requires access to port 443 / ต้องเข้าถึงพอร์ต 443
  - Server must support ALPN extension / เซิร์ฟเวอร์ต้องรองรับส่วนขยาย ALPN

- **Use cases / กรณีใช้งาน**:
  - Used by some embedded devices and reverse proxies / ใช้โดยอุปกรณ์ฝังตัวและ reverse proxy บางตัว
  - Less common than HTTP-01 and DNS-01 / ไม่ค่อยพบบ่อยเท่า HTTP-01 และ DNS-01

### Challenge Comparison / เปรียบเทียบความท้าทาย

| Feature / คุณสมบัติ | HTTP-01 | DNS-01 | TLS-ALPN-01 |
|---------------------|---------|--------|-------------|
| Wildcard support / รองรับไวลด์การ์ด | No / ไม่ใช่ | Yes / ใช่ | No / ไม่ใช่ |
| Port required / พอร์ตที่ต้องการ | 80 (HTTP) | None / ไม่มี | 443 (TLS) |
| DNS API needed / ต้องใช้ DNS API | No / ไม่ | Yes / ใช่ | No / ไม่ |
| Security / ความปลอดภัย | Moderate / ปานกลาง | High / สูง | High / สูง |
| Automation / อัตโนมัติ | Easy / ง่าย | Moderate / ปานกลาง | Moderate / ปานกลาง |

---

## Practical Task: Issue TLS Certificate with Let's Encrypt / ปฏิบัติการ: ออกใบรับรอง TLS ด้วย Let's Encrypt

### Prerequisites / ข้อกำหนดเบื้องต้น

- A registered domain name pointing to your server / โดเมนที่ลงทะเบียนแล้วชี้ไปยังเซิร์ฟเวอร์ของคุณ
- A server with public internet access / เซิร์ฟเวอร์ที่มีการเข้าถึงอินเทอร์เน็ตสาธารณะ
- Root or sudo access / สิทธิ์ root หรือ sudo

---

### Method 1: Using Certbot (HTTP-01) / วิธีที่ 1: ใช้ Certbot (HTTP-01)

Certbot is the most popular ACME client developed by the Electronic Frontier Foundation (EFF).

Certbot คือไคลเอนต์ ACME ที่นิยมมากที่สุดพัฒนาโดย Electronic Frontier Foundation (EFF)

#### Step 1: Install Certbot / ขั้นตอนที่ 1: ติดตั้ง Certbot

```bash
# Update package list / อัพเดทรายการแพ็กเกจ
apt update

# Install Certbot / ติดตั้ง Certbot
apt install certbot -y

# Verify installation / ตรวจสอบการติดตั้ง
certbot --version
```

#### Step 2: Issue Certificate - Standalone Mode / ขั้นตอนที่ 2: ออกใบรับรอง - โหมด Standalone

Standalone mode runs a temporary web server on port 80 for validation. You must stop any existing web server first.

โหมด Standalone จะเรียกใช้เว็บเซิร์ฟเวอร์ชั่วคราวบนพอร์ต 80 สำหรับการตรวจสอบ คุณต้องหยุดเว็บเซิร์ฟเวอร์ที่มีอยู่ก่อน

```bash
# Stop existing web server (if running) / หยุดเว็บเซิร์ฟเวอร์ที่มีอยู่ (ถ้าทำงานอยู่)
systemctl stop nginx
# หรือ / or
systemctl stop apache2

# Issue certificate using standalone mode / ออกใบรับรองโดยใช้โหมด standalone
# Replace 'yourdomain.com' with your actual domain / แทนที่ 'yourdomain.com' ด้วยโดเมนจริงของคุณ
certbot certonly --standalone -d yourdomain.com

# For multiple domains / สำหรับหลายโดเมน
certbot certonly --standalone -d yourdomain.com -d www.yourdomain.com
```

#### Step 3: Issue Certificate - Webroot Mode / ขั้นตอนที่ 3: ออกใบรับรอง - โหมด Webroot

Webroot mode uses an existing web server to serve the challenge files. No need to stop the server.

โหมด Webroot ใช้เว็บเซิร์ฟเวอร์ที่มีอยู่เพื่อให้บริการไฟล์ความท้าทาย ไม่จำเป็นต้องหยุดเซิร์ฟเวอร์

```bash
# Issue certificate using webroot mode / ออกใบรับรองโดยใช้โหมด webroot
# -w specifies the web root directory / -w ระบุไดเรกทอรีรากของเว็บ
certbot certonly --webroot -w /var/www/html -d yourdomain.com

# For multiple domains with different web roots / สำหรับหลายโดเมนที่มีรากเว็บต่างกัน
certbot certonly --webroot \
  -w /var/www/html -d yourdomain.com \
  -w /var/www/another-site -d anotherdomain.com
```

#### Step 4: Verify Certificate / ขั้นตอนที่ 4: ตรวจสอบใบรับรอง

```bash
# List all managed certificates / แสดงใบรับรองทั้งหมดที่จัดการ
certbot certificates

# Check certificate details / ตรวจสอบรายละเอียดใบรับรอง
certbot show-account
```

---

### Method 2: Using Certbot (DNS-01) / วิธีที่ 2: ใช้ Certbot (DNS-01)

DNS-01 challenge is required for wildcard certificates and offers better security.

ความท้าทาย DNS-01 จำเป็นสำหรับใบรับรองไวลด์การ์ดและให้ความปลอดภัยที่ดีกว่า

#### Step 1: Install DNS Plugin / ขั้นตอนที่ 1: ติดตั้งปลั๊กอิน DNS

Each DNS provider has its own Certbot plugin. Here we use Cloudflare as an example.

ผู้ให้บริการ DNS แต่ละรายมีปลั๊กอิน Certbot ของตัวเอง ที่นี่เราใช้ Cloudflare เป็นตัวอย่าง

```bash
# Install Cloudflare DNS plugin for Certbot / ติดตั้งปลั๊กอิน DNS Cloudflare สำหรับ Certbot
apt install python3-certbot-dns-cloudflare -y

# Other popular DNS plugins / ปลั๊กอิน DNS อื่นๆ ที่นิยม:
# apt install python3-certbot-dns-route53    # AWS Route 53
# apt install python3-certbot-dns-google     # Google Cloud DNS
# apt install python3-certbot-dns-azure      # Azure DNS
```

#### Step 2: Create Credentials File / ขั้นตอนที่ 2: สร้างไฟล์ข้อมูลรับรอง

Create a credentials file with your DNS provider's API credentials.

สร้างไฟล์ข้อมูลรับรองด้วยข้อมูลรับรอง API ของผู้ให้บริการ DNS ของคุณ

```bash
# Create credentials file for Cloudflare / สร้างไฟล์ข้อมูลรับรองสำหรับ Cloudflare
cat > cloudflare.ini << 'EOF'
# For Cloudflare with API Token (recommended) / สำหรับ Cloudflare ด้วย API Token (แนะนำ)
dns_cloudflare_api_token = your-api-token-here

# OR for Cloudflare with Global API Key (less secure) / หรือสำหรับ Cloudflare ด้วย Global API Key (ปลอดภัยน้อยกว่า)
# dns_cloudflare_email = your-email@example.com
# dns_cloudflare_api_key = your-global-api-key
EOF

# Set restrictive permissions (owner read/write only) / ตั้งค่าสิทธิ์จำกัด (เจ้าของอ่าน/เขียนเท่านั้น)
chmod 600 cloudflare.ini

# IMPORTANT: Never commit this file to version control! / สำคัญ: อย่า commit ไฟล์นี้ไปยัง version control!
echo "cloudflare.ini" >> .gitignore
```

#### Step 3: Issue Wildcard Certificate / ขั้นตอนที่ 3: ออกใบรับรองไวลด์การ์ด

```bash
# Issue wildcard certificate using DNS-01 challenge / ออกใบรับรองไวลด์การ์ดโดยใช้ความท้าทาย DNS-01
certbot certonly --dns-cloudflare \
  --dns-cloudflare-credentials cloudflare.ini \
  -d yourdomain.com \
  -d '*.yourdomain.com'

# Note: DNS propagation may take a few minutes / หมายเหตุ: การแพร่กระจาย DNS อาจใช้เวลาไม่กี่นาที
# Certbot will automatically wait for propagation / Certbot จะรอการแพร่กระจายอัตโนมัติ
```

#### Step 4: Verify / ขั้นตอนที่ 4: ตรวจสอบ

```bash
# Check issued certificates / ตรวจสอบใบรับรองที่ออก
certbot certificates

# Expected output should show your domain and wildcard domain / ผลลัพธ์ที่คาดหวังควรแสดงโดเมนและโดเมนไวลด์การ์ด
# Certificate Name: yourdomain.com
#   Domains: yourdomain.com *.yourdomain.com
#   Expiry Date: 2026-07-10 12:00:00+00:00 (VALID: 89 days)
```

---

### Certificate Files Location / ตำแหน่งไฟล์ใบรับรอง

After issuance, certificates are stored in the Let's Encrypt directory:
หลังการออก ใบรับรองจะถูกเก็บในไดเรกทอรี Let's Encrypt:

```
/etc/letsencrypt/live/yourdomain.com/
├── cert.pem      # Server certificate only / ใบรับรองเซิร์ฟเวอร์เท่านั้น
├── chain.pem     # Certificate chain (intermediate + root CA) / ห่วงโซ่ใบรับรอง (intermediate + root CA)
├── fullchain.pem # cert.pem + chain.pem (use this in web servers) / cert.pem + chain.pem (ใช้ไฟล์นี้ในเว็บเซิร์ฟเวอร์)
└── privkey.pem   # Private key (KEEP SECRET!) / คีย์ส่วนตัว (เก็บเป็นความลับ!)

/etc/letsencrypt/archive/yourdomain.com/
├── cert1.pem     # First version of certificate / ใบรับรองเวอร์ชันแรก
├── cert2.pem     # Renewed certificate / ใบรับรองที่ต่ออายุ
└── ...           # Previous versions are kept for rollback / เวอร์ชันก่อนหน้าถูกเก็บไว้สำหรับการ rollback

/etc/letsencrypt/renewal/yourdomain.com.conf
└── Renewal configuration file / ไฟล์กำหนดค่าการต่ออายุ
```

> **Important / สำคัญ**: The `live/` directory contains symlinks to the latest versions in `archive/`. Always use the `live/` paths in your web server configuration.
> ไดเรกทอรี `live/` มี symlinks ไปยังเวอร์ชันล่าสุดใน `archive/` ใช้พาธ `live/` ในการกำหนดค่าเว็บเซิร์ฟเวอร์เสมอ

---

### View Certificate Details / ดูรายละเอียดใบรับรอง

```bash
# Check certificate expiry date / ตรวจสอบวันหมดอายุใบรับรอง
openssl x509 -enddate -noout -in /etc/letsencrypt/live/yourdomain.com/cert.pem
# Output: notAfter=Jul 10 12:00:00 2026 GMT

# View full certificate details in text format / ดูรายละเอียดใบรับรองเต็มรูปแบบในรูปแบบข้อความ
openssl x509 -text -noout -in /etc/letsencrypt/live/yourdomain.com/cert.pem

# Check specific fields / ตรวจสอบฟิลด์เฉพาะ
# View subject (domain names) / ดู subject (ชื่อโดเมน)
openssl x509 -subject -noout -in /etc/letsencrypt/live/yourdomain.com/cert.pem

# View issuer (CA name) / ดูผู้ออก (ชื่อ CA)
openssl x509 -issuer -noout -in /etc/letsencrypt/live/yourdomain.com/cert.pem

# View serial number / ดูหมายเลขซีเรียล
openssl x509 -serial -noout -in /etc/letsencrypt/live/yourdomain.com/cert.pem

# Check certificate from a remote server (simulate client connection) / ตรวจสอบใบรับรองจากเซิร์ฟเวอร์ระยะไกล (จำลองการเชื่อมต่อไคลเอนต์)
openssl s_client -connect yourdomain.com:443 -servername yourdomain.com </dev/null 2>/dev/null | openssl x509 -noout -dates

# Check certificate expiry with human-readable format / ตรวจสอบวันหมดอายุใบรับรองในรูปแบบอ่านง่าย
echo | openssl s_client -connect yourdomain.com:443 2>/dev/null | openssl x509 -noout -dates

# Check how many days until expiry (useful for monitoring scripts) / ตรวจสอบจำนวนวันจนกว่าจะหมดอายุ (มีประโยชน์สำหรับสคริปต์ตรวจสอบ)
echo | openssl s_client -connect yourdomain.com:443 2>/dev/null | \
  openssl x509 -noout -enddate | \
  cut -d= -f2 | \
  xargs -I{} date -d {} +%s | \
  xargs -I{} expr {} - $(date +%s) | \
  xargs -I{} expr {} / 86400
```

---

## Certificate Lifecycle / วงจรชีวิตใบรับรอง

Understanding the certificate lifecycle is crucial for proper TLS management.

การเข้าใจวงจรชีวิตใบรับรองเป็นสิ่งสำคัญสำหรับการจัดการ TLS ที่เหมาะสม

```
┌──────────────────────────────────────────────────────────────────────┐
│                    Certificate Lifecycle / วงจรชีวิตใบรับรอง           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. Request / ขอ             → Generate CSR and private key          │
│                                 สร้าง CSR และคีย์ส่วนตัว              │
│                                                                      │
│  2. Validate / ตรวจสอบ        → Complete ACME challenge (HTTP/DNS)   │
│                                 ทำความท้าทาย ACME ให้เสร็จ (HTTP/DNS) │
│                                                                      │
│  3. Issue / ออก               → CA issues certificate                │
│                                 CA ออกใบรับรอง                         │
│                                                                      │
│  4. Install / ติดตั้ง         → Configure web server with cert       │
│                                 กำหนดค่าเว็บเซิร์ฟเวอร์ด้วยใบรับรอง    │
│                                                                      │
│  5. Monitor / ติดตาม          → Track expiry dates                   │
│                                 ติดตามวันหมดอายุ                        │
│                                                                      │
│  6. Auto-renew / ต่ออายุอัตโนมัติ → Renew before expiry (~60 days)   │
│                                       ต่ออายุ قبلหมดอายุ (~60 วัน)     │
│                                                                      │
│  7. Deploy / ปรับใช้          → Reload web server with new cert      │
│                                 โหลดเว็บเซิร์ฟเวอร์ใหม่ด้วยใบรับรองใหม่ │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

### Key Facts / ข้อเท็จจริงสำคัญ

- **Let's Encrypt certificates expire in 90 days / ใบรับรอง Let's Encrypt หมดอายุใน 90 วัน**
  - This is intentional to encourage automation / นี่เป็นไปโดยเจตนาเพื่อส่งเสริมการใช้ระบบอัตโนมัติ

- **Auto-renewal should be attempted every 60 days / ควรพยายามต่ออายุอัตโนมัติทุก 60 วัน**
  - Certbot checks twice daily by default / Certbot ตรวจสอบสองครั้งต่อวันโดยค่าเริ่มต้น
  - Renewal only happens if certificate is within 30 days of expiry / การต่ออายุจะเกิดขึ้นเฉพาะเมื่อใบรับรองอยู่ภายใน 30 วันก่อนหมดอายุ

- **Certbot can automate renewal with systemd timer or cron / Certbot สามารถทำให้การต่ออายุอัตโนมัติด้วย systemd timer หรือ cron**
  - systemd timer is the recommended method on modern Linux / systemd timer คือวิธีที่แนะนำบน Linux สมัยใหม่

### Certificate States / สถานะของใบรับรอง

| State / สถานะ | Description / คำอธิบาย | Action / การกระทำ |
|---------------|------------------------|-------------------|
| **Valid / ใช้งานได้** | Certificate is within validity period / ใบรับรองอยู่ในระยะเวลาใช้งาน | Normal operation / ทำงานปกติ |
| **Expiring Soon / ใกล้หมดอายุ** | Less than 30 days until expiry / น้อยกว่า 30 วันจนหมดอายุ | Renewal should happen / ควรต่ออายุ |
| **Expired / หมดอายุ** | Past the `Not After` date / เกินวันที่ `Not After` | Browsers show warning / เบราว์เซอร์แสดงคำเตือน |
| **Revoked / เพิกถอน** | Certificate invalidated by CA / ใบรับรองถูกทำให้เสียโดย CA | Get new certificate immediately / รับใบรับรองใหม่ทันที |

---

## Auto-Renewal Setup / การตั้งค่าการต่ออายุอัตโนมัติ

Automatic renewal ensures your certificates never expire, maintaining continuous secure connections.

การต่ออายุอัตโนมัติช่วยให้ใบรับรองของคุณไม่มีวันหมดอายุ รักษาการเชื่อมต่อที่ปลอดภัยอย่างต่อเนื่อง

### Method 1: Using systemd Timer (Recommended) / วิธีที่ 1: ใช้ systemd Timer (แนะนำ)

Modern Linux distributions with Certbot installed typically have a systemd timer set up automatically.

การแจกจ่าย Linux สมัยใหม่ที่มี Certbot ติดตั้งมักจะมี systemd timer ตั้งค่าอัตโนมัติ

```bash
# Check if certbot timer is active / ตรวจสอบว่า timer ของ certbot ทำงานอยู่หรือไม่
systemctl status certbot.timer

# Expected output shows timer is active / ผลลัพธ์ที่คาดหวังแสดงว่า timer ทำงานอยู่
# ● certbot.timer - Run certbot twice daily
#      Loaded: loaded (/lib/systemd/system/certbot.timer; enabled)
#      Active: active (waiting) since ...

# Enable and start the timer (if not already active) / เปิดใช้งานและเริ่ม timer (ถ้ายังไม่ทำงาน)
systemctl enable certbot.timer
systemctl start certbot.timer

# View timer details / ดูรายละเอียด timer
systemctl list-timers certbot.timer

# Check timer configuration file / ดูไฟล์กำหนดค่า timer
cat /lib/systemd/system/certbot.timer
```

### Method 2: Using Cron / วิธีที่ 2: ใช้ Cron

If systemd is not available, use cron for automatic renewal.

ถ้า systemd ไม่สามารถใช้ cron สำหรับการต่ออายุอัตโนมัติ

```bash
# Edit root's crontab / แก้ไข crontab ของ root
sudo crontab -e

# Add the following line to run twice daily (at midnight and noon) / เพิ่มบรรทัดต่อไปนี้เพื่อรันสองครั้งต่อวัน (เวลาเที่ยงคืนและเที่ยงวัน)
# This checks for renewal and only renews if within 30 days of expiry / ตรวจสอบการต่ออายุและต่ออายุเฉพาะถ้าอยู่ภายใน 30 วันก่อนหมดอายุ
0 0,12 * * * certbot renew --quiet --deploy-hook "systemctl reload nginx"

# Explanation of the cron line / คำอธิบายบรรทัด cron:
# 0 0,12 * * *           → Run at minute 0 of hour 0 and 12 every day / รันที่นาที 0 ของชั่วโมง 0 และ 12 ทุกวัน
# certbot renew           → Check and renew all managed certificates / ตรวจสอบและต่ออายุใบรับรองทั้งหมดที่จัดการ
# --quiet                 → Suppress output unless there's an error / ปิดเสียงยกเว้นมีข้อผิดพลาด
# --deploy-hook           → Run command after successful renewal / รันคำสั่งหลังต่ออายุสำเร็จ
# "systemctl reload nginx" → Reload Nginx to use the new certificate / โหลด Nginx ใหม่เพื่อใช้ใบรับรองใหม่
```

### Testing Auto-Renewal / การทดสอบการต่ออายุอัตโนมัติ

```bash
# Dry-run renewal (simulates renewal without actually renewing) / ทดสอบต่ออายุ (จำลองการต่ออายุโดยไม่ต่ออายุจริง)
certbot renew --dry-run

# Expected output should show "Congratulations, all simulated renewals succeeded." / ผลลัพธ์ที่คาดหวังควรแสดง "ขอแสดงความพอใจ, การจำลองการต่ออายุทั้งหมดสำเร็จ"

# Force renewal (use with caution - counts against rate limits) / บังคับต่ออายุ (ใช้ด้วยความระวัง - นับต่ออัตราจำกัด)
certbot renew --force-renewal
```

> **Rate Limits / อัตราจำกัด**: Let's Encrypt has rate limits. Duplicate Certificate limit is 5 per week per exact set of domains. Use `--dry-run` for testing.
> Let's Encrypt มีอัตราจำกัด ขอบเขตใบรับรองซ้ำคือ 5 ครั้งต่อสัปดาห์ต่อชุดโดเมนที่เหมือนกัน ใช้ `--dry-run` สำหรับการทดสอบ

---

## Nginx Configuration with TLS / การกำหนดค่า Nginx พร้อม TLS

### Basic TLS Configuration / การกำหนดค่า TLS พื้นฐาน

```nginx
# HTTPS server block / บล็อกเซิร์ฟเวอร์ HTTPS
server {
    # Listen on port 443 with SSL and HTTP/2 enabled / ฟังบนพอร์ต 443 พร้อมเปิดใช้งาน SSL และ HTTP/2
    listen 443 ssl http2;
    listen [::]:443 ssl http2;  # IPv6 support / รองรับ IPv6

    # Server domain name / ชื่อโดเมนเซิร์ฟเวอร์
    server_name yourdomain.com www.yourdomain.com;

    # SSL certificate paths (use fullchain.pem, NOT cert.pem) / พาธใบรับรอง SSL (ใช้ fullchain.pem, ไม่ใช่ cert.pem)
    # fullchain.pem includes the server cert + intermediate chain / fullchain.pem รวมใบรับรองเซิร์ฟเวอร์ + intermediate chain
    ssl_certificate     /etc/letsencrypt/live/yourdomain.com/fullchain.pem;

    # Private key file (keep this secure!) / ไฟล์คีย์ส่วนตัว (เก็บไฟล์นี้ให้ปลอดภัย!)
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

    # SSL protocol versions (only use TLS 1.2 and 1.3) / เวอร์ชันโปรโตคอล SSL (ใช้เฉพาะ TLS 1.2 และ 1.3)
    # TLS 1.0 and 1.1 are deprecated and insecure / TLS 1.0 และ 1.1 ถูกยกเลิกและไม่ปลอดภัย
    ssl_protocols TLSv1.2 TLSv1.3;

    # SSL cipher suites (strong ciphers only) / ชุด cipher SSL (เฉพาะ cipher ที่แข็งแกร่ง)
    # HIGH = strong ciphers, !aNULL = no anonymous, !MD5 = no MD5 hash / HIGH = cipher แข็งแกร่ง, !aNULL = ไม่มี anonymous, !MD5 = ไม่มี hash MD5
    ssl_ciphers HIGH:!aNULL:!MD5;

    # Prefer server ciphers over client ciphers / ต้องการ cipher ของเซิร์ฟเวอร์มากกว่า cipher ของไคลเอนต์
    ssl_prefer_server_ciphers on;

    # SSL session settings (improves performance for returning visitors) / การตั้งค่าเซสชัน SSL (ปรับปรุงประสิทธิภาพสำหรับผู้เยี่ยมชมที่กลับมา)
    ssl_session_cache shared:SSL:10m;    # Shared cache for all server blocks / แคชร่วมสำหรับบล็อกเซิร์ฟเวอร์ทั้งหมด
    ssl_session_timeout 10m;             # Session valid for 10 minutes / เซสชันใช้ได้ 10 นาที
    ssl_session_tickets off;             # Disable session tickets for better security / ปิด session tickets เพื่อความปลอดภัยที่ดีขึ้น

    # OCSP Stapling (improves TLS handshake performance) / OCSP Stapling (ปรับปรุงประสิทธิภาพ TLS handshake)
    # OCSP allows server to send certificate status to client / OCSP ให้เซิร์ฟเวอร์ส่งสถานะใบรับรองไปยังไคลเอนต์
    ssl_stapling on;
    ssl_stapling_verify on;
    ssl_trusted_certificate /etc/letsencrypt/live/yourdomain.com/chain.pem;

    # DNS resolver for OCSP stapling / ตัวแก้ไข DNS สำหรับ OCSP Stapling
    resolver 8.8.8.8 8.8.4.4 valid=300s;
    resolver_timeout 5s;

    # Security headers / ส่วนหัวความปลอดภัย
    # HSTS: Force browsers to use HTTPS for 1 year / HSTS: บังคับเบราว์เซอร์ให้ใช้ HTTPS เป็นเวลา 1 ปี
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;

    # Prevent clickjacking / ป้องกัน clickjacking
    add_header X-Frame-Options SAMEORIGIN always;

    # Prevent MIME-type sniffing / ป้องกันการ sniff MIME-type
    add_header X-Content-Type-Options nosniff always;

    # Enable XSS protection / เปิดใช้งานการป้องกัน XSS
    add_header X-XSS-Protection "1; mode=block" always;

    # Reverse proxy configuration / การกำหนดค่า reverse proxy
    location / {
        proxy_pass http://localhost:3000;        # Forward to backend / ส่งต่อไปยัง backend
        proxy_set_header Host $host;              # Pass original host / ส่ง host ดั้งเดิม
        proxy_set_header X-Real-IP $remote_addr;  # Pass real client IP / ส่ง IP ไคลเอนต์จริง
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;  # Forwarded IPs / IPs ที่ส่งต่อ
        proxy_set_header X-Forwarded-Proto $scheme;  # Original protocol (http/https) / โปรโตค ลดั้งเดิม
    }
}

# HTTP to HTTPS redirect / เปลี่ยนเส้นทาง HTTP เป็น HTTPS
server {
    # Listen on port 80 (HTTP) / ฟังบนพอร์ต 80 (HTTP)
    listen 80;
    listen [::]:80;  # IPv6 support / รองรับ IPv6

    server_name yourdomain.com www.yourdomain.com;

    # Redirect all HTTP traffic to HTTPS (301 = permanent redirect) / เปลี่ยนเส้นทาง HTTP ทั้งหมดเป็น HTTPS (301 = เปลี่ยนเส้นทางถาวร)
    # $server_name = domain from server_name directive / $server_name = โดเมนจากคำสั่ง server_name
    # $request_uri = original request path and query / $request_uri = พาธและคิวรีคำขอดั้งเดิม
    return 301 https://$server_name$request_uri;
}
```

### Apply Configuration / นำการกำหนดค่าไปใช้

```bash
# Test Nginx configuration for syntax errors / ทดสอบการกำหนดค่า Nginx สำหรับข้อผิดพลาดทางไวยากรณ์
nginx -t

# Expected output: "syntax is ok" and "test is successful" / ผลลัพธ์ที่คาดหวัง: "syntax is ok" และ "test is successful"

# Reload Nginx to apply changes (zero downtime) / โหลด Nginx ใหม่เพื่อนำการเปลี่ยนแปลงไปใช้ (downtime เป็นศูนย์)
systemctl reload nginx

# Or restart Nginx completely / หรือรีสตาร์ท Nginx ทั้งหมด
systemctl restart nginx

# Check Nginx status / ตรวจสอบสถานะ Nginx
systemctl status nginx
```

### Advanced: Multiple Domains with TLS / ขั้นสูง: หลายโดเมนพร้อม TLS

```nginx
# Server block for multiple domains with same certificate / บล็อกเซิร์ฟเวอร์สำหรับหลายโดเมนพร้อมใบรับรองเดียวกัน
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    # This certificate covers both domains / ใบรับรองนี้ครอบคลุมทั้งสองโดเมน
    server_name example.com www.example.com;

    ssl_certificate     /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    # ... rest of SSL configuration ... / ... การกำหนดค่า SSL ที่เหลือ ...

    root /var/www/example.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

---

## Troubleshooting / การแก้ไขปัญหา

### Verify TLS Connection / ตรวจสอบการเชื่อมต่อ TLS

```bash
# Test HTTPS connection with verbose output / ทดสอบการเชื่อมต่อ HTTPS พร้อมผลลัพธ์แบบ verbose
curl -vI https://yourdomain.com

# Look for these indicators in output / มองหาตัวบ่งชี้เหล่านี้ในผลลัพธ์:
# * SSL connection using TLSv1.3 / * การเชื่อมต่อ SSL ใช้ TLSv1.3
# * subject: CN=yourdomain.com / * subject: CN=yourdomain.com
# * issuer: C=US; O=Let's Encrypt; CN=R3 / * ผู้ออก: C=US; O=Let's Encrypt; CN=R3
# < HTTP/2 200 / < HTTP/2 200

# Check specific TLS version / ตรวจสอบเวอร์ชัน TLS เฉพาะ
# Test TLS 1.2 / ทดสอบ TLS 1.2
openssl s_client -connect yourdomain.com:443 -tls1_2 </dev/null 2>&1 | grep -E "Protocol|Cipher"

# Test TLS 1.3 / ทดสอบ TLS 1.3
openssl s_client -connect yourdomain.com:443 -tls1_3 </dev/null 2>&1 | grep -E "Protocol|Cipher"

# Test that TLS 1.1 is rejected (should fail) / ทดสอบว่า TLS 1.1 ถูกปฏิเสธ (ควรล้มเหลว)
openssl s_client -connect yourdomain.com:443 -tls1_1 </dev/null 2>&1 | grep -E "error|Protocol"

# View complete certificate chain / ดูห่วงโซ่ใบรับรองทั้งหมด
openssl s_client -showcerts -connect yourdomain.com:443 </dev/null 2>&1 | openssl x509 -noout -subject -issuer

# Check certificate chain of trust / ตรวจสอบห่วงโซ่ความเชื่อถือของใบรับรอง
openssl s_client -connect yourdomain.com:443 -servername yourdomain.com </dev/null 2>&1 | grep -E "depth|verify"
```

### Common Issues and Solutions / ปัญหาทั่วไปและวิธีแก้ไข

#### Issue 1: Certificate Not Trusted / ปัญหาที่ 1: ใบรับรองไม่เชื่อถือ

**Symptoms / อาการ**: Browser shows "Your connection is not private" / เบราว์เซอร์แสดง "การเชื่อมต่อของคุณไม่ปลอดภัย"

**Possible Causes / สาเหตุที่เป็นไปได้**:
```
1. Missing intermediate certificate chain / หายใบรับรอง intermediate chain
   → Solution: Use fullchain.pem instead of cert.pem / วิธีแก้: ใช้ fullchain.pem แทน cert.pem

2. Self-signed certificate (not issued by trusted CA) / ใบรับรองเซ็นตัวเอง (ไม่ได้ออกโดย CA ที่เชื่อถือ)
   → Solution: Get certificate from Let's Encrypt or trusted CA / วิธีแก้: รับใบรับรองจาก Let's Encrypt หรือ CA ที่เชื่อถือ

3. Certificate issued for wrong domain / ใบรับรองออกสำหรับโดเมนผิด
   → Solution: Re-issue certificate with correct domain / วิธีแก้: ออกใบรับรองใหม่ด้วยโดเมนที่ถูกต้อง
```

#### Issue 2: Mixed Content Warning / ปัญหาที่ 2: คำเตือนเนื้อหาแบบผสม

**Symptoms / อาการ**: Padlock shows warning, some resources load over HTTP / แม่กุญแจแสดงคำเตือน, บางทรัพยากรโหลดผ่าน HTTP

**Solution / วิธีแก้**:
```
1. Ensure all resources (images, CSS, JS) use HTTPS or relative paths / ตรวจสอบว่าทรัพยากรทั้งหมดใช้ HTTPS หรือพาธสัมพัทธ์

2. Add Content-Security-Policy header in Nginx / เพิ่มส่วนหัว Content-Security-Policy ใน Nginx
   add_header Content-Security-Policy "upgrade-insecure-requests" always;

3. Check HTML source for http:// references / ตรวจสอบแหล่ง HTML สำหรับการอ้างอิง http://
   curl -s https://yourdomain.com | grep -o 'http://[^"]*'
```

#### Issue 3: Certificate Expired / ปัญหาที่ 3: ใบรับรองหมดอายุ

**Symptoms / อาการ**: Browser shows "Your connection is not private - NET::ERR_CERT_DATE_INVALID" / เบราว์เซอร์แสดง "การเชื่อมต่อของคุณไม่ปลอดภัย - NET::ERR_CERT_DATE_INVALID"

**Diagnosis and Solution / การวินิจฉัยและวิธีแก้**:
```bash
# Check certificate expiry / ตรวจสอบวันหมดอายุใบรับรอง
openssl x509 -enddate -noout -in /etc/letsencrypt/live/yourdomain.com/cert.pem

# Check if Certbot auto-renewal is working / ตรวจสอบว่าการต่ออายุอัตโนมัติของ Certbot ทำงานอยู่หรือไม่
systemctl status certbot.timer
cat /var/log/letsencrypt/letsencrypt.log

# Force renewal / บังคับต่ออายุ
certbot renew --force-renewal

# Reload web server after renewal / โหลดเว็บเซิร์ฟเวอร์ใหม่หลังต่ออายุ
systemctl reload nginx
```

#### Issue 4: Wrong Domain / ปัญหาที่ 4: โดเมนผิด

**Symptoms / อาการ**: `SSL_ERROR_BAD_CERT_DOMAIN` / เบราว์เซอร์แสดงข้อผิดพลาดโดเมน

**Solution / วิธีแก้**:
```bash
# Check which domains the certificate covers / ตรวจสอบว่าใบรับรองครอบคลุมโดเมนใด
openssl x509 -text -noout -in /etc/letsencrypt/live/yourdomain.com/cert.pem | grep -A1 "Subject Alternative Name"

# Re-issue certificate with additional domains / ออกใบรับรองใหม่พร้อมโดเมนเพิ่มเติม
certbot certonly --expand -d yourdomain.com -d www.yourdomain.com -d newdomain.com
```

#### Issue 5: Renewal Hook Failed / ปัญหาที่ 5: Hook การต่ออายุล้มเหลว

**Symptoms / อาการ**: Certificate renewed but website still shows old certificate / ใบรับรองต่ออายุแล้วแต่เว็บไซต์ยังคงแสดงใบรับรองเก่า

**Solution / วิธีแก้**:
```bash
# Check deploy hooks in renewal configuration / ตรวจสอบ deploy hooks ในการกำหนดค่าการต่ออายุ
cat /etc/letsencrypt/renewal/yourdomain.com.conf

# Look for deploy_hook line / มองหาบรรทัด deploy_hook
# deploy_hook = systemctl reload nginx

# Manually run the deploy hook / รัน deploy hook ด้วยตนเอง
systemctl reload nginx

# Test renewal with deploy hook / ทดสอบการต่ออายุพร้อม deploy hook
certbot renew --dry-run --deploy-hook "systemctl reload nginx"
```

### Useful Debugging Commands / คำสั่งดีบักที่มีประโยชน์

```bash
# View Certbot logs (last 50 lines) / ดูบันทึก Certbot (50 บรรทัดล่าสุด)
tail -50 /var/log/letsencrypt/letsencrypt.log

# Check Nginx error logs / ตรวจสอบบันทึกข้อผิดพลาด Nginx
tail -50 /var/log/nginx/error.log

# Check if port 443 is open and listening / ตรวจสอบว่าพอร์ต 443 เปิดและฟังอยู่หรือไม่
ss -tlnp | grep 443
# หรือ / or
netstat -tlnp | grep 443

# Check firewall rules / ตรวจสอบกฎไฟร์วอลล์
sudo ufw status
# หรือ / or
sudo iptables -L -n | grep 443

# Verify DNS resolution / ตรวจสอบการแก้ไข DNS
dig yourdomain.com +short
# หรือ / or
nslookup yourdomain.com

# Check certificate file permissions (should be restrictive) / ตรวจสอบสิทธิ์ไฟล์ใบรับรอง (ควรจำกัด)
ls -la /etc/letsencrypt/live/yourdomain.com/
ls -la /etc/letsencrypt/archive/yourdomain.com/
# Private key should be readable only by root / คีย์ส่วนตัวควรอ่านได้เฉพาะ root
```

---

## Practice Exercises (Task 1) / แบบฝึกหัดปฏิบัติ (งานที่ 1)

Complete the following exercises to gain hands-on experience with TLS certificates.

ทำแบบฝึกหัดต่อไปนี้เพื่อรับประสบการณ์จริงกับใบรับรอง TLS

### Exercise 1: Issue Certificate for Your Domain / แบบฝึกหัดที่ 1: ออกใบรับรองสำหรับโดเมนของคุณ

**Objective / วัตถุประสงค์**: Install and configure Certbot, then issue a certificate using HTTP-01 challenge.
**เป้าหมาย**: ติดตั้งและกำหนดค่า Certbot จากนั้นออกใบรับรองโดยใช้ความท้าทาย HTTP-01

**Steps / ขั้นตอน**:
```
1. Install Certbot on your server / ติดตั้ง Certbot บนเซิร์ฟเวอร์ของคุณ
   apt update && apt install certbot -y

2. Issue a certificate for your domain using HTTP-01 / ออกใบรับรองสำหรับโดเมนของคุณโดยใช้ HTTP-01
   certbot certonly --standalone -d yourdomain.com

3. Document the entire process with screenshots / บันทึกกระบวนการทั้งหมดพร้อมภาพหน้าจอ
   - Screenshot the installation output / ภาพหน้าจอผลลัพธ์การติดตั้ง
   - Screenshot the certificate issuance output / ภาพหน้าจอผลลัพธ์การออกใบรับรอง
   - Screenshot the certificate files in /etc/letsencrypt/ / ภาพหน้าจอไฟล์ใบรับรองใน /etc/letsencrypt/

4. Verify the certificate / ตรวจสอบใบรับรอง
   certbot certificates
   openssl x509 -enddate -noout -in /etc/letsencrypt/live/yourdomain.com/cert.pem
```

**Deliverables / สิ่งที่ต้องส่ง**:
- Screenshot of Certbot installation / ภาพหน้าจอการติดตั้ง Certbot
- Screenshot of successful certificate issuance / ภาพหน้าจอการออกใบรับรองสำเร็จ
- Screenshot of certificate details showing expiry date / ภาพหน้าจอรายละเอียดใบรับรองแสดงวันหมดอายุ

---

### Exercise 2: Issue Certificate with Different Challenges / แบบฝึกหัดที่ 2: ออกใบรับรองด้วยความท้าทายที่แตกต่างกัน

**Objective / วัตถุประสงค์**: Compare HTTP-01 and DNS-01 challenge methods.
**เป้าหมาย**: เปรียบเทียบวิธีการความท้าทาย HTTP-01 และ DNS-01

**Steps / ขั้นตอน**:
```
Part A: HTTP-01 Challenge / ส่วน A: ความท้าทาย HTTP-01
1. Issue certificate using HTTP-01 (standalone or webroot) / ออกใบรับรองโดยใช้ HTTP-01 (standalone หรือ webroot)
2. Note the requirements and limitations / ระบุข้อกำหนดและข้อจำกัด
3. Screenshot the process / ภาพหน้าจอกระบวนการ

Part B: DNS-01 Challenge / ส่วน B: ความท้าทาย DNS-01
1. Install appropriate DNS plugin / ติดตั้งปลั๊กอิน DNS ที่เหมาะสม
2. Create DNS credentials file / สร้างไฟล์ข้อมูลรับรอง DNS
3. Issue wildcard certificate using DNS-01 / ออกใบรับรองไวลด์การ์ดโดยใช้ DNS-01
   certbot certonly --dns-cloudflare \
     --dns-cloudflare-credentials cloudflare.ini \
     -d yourdomain.com \
     -d '*.yourdomain.com'
4. Screenshot the process / ภาพหน้าจอกระบวนการ

Comparison Table / ตารางเปรียบเทียบ:
┌─────────────────────┬─────────────┬─────────────┐
│ Feature / คุณสมบัติ  │   HTTP-01   │   DNS-01    │
├─────────────────────┼─────────────┼─────────────┤
│ Wildcard support    │     No      │     Yes     │
│ รองรับไวลด์การ์ด     │     ไม่ใช่   │     ใช่      │
├─────────────────────┼─────────────┼─────────────┤
│ Requires port 80    │     Yes     │     No      │
│ ต้องใช้พอร์ต 80      │     ใช่      │     ไม่ใช่   │
├─────────────────────┼─────────────┼─────────────┤
│ Requires DNS API    │     No      │     Yes     │
│ ต้องใช้ DNS API     │     ไม่ใช่   │     ใช่      │
├─────────────────────┼─────────────┼─────────────┤
│ Security level      │  Moderate   │    High     │
│ ระดับความปลอดภัย     │  ปานกลาง    │    สูง       │
└─────────────────────┴─────────────┴─────────────┘
```

---

### Exercise 3: Certificate Lifecycle Documentation / แบบฝึกหัดที่ 3: เอกสารวงจรชีวิตใบรับรอง

**Objective / วัตถุประสงค์**: Document the entire certificate lifecycle from issuance to expiry.
**เป้าหมาย**: บันทึกเอกสารวงจรชีวิตใบรับรองทั้งหมดตั้งแต่การออกจนถึงหมดอายุ

**Steps / ขั้นตอน**:
```
1. Issue a certificate and capture the output / ออกใบรับรองและจับผลลัพธ์
   - Screenshot after successful issuance / ภาพหน้าจอหลังออกใบรับรองสำเร็จ

2. Show certificate details / แสดงรายละเอียดใบรับรอง
   certbot certificates
   openssl x509 -text -noout -in /etc/letsencrypt/live/yourdomain.com/cert.pem | head -30

3. Document the renewal process / บันทึกกระบวนการต่ออายุ
   - Screenshot the renewal test / ภาพหน้าจอการทดสอบการต่ออายุ
   certbot renew --dry-run

4. Simulate certificate expiry behavior / จำลองพฤติกรรมเมื่อใบรับรองหมดอายุ
   - Research what happens when a certificate expires / วิจัยว่าเกิดอะไรขึ้นเมื่อใบรับรองหมดอายุ
   - Document browser warnings / บันทึกคำเตือนเบราว์เซอร์
   - Document application behavior / บันทึกพฤติกรรมแอปพลิเคชัน

5. Document the auto-renewal setup / บันทึกการตั้งค่าการต่ออายุอัตโนมัติ
   - Screenshot systemd timer status / ภาพหน้าจอสถานะ systemd timer
   - OR screenshot crontab entry / หรือภาพหน้าจอรายการ crontab
```

**Deliverables / สิ่งที่ต้องส่ง**:
- Screenshot of certificate issuance / ภาพหน้าจอการออกใบรับรอง
- Screenshot of certificate details / ภาพหน้าจอรายละเอียดใบรับรอง
- Screenshot of renewal test / ภาพหน้าจอการทดสอบการต่ออายุ
- Written explanation of what happens when certificate expires / คำอธิบายว่าเกิดอะไรขึ้นเมื่อใบรับรองหมดอายุ

---

### Exercise 4: Bonus - Full TLS Setup / แบบฝึกหัดที่ 4: โบนัส - การตั้งค่า TLS เต็มรูปแบบ

**Objective / วัตถุประสงค์**: Configure a complete TLS setup with Nginx and auto-renewal.
**เป้าหมาย**: กำหนดค่าการตั้งค่า TLS ทั้งหมดพร้อม Nginx และการต่ออายุอัตโนมัติ

**Steps / ขั้นตอน**:
```
1. Install and configure Nginx with TLS / ติดตั้งและกำหนดค่า Nginx พร้อม TLS
   apt install nginx -y

   # Create Nginx configuration / สร้างการกำหนดค่า Nginx
   nano /etc/nginx/sites-available/yourdomain.com

   # Add the configuration from the "Nginx Configuration with TLS" section above /
   # เพิ่มการกำหนดค่าจากส่วน "การกำหนดค่า Nginx พร้อม TLS" ด้านบน

   # Enable the site / เปิดใช้งานไซต์
   ln -s /etc/nginx/sites-available/yourdomain.com /etc/nginx/sites-enabled/

   # Test and reload / ทดสอบและโหลดใหม่
   nginx -t
   systemctl reload nginx

2. Configure auto-renewal / กำหนดค่าการต่ออายุอัตโนมัติ
   # Verify systemd timer is active / ตรวจสอบว่า systemd timer ทำงานอยู่
   systemctl status certbot.timer

   # Test renewal / ทดสอบการต่ออายุ
   certbot renew --dry-run

3. Test with SSL Labs / ทดสอบด้วย SSL Labs
   # Visit https://www.ssllabs.com/ssltest/ / เยี่ยมชม https://www.ssllabs.com/ssltest/
   # Enter your domain / ใส่โดเมนของคุณ
   # Screenshot the results / ภาพหน้าจอผลลัพธ์
   # Aim for an A+ rating / ตั้งเป้าคะแนน A+

4. Verify HTTP to HTTPS redirect / ตรวจสอบการเปลี่ยนเส้นทาง HTTP เป็น HTTPS
   curl -I http://yourdomain.com
   # Should return: HTTP/1.1 301 Moved Permanently
   #               Location: https://yourdomain.com/

5. Verify TLS version / ตรวจสอบเวอร์ชัน TLS
   curl -vI https://yourdomain.com 2>&1 | grep -E "SSL|TLS"
```

**Deliverables / สิ่งที่ต้องส่ง**:
- Screenshot of Nginx configuration / ภาพหน้าจอการกำหนดค่า Nginx
- Screenshot of `nginx -t` output / ภาพหน้าจอผลลัพธ์ `nginx -t`
- Screenshot of SSL Labs test results (aim for A+) / ภาพหน้าจอผลลัพธ์การทดสอบ SSL Labs (ตั้งเป้า A+)
- Screenshot of HTTP to HTTPS redirect verification / ภาพหน้าจอการตรวจสอบการเปลี่ยนเส้นทาง HTTP เป็น HTTPS

---

## Resources / ทรัพยากร

### Official Documentation / เอกสารทางการ

- **[Let's Encrypt Official Site / เว็บไซต์ทางการ Let's Encrypt](https://letsencrypt.org/)**
  - Free, automated, and open Certificate Authority / Certificate Authority ฟรี, อัตโนมัติ, และเปิด

- **[Certbot Official Documentation / เอกสาร Certbot ทางการ](https://certbot.eff.org/)**
  - Step-by-step instructions for your server / คำแนะนำทีละขั้นตอนสำหรับเซิร์ฟเวอร์ของคุณ

- **[ACME Protocol (RFC 8555) / โปรโตคอล ACME (RFC 8555)](https://datatracker.ietf.org/doc/html/rfc8555)**
  - The official IETF specification for ACME / ข้อมูลจำเพาะ IETF ทางการสำหรับ ACME

### SSL/TLS Testing Tools / เครื่องมือทดสอบ SSL/TLS

- **[SSL Labs Server Test / การทดสอบเซิร์ฟเวอร์ SSL Labs](https://www.ssllabs.com/ssltest/)**
  - Deep analysis of SSL/TLS configuration / การวิเคราะห์เชิงลึกของการกำหนดค่า SSL/TLS
  - Grades from A+ to F / เกรดตั้งแต่ A+ ถึง F

- **[TestSSL.sh (CLI tool) / TestSSL.sh (เครื่องมือ CLI)](https://testssl.sh/)**
  - Command-line SSL testing script / สคริปต์ทดสอบ SSL แบบ command-line

### DNS Provider Plugins / ปลั๊กอินผู้ให้บริการ DNS

- **[Certbot DNS Plugins / ปลั๊กอิน DNS Certbot](https://eff-certbot.readthedocs.io/en/stable/using.html#dns-plugins)**
  - List of all supported DNS providers / รายชื่อผู้ให้บริการ DNS ที่รองรับทั้งหมด
  - Cloudflare, Route 53, Google Cloud, Azure, and more / Cloudflare, Route 53, Google Cloud, Azure, และอื่นๆ

### Learning Resources / ทรัพยากรการเรียนรู้

- **[Mozilla SSL Configuration Generator / เครื่องมือสร้างการกำหนดค่า SSL Mozilla](https://ssl-config.mozilla.org/)**
  - Generate secure SSL configurations for various servers / สร้างการกำหนดค่า SSL ที่ปลอดภัยสำหรับเซิร์ฟเวอร์ต่างๆ

- **[TLS 1.3 Explained / อธิบาย TLS 1.3](https://blog.cloudflare.com/tls-1-3-explained/)**
  - Cloudflare's detailed guide on TLS 1.3 / คู่มือเชิงลึกของ Cloudflare เกี่ยวกับ TLS 1.3

- **[How HTTPS Works / HTTPS ทำงานอย่างไร](https://howhttps.works/)**
  - Interactive explanation of HTTPS / คำอธิบายแบบโต้ตอบของ HTTPS

### Quick Reference Commands / คำสั่งอ้างอิงด่วน

```bash
# Issue certificate (HTTP-01 standalone) / ออกใบรับรอง (HTTP-01 standalone)
certbot certonly --standalone -d yourdomain.com

# Issue certificate (DNS-01 wildcard) / ออกใบรับรอง (DNS-01 ไวลด์การ์ด)
certbot certonly --dns-cloudflare --dns-cloudflare-credentials cloudflare.ini \
  -d yourdomain.com -d '*.yourdomain.com'

# List certificates / แสดงใบรับรอง
certbot certificates

# Test renewal / ทดสอบการต่ออายุ
certbot renew --dry-run

# Check expiry / ตรวจสอบวันหมดอายุ
openssl x509 -enddate -noout -in /etc/letsencrypt/live/yourdomain.com/cert.pem

# Test TLS connection / ทดสอบการเชื่อมต่อ TLS
openssl s_client -connect yourdomain.com:443 -servername yourdomain.com </dev/null 2>/dev/null | openssl x509 -noout -subject -issuer

# Reload Nginx after renewal / โหลด Nginx ใหม่หลังต่ออายุ
systemctl reload nginx
```

---

> **Summary / สรุป**: TLS is fundamental for secure web communication. Understanding certificate management, ACME challenges, and auto-renewal is essential for any DevOps engineer. Practice these skills regularly!
> **สรุป**: TLS เป็นพื้นฐานสำหรับการสื่อสารเว็บที่ปลอดภัย การเข้าใจการจัดการใบรับรอง, ความท้าทาย ACME, และการต่ออายุอัตโนมัติเป็นสิ่งสำคัญสำหรับวิศวกร DevOps ทุกคน ฝึกทักษะเหล่านี้อย่างสม่ำเสมอ!
