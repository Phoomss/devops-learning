# DevOps Learning Guide

## Table of Contents

1. [Linux Fundamentals](#1-linux-fundamentals)
2. [OSI Model](#2-osi-model)
3. [Docker](#3-docker)
4. [Kubernetes Components](#4-kubernetes-components)
5. [TLS (Transport Layer Security)](#5-tls-transport-layer-security)
6. [SRE (Site Reliability Engineering)](#6-sre-site-reliability-engineering)
7. [Git Workflow](#7-git-workflow)
8. [Helm](#8-helm)
9. [Kubernetes Installation](#9-kubernetes-installation)
10. [Kubernetes CNI & OSI Model](#10-kubernetes-cni--osi-model)

---

## 1. Linux Fundamentals

### What is Linux?
Linux is an open-source, Unix-like operating system kernel that powers most of the internet's servers, cloud infrastructure, and DevOps tools.

### Key Concepts

#### File System Hierarchy
```
/         - Root directory
/bin      - Essential binaries (ls, cp, cat)
/etc      - Configuration files
/home     - User home directories
/var      - Variable data (logs, databases)
/usr      - User programs and libraries
/tmp      - Temporary files
```

#### Essential Commands

**File Operations:**
```bash
ls -la          # List all files with details
cd /path        # Change directory
pwd             # Print working directory
cp src dest     # Copy files
mv src dest     # Move/rename files
rm -rf dir      # Remove directory recursively
```

**System Information:**
```bash
top             # Process monitor
htop            # Interactive process viewer
df -h           # Disk space
free -m         # Memory usage
uname -a        # System information
```

**Permissions:**
```bash
chmod 755 file  # Change permissions
chown user:group file  # Change ownership
```

**Networking:**
```bash
ip addr         # Show IP addresses
ss -tuln        # Show listening ports
ping host       # Test connectivity
curl URL        # HTTP requests
```

#### Package Management (Ubuntu/Debian)
```bash
apt update              # Update package list
apt install package     # Install package
apt remove package      # Remove package
apt upgrade             # Upgrade all packages
```

#### Service Management (systemd)
```bash
systemctl start service     # Start service
systemctl stop service      # Stop service
systemctl enable service    # Enable on boot
systemctl status service    # Check status
journalctl -u service       # View logs
```

---

## 2. OSI Model

### What is the OSI Model?
The Open Systems Interconnection (OSI) model is a conceptual framework that standardizes network communications into 7 layers.

### The 7 Layers

| Layer | Name | Function | Examples |
|-------|------|----------|----------|
| 7 | Application | Network services to applications | HTTP, FTP, DNS |
| 6 | Presentation | Data translation, encryption | SSL/TLS, JPEG |
| 5 | Session | Connection management | NetBIOS, RPC |
| 4 | Transport | End-to-end communication | TCP, UDP |
| 3 | Network | Routing, IP addressing | IP, ICMP, routers |
| 2 | Data Link | MAC addressing, switching | Ethernet, switches |
| 1 | Physical | Physical transmission | Cables, hubs |

### Why DevOps Engineers Need to Know OSI

- **Troubleshooting**: Identify which layer has issues
- **Network Policies**: Configure firewalls and security groups
- **Load Balancing**: Understand L4 vs L7 load balancing
- **CNI Plugins**: Kubernetes networking operates at L2-L4

### Real-World Example

When you access `https://example.com`:
1. **L7**: HTTP request
2. **L6**: TLS encryption
3. **L5**: Session established
4. **L4**: TCP connection (port 443)
5. **L3**: IP routing to server
6. **L2**: Ethernet frames
7. **L1**: Electrical signals over cable

---

## 3. Docker

### What is Docker?
Docker is a containerization platform that packages applications and dependencies into isolated containers.

### Key Concepts

**Image vs Container:**
- **Image**: Blueprint/template (read-only)
- **Container**: Running instance of an image

### Essential Commands

```bash
# Image management
docker pull nginx              # Download image
docker images                  # List images
docker rmi image_id            # Remove image

# Container management
docker run -d --name web nginx # Run container
docker ps                      # List running containers
docker ps -a                   # List all containers
docker stop container          # Stop container
docker rm container            # Remove container

# Debugging
docker logs container          # View logs
docker exec -it container bash # Enter container
docker inspect container       # Detailed info

# Cleanup
docker system prune            # Remove unused resources
```

### Dockerfile

A Dockerfile is a script to build Docker images:

```dockerfile
# Base image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy dependencies
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy application code
COPY . .

# Expose port
EXPOSE 3000

# Run application
CMD ["node", "server.js"]
```

Build and run:
```bash
docker build -t myapp .
docker run -d -p 3000:3000 myapp
```

### Docker Compose

Manage multi-container applications:

```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "3000:3000"
  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: secret
```

```bash
docker-compose up -d
docker-compose down
```

---

## 4. Kubernetes Components

### What is Kubernetes?
Kubernetes (K8s) is a container orchestration platform for automating deployment, scaling, and management of containerized applications.

### Architecture

#### Control Plane Components

1. **API Server**: Front-end to K8s control plane, handles REST requests
2. **etcd**: Distributed key-value store (cluster data)
3. **Scheduler**: Assigns pods to nodes based on resources
4. **Controller Manager**: Runs controller processes
5. **Cloud Controller**: Interfaces with cloud providers

#### Node Components

1. **kubelet**: Agent that runs on each node
2. **kube-proxy**: Network proxy for services
3. **Container Runtime**: Runs containers (containerd, CRI-O)

### Key Objects

**Pod**: Smallest deployable unit (one or more containers)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

**Deployment**: Manages replica sets and rolling updates
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
```

**Service**: Stable network endpoint for pods
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
  type: ClusterIP
```

**Namespace**: Virtual cluster isolation
```bash
kubectl create namespace dev
kubectl get pods -n dev
```

### Essential Commands

```bash
# Cluster info
kubectl cluster-info
kubectl get nodes

# Resources
kubectl get pods
kubectl get services
kubectl get deployments
kubectl get all

# Debugging
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- bash

# Apply/Delete
kubectl apply -f manifest.yaml
kubectl delete -f manifest.yaml
```

---

## 5. TLS (Transport Layer Security)

### What is TLS?
TLS is a cryptographic protocol that provides secure communication over networks by encrypting data in transit.

### TLS Certificate Components

- **Public Key**: Encrypts data (shared openly)
- **Private Key**: Decrypts data (kept secret)
- **Certificate**: Binds public key to identity (signed by CA)
- **CA (Certificate Authority)**: Trusted entity that issues certificates

### ACME Protocol

Automated Certificate Management Environment (ACME) automates TLS certificate issuance.

#### Challenge Types

**HTTP-01 Challenge:**
- Proves domain control by placing a file at `http://<domain>/.well-known/acme-challenge/`
- Works for standard web domains
- Requires HTTP server accessible from internet

**DNS-01 Challenge:**
- Proves domain control by adding a TXT record
- Works for wildcard certificates (`*.example.com`)
- Requires DNS API access

### Practical Task: Issue TLS Certificate with Let's Encrypt

#### Method 1: Using Certbot (HTTP-01)

```bash
# Install certbot
apt install certbot

# Issue certificate (standalone mode)
certbot certonly --standalone -d yourdomain.com

# Issue certificate (webroot mode)
certbot certonly --webroot -w /var/www/html -d yourdomain.com

# Check certificate
certbot certificates

# Auto-renewal
certbot renew --dry-run
```

#### Method 2: Using Certbot (DNS-01)

```bash
# Install DNS plugin (example: Cloudflare)
apt install python3-certbot-dns-cloudflare

# Create credentials file
cat > cloudflare.ini << EOF
dns_cloudflare_email = your-email@example.com
dns_cloudflare_api_key = your-api-key
EOF

chmod 600 cloudflare.ini

# Issue certificate
certbot certonly --dns-cloudflare \
  --dns-cloudflare-credentials cloudflare.ini \
  -d yourdomain.com \
  -d '*.yourdomain.com'
```

#### View Certificate Details

```bash
# Check expiry
openssl x509 -enddate -noout -in /etc/letsencrypt/live/yourdomain.com/cert.pem

# View full details
openssl x509 -text -noout -in /etc/letsencrypt/live/yourdomain.com/cert.pem
```

#### Screenshot Documentation

**When certificate is issued:**
```bash
certbot certificates
```
Expected output shows:
- Certificate path
- Expiry date
- Domain names

**When certificate is near expiry:**
```bash
# Certificates expire in 90 days
# Check expiry date
openssl x509 -enddate -noout -in /etc/letsencrypt/live/yourdomain.com/cert.pem
```

**Renew expired certificates:**
```bash
certbot renew
```

### Certificate Lifecycle

```
Request → Validate (Challenge) → Issue → Install → Auto-renew (every 60 days)
```

---

## 6. SRE (Site Reliability Engineering)

### What is SRE?
SRE is a discipline that combines software engineering and operations to create scalable and highly reliable systems.

### Key Concepts

**SLI (Service Level Indicator):**
- What you measure (latency, error rate, throughput)
- Example: 99% of requests respond in <200ms

**SLO (Service Level Objective):**
- Your target for an SLI
- Example: 99.9% uptime per month

**SLA (Service Level Agreement):**
- Contract with users (consequences if SLO not met)
- Example: 99.9% uptime or refund

### The Four Golden Signals

1. **Latency**: Time to serve a request
2. **Traffic**: Demand on your system
3. **Errors**: Rate of failed requests
4. **Saturation**: How "full" your system is

### Error Budget

```
Error Budget = 100% - SLO

Example:
SLO = 99.9%
Error Budget = 0.1% = 43 minutes downtime/month allowed
```

### SRE Practices

- **Toil Reduction**: Automate manual work
- **Blameless Postmortems**: Focus on system fixes, not blame
- **Monitoring & Alerting**: Detect issues proactively
- **Capacity Planning**: Scale before problems occur

### Tools

- **Monitoring**: Prometheus, Grafana, Datadog
- **Logging**: ELK Stack, Loki
- **Tracing**: Jaeger, Zipkin
- **Alerting**: PagerDuty, OpsGenie

---

## 7. Git Workflow

### What is Git?
Git is a distributed version control system for tracking code changes.

### Trunk-Based Development vs Git Flow

#### Trunk-Based Development

**How it works:**
- Single main branch (trunk)
- Developers merge small changes frequently
- Feature flags hide incomplete features
- Continuous delivery from trunk

**Pros:**
✅ Faster integration
✅ Fewer merge conflicts
✅ Simpler workflow
✅ Better for CI/CD

**Cons:**
❌ Requires discipline (small commits)
❌ Feature flags needed

**Best for:** Teams practicing CI/CD, DevOps culture

#### Git Flow

**How it works:**
- `main` branch (production)
- `develop` branch (integration)
- Feature branches (`feature/*`)
- Release branches (`release/*`)
- Hotfix branches (`hotfix/*`)

**Workflow:**
```
1. Create feature branch from develop
2. Develop feature
3. Merge to develop
4. Create release branch
5. Test and fix bugs
6. Merge to main and tag
```

**Pros:**
✅ Structured
✅ Clear release process
✅ Good for versioned software

**Cons:**
❌ Complex
❌ Merge conflicts common
❌ Slower delivery

**Best for:** Traditional releases, versioned products

### Why the Difference Matters

| Aspect | Trunk-Based | Git Flow |
|--------|-------------|----------|
| Branches | 1 main + short-lived features | 5+ branch types |
| Merge frequency | Multiple times/day | At end of feature/sprint |
| Release cadence | Continuous | Scheduled |
| Team size | Any | Larger teams |
| CI/CD fit | Excellent | Moderate |

### Common Git Commands

```bash
# Basic operations
git init                    # Initialize repository
git clone <url>             # Clone repository
git add .                   # Stage changes
git commit -m "message"     # Commit changes
git push origin main        # Push to remote
git pull                    # Fetch and merge

# Branching
git branch feature          # Create branch
git checkout feature        # Switch branch
git checkout -b feature     # Create and switch
git merge feature           # Merge into current
git branch -d feature       # Delete branch

# History
git log --oneline           # Compact history
git diff                    # Show changes
git status                  # Show state
```

---

## 8. Helm

### What is Helm?
Helm is the package manager for Kubernetes. It simplifies deploying complex applications with reusable charts.

### Why Use Helm?

- **Templating**: Parameterize Kubernetes manifests
- **Reusability**: Share configurations
- **Versioning**: Track chart versions
- **Rollbacks**: Easy rollback to previous versions
- **Dependencies**: Manage multi-component apps

### Key Concepts

**Chart**: Package containing Kubernetes resources
**Release**: Running instance of a chart
**Values**: Configuration parameters
**Repository**: Chart storage location

### Installation

```bash
# Install Helm
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Verify installation
helm version
```

### Chart Structure

```
my-chart/
├── Chart.yaml          # Chart metadata
├── values.yaml         # Default values
├── charts/             # Dependencies
└── templates/          # Kubernetes manifests
    ├── deployment.yaml
    ├── service.yaml
    └── ingress.yaml
```

### Basic Commands

```bash
# Repository management
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm repo list

# Search charts
helm search repo nginx

# Install chart
helm install my-release bitnami/nginx

# List releases
helm list

# Upgrade release
helm upgrade my-release bitnami/nginx --set replicaCount=3

# Uninstall release
helm uninstall my-release

# Create your own chart
helm create my-app
```

### Example: Custom Values

```yaml
# values.yaml
replicaCount: 3

image:
  repository: nginx
  tag: "1.21"
  pullPolicy: IfNotPresent

service:
  type: LoadBalancer
  port: 80

resources:
  limits:
    cpu: 100m
    memory: 128Mi
  requests:
    cpu: 100m
    memory: 128Mi
```

```bash
# Install with custom values
helm install my-web ./my-chart -f values.yaml

# Override via CLI
helm install my-web ./my-chart --set replicaCount=5
```

### Real-World Example: Deploy WordPress

```bash
# Add repository
helm repo add bitnami https://charts.bitnami.com/bitnami

# Install WordPress
helm install my-blog bitnami/wordpress \
  --set wordpressUsername=admin \
  --set wordpressPassword=password \
  --set service.type=LoadBalancer

# Check status
kubectl get svc -w

# Get WordPress password
kubectl get secret --namespace default my-blog-wordpress \
  -o jsonpath="{.data.wordpress-password}" | base64 --decode
```

---

## 9. Kubernetes Installation

### Installation Methods Comparison

| Method | Complexity | Use Case | Features |
|--------|-----------|----------|----------|
| kubeadm | Medium | Learning, custom setups | Minimal, official tool |
| Kubespray | Medium-Hard | Production, automation | Ansible-based, flexible |
| Rancher | Easy | Enterprise, multi-cluster | GUI, full platform |

### Method 1: kubeadm

**What is it?**
Official Kubernetes tool for bootstrapping a minimum viable cluster.

**Prerequisites:**
- Ubuntu 20.04+ (2+ nodes)
- 2+ CPU, 2GB RAM per node
- Network connectivity between nodes
- Disabled swap

**Installation Steps:**

```bash
# ON ALL NODES

# 1. Disable swap
swapoff -a
sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

# 2. Install container runtime (containerd)
apt update
apt install -y containerd
mkdir -p /etc/containerd
containerd config default > /etc/containerd/config.toml
systemctl restart containerd

# 3. Install kubeadm, kubelet, kubectl
apt install -y apt-transport-https ca-certificates curl
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | tee /etc/apt/sources.list.d/kubernetes.list
apt update
apt install -y kubelet kubeadm kubectl
apt-mark hold kubelet kubeadm kubectl

# ON CONTROL PLANE NODE

# 4. Initialize cluster
kubeadm init --pod-network-cidr=10.244.0.0/16

# 5. Setup kubeconfig
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config

# 6. Install CNI (Calico)
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml

# ON WORKER NODES

# 7. Join cluster (use command from kubeadm init output)
kubeadm join <control-plane-ip>:6443 --token <token> --discovery-token-ca-cert-hash <hash>
```

### Method 2: Kubespray

**What is it?**
Ansible-based Kubernetes deployment tool for production-grade clusters.

**Prerequisites:**
- Ansible installed
- SSH access to all nodes
- Python on all nodes

**Installation Steps:**

```bash
# 1. Clone Kubespray
git clone https://github.com/kubernetes-sigs/kubespray.git
cd kubespray

# 2. Install dependencies
pip install -r requirements.txt

# 3. Create inventory
cp -rfp inventory/sample inventory/mycluster
declare -a IPS=(192.168.1.10 192.168.1.11 192.168.1.12)
CONFIG_FILE=inventory/mycluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}

# 4. Review inventory
cat inventory/mycluster/hosts.yaml

# 5. Deploy cluster
ansible-playbook -i inventory/mycluster/hosts.yaml --user=ubuntu --become cluster.yml
```

**Sample Inventory:**
```yaml
all:
  hosts:
    node1: {ansible_host: 192.168.1.10}
    node2: {ansible_host: 192.168.1.11}
    node3: {ansible_host: 192.168.1.12}
  children:
    kube_control_plane: [node1]
    kube_node: [node1, node2, node3]
    k8s_cluster:
      children:
        kube_control_plane:
        kube_node:
```

### Method 3: Rancher

**What is it?**
Complete Kubernetes management platform with GUI.

**Installation Steps:**

```bash
# 1. Install Docker (on Rancher server)
apt update
apt install -y docker.io

# 2. Run Rancher container
docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  --privileged \
  --name rancher \
  rancher/rancher:latest

# 3. Access Rancher UI
# Browse to https://<server-ip>
# Get initial password from logs:
docker logs rancher 2>&1 | grep "Bootstrap Password"

# 4. Create cluster via UI
# - Add Cluster → Custom
# - Select Kubernetes version
# - Copy node registration command
# - Run command on worker nodes

# 5. Download kubeconfig
# Rancher UI → Cluster → Kubeconfig File
```

---

## 10. Kubernetes CNI & OSI Model

### What is CNI?
Container Network Interface (CNI) is a specification and libraries for writing plugins to configure network interfaces in Linux containers.

### Why CNI is Needed in Kubernetes

Kubernetes requires:
1. **Pod-to-Pod communication**: All pods can communicate without NAT
2. **Node-to-Pod communication**: Nodes can reach all pods
3. **Pod-to-Pod (cross-node)**: Pods on different nodes can communicate
4. **Service networking**: ClusterIP, external access

### Popular CNI Plugins

| Plugin | Features | OSI Layer | Use Case |
|--------|----------|-----------|----------|
| Calico | BGP routing, network policies | L3 | Production, security |
| Flannel | Simple overlay networking | L2/L3 | Development, learning |
| Weave | Mesh networking, encryption | L2/L3 | Multi-host |
| Cilium | eBPF-based, advanced observability | L3/L4/L7 | Advanced, cloud-native |

### How CNI Relates to OSI Model

**Layer 2 (Data Link):**
- MAC addressing
- Ethernet frames
- Bridge networking
- Example: Flannel (VXLAN mode)

**Layer 3 (Network):**
- IP routing
- Subnet allocation
- Example: Calico (BGP routing)

**Layer 4 (Transport):**
- TCP/UDP ports
- Network policies (port-based)
- Example: Cilium (L4 filtering)

**Layer 7 (Application):**
- HTTP routing
- Advanced policies (path, method)
- Example: Cilium (L7 visibility)

### CNI in Action: Calico

```bash
# Install Calico
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml

# Verify installation
kubectl get pods -n kube-system | grep calico

# Check nodes
kubectl get nodes -o wide

# Test pod-to-pod communication
kubectl run test1 --image=busybox --restart=Never -- sleep 3600
kubectl run test2 --image=busybox --restart=Never -- sleep 3600

# Get pod IPs
kubectl get pods -o wide

# From test1, ping test2
kubectl exec test1 -- ping <test2-ip>
```

### Network Policies (L3/L4/L7)

**Deny all ingress traffic:**
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

**Allow specific traffic:**
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-nginx
spec:
  podSelector:
    matchLabels:
      app: nginx
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
    ports:
    - protocol: TCP
      port: 80
```

### Troubleshooting CNI

```bash
# Check CNI plugin pods
kubectl get pods -n kube-system

# Check node network status
kubectl describe node <node-name>

# Test DNS resolution
kubectl run dns-test --image=busybox:1.28 --restart=Never -- sleep 3600
kubectl exec dns-test -- nslookup kubernetes.default

# Check network policies
kubectl get networkpolicies

# View CNI logs
kubectl logs -n kube-system -l k8s-app=calico-node
```

### Why CNI and OSI Matter Together

Understanding OSI layers helps you:
1. **Choose the right CNI**: L3 (Calico) vs L2/L3 (Flannel)
2. **Debug network issues**: Identify which layer is broken
3. **Design network policies**: L3/L4/L7 filtering
4. **Optimize performance**: Reduce hops, choose routing method
5. **Implement security**: Encryption at different layers

---

## Practice Exercises

### Exercise 1: TLS Certificate Management
1. Install Certbot
2. Issue a certificate using HTTP-01 challenge
3. Issue a certificate using DNS-01 challenge
4. Document expiry dates with screenshots
5. Test auto-renewal

### Exercise 2: Git Workflow Comparison
1. Create a repository with Git Flow
2. Create a feature branch, merge it
3. Create another repository with Trunk-Based Development
4. Compare the workflows
5. Document pros and cons

### Exercise 3: Helm Chart Creation
1. Create a Helm chart for a simple web app
2. Parameterize replicas, image tag, service type
3. Deploy to your Kubernetes cluster
4. Upgrade the release with new values
5. Rollback to previous version

### Exercise 4: Kubernetes Installation
1. Install a 3-node cluster using kubeadm
2. Install a different cluster using Kubespray
3. Compare the experiences
4. Document troubleshooting steps

### Exercise 5: CNI and Network Policies
1. Deploy a cluster with Calico CNI
2. Create network policies to restrict traffic
3. Test pod-to-pod communication
4. Document which OSI layers are involved
5. Compare with a different CNI plugin

---

## Resources

### Documentation
- [Kubernetes Official Docs](https://kubernetes.io/docs/)
- [Docker Docs](https://docs.docker.com/)
- [Helm Docs](https://helm.sh/docs/)
- [Let's Encrypt](https://letsencrypt.org/)
- [Certbot](https://certbot.eff.org/)

### Practice Labs
- [Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)
- [Killercoda](https://killercoda.com/)
- [Play with Kubernetes](https://labs.play-with-k8s.com/)

### Books
- "Site Reliability Engineering" by Google
- "Kubernetes Up & Running"
- "The Phoenix Project"

---

*Happy Learning! 🚀*
