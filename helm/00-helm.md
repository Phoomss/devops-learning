# Helm / เครื่องมือจัดการแพ็กเกจสำหรับ Kubernetes

## What is Helm? / Helm คืออะไร?

Helm is the package manager for Kubernetes. It simplifies deploying complex applications with reusable charts. Helm allows you to define, install, and upgrade even the most complex Kubernetes applications using a templating system.

Helm คือเครื่องมือจัดการแพ็กเกจ (package manager) สำหรับ Kubernetes ช่วยให้การติดตั้งแอปพลิเคชันที่ซับซ้อนทำได้ง่ายขึ้นโดยใช้ "charts" ที่สามารถนำกลับมาใช้ใหม่ได้ Helm ช่วยให้คุณสามารถกำหนด ติดตั้ง และอัปเดตแอปพลิเคชัน Kubernetes ที่ซับซ้อนที่สุดได้โดยใช้ระบบเทมเพลต

Think of Helm as the "apt" or "yum" for Kubernetes — but instead of installing software on a Linux system, you're deploying applications on a Kubernetes cluster.

ให้คิดว่า Helm เป็นเหมือน "apt" หรือ "yum" สำหรับ Kubernetes — แต่แทนที่จะติดตั้งซอฟต์แวร์บนระบบ Linux คุณกำลังติดตั้งแอปพลิเคชันบนคลัสเตอร์ Kubernetes

## Why Use Helm? / ทำไมต้องใช้ Helm?

Helm provides several key benefits that make Kubernetes application management significantly easier:

Helm มีข้อดีหลายประการที่ทำให้การจัดการแอปพลิเคชัน Kubernetes ง่ายขึ้นอย่างมาก:

- **Templating / เทมเพลต**: Parameterize Kubernetes manifests — define reusable templates with placeholders for values that change between environments
  - สร้างพารามิเตอร์ให้กับ Kubernetes manifests — กำหนดเทมเพลตที่ใช้ซ้ำได้พร้อมช่องใส่ค่าที่เปลี่ยนแปลงระหว่างสภาพแวดล้อม

- **Reusability / การใช้ซ้ำ**: Share configurations across teams — package an application once and deploy it anywhere
  - แบ่งปันการตั้งค่าระหว่างทีม — แพ็กแอปพลิเคชันครั้งเดียวแล้วติดตั้งได้ทุกที่

- **Versioning / การควบคุมเวอร์ชัน**: Track chart versions — know exactly what version of your application is deployed
  - ติดตามเวอร์ชันของ chart — รู้ว่าแอปพลิเคชันเวอร์ชันใดถูกติดตั้งอยู่

- **Rollbacks / การย้อนกลับ**: Easy rollback to previous versions — if something breaks, go back instantly
  - ย้อนกลับไปเวอร์ชันก่อนหน้าได้ง่าย — หากเกิดปัญหา สามารถย้อนกลับได้ทันที

- **Dependencies / การจัดการ_DEPENDENCIES**: Manage multi-component apps — deploy databases, caches, and apps together
  - จัดการแอปหลายคอมโพเนนต์ — ติดตั้งฐานข้อมูล, cache และแอปร่วมกันได้

- **Simplify / ทำให้เรียบง่าย**: One command instead of multiple `kubectl apply` — deploy entire applications with a single command
  - ใช้คำสั่งเดียวแทน `kubectl apply` หลายครั้ง — ติดตั้งแอปทั้งหมดด้วยคำสั่งเดียว

## Key Concepts / แนวคิดหลัก

| Term / คำศัพท์ | Description / คำอธิบาย |
|----------------|------------------------|
| **Chart / ชาร์ต** | A package containing Kubernetes resource templates and configuration. Think of it as a "recipe" for deploying an application. |
| | แพ็กเกจที่มีเทมเพลตและการตั้งค่าของ Kubernetes ถือเป็น "สูตรอาหาร" สำหรับการติดตั้งแอปพลิเคชัน |
| **Release / รีลีส** | A running instance of a chart in a cluster. Each time you install a chart, you create a new release. |
| | อินสแตนซ์ของ chart ที่กำลังทำงานอยู่ในคลัสเตอร์ ทุกครั้งที่ติดตั้ง chart จะเกิด release ใหม่ |
| **Values / ค่าตั้งค่า** | Configuration parameters that customize a chart. These override default values in `values.yaml`. |
| | พารามิเตอร์การตั้งค่าที่ใช้ปรับแต่ง chart ค่าเหล่านี้จะแทนที่ค่าเริ่มต้นใน `values.yaml` |
| **Repository / ที่เก็บชาร์ต** | A storage location (HTTP server, OCI registry) where charts are stored and shared. |
| | ที่อยู่เก็บ charts (เซิร์ฟเวอร์ HTTP, OCI registry) สำหรับเก็บและแบ่งปัน charts |
| **Template / เทมเพลต** | Kubernetes manifest files using Go template syntax that generate actual YAML when combined with values. |
| | ไฟล์ Kubernetes manifest ที่ใช้ Go template syntax ซึ่งจะสร้าง YAML จริงเมื่อรวมกับค่า values |

## Helm vs kubectl / Helm กับ kubectl

**Without Helm / โดยไม่ใช้ Helm:**
You need to apply each file individually, and managing different environments becomes complex:
คุณต้อง apply แต่ละไฟล์แยกกัน และการจัดการสภาพแวดล้อมต่าง ๆ จะซับซ้อนขึ้น:

