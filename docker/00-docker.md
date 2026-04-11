# Docker / ด็อกเกอร์

> Docker is a containerization platform that packages applications and their dependencies into isolated, portable containers.
>
> Docker คือแพลตฟอร์มสำหรับทำ Containerization ซึ่งบรรจุแอปพลิเคชันและ Dependencies ทั้งหมดลงใน Container ที่แยกอิสระและพกพาไปได้ทุกที่

---

## Table of Contents / สารบัญ

1. [What is Docker? / Docker คืออะไร?](#what-is-docker--docker-คืออะไร)
2. [Key Concepts / แนวคิดหลัก](#key-concepts--แนวคิดหลัก)
3. [Architecture / สถาปัตยกรรม](#architecture--สถาปัตยกรรม)
4. [Essential Commands / คำสั่งที่จำเป็น](#essential-commands--คำสั่งที่จำเป็น)
5. [Dockerfile](#dockerfile)
6. [Docker Compose / ด็อกเกอร์คอมโพส](#docker-compose--ด็อกเกอร์คอมโพส)
7. [Networking / ระบบเครือข่าย](#networking--ระบบเครือข่าย)
8. [Volumes / ปริมาณข้อมูล](#volumes--ปริมาณข้อมูล)
9. [Docker Registry / ด็อกเกอร์รีจิสทรี](#docker-registry--ด็อกเกอร์รีจิสทรี)
10. [Best Practices / แนวทางปฏิบัติที่ดี](#best-practices--แนวทางปฏิบัติที่ดี)
11. [Multi-Stage Build / การสร้างหลายขั้นตอน](#multi-stage-build--การสร้างหลายขั้นตอน)
12. [Practice Exercises / แบบฝึกหัด](#practice-exercises--แบบฝึกหัด)
13. [Resources / แหล่งข้อมูล](#resources--แหล่งข้อมูล)

---

## What is Docker? / Docker คืออะไร?

**English:**

Docker is an open-source platform that automates the deployment, scaling, and management of applications using containerization technology. It allows developers to package an application with all its dependencies (libraries, runtime, system tools, etc.) into a standardized unit called a **container**.

Key characteristics:
- **Lightweight**: Containers share the host OS kernel, making them much lighter than virtual machines.
- **Portable**: A container runs the same way on any system that has Docker installed.
- **Isolated**: Each container runs in its own isolated environment, preventing conflicts between applications.
- **Fast**: Containers can start and stop in seconds.

**ภาษาไทย:**

Docker คือแพลตฟอร์มโอเพ่นซอร์สที่ช่วยในการทำอัตโนมัติของการ Deployment, Scaling และการจัดการแอปพลิเคชันโดยใช้เทคโนโลยี Containerization Docker ช่วยให้นักพัฒนาสามารถบรรจุแอปพลิเคชันพร้อม Dependencies ทั้งหมด (ไลบรารี, Runtime, เครื่องมือระบบ ฯลฯ) ลงในหน่วยมาตรฐานที่เรียกว่า **Container**

ลักษณะสำคัญ:
- **น้ำหนักเบา**: Container ใช้ Kernel ของ Host OS ร่วมกัน ทำให้เบากว่า Virtual Machine มาก
- **พกพาได้**: Container ทำงานเหมือนกันทุกระบบที่มี Docker ติดตั้งอยู่
- **แยกอิสระ**: แต่ละ Container ทำงานในสภาพแวดล้อมที่แยกจากกัน ป้องกันความขัดแย้งระหว่างแอปพลิเคชัน
- **รวดเร็ว**: Container สามารถ Start และ Stop ได้ภายในไม่กี่วินาที

---

## Key Concepts / แนวคิดหลัก

### Image vs Container / อิมเมจ vs คอนเทนเนอร์

**English:**

| Concept / แนวคิด | Description / คำอธิบาย |
|---|---|
| **Image / อิมเมจ** | A read-only template that contains the application code, runtime, libraries, and dependencies. It is the blueprint used to create containers. / เทมเพลตแบบอ่านอย่างเดียวที่มีโค้ดแอปพลิเคชัน, Runtime, ไลบรารี และ Dependencies ทั้งหมด เป็นต้นแบบที่ใช้สร้าง Container |
| **Container / คอนเทนเนอร์** | A runnable instance of an image. You can create, start, stop, move, and delete containers. Each container is isolated from others. / อินสแตนซ์ที่รันได้ของ Image คุณสามารถสร้าง, เริ่ม, หยุด, ย้าย และลบ Container ได้ แต่ละ Container จะแยกออกจากกัน |

**Analogy / การเปรียบเทียบ:**
- **Image = Class** (in Object-Oriented Programming) / Image คือ คลาส (ในการเขียนโปรแกรมเชิงวัตถุ)
- **Container = Object** (instance of the class) / Container คือ ออบเจกต์ (อินสแตนซ์ของคลาส)

**English:**

### Benefits of Docker / ประโยชน์ของ Docker

- **Consistent Environments / สภาพแวดล้อมที่สม่ำเสมอ**: "Works on my machine" is no longer an issue. Development, testing, and production environments are identical. / ปัญหา "เครื่องฉันรันได้นะ" จะหมดไป สภาพแวดล้อมการพัฒนา, ทดสอบ และผลิตเหมือนกันทุกประการ
- **Isolation / การแยกอิสระ**: Each application runs in its own container without interfering with others. / แต่ละแอปพลิเคชันรันใน Container ของตัวเองโดยไม่รบกวนกัน
- **Portability / การพกพา**: Run anywhere Docker is supported — laptop, data center, or cloud. / รันได้ทุกที่ที่รองรับ Docker — แล็ปท็อป, Data Center หรือ Cloud
- **Fast Startup / เริ่มทำงานรวดเร็ว**: Containers start in seconds compared to minutes for VMs. / Container เริ่มทำงานในไม่กี่วินาที เทียบกับ VM ที่ใช้นาที
- **Resource Efficient / ใช้ทรัพยากรอย่างมีประสิทธิภาพ**: Containers share the host OS kernel, using less memory and CPU. / Container ใช้ Kernel ของ Host OS ร่วมกัน ใช้หน่วยความจำและ CPU น้อยกว่า

---

## Architecture / สถาปัตยกรรม

**English:**

Docker uses a client-server architecture. The Docker client communicates with the Docker daemon, which handles the heavy lifting of building, running, and managing containers.

**ภาษาไทย:**

Docker ใช้สถาปัตยกรรมแบบ Client-Server โดย Docker Client จะสื่อสารกับ Docker Daemon ซึ่งทำหน้าที่หนักในการสร้าง, รัน และจัดการ Container

```
┌──────────────────────────────────────────────────┐
│              Docker Client (CLI)                 │
│        (Command-line interface you use)           │
│   (อินเทอร์เฟซบรรทัดคำสั่งที่คุณใช้)              │
├──────────────────────────────────────────────────┤
│                   REST API                       │
│            (Communication layer)                 │
│               (ชั้นสื่อสารข้อมูล)                 │
├──────────────────────────────────────────────────┤
│              Docker Daemon (dockerd)             │
│         (Background service managing Docker)     │
│       (บริการเบื้องหลังที่จัดการ Docker)          │
├──────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌──────────┐       │
│  │Container 1│  │Container 2│  │Container 3│      │
│  │(App + Deps)│  │(App + Deps)│  │(App + Deps)│   │
│  └──────────┘  └──────────┘  └──────────┘       │
├──────────────────────────────────────────────────┤
│              Docker Images (Stored)              │
│          (อิมเมจที่จัดเก็บไว้ในเครื่อง)            │
├──────────────────────────────────────────────────┤
│                  Host OS / Kernel                │
│            (Linux, macOS, Windows)               │
│              (ระบบปฏิบัติการโฮสต์)                 │
└──────────────────────────────────────────────────┘
```

**Components / ส่วนประกอบ:**

- **Docker Client / ไคลเอนต์**: The primary way you interact with Docker (via `docker` command). / วิธีหลักที่คุณใช้โต้ตอบกับ Docker (ผ่านคำสั่ง `docker`)
- **Docker Daemon / ดีมอน**: Background process that manages Docker objects (images, containers, networks, volumes). / โปรเซสเบื้องหลังที่จัดการออบเจกต์ Docker ทั้งหมด (อิมเมจ, คอนเทนเนอร์, เน็ตเวิร์ก, วอลุ่ม)
- **Docker Images / อิมเมจ**: Read-only templates used to create containers. / เทมเพลตแบบอ่านอย่างเดียวที่ใช้สร้าง Container
- **Docker Containers / คอนเทนเนอร์**: Runnable instances of images. / อินสแตนซ์ที่รันได้ของอิมเมจ
- **Docker Registry / รีจิสทรี**: Storage for Docker images (e.g., Docker Hub). / ที่เก็บอิมเมจ Docker (เช่น Docker Hub)

---

## Essential Commands / คำสั่งที่จำเป็น

### Image Management / การจัดการอิมเมจ

```bash
# Download an image from registry / ดาวน์โหลดอิมเมจจาก Registry
docker pull nginx

# List all local images / แสดงอิมเมจทั้งหมดในเครื่อง
docker images

# Remove an image by ID or tag / ลบอิมเมจตาม ID หรือ Tag
docker rmi image_id

# Build an image from Dockerfile in current directory / สร้างอิมเมจจาก Dockerfile ในไดเรกทอรีปัจจุบัน
docker build -t myapp .

# Tag an image for a registry / แท็กอิมเมจสำหรับ Registry
docker tag myapp:latest myapp:v1.0

# Push an image to registry / อัปโหลดอิมเมจไปยัง Registry
docker push myapp:latest

# Search for images on Docker Hub / ค้นหาอิมเมจบน Docker Hub
docker search nginx

# Show image history (layers) / แสดงประวัติอิมเมจ (Layer)
docker history nginx

# Save an image to a tar archive / บันทึกอิมเมจเป็นไฟล์ Tar
docker save -o myapp.tar myapp:latest

# Load an image from a tar archive / โหลดอิมเมจจากไฟล์ Tar
docker load -i myapp.tar
```

### Container Management / การจัดการคอนเทนเนอร์

```bash
# Run a container in detached (background) mode / รัน Container ในโหมดพื้นหลัง
docker run -d --name web nginx

# Run a container interactively (with terminal) / รัน Container แบบโต้ตอบ (พร้อมเทอร์มินัล)
docker run -it ubuntu bash

# List running containers / แสดง Container ที่กำลังรัน
docker ps

# List all containers (including stopped) / แสดง Container ทั้งหมด (รวมที่หยุดแล้ว)
docker ps -a

# Stop a running container / หยุด Container ที่กำลังรัน
docker stop container_name

# Start a stopped container / เริ่ม Container ที่หยุดอยู่
docker start container_name

# Remove a stopped container / ลบ Container ที่หยุดแล้ว
docker rm container_name

# Remove a running container forcefully / ลบ Container ที่กำลังรันแบบบังคับ
docker rm -f container_name

# Restart a container / รีสตาร์ท Container
docker restart container_name

# Pause a container (freeze processes) / หยุด Container ชั่วคราว (แช่แข็งโปรเซส)
docker pause container_name

# Unpause a container / กลับมาทำงาน Container ที่หยุดชั่วคราว
docker unpause container_name

# Run with port mapping (host:container) / รันพร้อมการแมปพอร์ต (โฮสต์:คอนเทนเนอร์)
docker run -d -p 8080:80 nginx

# Run with environment variables / รันพร้อมตัวแปรสภาพแวดล้อม
docker run -d -e MYSQL_ROOT_PASSWORD=secret mysql

# Run with resource limits (512MB memory, 0.5 CPU) / รันพร้อมจำกัดทรัพยากร (หน่วยความจำ 512MB, CPU 0.5 ตัว)
docker run -d --memory=512m --cpus=0.5 nginx

# Run with restart policy (always restart on failure) / รันพร้อมนโยบายรีสตาร์ท (รีสตาร์ทเสมอเมื่อล้มเหลว)
docker run -d --restart=always nginx
```

### Debugging / การดีบัก

```bash
# View container logs / ดู Log ของ Container
docker logs container_name

# Follow logs in real-time / ติดตาม Log แบบเรียลไทม์
docker logs -f container_name

# Show last 100 lines of logs / แสดง Log 100 บรรทัดล่าสุด
docker logs --tail 100 container_name

# Execute a command inside a running container / รันคำสั่งภายใน Container ที่กำลังทำงาน
docker exec -it container_name bash

# Execute with a specific user / รันด้วยผู้ใช้งานเฉพาะ
docker exec -it -u root container_name bash

# Get detailed information about a container / ดูข้อมูลรายละเอียดของ Container
docker inspect container_name

# View resource usage (CPU, memory, network, disk) / ดูการใช้ทรัพยากร (CPU, หน่วยความจำ, เครือข่าย, ดิสก์)
docker stats

# Show processes running inside a container / แสดงโปรเซสที่ทำงานภายใน Container
docker top container_name

# Check container's IP address / ตรวจสอบที่อยู่ IP ของ Container
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name

# Copy files from container to host / คัดลอกไฟล์จาก Container ไปยังโฮสต์
docker cp container_name:/path/to/file ./local_path

# Copy files from host to container / คัดลอกไฟล์จากโฮสต์ไปยัง Container
docker cp ./local_file container_name:/path/to/destination

# View container's mounted volumes / ดู Volume ที่เมานต์ใน Container
docker inspect -f '{{json .Mounts}}' container_name
```

### Cleanup / การทำความสะอาด

```bash
# Remove all unused resources (images, containers, networks, caches) / ลบทรัพยากรที่ไม่ได้ใช้ทั้งหมด (อิมเมจ, คอนเทนเนอร์, เน็ตเวิร์ก, แคช)
docker system prune

# Remove all stopped containers / ลบ Container ที่หยุดทั้งหมด
docker container prune

# Remove all dangling (untagged) images / ลบอิมเมจที่ไม่มีแท็กทั้งหมด
docker image prune

# Remove all unused volumes / ลบ Volume ที่ไม่ได้ใช้ทั้งหมด
docker volume prune

# Remove all unused networks / ลบ Network ที่ไม่ได้ใช้ทั้งหมด
docker network prune

# Remove everything at once (prune all) / ลบทุกอย่างในครั้งเดียว
docker system prune -a --volumes

# Check disk usage by Docker / ตรวจสอบการใช้ดิสก์โดย Docker
docker system df

# Show detailed disk usage / แสดงการใช้ดิสก์แบบละเอียด
docker system df -v
```

---

## Dockerfile

**English:**

A Dockerfile is a text file containing a series of instructions that Docker uses to automatically build an image. Each instruction creates a new layer in the image.

**ภาษาไทย:**

Dockerfile คือไฟล์ข้อความที่มีชุดคำสั่งที่ Docker ใช้ในการสร้างอิมเมจโดยอัตโนมัติ แต่ละคำสั่งจะสร้าง Layer ใหม่ในอิมเมจ

### Example Dockerfile / ตัวอย่าง Dockerfile

```dockerfile
# ============================================
# Example: Node.js Application Dockerfile
# ตัวอย่าง: Dockerfile สำหรับแอปพลิเคชัน Node.js
# ============================================

# Use official Node.js 18 Alpine as the base image
# ใช้ Node.js 18 Alpine ทางการเป็นอิมเมจฐาน
FROM node:18-alpine

# Set the maintainer label (optional)
# ตั้งค่าป้ายกำกับผู้ดูแล (ไม่บังคับ)
LABEL maintainer="developer@example.com"

# Set the working directory inside the container
# ตั้งค่าไดเรกทอรีทำงานภายใน Container
WORKDIR /app

# Copy package files first (for better layer caching)
# คัดลอกไฟล์ Package ก่อน (เพื่อการใช้ Cache Layer ที่ดีกว่า)
COPY package*.json ./

# Install production dependencies
# ติดตั้ง Dependencies สำหรับการผลิต
RUN npm install --production

# Copy the rest of the application code
# คัดลอกโค้ดแอปพลิเคชันส่วนที่เหลือ
COPY . .

# Create a non-root user for security
# สร้างผู้ใช้ที่ไม่ใช่ Root เพื่อความปลอดภัย
RUN addgroup -g 1001 -S appgroup && \
    adduser -S appuser -u 1001 -G appgroup

# Change ownership of the app directory
# เปลี่ยนเจ้าของไดเรกทอรีแอป
RUN chown -R appuser:appgroup /app

# Switch to non-root user
# สลับไปใช้ผู้ใช้ที่ไม่ใช่ Root
USER appuser

# Document the port the application listens on
# บันทึกพอร์ตที่แอปพลิเคชันรอรับข้อมูล
EXPOSE 3000

# Define environment variables
# กำหนดตัวแปรสภาพแวดล้อม
ENV NODE_ENV=production

# Set the default command to run when the container starts
# ตั้งค่าคำสั่งเริ่มต้นที่จะรันเมื่อ Container เริ่มทำงาน
CMD ["node", "server.js"]
```

### Dockerfile Instructions / คำสั่ง Dockerfile

| Instruction / คำสั่ง | Description / คำอธิบาย | Example / ตัวอย่าง |
|---|---|---|
| **FROM** | Sets the base image. Must be the first instruction. / ตั้งค่าอิมเมจฐาน ต้องเป็นคำสั่งแรก | `FROM node:18-alpine` |
| **WORKDIR** | Sets the working directory for subsequent instructions. / ตั้งค่าไดเรกทอรีทำงานสำหรับคำสั่งถัดไป | `WORKDIR /app` |
| **COPY** | Copies files from host to image. / คัดลอกไฟล์จากโฮสต์ไปยังอิมเมจ | `COPY . .` |
| **ADD** | Like COPY but can extract archives and fetch URLs. / เหมือน COPY แต่สามารถแตก Archive และดึง URL ได้ | `ADD app.tar.gz /app` |
| **RUN** | Executes a command during image build. Creates a new layer. / รันคำสั่งระหว่างการสร้างอิมเมจ สร้าง Layer ใหม่ | `RUN npm install` |
| **CMD** | Default command when container starts. Can be overridden. / คำสั่งเริ่มต้นเมื่อ Container เริ่ม สามารถถูกแทนที่ได้ | `CMD ["node", "server.js"]` |
| **ENTRYPOINT** | Configures the container to run as an executable. Cannot be easily overridden. / ตั้งค่า Container ให้รันเป็น Executable ไม่สามารถถูกแทนที่ได้ง่าย | `ENTRYPOINT ["nginx"]` |
| **ENV** | Sets environment variables. / ตั้งค่าตัวแปรสภาพแวดล้อม | `ENV NODE_ENV=production` |
| **EXPOSE** | Documents which port the application uses. Does not actually publish the port. / บันทึกว่าแอปพลิเคชันใช้พอร์ตใด ไม่ได้ Publish พอร์ตจริง | `EXPOSE 3000` |
| **USER** | Sets the user for subsequent instructions and when the container runs. / ตั้งค่าผู้ใช้สำหรับคำสั่งถัดไปและเมื่อ Container รัน | `USER node` |
| **VOLUME** | Creates a mount point for external volumes. / สร้างจุดเมานต์สำหรับ Volume ภายนอก | `VOLUME ["/data"]` |
| **ARG** | Defines build-time variables (not available at runtime). / กำหนดตัวแปรช่วง Build (ไม่พร้อมใช้ช่วง Runtime) | `ARG VERSION=1.0` |
| **LABEL** | Adds metadata to the image. / เพิ่ม Metadata ให้กับอิมเมจ | `LABEL version="1.0"` |
| **HEALTHCHECK** | Defines how to check if the container is healthy. / กำหนดวิธีตรวจสอบว่า Container มีสุขภาพดี | `HEALTHCHECK CMD curl -f http://localhost/ || exit 1` |
| **ONBUILD** | Adds a trigger instruction for when this image is used as a base. / เพิ่มคำสั่ง Trigger เมื่ออิมเมจนี้ถูกใช้เป็นฐาน | `ONBUILD COPY . /app` |

### Build and Run / สร้างและรัน

```bash
# Build an image with tag 'myapp' from current directory
# สร้างอิมเมจด้วย Tag 'myapp' จากไดเรกทอรีปัจจุบัน
docker build -t myapp .

# Build with a specific Dockerfile
# สร้างด้วย Dockerfile เฉพาะ
docker build -t myapp -f Dockerfile.dev .

# Build with build arguments
# สร้างพร้อม Build Arguments
docker build -t myapp --build-arg VERSION=1.0 .

# Build without using cache
# สร้างโดยไม่ใช้ Cache
docker build --no-cache -t myapp .

# Run the container with port mapping
# รัน Container พร้อมแมปพอร์ต
docker run -d -p 3000:3000 --name web myapp

# Run with environment variables
# รันพร้อมตัวแปรสภาพแวดล้อม
docker run -d -e DB_HOST=localhost -e DB_PORT=5432 myapp
```

---

## Docker Compose / ด็อกเกอร์คอมโพส

**English:**

Docker Compose is a tool for defining and running multi-container Docker applications. With a single YAML file, you can configure all your application's services, networks, and volumes, then start everything with one command.

**ภาษาไทย:**

Docker Compose คือเครื่องมือสำหรับกำหนดและรันแอปพลิเคชัน Docker ที่มีหลาย Container ด้วยไฟล์ YAML เพียงไฟล์เดียว คุณสามารถกำหนดค่าบริการ, เน็ตเวิร์ก และ Volume ทั้งหมดของแอปพลิเคชัน จากนั้นเริ่มทุกอย่างด้วยคำสั่งเดียว

### Example docker-compose.yml / ตัวอย่าง docker-compose.yml

```yaml
# ============================================
# Docker Compose Example
# ตัวอย่าง Docker Compose
# ============================================

version: '3.8'  # Compose file format version / เวอร์ชันรูปแบบไฟล์ Compose

services:  # Define all application services / กำหนดบริการแอปพลิเคชันทั้งหมด

  # Web application service / บริการแอปพลิเคชันเว็บ
  web:
    build: .              # Build from current directory / สร้างจากไดเรกทอรีปัจจุบัน
    ports:
      - "3000:3000"       # Map host:container port / แมปพอร์ตโฮสต์:คอนเทนเนอร์
    environment:          # Environment variables / ตัวแปรสภาพแวดล้อม
      - DATABASE_URL=postgresql://postgres:secret@db:5432/mydb
    depends_on:           # Start after these services / เริ่มหลังจากบริการเหล่านี้
      - db
      - redis
    restart: unless-stopped  # Restart policy / นโยบายรีสตาร์ท
    volumes:              # Mount volumes / เมาต์วอลุ่ม
      - ./src:/app/src    # Bind mount for development / Bind mount สำหรับการพัฒนา

  # PostgreSQL database / ฐานข้อมูล PostgreSQL
  db:
    image: postgres:15    # Use official image / ใช้อิมเมจทางการ
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
    volumes:
      - pgdata:/var/lib/postgresql/data  # Named volume for data persistence / Named volume สำหรับเก็บข้อมูลถาวร
    ports:
      - "5432:5432"
    healthcheck:          # Health check configuration / การกำหนด Health Check
      test: ["CMD-SHELL", "pg_isready -U admin"]
      interval: 10s
      timeout: 5s
      retries: 5

  # Redis cache / แคช Redis
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redisdata:/data
    command: redis-server --appendonly yes  # Override default command / แทนที่คำสั่งเริ่มต้น

# Named volumes definition / กำหนด Named Volume
volumes:
  pgdata:    # PostgreSQL data volume / ปริมาณข้อมูล PostgreSQL
  redisdata: # Redis data volume / ปริมาณข้อมูล Redis

# Networks definition / กำหนดเครือข่าย
networks:
  frontend:  # Frontend network / เครือข่ายส่วนหน้า
  backend:   # Backend network / เครือข่ายส่วนหลัง
```

### Compose Commands / คำสั่ง Compose

```bash
# Build and start all services in detached mode
# สร้างและเริ่มบริการทั้งหมดในโหมดพื้นหลัง
docker-compose up -d

# Build images before starting containers
# สร้างอิมเมจก่อนเริ่ม Container
docker-compose up -d --build

# Start specific services only
# เริ่มเฉพาะบริการที่กำหนด
docker-compose up -d web db

# Stop and remove all containers, networks, and images
# หยุดและลบ Container, Network และ Image ทั้งหมด
docker-compose down

# Stop and remove everything including volumes (destroys data!)
# หยุดและลบทุกอย่างรวม Volume (ข้อมูลจะหายไป!)
docker-compose down -v

# View logs from all services
# ดู Log จากบริการทั้งหมด
docker-compose logs

# Follow logs in real-time
# ติดตาม Log แบบเรียลไทม์
docker-compose logs -f

# Show logs from a specific service
# แสดง Log ของบริการเฉพาะ
docker-compose logs -f web

# List running services
# แสดงบริการที่กำลังทำงาน
docker-compose ps

# List all services (including stopped)
# แสดงบริการทั้งหมด (รวมที่หยุด)
docker-compose ps -a

# Execute a command inside a running service
# รันคำสั่งภายในบริการที่กำลังทำงาน
docker-compose exec web bash

# Restart a specific service
# รีสตาร์ทบริการเฉพาะ
docker-compose restart web

# Pause all services
# หยุดบริการทั้งหมดชั่วคราว
docker-compose pause

# Resume paused services
# กลับมาทำงานบริการที่หยุดชั่วคราว
docker-compose unpause

# Show running processes inside containers
# แสดงโปรเซสที่กำลังทำงานภายใน Container
docker-compose top

# Pull latest images for services
# ดาวน์โหลดอิมเจจล่าสุดสำหรับบริการ
docker-compose pull

# Build images without starting containers
# สร้างอิมเมจโดยไม่เริ่ม Container
docker-compose build

# Validate and view the Compose file
# ตรวจสอบและดูไฟล์ Compose
docker-compose config
```

---

## Networking / ระบบเครือข่าย

**English:**

Docker networking allows containers to communicate with each other and with the outside world. Docker provides several network drivers for different use cases.

**ภาษาไทย:**

ระบบเครือข่าย Docker ช่วยให้ Container สามารถสื่อสารระหว่างกันและกับโลกภายนอก Docker มีไดรเวอร์เครือข่ายหลายแบบสำหรับกรณีการใช้งานที่แตกต่างกัน

### Network Commands / คำสั่งเครือข่าย

```bash
# List all networks
# แสดงเครือข่ายทั้งหมด
docker network ls

# Create a new bridge network
# สร้าง Bridge Network ใหม่
docker network create mynet

# Create network with specific driver
# สร้างเครือข่ายด้วยไดรเวอร์ที่กำหนด
docker network create --driver bridge mynet

# Create network with subnet
# สร้างเครือข่ายพร้อม Subnet
docker network create --subnet 172.20.0.0/16 mynet

# Connect a running container to a network
# เชื่อมต่อ Container ที่กำลังทำงานไปยังเครือข่าย
docker network connect mynet container_name

# Disconnect a container from a network
# ยกเลิกการเชื่อมต่อ Container จากเครือข่าย
docker network disconnect mynet container_name

# Inspect network details
# ตรวจสอบรายละเอียดเครือข่าย
docker network inspect mynet

# Remove a network
# ลบเครือข่าย
docker network rm mynet

# Run container attached to a specific network
# รัน Container ที่เชื่อมต่อกับเครือข่ายที่กำหนด
docker run -d --network=mynet --name web nginx
```

### Network Types / ประเภทเครือข่าย

| Type / ประเภท | Description / คำอธิบาย | Use Case / กรณีใช้งาน |
|---|---|---|
| **bridge (Default / ค่าเริ่มต้น)** | Default network driver. Containers on the same bridge can communicate via IP. Containers can communicate with each other by name if on the same user-defined bridge. / ไดรเวอร์เครือข่ายเริ่มต้น Container บน Bridge เดียวกันสามารถสื่อสารผ่าน IP Container สามารถสื่อสารด้วยชื่อถ้าอยู่บน User-defined Bridge เดียวกัน | Single-host communication / การสื่อสารในโฮสต์เดียว |
| **host** | Removes network isolation and uses the host's network directly. No port mapping needed. / ลบการแยกเครือข่ายและใช้เครือข่ายโฮสต์โดยตรง ไม่ต้องแมปพอร์ต | Performance-critical apps / แอปที่ต้องการประสิทธิภาพสูง |
| **none** | Disables all networking. Container has no network access. / ปิดเครือข่ายทั้งหมด Container ไม่มีสิทธิ์เข้าถึงเครือข่าย | Isolated/secure workloads / เวิร์กโหลดที่แยก/ปลอดภัย |
| **overlay** | Enables communication between containers across multiple Docker daemon hosts (used in Swarm mode). / เปิดใช้งานการสื่อสารระหว่าง Container ข้ามโฮสต์ Docker Daemon หลายตัว (ใช้ในโหมด Swarm) | Multi-host / Swarm clusters / คลัสเตอร์หลายโฮสต์ |
| **macvlan** | Assigns a MAC address to the container, making it appear as a physical device on the network. / กำหนดที่อยู่ MAC ให้ Container ทำให้ดูเหมือนอุปกรณ์จริงในเครือข่าย | Legacy applications / Legacy application ที่ต้องการ MAC address |

### DNS in Docker / DNS ใน Docker

**English:**

Containers on the same user-defined bridge network can communicate with each other using container names as hostnames. Docker provides automatic DNS resolution.

**ภาษาไทย:**

Container บน user-defined bridge network เดียวกันสามารถสื่อสารกันโดยใช้ชื่อ Container เป็น Hostname Docker มีระบบ DNS อัตโนมัติ

```bash
# Example: Two containers communicating by name
# ตัวอย่าง: Container สองตัวสื่อสารด้วยชื่อ

# Create network / สร้างเครือข่าย
docker network create mynet

# Run first container / รัน Container แรก
docker run -d --network=mynet --name=db postgres:15

# Run second container / รัน Container ที่สอง
docker run -d --network=mynet --name=web myapp

# Inside 'web', you can reach 'db' by hostname
# ภายใน 'web' คุณสามารถเข้าถึง 'db' ได้ผ่าน Hostname
# Connection string: postgresql://db:5432/mydb
```

---

## Volumes / ปริมาณข้อมูล

**English:**

Volumes are the preferred mechanism for persisting data generated by and used by Docker containers. By default, data inside a container is lost when the container is removed.

**ภาษาไทย:**

Volume คือกลไกที่แนะนำสำหรับการเก็บข้อมูลถาวรที่สร้างและใช้โดย Container Docker โดยค่าเริ่มต้น ข้อมูลภายใน Container จะหายไปเมื่อ Container ถูกลบ

### Volume Commands / คำสั่ง Volume

```bash
# List all volumes
# แสดง Volume ทั้งหมด
docker volume ls

# Create a new named volume
# สร้าง Named Volume ใหม่
docker volume create mydata

# Inspect volume details
# ตรวจสอบรายละเอียด Volume
docker volume inspect mydata

# Mount volume when running a container
# เมาต์ Volume เมื่อรัน Container
docker run -d -v mydata:/data nginx

# Remove a specific volume
# ลบ Volume เฉพาะ
docker volume rm mydata

# Remove all unused volumes
# ลบ Volume ที่ไม่ได้ใช้ทั้งหมด
docker volume prune
```

### Volume Types / ประเภท Volume

| Type / ประเภท | Description / คำอธิบาย | Command / คำสั่ง |
|---|---|---|
| **Named Volumes** | Managed entirely by Docker. Stored in Docker's storage directory (`/var/lib/docker/volumes/`). Best for database data and persistent storage. / จัดการโดย Docker ทั้งหมด เก็บในไดเรกทอรีจัดเก็บ Docker เหมาะกับข้อมูลฐานข้อมูลและการจัดเก็บถาวร | `docker run -v mydata:/data app` |
| **Bind Mounts** | Maps a specific host directory to a container directory. Useful for development (code syncing). Path can be absolute or relative. / แมปไดเรกทอรีโฮสต์เฉพาะไปยังไดเรกทอรี Container เหมาะสำหรับการพัฒนา (ซิงก์โค้ด) | `docker run -v /host/path:/container/path app` |
| **tmpfs** | In-memory storage only. Data is not persisted on disk. Fastest option, but data is lost when container stops. / เก็บในหน่วยความจำเท่านั้น ข้อมูลไม่ถูกเก็บลงดิสก์ เร็วที่สุด แต่ข้อมูลหายไปเมื่อ Container หยุด | `docker run --tmpfs /data app` |

### Examples / ตัวอย่าง

```bash
# Named volume (recommended for databases)
# Named Volume (แนะนำสำหรับฐานข้อมูล)
docker volume create pgdata
docker run -d -v pgdata:/var/lib/postgresql/data postgres:15

# Bind mount (great for development - code changes reflect immediately)
# Bind Mount (เหมาะสำหรับการพัฒนา - การเปลี่ยนโค้ดสะท้อนทันที)
docker run -d -v ./src:/app/src -p 3000:3000 node:18 npm start

# Read-only bind mount
# Bind Mount แบบอ่านอย่างเดียว
docker run -d -v ./config:/etc/config:ro nginx

# Multiple volumes
# หลาย Volume
docker run -d \
  -v pgdata:/var/lib/postgresql/data \
  -v ./init.sql:/docker-entrypoint-initdb.d/init.sql \
  -v ./config:/etc/postgresql \
  postgres:15

# tmpfs mount (temporary data, fast I/O)
# tmpfs mount (ข้อมูลชั่วคราว, I/O เร็ว)
docker run -d --tmpfs /tmp:rw,size=100m nginx
```

---

## Docker Registry / ด็อกเกอร์รีจิสทรี

**English:**

A Docker Registry is a storage and distribution system for Docker images. Docker Hub is the default public registry, but you can also run your own private registry.

**ภาษาไทย:**

Docker Registry คือระบบจัดเก็บและแจกจ่ายอิมเมจ Docker Docker Hub คือ Registry สาธารณะเริ่มต้น แต่คุณสามารถรัน Registry ส่วนตัวของคุณเองได้

### Working with Registries / การทำงานกับ Registry

```bash
# Login to Docker Hub
# เข้าสู่ระบบ Docker Hub
docker login

# Login to a private registry
# เข้าสู่ระบบ Registry ส่วนตัว
docker login registry.example.com

# Login with username and password
# เข้าสู่ระบบด้วย Username และ Password
docker login -u username -p password registry.example.com

# Tag image for a specific registry
# แท็กอิมเมจสำหรับ Registry เฉพาะ
docker tag myapp:latest registry.example.com/myapp:v1.0

# Push image to registry
# อัปโหลดอิมเมจไปยัง Registry
docker push registry.example.com/myapp:v1.0

# Pull image from registry
# ดาวน์โหลดอิมเมจจาก Registry
docker pull registry.example.com/myapp:v1.0

# Logout from registry
# ออกจากระบบ Registry
docker logout

# Search images on Docker Hub
# ค้นหาอิมเมจบน Docker Hub
docker search nginx

# Pull a specific version tag
# ดาวน์โหลดแท็กเวอร์ชันเฉพาะ
docker pull postgres:15.4-alpine
```

### Running a Private Registry / การรัน Registry ส่วนตัว

```bash
# Run a local registry container
# รัน Container Registry ในเครื่อง
docker run -d -p 5000:5000 --name registry registry:2

# Tag image for local registry
# แท็กอิมเมจสำหรับ Registry ในเครื่อง
docker tag myapp:latest localhost:5000/myapp:v1

# Push to local registry
# อัปโหลดไปยัง Registry ในเครื่อง
docker push localhost:5000/myapp:v1

# Pull from local registry
# ดาวน์โหลดจาก Registry ในเครื่อง
docker pull localhost:5000/myapp:v1
```

---

## Best Practices / แนวทางปฏิบัติที่ดี

### 1. Use Specific Image Tags / ใช้แท็กอิมเมจเฉพาะ

**English:**

Always use specific version tags instead of `latest` in production. This ensures reproducibility and prevents unexpected changes.

**ภาษาไทย:**

ใช้แท็กเวอร์ชันเฉพาะเสมอแทน `latest` ในสภาพแวดล้อมการผลิต เพื่อให้สามารถทำซ้ำได้และป้องกันการเปลี่ยนแปลงที่ไม่คาดคิด

```dockerfile
# Good / ดี - Specific version / เวอร์ชันเฉพาะ
FROM node:18.19.0-alpine

# Bad / ไม่ดี - Latest can change / Latest สามารถเปลี่ยนแปลงได้
FROM node:latest
```

### 2. Use Multi-Stage Builds / ใช้ Multi-Stage Builds

**English:**

Multi-stage builds reduce final image size by separating build dependencies from runtime dependencies.

**ภาษาไทย:**

Multi-Stage Builds ลดขนาดอิมเมจสุดท้ายโดยแยก Build Dependencies ออกจาก Runtime Dependencies

```dockerfile
# See Multi-Stage Build section below / ดูส่วน Multi-Stage Build ด้านล่าง
```

### 3. Run as Non-Root User / รันด้วยผู้ใช้ที่ไม่ใช่ Root

**English:**

Running containers as root poses security risks. Always create and use a non-root user.

**ภาษาไทย:**

การรัน Container ด้วย Root มีความเสี่ยงด้านความปลอดภัย เสมอให้สร้างและใช้ผู้ใช้ที่ไม่ใช่ Root

```dockerfile
# Create non-root user / สร้างผู้ใช้ที่ไม่ใช่ Root
RUN addgroup -g 1001 -S appgroup && \
    adduser -S appuser -u 1001 -G appgroup

# Switch to non-root user / สลับไปใช้ผู้ใช้ที่ไม่ใช่ Root
USER appuser
```

### 4. Use .dockerignore / ใช้ .dockerignore

**English:**

Create a `.dockerignore` file to exclude unnecessary files from the build context, reducing build time and image size.

**ภาษาไทย:**

สร้างไฟล์ `.dockerignore` เพื่อยกเว้นไฟล์ที่ไม่จำเป็นจาก Build Context ลดเวลา Build และขนาดอิมเมจ

```
# .dockerignore Example / ตัวอย่าง .dockerignore

# Version control / การควบคุมเวอร์ชัน
.git
.gitignore
.svn

# IDE / Editor files
.vscode
.idea
*.swp
*.swo

# Dependencies / Dependencies
node_modules

# Build output / เอาต์พุต Build
dist
build
*.log

# OS files / ไฟล์ OS
.DS_Store
Thumbs.db

# Docker files / ไฟล์ Docker
Dockerfile
docker-compose.yml
.dockerignore

# Documentation / เอกสาร
README.md
docs/

# Environment files / ไฟล์สภาพแวดล้อม
.env
.env.*
```

### 5. Minimize Layers / ลดจำนวน Layer

**English:**

Combine multiple `RUN` commands to reduce the number of layers, which reduces image size.

**ภาษาไทย:**

รวมคำสั่ง `RUN` หลายคำสั่งเพื่อลดจำนวน Layer ซึ่งลดขนาดอิมเมจ

```dockerfile
# Bad / ไม่ดี - Multiple layers / หลาย Layer
RUN apt-get update
RUN apt-get install -y curl
RUN apt-get install -y wget
RUN rm -rf /var/lib/apt/lists/*

# Good / ดี - Combined into one layer / รวมเป็น Layer เดียว
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl wget && \
    rm -rf /var/lib/apt/lists/*
```

### 6. Use Alpine-Based Images / ใช้อิมเมจ基于 Alpine

**English:**

Alpine-based images are much smaller than full OS images, reducing attack surface and download time.

**ภาษาไทย:**

อิมเมจ基于 Alpine มีขนาดเล็กกว่าอิมเมจ OS เต็มรูปแบบ ลด Attack Surface และเวลาดาวน์โหลด

```dockerfile
# Small: ~5MB / เล็ก: ~5MB
FROM alpine:3.18

# Small with Node.js: ~45MB / เล็กพร้อม Node.js: ~45MB
FROM node:18-alpine
```

### 7. Scan Images for Vulnerabilities / สแกนอิมเมจหาช่องโหว่

**English:**

Regularly scan your images for known vulnerabilities using tools like Docker Scout, Trivy, or Snyk.

**ภาษาไทย:**

สแกนอิมเมจหาช่องโหว่ที่รู้จักเป็นประจำโดยใช้เครื่องมือเช่น Docker Scout, Trivy หรือ Snyk

```bash
# Using Docker Scout (built into Docker)
# ใช้ Docker Scout (มีอยู่ใน Docker)
docker scout cves myapp:latest

# Using Trivy
# ใช้ Trivy
trivy image myapp:latest
```

### 8. Set Resource Limits / ตั้งค่าจำกัดทรัพยากร

**English:**

Always set memory and CPU limits to prevent a single container from consuming all host resources.

**ภาษาไทย:**

ตั้งค่าจำกัดหน่วยความจำและ CPU เสมอเพื่อป้องกัน Container เดียวใช้ทรัพยากรโฮสต์ทั้งหมด

```bash
# Limit memory to 512MB, CPU to 0.5 cores
# จำกัดหน่วยความจำ 512MB, CPU 0.5 คอร์
docker run -d --memory=512m --cpus=0.5 myapp
```

### 9. Use Healthchecks / ใช้ Healthchecks

**English:**

Define healthchecks to monitor container health and enable automatic restarts.

**ภาษาไทย:**

กำหนด Healthchecks เพื่อตรวจสอบสุขภาพ Container และเปิดใช้งานการรีสตาร์ทอัตโนมัติ

```dockerfile
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1
```

### 10. Log and Monitor / บันทึก Log และตรวจสอบ

**English:**

Use structured logging and integrate with monitoring tools for production workloads.

**ภาษาไทย:**

ใช้ Structured Logging และผสานรวมกับเครื่องมือตรวจสอบสำหรับเวิร์กโหลดการผลิต

```bash
# Use JSON logging output
# ใช้เอาต์พุต Logging แบบ JSON
docker run -d myapp --log-driver json-file
```

---

## Multi-Stage Build / การสร้างหลายขั้นตอน

**English:**

Multi-stage builds allow you to use multiple `FROM` statements in a single Dockerfile. Each `FROM` starts a new stage. You can selectively copy artifacts from one stage to another, leaving behind everything you don't need in the final image.

**ภาษาไทย:**

Multi-Stage Builds ช่วยให้คุณใช้คำสั่ง `FROM` หลายคำสั่งใน Dockerfile เดียว แต่ละ `FROM` จะเริ่ม Stage ใหม่ คุณสามารถคัดลอก Artifact จาก Stage หนึ่งไปยังอีก Stage ทิ้งทุกอย่างที่คุณไม่ต้องการในอิมเมจสุดท้าย

### Go Application Example / ตัวอย่างแอปพลิเคชัน Go

```dockerfile
# ============================================
# Stage 1: Build stage (contains Go compiler and build tools)
# Stage 1: ขั้นตอน Build (มี Go Compiler และเครื่องมือ Build)
# ============================================
FROM golang:1.21-alpine AS builder

# Set working directory / ตั้งค่าไดเรกทอรีทำงาน
WORKDIR /app

# Copy go module files first (for caching) / คัดลอกไฟล์ Go Module ก่อน (สำหรับ Cache)
COPY go.mod go.sum ./

# Download dependencies / ดาวน์โหลด Dependencies
RUN go mod download

# Copy source code / คัดลอกโค้ดต้นฉบับ
COPY . .

# Build the Go binary (statically linked) / สร้าง Binary Go (Link แบบ Static)
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

# ============================================
# Stage 2: Production stage (minimal runtime)
# Stage 2: ขั้นตอน Production (Runtime ขั้นต่ำ)
# ============================================
FROM alpine:3.18

# Install CA certificates for HTTPS
# ติดตั้งใบรับรอง CA สำหรับ HTTPS
RUN apk --no-cache add ca-certificates

# Create non-root user / สร้างผู้ใช้ที่ไม่ใช่ Root
RUN addgroup -g 1001 -S appgroup && \
    adduser -S appuser -u 1001 -G appgroup

# Set working directory / ตั้งค่าไดเรกทอรีทำงาน
WORKDIR /app

# Copy binary from builder stage / คัดลอก Binary จาก Stage Builder
COPY --from=builder /app/main .

# Change ownership / เปลี่ยนเจ้าของ
RUN chown -R appuser:appgroup /app

# Switch to non-root user / สลับไปใช้ผู้ใช้ที่ไม่ใช่ Root
USER appuser

# Expose port / เปิดพอร์ต
EXPOSE 8080

# Set healthcheck / ตั้งค่า Healthcheck
HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget --no-verbose --tries=1 --spider http://localhost:8080/health || exit 1

# Run the binary / รัน Binary
CMD ["./main"]
```

### Node.js Application Example / ตัวอย่างแอปพลิเคชัน Node.js

```dockerfile
# ============================================
# Stage 1: Build stage (compile/bundle the app)
# Stage 1: ขั้นตอน Build (คอมไพล์/รวม Bundle แอป)
# ============================================
FROM node:18-alpine AS builder

WORKDIR /app

# Copy package files / คัดลอกไฟล์ Package
COPY package*.json ./

# Install ALL dependencies (including devDependencies) / ติดตั้ง Dependencies ทั้งหมด (รวม devDependencies)
RUN npm ci

# Copy source code / คัดลอกโค้ดต้นฉบับ
COPY . .

# Build the application (e.g., TypeScript compile, webpack, etc.)
# Build แอปพลิเคชัน (เช่น คอมไพล์ TypeScript, webpack ฯลฯ)
RUN npm run build

# ============================================
# Stage 2: Production stage (minimal runtime)
# Stage 2: ขั้นตอน Production (Runtime ขั้นต่ำ)
# ============================================
FROM node:18-alpine AS production

WORKDIR /app

# Copy package files / คัดลอกไฟล์ Package
COPY package*.json ./

# Install production dependencies only / ติดตั้ง Dependencies สำหรับการผลิตเท่านั้น
RUN npm ci --production && npm cache clean --force

# Copy built artifacts from builder / คัดลอก Artifact ที่สร้างแล้วจาก Builder
COPY --from=builder /app/dist ./dist

# Create non-root user / สร้างผู้ใช้ที่ไม่ใช่ Root
RUN addgroup -g 1001 -S appgroup && \
    adduser -S appuser -u 1001 -G appgroup && \
    chown -R appuser:appgroup /app

USER appuser

EXPOSE 3000

ENV NODE_ENV=production

CMD ["node", "dist/main.js"]
```

### Benefits of Multi-Stage Builds / ประโยชน์ของ Multi-Stage Builds

| Aspect / ด้าน | Single Stage / Stage เดียว | Multi-Stage / หลาย Stage |
|---|---|---|
| **Image Size / ขนาดอิมเมจ** | Large (includes build tools) / ใหญ่ (มีเครื่องมือ Build) | Small (only runtime) / เล็ก (มีเฉพาะ Runtime) |
| **Security / ความปลอดภัย** | More attack surface / Attack Surface มากกว่า | Minimal surface / Surface ขั้นต่ำ |
| **Build Time / เวลา Build** | Faster / เร็วกว่า | Slightly slower / ช้ากว่าเล็กน้อย |
| **Production Ready / พร้อมผลิต** | Not ideal / ไม่เหมาะ | Ideal / เหมาะสม |

---

## Practice Exercises / แบบฝึกหัด

### Exercise 1: Create a Dockerfile for a Web Application
### แบบฝึกหัดที่ 1: สร้าง Dockerfile สำหรับเว็บแอปพลิเคชัน

**English:**

1. Create a simple Express.js or Flask application
2. Write a Dockerfile that:
   - Uses an Alpine-based image
   - Runs as a non-root user
   - Includes a healthcheck
   - Uses multi-stage build if applicable
3. Build and run the container
4. Verify the application is accessible on the correct port

**ภาษาไทย:**

1. สร้างแอปพลิเคชัน Express.js หรือ Flask อย่างง่าย
2. เขียน Dockerfile ที่:
   - ใช้อิมเมจ基于 Alpine
   - รันด้วยผู้ใช้ที่ไม่ใช่ Root
   - มี Healthcheck
   - ใช้ Multi-Stage Build ถ้าใช้ได้
3. สร้างและรัน Container
4. ตรวจสอบว่าแอปพลิเคชันเข้าถึงได้ที่พอร์ตที่ถูกต้อง

### Exercise 2: Deploy WordPress + MySQL with Docker Compose
### แบบฝึกหัดที่ 2: Deploy WordPress + MySQL ด้วย Docker Compose

**English:**

1. Create a `docker-compose.yml` file with:
   - WordPress service (exposed on port 8080)
   - MySQL service (with persistent data volume)
   - Proper environment variables for both services
2. Start the services with `docker-compose up -d`
3. Access WordPress setup at `http://localhost:8080`
4. Verify data persists after `docker-compose down && docker-compose up -d`

**ภาษาไทย:**

1. สร้างไฟล์ `docker-compose.yml` ที่มี:
   - บริการ WordPress (แสดงพอร์ต 8080)
   - บริการ MySQL (มี Volume เก็บข้อมูลถาวร)
   - ตัวแปรสภาพแวดล้อมที่เหมาะสมสำหรับทั้งสองบริการ
2. เริ่มบริการด้วย `docker-compose up -d`
3. เข้าถึงตั้งค่า WordPress ที่ `http://localhost:8080`
4. ตรวจสอบว่าข้อมูลยังคงอยู่หลัง `docker-compose down && docker-compose up -d`

### Exercise 3: Multi-Stage Build for a Go Application
### แบบฝึกหัดที่ 3: Multi-Stage Build สำหรับแอปพลิเคชัน Go

**English:**

1. Create a simple Go HTTP server
2. Write a multi-stage Dockerfile:
   - Stage 1: Build the Go binary
   - Stage 2: Minimal Alpine image with only the binary
3. Compare image sizes between single-stage and multi-stage builds

**ภาษาไทย:**

1. สร้าง HTTP Server Go อย่างง่าย
2. เขียน Dockerfile แบบ Multi-Stage:
   - Stage 1: Build Binary Go
   - Stage 2: อิมเมจ Alpine ขั้นต่ำที่มีเฉพาะ Binary
3. เปรียบเทียบขนาดอิมเมจระหว่าง Single-Stage และ Multi-Stage Builds

### Exercise 4: Set Up a Private Docker Registry
### แบบฝึกหัดที่ 4: ตั้งค่า Registry Docker ส่วนตัว

**English:**

1. Run a local registry container
2. Push an image to your local registry
3. Remove the local image
4. Pull the image back from your local registry
5. Verify it works by running a container

**ภาษาไทย:**

1. รัน Container Registry ในเครื่อง
2. อัปโหลดอิมเมจไปยัง Registry ในเครื่อง
3. ลบอิมเมจในเครื่อง
4. ดาวน์โหลดอิมเมจกลับมาจาก Registry ในเครื่อง
5. ตรวจสอบว่าใช้งานได้โดยการรัน Container

### Exercise 5: Configure Custom Networks for Container Isolation
### แบบฝึกหัดที่ 5: กำหนดค่าเครือข่ายเฉพาะสำหรับการแยก Container

**English:**

1. Create two separate networks: `frontend` and `backend`
2. Run a web app container on the `frontend` network
3. Run a database container on the `backend` network
4. Run an API container on BOTH networks
5. Verify: web can reach API, API can reach DB, but web cannot directly reach DB

**ภาษาไทย:**

1. สร้างเครือข่ายสองเครือข่าย: `frontend` และ `backend`
2. รัน Container เว็บแอปบนเครือข่าย `frontend`
3. รัน Container ฐานข้อมูลบนเครือข่าย `backend`
4. รัน Container API บนทั้งสองเครือข่าย
5. ตรวจสอบ: เว็บเข้าถึง API ได้, API เข้าถึง DB ได้, แต่เว็บไม่สามารถเข้าถึง DB โดยตรง

### Exercise 6: Use Volumes to Persist Database Data
### แบบฝึกหัดที่ 6: ใช้ Volume เพื่อเก็บข้อมูลฐานข้อมูลถาวร

**English:**

1. Create a named volume for PostgreSQL/MySQL data
2. Run a database container with the volume mounted
3. Insert some test data
4. Stop and remove the container
5. Run a new container with the same volume
6. Verify the data still exists

**ภาษาไทย:**

1. สร้าง Named Volume สำหรับข้อมูล PostgreSQL/MySQL
2. รัน Container ฐานข้อมูลพร้อม Volume ที่เมานต์
3. ใส่ข้อมูลทดสอบ
4. หยุดและลบ Container
5. รัน Container ใหม่พร้อม Volume เดียวกัน
6. ตรวจสอบว่าข้อมูลยังคงอยู่

---

## Resources / แหล่งข้อมูล

### Official Documentation / เอกสารทางการ

- [Docker Official Documentation / เอกสารทางการ Docker](https://docs.docker.com/)
- [Docker Hub / ด็อกเกอร์ฮับ](https://hub.docker.com/)
- [Docker Compose Documentation / เอกสาร Docker Compose](https://docs.docker.com/compose/)

### Learning Platforms / แพลตฟอร์มการเรียนรู้

- [Play with Docker (Interactive Lab / ห้องทดลองโต้ตอบ)](https://labs.play-with-docker.com/)
- [Docker Curriculum / หลักสูตร Docker](https://docker-curriculum.com/)
- [KodeKloud Docker Course / คอร์สดocker KodeKloud](https://kodekloud.com/courses/docker-course/)

### Tools / เครื่องมือ

- [Docker Scout (Image Scanning / สแกนอิมเมจ)](https://docs.docker.com/scout/)
- [Trivy (Vulnerability Scanner / เครื่องมือสแกนช่องโหว่)](https://github.com/aquasecurity/trivy)
- [Portainer (Docker UI Management / จัดการ Docker ผ่าน UI)](https://www.portainer.io/)
- [Lazydocker (Terminal UI for Docker / UI เทอร์มินัลสำหรับ Docker)](https://github.com/jesseduffield/lazydocker)

### Cheat Sheets / ชีทอ้างอิง

- [Docker Cheat Sheet by Docker / ชีทอ้างอิงโดย Docker](https://docs.docker.com/get-started/docker_cheatsheet.pdf)
- [Docker Commands Quick Reference / อ้างอิงคำสั่ง Docker ด่วน](https://docs.docker.com/engine/reference/commandline/cli/)

---

## Quick Reference Card / บัตรอ้างอิงด่วน

### Most Common Commands / คำสั่งที่ใช้บ่อยที่สุด

| Task / งาน | Command / คำสั่ง |
|---|---|
| Build image / สร้างอิมเมจ | `docker build -t myapp .` |
| Run container / รัน Container | `docker run -d -p 8080:80 myapp` |
| View logs / ดู Log | `docker logs -f container_name` |
| Enter container / เข้าสู่ Container | `docker exec -it container_name bash` |
| Stop all / หยุดทั้งหมด | `docker stop $(docker ps -q)` |
| Remove all / ลบทั้งหมด | `docker rm $(docker ps -aq)` |
| System prune / ทำความสะอาดระบบ | `docker system prune -a --volumes` |
| Compose up / Compose เริ่ม | `docker-compose up -d` |
| Compose down / Compose หยุด | `docker-compose down -v` |

---

> **Tip / เคล็ดลับ:** Use `docker --help` or `docker <command> --help` for detailed information on any command.
>
> **เคล็ดลับ:** ใช้ `docker --help` หรือ `docker <command> --help` สำหรับข้อมูลรายละเอียดของคำสั่งใด ๆ
