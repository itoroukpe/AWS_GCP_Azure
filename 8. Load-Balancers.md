Here's a comprehensive, hands-on practical exercise with step-by-step instructions on implementing load balancers, designed specifically for beginner-level DevOps students. This practical is divided into **three scenarios**, each using different load balancer technologies and situations:

---

## **Scenario 1: NGINX Load Balancer – Basic Web Load Balancing**

### **Objective:**
Deploy and configure a simple HTTP load balancer using NGINX.

### **Prerequisites:**
- Ubuntu/Debian VM or instance
- Docker installed

### **Step-by-step Guide:**

### **Step 1: Setup backend web servers**
1. Launch two containers serving a simple web page.

```bash
docker run -d --name webserver1 -p 8081:80 nginx
docker run -d --name webserver2 -p 8082:80 nginx
```

2. Test containers directly:
```bash
curl http://localhost:8081
curl http://localhost:8082
```

### **Step 2: Install and configure NGINX as Load Balancer**
```bash
sudo apt update
sudo apt install nginx -y
```

Edit NGINX configuration:
```bash
sudo nano /etc/nginx/sites-available/loadbalancer
```

Paste the following configuration:

```nginx
upstream backend {
    server localhost:8081;
    server localhost:8082;
}

server {
    listen 80;

    location / {
        proxy_pass http://backend;
    }
}
```

Activate this configuration:
```bash
sudo ln -s /etc/nginx/sites-available/loadbalancer /etc/nginx/sites-enabled/
sudo unlink /etc/nginx/sites-enabled/default
```

Restart NGINX:
```bash
sudo systemctl restart nginx
```

### **Step 3: Testing Load Balancing**
Open browser or use curl:
```bash
curl http://localhost
```

Repeat multiple times and observe the round-robin balancing between servers.

---

## **Scenario 2: HAProxy Load Balancer – Health Checks and Failover**

### **Objective:**
Implement HAProxy for load balancing with health-checks to handle backend failure.

### **Prerequisites:**
- Ubuntu VM or instance
- Docker installed

### **Step-by-step Guide:**

### **Step 1: Set up backend web servers**
Launch two backend servers:
```bash
docker run -d --name app1 -p 9091:80 nginx
docker run -d --name app2 -p 9092:80 nginx
```

### **Step 2: Install and configure HAProxy**
Install HAProxy:
```bash
sudo apt update
sudo apt install haproxy -y
```

Configure HAProxy:
```bash
sudo nano /etc/haproxy/haproxy.cfg
```

Replace with the following configuration:

```haproxy
global
    log /dev/log    local0
    log /dev/log    local1 notice
    daemon

defaults
    log     global
    mode    http
    option  httplog
    retries 3
    timeout connect 5000
    timeout client  50000
    timeout server  50000

frontend http_front
    bind *:80
    default_backend web_servers

backend backend_servers
    balance roundrobin
    server app1 127.0.0.1:9091 check
    server app2 127.0.0.1:9092 check
```

### **Step 2: Install HAProxy**
```bash
sudo systemctl restart haproxy
```

### **Step 3: Test Load Balancing and Failover**
Test by accessing:
```bash
curl http://localhost
```

Stop one backend to test failover:
```bash
docker stop app1
curl http://localhost
```

Observe that HAProxy handles requests gracefully by sending them to the available backend only.

---

---

## Scenario 3: Cloud-based Load Balancer (AWS Elastic Load Balancer)

### **Objective:**
Use AWS Application Load Balancer (ALB) to balance HTTP traffic.

### **Prerequisites:**
- AWS account
- Basic understanding of AWS EC2

### **Step-by-step Guide:**

### **Step 1: Launch EC2 Instances**
- Launch two Amazon Linux 2 EC2 instances in the same region.
- Install web server on each:

```bash
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
echo "Hello from Server 1" | sudo tee /var/www/html/index.html
```
*(Repeat on the second server with a slightly different message.)*

### **Step 2: Create Load Balancer**
- Navigate to EC2 console → Load Balancing → Load Balancers.
- Click **"Create Load Balancer"** → **Application Load Balancer**.
- Name: **MyALB**
- Scheme: Internet-facing.
- Select appropriate VPC, Subnets.
- Security group: Allow HTTP (port 80).

### **Step 3: Configure Target Groups**
- Name: **MyALBTargetGroup**
- Protocol: HTTP, Port 80.
- Health check path: `/`

### **Step 3: Register EC2 Instances**
- Add the two EC2 instances you created earlier.

### **Step 4: Complete Setup**
- Review and click **Create Load Balancer**.

### **Step 5: Test ALB**
- Obtain DNS name of ALB from AWS console.
- Open browser and enter ALB DNS name.
- Refresh to see traffic distributed to both instances.

---

## Bonus Scenario: Kubernetes Load Balancing (Optional Advanced Task)
Use Kubernetes Service of type LoadBalancer for distributing traffic.

### **Step-by-step:**
1. Create Deployment:
```bash
kubectl create deployment webapp --image=nginx --replicas=2
```

### **Step 2: Expose Deployment**
```bash
kubectl expose deployment web --type=LoadBalancer --port=80
```

### **Step 3: Verify Load Balancer**
- Use `kubectl get svc` to view External-IP.
- Test via browser.

---

## Scenarios Summary for Students:

| Load Balancer | Scenario             | Typical Use-case                  | Key Learning Outcomes        |
|---------------|----------------------|-----------------------------------|
| **NGINX** | Lightweight, simple load balancing | Use for simple HTTP applications, lightweight traffic, quick setups |
| **HAProxy** | Advanced HTTP load balancing with health checks and failover | Used in environments that require high availability and reliability|
| **AWS ALB** | Cloud-based, scalable, HTTP(S) balancing with managed service capabilities | Suitable for cloud-native applications with automatic scaling|
| **Kubernetes** | Container-based, automatically provisioned load balancers via cloud providers | Container-orchestrated environments, microservices |

---