```bash
# Deploying without Helm requires many commands / การติดตั้งโดยไม่ใช้ Helm ต้องใช้หลายคำสั่ง
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f configmap.yaml
kubectl apply -f ingress.yaml
# ... and many more files / ... และไฟล์อื่น ๆ อีกมาก
```

**With Helm / เมื่อใช้ Helm:**
A single command handles everything with proper ordering and dependency management:
คำสั่งเดียวจัดการทุกอย่างพร้อมเรียงลำดับและจัดการ dependencies อย่างถูกต้อง:

```bash
# Deploying with Helm — one command does it all / ติดตั้งด้วย Helm — คำสั่งเดียวจัดการทั้งหมด
helm install my-app ./my-chart
```

## Installation / การติดตั้ง

### Method 1: Official Script / วิธีที่ 1: สคริปต์ทางการ

```bash
# Install Helm on Linux/macOS using the official installation script
# ติดตั้ง Helm บน Linux/macOS โดยใช้สคริปต์ติดตั้งทางการ
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Verify the installation was successful
# ตรวจสอบว่าการติดตั้งสำเร็จ
helm version
```

### Method 2: Homebrew (macOS) / วิธีที่ 2: Homebrew (macOS)

```bash
# Install Helm via Homebrew package manager
# ติดตั้ง Helm ผ่านตัวจัดการแพ็กเกจ Homebrew
brew install helm

# Verify installation
# ตรวจสอบการติดตั้ง
helm version
```

### Method 3: Chocolatey (Windows) / วิธีที่ 3: Chocolatey (Windows)

```bash
# Install Helm via Chocolatey package manager on Windows
# ติดตั้ง Helm ผ่านตัวจัดการแพ็กเกจ Chocolatey บน Windows
choco install kubernetes-helm

# Verify installation
# ตรวจสอบการติดตั้ง
helm version
```

### Method 4: Snap (Linux) / วิธีที่ 4: Snap (Linux)

```bash
# Install Helm via Snap on Linux
# ติดตั้ง Helm ผ่าน Snap บน Linux
sudo snap install helm --classic
```

## Chart Structure / โครงสร้างของ Chart

A Helm chart follows a standard directory structure. When you run `helm create`, it generates this layout:

Chart ของ Helm มีโครงสร้างไดเรกทอรีมาตรฐาน เมื่อคุณรัน `helm create` จะสร้างโครงสร้างนี้ให้:

```
my-chart/
├── Chart.yaml          # Chart metadata: name, version, description, dependencies
│                       # ข้อมูล chart: ชื่อ, เวอร์ชัน, คำอธิบาย, dependencies
├── values.yaml         # Default configuration values that can be overridden at install time
│                       # ค่าตั้งค่าเริ่มต้นที่สามารถแทนที่ได้ตอนติดตั้ง
├── charts/             # Directory containing subchart dependencies
│                       # ไดเรกทอรีที่มี subchart dependencies
├── templates/          # Kubernetes manifest templates that become actual resources
│   ├── deployment.yaml # Template for Deployment resource
│   │                   # เทมเพลตสำหรับทรัพยากร Deployment
│   ├── service.yaml    # Template for Service resource
│   │                   # เทมเพลตสำหรับทรัพยากร Service
│   ├── ingress.yaml    # Template for Ingress resource
│   │                   # เทมเพลตสำหรับทรัพยากร Ingress
│   ├── configmap.yaml  # Template for ConfigMap resource
│   │                   # เทมเพลตสำหรับทรัพยากร ConfigMap
│   ├── _helpers.tpl    # Named template helper functions (reusable snippets)
│   │                   # ฟังก์ชันช่วยเทมเพลต (โค้ดที่ใช้ซ้ำได้)
│   └── tests/
│       └── test-connection.yaml  # Test files to verify deployment works
│                                 # ไฟล์ทดสอบเพื่อยืนยันว่าการติดตั้งใช้งานได้
└── NOTES.txt           # Post-install instructions displayed after helm install
                        # คำแนะนำหลังติดตั้งที่แสดงหลัง helm install
```

### Chart.yaml

This file defines the metadata of your chart:

ไฟล์นี้กำหนดข้อมูลเมตาของ chart ของคุณ:

```yaml
# Chart.yaml — Defines chart metadata / กำหนดข้อมูลเมตาของ chart
apiVersion: v2                # Helm chart API version (v2 for Helm 3)
                              # เวอร์ชัน API ของ chart (v2 สำหรับ Helm 3)
name: my-application          # The name of the chart (must be lowercase, no spaces)
                              # ชื่อของ chart (ต้องเป็นตัวพิมพ์เล็ก ไม่มีช่องว่าง)
description: A Helm chart for my app  # Brief description of what the chart does
                                      # คำอธิบายสั้นว่า chart นี้ทำอะไร
type: application             # Chart type: "application" or "library"
                              # ประเภท chart: "application" หรือ "library"
version: 0.1.0                # Chart version (follows semantic versioning)
                              # เวอร์ชันของ chart (ตาม semantic versioning)
appVersion: "1.0.0"           # Application version (the actual app being deployed)
                              # เวอร์ชันของแอปพลิเคชัน (แอปจริงที่กำลังติดตั้ง)
```

### values.yaml

This file contains the default configuration values used by templates:

ไฟล์นี้มีค่าตั้งค่าเริ่มต้นที่เทมเพลตจะใช้:

```yaml
# values.yaml — Default configuration values / ค่าตั้งค่าเริ่มต้น

# Number of pod replicas to run / จำนวน pod replicas ที่จะรัน
replicaCount: 3

# Container image configuration / การตั้งค่า container image
image:
  repository: nginx           # Container image name / ชื่อ container image
  tag: "1.21"                 # Container image tag / แท็กของ container image
  pullPolicy: IfNotPresent    # Image pull policy: Always, IfNotPresent, Never
                              # นโยบายดึง image: Always, IfNotPresent, Never

# Service configuration / การตั้งค่า Service
service:
  type: ClusterIP             # Service type: ClusterIP, NodePort, LoadBalancer
                              # ประเภท Service: ClusterIP, NodePort, LoadBalancer
  port: 80                    # Port the service exposes / พอร์ตที่ service เปิดให้บริการ

# Resource limits and requests / ขีดจำกัดและคำขอทรัพยากร
resources:
  limits:                     # Maximum resources the container can use
    cpu: 100m                 # Maximum CPU (100 millicores = 0.1 CPU)
    memory: 128Mi             # Maximum memory (128 Mebibytes)
  requests:                   # Minimum resources guaranteed to the container
    cpu: 100m                 # Guaranteed CPU
    memory: 128Mi             # Guaranteed memory

# Ingress configuration / การตั้งค่า Ingress
ingress:
  enabled: true               # Whether to create an Ingress resource
                              # จะสร้างทรัพยากร Ingress หรือไม่
  hostname: myapp.example.com # The hostname for the Ingress
                              # ชื่อโฮสต์สำหรับ Ingress
```

## Basic Commands / คำสั่งพื้นฐาน

### Repository Management / การจัดการ Repository

```bash
# Add a chart repository / เพิ่ม repository ของ chart
helm repo add bitnami https://charts.bitnami.com/bitnami

# Update the local cache of repository charts
# อัปเดต cache ท้องถิ่นของ charts ใน repository
helm repo update

# List all configured repositories / แสดง repositories ที่ตั้งค่าไว้ทั้งหมด
helm repo list

# Remove a repository / ลบ repository
helm repo remove bitnami

# Add multiple repositories / เพิ่มหลาย repositories
helm repo add stable https://charts.helm.sh/stable
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

### Search Charts / การค้นหา Charts

```bash
# Search for charts in added repositories / ค้นหา charts ใน repositories ที่เพิ่มไว้
helm search repo nginx

# Search Artifact Hub (global Helm chart registry)
# ค้นหาใน Artifact Hub (registry ของ Helm chart ระดับโลก)
helm search hub nginx

# List all available versions of a chart
# แสดงเวอร์ชันทั้งหมดของ chart ที่มี
helm search repo -l nginx
```

### Install Chart / การติดตั้ง Chart

```bash
# Install a chart from a repository / ติดตั้ง chart จาก repository
helm install my-release bitnami/nginx

# Install a chart from a local directory / ติดตั้ง chart จากไดเรกทอรีท้องถิ่น
helm install my-release ./my-chart

# Install with a custom values file / ติดตั้งด้วยไฟล์ values แบบกำหนดเอง
helm install my-release ./my-chart -f custom-values.yaml

# Install with inline value overrides
# ติดตั้งพร้อมแทนที่ค่าแบบ inline
helm install my-release ./my-chart --set replicaCount=5

# Install in a specific namespace / ติดตั้งใน namespace ที่ระบุ
helm install my-release ./my-chart --namespace production

# Install with dry-run to preview / ติดตั้งแบบ dry-run เพื่อ preview
helm install my-release ./my-chart --dry-run --debug
```

### List Releases / การแสดงรายการ Releases

```bash
# List all releases in current namespace / แสดง releases ทั้งหมดใน namespace ปัจจุบัน
helm list

# List releases in all namespaces / แสดง releases ในทุก namespace
helm list -A

# Shorter alias for list / ตัวเขียนย่อสำหรับ list
helm ls

# Show releases with a specific filter / แสดง releases พร้อมตัวกรอง
helm list --filter "my-"
```

### Upgrade Release / การอัปเกรด Release

```bash
# Upgrade a release with chart changes / อัปเกรด release ด้วยการเปลี่ยนแปลง chart
helm upgrade my-release ./my-chart

# Upgrade with a new values file / อัปเกรดด้วยไฟล์ values ใหม่
helm upgrade my-release ./my-chart -f new-values.yaml

# Upgrade with inline value override / อัปเกรดพร้อมแทนที่ค่าแบบ inline
helm upgrade my-release ./my-chart --set image.tag=1.22

# Upgrade only if it's a new install or differs from current
# อัปเกรดเฉพาะเมื่อเป็นการติดตั้งใหม่หรือแตกต่างจากปัจจุบัน
helm upgrade --install my-release ./my-chart

# Wait for resources to be ready during upgrade
# รอให้ resources พร้อมระหว่างอัปเกรด
helm upgrade my-release ./my-chart --wait --timeout 5m
```

### Rollback / การย้อนกลับ

```bash
# Show revision history for a release
# แสดงประวัติ revision ของ release
helm history my-release

# Rollback to a specific revision (e.g., revision 1)
# ย้อนกลับไป revision ที่ระบุ (เช่น revision 1)
helm rollback my-release 1

# Rollback to the previous release
# ย้อนกลับไป release ก่อนหน้า
helm rollback my-release
```

### Uninstall / การลบ

```bash
# Uninstall a release (deletes all associated resources)
# ลบ release (ลบทรัพยากรที่เกี่ยวข้องทั้งหมด)
helm uninstall my-release

# Uninstall and keep the release history
# ลบแต่เก็บประวัติ release ไว้
helm uninstall my-release --keep-history
```

### Test Release / การทดสอบ Release

```bash
# Run tests defined in the chart / รันการทดสอบที่กำหนดใน chart
helm test my-release

# Show test logs / แสดง log การทดสอบ
helm test my-release --logs
```

### Get Values / การดึงค่า Values

```bash
# Get the values for a release / ดึงค่า values ของ release
helm get values my-release

# Get all release information / ดึงข้อมูล release ทั้งหมด
helm get all my-release

# Get specific manifest data / ดึงข้อมูล manifest เฉพาะ
helm get manifest my-release
```

## Template Syntax / ไวยากรณ์เทมเพลต

Helm uses Go's `text/template` language to create dynamic Kubernetes manifests. Templates are processed and combined with values to produce final YAML files.

Helm ใช้ภาษา `text/template` ของ Go เพื่อสร้าง Kubernetes manifests แบบไดนามิก เทมเพลตจะถูกประมวลผลและรวมกับ values เพื่อสร้างไฟล์ YAML สุดท้าย

### Basic Usage / การใช้งานพื้นฐาน

**values.yaml:**
```yaml
# Application configuration / การตั้งค่าแอปพลิเคชัน
app:
  name: my-application      # Application name / ชื่อแอปพลิเคชัน
  port: 8080                # Application port / พอร์ตแอปพลิเคชัน
```

**templates/deployment.yaml:**
```yaml
# Deployment template using Go template syntax
# เทมเพลต Deployment ที่ใช้ Go template syntax
apiVersion: apps/v1
kind: Deployment
metadata:
  # .Values.app.name pulls from values.yaml
  # .Values.app.name ดึงค่าจาก values.yaml
  name: {{ .Values.app.name }}
  labels:
    app: {{ .Values.app.name }}
spec:
  # .Values.replicaCount — number of replicas (can be set in values.yaml)
  # .Values.replicaCount — จำนวน replicas (ตั้งค่าใน values.yaml ได้)
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Values.app.name }}
  template:
    metadata:
      labels:
        app: {{ .Values.app.name }}
    spec:
      containers:
      - name: {{ .Values.app.name }}
        # Combine repository and tag to form the full image reference
        # รวม repository และ tag เพื่อสร้างข้อมูล image เต็มรูปแบบ
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
        ports:
        - containerPort: {{ .Values.app.port }}
```

### Control Structures / โครงสร้างควบคุม

#### If/Else / เงื่อนไข

```yaml
# Only create Ingress if ingress.enabled is true in values.yaml
# สร้าง Ingress เฉพาะเมื่อ ingress.enabled เป็น true ใน values.yaml
{{- if .Values.ingress.enabled }}
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: {{ .Values.app.name }}
spec:
  rules:
  - host: {{ .Values.ingress.hostname }}
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: {{ .Values.app.name }}
            port:
              number: {{ .Values.service.port }}
{{- end }}
```

#### Range (Loops) / การวนลูป

```yaml
# values.yaml snippet / ตัวอย่างใน values.yaml:
# ports:
#   - port: 80
#     targetPort: 8080
#   - port: 443
#     targetPort: 8443

# In template / ในเทมเพลต:
{{- range .Values.ports }}
- port: {{ .port }}
  targetPort: {{ .targetPort }}
{{- end }}
```

#### With / การกำหนดขอบเขต

```yaml
# with changes the context (.) to the specified object
# with เปลี่ยนบริบท (.) เป็น object ที่ระบุ
{{- with .Values.resources }}
resources:
  {{- toYaml . | nindent 2 }}
{{- end }}
```

#### Default Values / ค่าเริ่มต้น

```yaml
# Use default value if .Values.image.tag is not set
# ใช้ค่าเริ่มต้นหากไม่ได้ตั้งค่า .Values.image.tag
image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default "latest" }}"
```

### Built-in Objects / Object ในตัว

| Object / Object | Description / คำอธิบาย |
|-----------------|------------------------|
| `.Release` | Release information: `.Release.Name`, `.Release.Namespace`, `.Release.Revision`, `.Release.IsUpgrade`, `.Release.IsInstall` |
| | ข้อมูล release: ชื่อ, namespace, revision, กำลังอัปเกรดหรือติดตั้ง |
| `.Values` | Values passed from `values.yaml` or `--set` flags — your primary way to customize charts |
| | ค่าที่ส่งจาก `values.yaml` หรือ `--set` — วิธีหลักในการปรับแต่ง charts |
| `.Chart` | Chart metadata from `Chart.yaml`: `.Chart.Name`, `.Chart.Version`, `.Chart.AppVersion` |
| | ข้อมูล chart จาก `Chart.yaml` |
| `.Files` | Access non-template files within the chart using `.Files.Get "filename"` |
| | เข้าถึงไฟล์ที่ไม่ใช่เทมเพลตใน chart ด้วย `.Files.Get "filename"` |
| `.Capabilities` | Kubernetes cluster capabilities: API versions, kube version |
| | ความสามารถของคลัสเตอร์ Kubernetes: เวอร์ชัน API, เวอร์ชัน kube |
| `.Template` | Information about the current template being executed |
| | ข้อมูลเกี่ยวกับเทมเพลตที่กำลังทำงานอยู่ |
| `.Subcharts` | Access values from subcharts (dependencies) |
| | เข้าถึงค่าจาก subcharts (dependencies) |

## Real-World Example: Deploy WordPress / ตัวอย่างจริง: ติดตั้ง WordPress

### Step 1: Add Repository / ขั้นตอนที่ 1: เพิ่ม Repository

```bash
# Add the Bitnami chart repository (popular source of production-ready charts)
# เพิ่ม repository ของ Bitnami chart (แหล่ง chart ที่พร้อมใช้งานจริง)
helm repo add bitnami https://charts.bitnami.com/bitnami

# Update the local repository cache
# อัปเดต cache repository ท้องถิ่น
helm repo update
```

### Step 2: Install WordPress / ขั้นตอนที่ 2: ติดตั้ง WordPress

```bash
# Install WordPress with custom configuration
# ติดตั้ง WordPress พร้อมการตั้งค่าแบบกำหนดเอง
helm install my-blog bitnami/wordpress \
  --set wordpressUsername=admin \
  --set wordpressPassword=password \
  --set service.type=LoadBalancer
```

Explanation of flags / คำอธิบาย flags:
- `--set wordpressUsername=admin` — Set the admin username / กำหนดชื่อผู้ใช้ admin
- `--set wordpressPassword=password` — Set the admin password / กำหนดรหัสผ่าน admin
- `--set service.type=LoadBalancer` — Expose via LoadBalancer service / เปิดใช้งานผ่าน service แบบ LoadBalancer

### Step 3: Check Status / ขั้นตอนที่ 3: ตรวจสอบสถานะ

```bash
# Watch the service to get the external IP (press Ctrl+C when ready)
# ตรวจสอบ service เพื่อรับ IP ภายนอก (กด Ctrl+C เมื่อพร้อม)
kubectl get svc -w
```

### Step 4: Retrieve Credentials / ขั้นตอนที่ 4: ดึงข้อมูลยืนยันตัวตน

```bash
# Get the WordPress admin password from the Kubernetes secret
# ดึงรหัสผ่าน admin ของ WordPress จาก Kubernetes secret
kubectl get secret --namespace default my-blog-wordpress \
  -o jsonpath="{.data.wordpress-password}" | base64 --decode
```

### Step 5: Access WordPress / ขั้นตอนที่ 5: เข้าถึง WordPress

```bash
# Get the external IP of the LoadBalancer service
# รับ IP ภายนอกของ service แบบ LoadBalancer
export SERVICE_IP=$(kubectl get svc --namespace default my-blog-wordpress \
  -o jsonpath='{.status.loadBalancer.ingress[0].ip}')

# Print the WordPress admin URL
# แสดง URL ของ WordPress admin
echo "WordPress admin: http://$SERVICE_IP/admin"
```

## Create Your Own Chart / สร้าง Chart ของคุณเอง

### Generate a New Chart / สร้าง Chart ใหม่

```bash
# Create a new chart with a standard structure
# สร้าง chart ใหม่พร้อมโครงสร้างมาตรฐาน
helm create my-app

# The command generates a complete chart with:
# คำสั่งนี้จะสร้าง chart ครบถ้วนพร้อม:
#   - Deployment (with configurable replicas, image, resources)
#     Deployment (พร้อม replicas, image, resources ที่ปรับแต่งได้)
#   - Service (with configurable type and ports)
#     Service (พร้อม type และ ports ที่ปรับแต่งได้)
#   - Ingress (optional, with hostname configuration)
#     Ingress (ไม่บังคับ พร้อมการตั้งค่า hostname)
#   - Tests (basic connectivity test)
#     Tests (ทดสอบการเชื่อมต่อพื้นฐาน)
#   - NOTES.txt (post-install instructions)
#     NOTES.txt (คำแนะนำหลังติดตั้ง)
```

### Package a Chart / แพ็กเกจ Chart

```bash
# Package the chart into a versioned .tgz archive
# แพ็กเกจ chart เป็นไฟล์ .tgz พร้อมเวอร์ชัน
helm package my-app
# Output: my-app-0.1.0.tgz
```

### Lint a Chart / ตรวจสอบ Chart

```bash
# Check the chart for common issues and best practices
# ตรวจสอบ chart เพื่อหาปัญหาทั่วไปและแนวปฏิบัติที่ดีที่สุด
helm lint my-app
```

### Dry Run / ทดสอบแบบ Dry Run

```bash
# Preview what would be installed without actually deploying
# ดูสิ่งที่กำลังจะติดตั้งโดยไม่ติดตั้งจริง
helm install my-release ./my-app --dry-run --debug
```

### Render Templates / ร้อนเดอร์เทมเพลต

```bash
# Render the templates locally (outputs the resulting YAML to stdout)
# ร้อนเดอร์เทมเพลตในเครื่อง (แสดง YAML ผลลัพธ์ออก stdout)
helm template my-release ./my-app

# Render with custom values
# ร้อนเดอร์ด้วยค่าที่กำหนดเอง
helm template my-release ./my-app -f custom-values.yaml
```

## Chart Dependencies / Dependencies ของ Chart

You can declare other charts as dependencies, enabling you to compose complex applications from reusable components:

คุณสามารถประกาศ charts อื่น ๆ เป็น dependencies ทำให้คุณสามารถประกอบแอปพลิเคชันที่ซับซ้อนจากคอมโพเนนต์ที่ใช้ซ้ำได้:

### Chart.yaml with Dependencies

```yaml
# Chart.yaml — Declaring chart dependencies
# Chart.yaml — ประกาศ dependencies ของ chart
apiVersion: v2
name: my-webapp
description: A web app with database and cache
type: application
version: 0.1.0
appVersion: "1.0.0"

# Dependencies: other charts this chart requires
# Dependencies: charts อื่น ๆ ที่ chart นี้ต้องการ
dependencies:
  - name: postgresql            # PostgreSQL database chart
                                # chart ฐานข้อมูล PostgreSQL
    version: "12.*"             # Version constraint (supports semver ranges)
                                # ข้อจำกัดเวอร์ชัน (รองรับ semver ranges)
    repository: "https://charts.bitnami.com/bitnami"  # Where to find it
                                                       # ที่ที่จะหา chart นี้
  - name: redis                 # Redis cache chart
                                # chart cache Redis
    version: "17.*"
    repository: "https://charts.bitnami.com/bitnami"
```

### Managing Dependencies / การจัดการ Dependencies

```bash
# Download and install chart dependencies
# ดาวน์โหลดและติดตั้ง dependencies ของ chart
helm dependency update

# Alternative: build dependencies (uses Chart.lock if it exists)
# ตัวเลือกอื่น: build dependencies (ใช้ Chart.lock ถ้ามี)
helm dependency build

# List chart dependencies / แสดง dependencies ของ chart
helm dependency list
```

## Best Practices / แนวปฏิบัติที่ดีที่สุด

1. **Version charts / ใช้เวอร์ชันให้ chart**: Use semantic versioning (MAJOR.MINOR.PATCH) for chart versions. Increment MAJOR for breaking changes, MINOR for new features, PATCH for bug fixes.
   - ใช้ semantic versioning (MAJOR.MINOR.PATCH) สำหรับเวอร์ชัน chart เพิ่ม MAJOR เมื่อมีการเปลี่ยนแปลงที่ทำให้เข้ากันไม่ได้, MINOR สำหรับฟีเจอร์ใหม่, PATCH สำหรับแก้ไขบั๊ก

2. **Use values.yaml / ใช้ values.yaml**: Don't hardcode values in templates. Always use `.Values` so users can customize deployments.
   - อย่า hardcode ค่าในเทมเพลต ใช้ `.Values` เสมอเพื่อให้ผู้ใช้ปรับแต่งการติดตั้งได้

3. **Document / ทำเอกสาร**: Add `NOTES.txt` with post-install instructions. Include connection details, default credentials (warning about changing them), and next steps.
   - เพิ่ม `NOTES.txt` พร้อมคำแนะนำหลังติดตั้ง ใส่รายละเอียดการเชื่อมต่อ, ข้อมูลยืนยันตัวตนเริ่มต้น (พร้อมเตือนให้เปลี่ยน), และขั้นตอนถัดไป

4. **Test charts / ทดสอบ chart**: Use `helm test` to verify deployments work correctly. Include connectivity tests in your chart.
   - ใช้ `helm test` เพื่อยืนยันว่าการติดตั้งทำงานถูกต้อง รวมการทดสอบการเชื่อมต่อใน chart ของคุณ

5. **Store in registry / เก็บใน registry**: Use OCI registries (like Harbor, Docker Hub, or cloud provider registries) to store and distribute charts.
   - ใช้ OCI registry (เช่น Harbor, Docker Hub, หรือ registry ของ cloud provider) เพื่อเก็บและแจกจ่าย charts

6. **Keep simple / ทำให้เรียบง่าย**: One concern per chart. Don't bundle unrelated services together. Use dependencies for related but separate services.
   - หนึ่ง concern ต่อหนึ่ง chart อย่ารวมบริการที่ไม่เกี่ยวข้องเข้าด้วยกัน ใช้ dependencies สำหรับบริการที่เกี่ยวข้องแต่แยกจากกัน

7. **Use helpers / ใช้ helper**: Extract common template patterns into `_helpers.tpl` named templates. This avoids duplication and keeps templates clean.
   - แยกรูปแบบเทมเพลตทั่วไปเป็นเทมเพลตชื่อใน `_helpers.tpl` ช่วยหลีกเลี่ยงการซ้ำซ้อนและทำให้เทมเพลตสะอาด

8. **Use `--set` sparingly / ใช้ `--set` อย่างประหยัด**: For complex configurations, use `-f values.yaml` files instead of long `--set` chains.
   - สำหรับการตั้งค่าที่ซับซ้อน ใช้ไฟล์ `-f values.yaml` แทน `--set` ยาว ๆ

## Helm with CI/CD / Helm กับ CI/CD

Integrating Helm into your CI/CD pipeline enables automated, repeatable deployments:

การรวม Helm เข้ากับ CI/CD pipeline ทำให้คุณสามารถติดตั้งอัตโนมัติและทำซ้ำได้:

### Example CI/CD Pipeline Script / ตัวอย่างสคริปต์ CI/CD Pipeline

```bash
#!/bin/bash
# CI/CD Pipeline example for Helm deployments
# ตัวอย่าง CI/CD Pipeline สำหรับการติดตั้งด้วย Helm

# Step 1: Lint the chart — catch errors early
# ขั้นตอนที่ 1: Lint chart — จับข้อผิดพลาดตั้งแต่เนิ่นๆ
helm lint ./chart

# Step 2: Package the chart into a versioned archive
# ขั้นตอนที่ 2: แพ็กเกจ chart เป็นไฟล์ archive พร้อมเวอร์ชัน
helm package ./chart

# Step 3: Push the chart to an OCI registry (e.g., Harbor, Docker Hub)
# ขั้นตอนที่ 3: ผลักดัน chart ไปยัง OCI registry (เช่น Harbor, Docker Hub)
helm push my-chart-0.1.0.tgz oci://registry.example.com

# Step 4: Deploy or upgrade the application
# ขั้นตอนที่ 4: ติดตั้งหรืออัปเกรดแอปพลิเคชัน
helm upgrade --install my-app oci://registry.example.com/my-chart \
  --version 0.1.0 \       # Specify exact chart version / ระบุเวอร์ชัน chart ที่แน่นอน
  --values production.yaml \  # Use environment-specific values / ใช้ค่าเฉพาะสภาพแวดล้อม
  --wait \                # Wait for all resources to be ready / รอให้ resources ทั้งหมดพร้อม
  --timeout 5m \          # Maximum wait time / เวลาสูงสุดที่รอ
  --atomic                # Auto-rollback on failure / ย้อนกลับอัตโนมัติเมื่อล้มเหลว
```

### GitOps with Helm / GitOps กับ Helm

You can also use GitOps tools like ArgoCD or Flux to manage Helm releases declaratively:

คุณยังสามารถใช้เครื่องมือ GitOps เช่น ArgoCD หรือ Flux เพื่อจัดการ Helm releases แบบ declarative:

```yaml
# ArgoCD Application example / ตัวอย่าง ArgoCD Application
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app              # Application name in ArgoCD / ชื่อแอปพลิเคชันใน ArgoCD
  namespace: argocd         # ArgoCD namespace / namespace ของ ArgoCD
spec:
  project: default          # ArgoCD project / โปรเจกต์ ArgoCD
  source:
    repoURL: https://github.com/myorg/helm-charts.git  # Git repo containing charts
    targetRevision: HEAD    # Git branch/tag / สาขากล/tag ของ Git
    path: charts/my-app     # Path to chart in repo / เส้นทางไปยัง chart ใน repo
    helm:
      valueFiles:
      - values-production.yaml  # Environment-specific values / ค่าเฉพาะสภาพแวดล้อม
  destination:
    server: https://kubernetes.default.srv  # Target cluster / คลัสเตอร์เป้าหมาย
    namespace: production   # Target namespace / namespace เป้าหมาย
```

## Practice Exercises (Task 3) / แบบฝึกหัด (Task 3)

### Exercise 1: Understand Helm / แบบฝึกหัดที่ 1: ทำความเข้าใจ Helm

- **Question / คำถาม**: What is Helm and what does it do? How does it simplify Kubernetes deployments?
  - **คำถาม**: Helm คืออะไรและทำหน้าที่อะไร? ทำให้การติดตั้ง Kubernetes ง่ายขึ้นอย่างไร?
- **Question / คำถาม**: Compare deploying an application with `kubectl apply -f` versus `helm install`. What are the advantages of each approach?
  - **คำถาม**: เปรียบเทียบการติดตั้งแอปพลิเคชันด้วย `kubectl apply -f` กับ `helm install` ข้อดีของแต่ละวิธีคืออะไร?

### Exercise 2: Install and Use Helm / แบบฝึกหัดที่ 2: ติดตั้งและใช้ Helm

- **Task / งาน**: Install Helm on your machine and verify the installation with `helm version`.
  - **งาน**: ติดตั้ง Helm บนเครื่องของคุณและตรวจสอบการติดตั้งด้วย `helm version`
- **Task / งาน**: Add the Bitnami repository and deploy an application (e.g., nginx, Redis, or WordPress) from it.
  - **งาน**: เพิ่ม repository ของ Bitnami และติดตั้งแอปพลิเคชัน (เช่น nginx, Redis, หรือ WordPress) จาก repository นั้น
- **Task / งาน**: Check the release status with `helm list` and inspect the deployed resources with `kubectl get all`.
  - **งาน**: ตรวจสอบสถานะ release ด้วย `helm list` และตรวจสอบ resources ที่ติดตั้งด้วย `kubectl get all`

### Exercise 3: Create Custom Chart / แบบฝึกหัดที่ 3: สร้าง Chart แบบกำหนดเอง

- **Task / งาน**: Create a Helm chart for a simple web application (e.g., a static HTML page served by Nginx).
  - **งาน**: สร้าง Helm chart สำหรับเว็บแอปพลิเคชันง่าย ๆ (เช่น หน้า HTML แบบ static ที่ให้บริการโดย Nginx)
- **Task / งาน**: Parameterize the following values: `replicas`, `image.tag`, `service.type`, and `service.port`.
  - **งาน**: ทำให้ค่าต่อไปนี้สามารถกำหนดพารามิเตอร์ได้: `replicas`, `image.tag`, `service.type`, และ `service.port`
- **Task / งาน**: Deploy the chart to your Kubernetes cluster and verify it works.
  - **งาน**: ติดตั้ง chart ลงในคลัสเตอร์ Kubernetes ของคุณและยืนยันว่าใช้งานได้

### Exercise 4: Upgrade and Rollback / แบบฝึกหัดที่ 4: อัปเกรดและย้อนกลับ

- **Task / งาน**: Upgrade the release by changing the number of replicas and the image tag.
  - **งาน**: อัปเกรด release โดยเปลี่ยนจำนวน replicas และ image tag
- **Task / งาน**: Rollback to the previous version using `helm rollback` and verify the change.
  - **งาน**: ย้อนกลับไปเวอร์ชันก่อนหน้าด้วย `helm rollback` และยืนยันการเปลี่ยนแปลง
- **Task / งาน**: Compare revisions using `helm history` and observe the differences.
  - **งาน**: เปรียบเทียบ revision ด้วย `helm history` และสังเกตความแตกต่าง

### Exercise 5: Bonus (Advanced) / แบบฝึกหัดที่ 5: โบนัส (ขั้นสูง)

- **Task / งาน**: Create a chart with dependencies (e.g., your web app + a Redis cache).
  - **งาน**: สร้าง chart พร้อม dependencies (เช่น เว็บแอปของคุณ + Redis cache)
- **Task / งาน**: Package the chart with `helm package` and push it to an OCI registry.
  - **งาน**: แพ็กเกจ chart ด้วย `helm package` และผลักดันไปยัง OCI registry
- **Task / งาน**: Write template tests to verify your deployment works correctly.
  - **งาน**: เขียนการทดสอบเทมเพลตเพื่อยืนยันว่าการติดตั้งทำงานถูกต้อง
- **Task / งาน**: Create a multi-environment setup using separate values files (`values-dev.yaml`, `values-staging.yaml`, `values-prod.yaml`).
  - **งาน**: สร้างการตั้งค่าหลายสภาพแวดล้อมโดยใช้ไฟล์ values แยกกัน (`values-dev.yaml`, `values-staging.yaml`, `values-prod.yaml`)

## Resources / แหล่งข้อมูล

### Official Documentation / เอกสารทางการ
- **[Helm Official Documentation / เอกสาร Helm ทางการ](https://helm.sh/docs/)** — The definitive guide to Helm 3, including installation, usage, and advanced topics
  - คู่มือทางการของ Helm 3 ครอบคลุมการติดตั้ง, การใช้งาน, และหัวข้อขั้นสูง
- **[Helm GitHub Repository / ที่เก็บ GitHub ของ Helm](https://github.com/helm/helm)** — Source code, issue tracking, and release notes
  - ซอร์สโค้ด, ติดตามปัญหา, และบันทึกการปล่อยเวอร์ชัน

### Chart Repositories / ที่เก็บ Charts
- **[Artifact Hub](https://artifacthub.io/)** — Search for community-maintained Helm charts. The largest collection of publicly available charts.
  - ค้นหา Helm charts ที่ดูแลโดยชุมชน การรวบรวม charts สาธารณะที่ใหญ่ที่สุด
- **[Bitnami Charts](https://github.com/bitnami/charts)** — Production-ready charts for popular applications (WordPress, Redis, PostgreSQL, etc.)
  - charts ที่พร้อมใช้งานจริงสำหรับแอปพลิเคชันยอดนิยม (WordPress, Redis, PostgreSQL ฯลฯ)

### Best Practices / แนวปฏิบัติที่ดีที่สุด
- **[Helm Chart Best Practices / แนวปฏิบัติที่ดีที่สุดสำหรับ Helm Chart](https://helm.sh/docs/chart_best_practices/)** — Official guidelines for writing high-quality Helm charts
  - แนวทางการเขียน Helm chart คุณภาพสูงอย่างเป็นทางการ
- **[Helm Template Developer Guide / คู่มือผู้พัฒนาเทมเพลต Helm](https://helm.sh/docs/chart_template_guide/)** — In-depth guide to Helm's template language and functions
  - คู่มือเชิงลึกเกี่ยวกับภาษาเทมเพลตและฟังก์ชันของ Helm

### Community and Learning / ชุมชนและการเรียนรู้
- **[Helm Slack Channel (#helm-users) / ช่อง Slack ของ Helm](https://kubernetes.slack.com/)** — Ask questions and get help from the Helm community
  - ถามคำถามและรับความช่วยเหลือจากชุมชน Helm
- **[Awesome Helm / Awesome Helm](https://github.com/rrimann/awesome-helm)** — Curated list of Helm resources, tools, and tutorials
  - รายการทรัพยากร, เครื่องมือ, และบทสอนเกี่ยวกับ Helm ที่คัดสรรแล้ว

### Related Kubernetes Tools / เครื่องมือ Kubernetes ที่เกี่ยวข้อง
- **[kubectl Documentation / เอกสาร kubectl](https://kubernetes.io/docs/reference/kubectl/)** — The Kubernetes command-line tool
  - เครื่องมือ command-line ของ Kubernetes
- **[Kustomize](https://kustomize.io/)** — An alternative configuration management tool for Kubernetes
  - เครื่องมือจัดการการตั้งค่าทางเลือกสำหรับ Kubernetes
- **[ArgoCD](https://argoproj.github.io/cd/)** — GitOps continuous delivery tool for Kubernetes with native Helm support
  - เครื่องมือส่งมอบแบบ GitOps สำหรับ Kubernetes พร้อมรองรับ Helm โดยตรง
