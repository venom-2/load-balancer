# Java Load Balancer Practice using NGINX + Spring Boot + Docker

This project demonstrates a simple **Load Balancer Architecture** using:

* Java Spring Boot Applications
* NGINX Load Balancer
* Docker & Docker Compose
* GitHub Actions CI/CD
* Oracle Cloud Infrastructure (OCI)

The setup distributes incoming requests across multiple backend servers using **Round Robin Load Balancing**.

---

# Architecture Diagram

<img width="860" height="778" alt="image" src="https://github.com/user-attachments/assets/c787acdc-6ba2-43e0-a778-3235bd20c296" />

---

# Features

* Reverse Proxy using NGINX
* Round Robin Load Balancing
* Multiple Spring Boot backend servers
* Dockerized applications
* Docker Compose orchestration
* Internal Docker networking
* CI/CD using GitHub Actions
* Automated deployment to Oracle Cloud Infrastructure (OCI)

---

# Tech Stack

* Java 17
* Spring Boot
* NGINX
* Docker
* Docker Compose
* GitHub Actions
* Oracle Cloud Infrastructure (OCI)

---

# Project Structure

```text
load-balancer/
│
├── docker-compose.yml
├── README.md
├── .gitignore
│
├── .github/
│   └── workflows/
│       └── deploy.yml
│
├── nginx/
│   └── nginx.conf
│
├── server1/
│   ├── Dockerfile
│   ├── pom.xml
│   └── src/
│
└── server2/
    ├── Dockerfile
    ├── pom.xml
    └── src/
```

---

# How It Works

1. User sends request to:

```text
http://localhost:8080
```

2. NGINX receives the request

3. NGINX forwards request to backend servers:

   * app1:8081
   * app2:8082

4. Requests are distributed using:

   * Round Robin Algorithm

5. Spring Boot applications process requests and return responses

---

# Round Robin Load Balancing

Example request distribution:

```text
Request 1 → App1
Request 2 → App2
Request 3 → App1
Request 4 → App2
```

NGINX distributes requests sequentially across backend servers.

---

# Docker Networking

Docker Compose automatically creates an internal network.

Containers communicate using service names:

```nginx
server app1:8081;
server app2:8082;
```

---

# Setup Instructions

## Clone Repository

```bash
git clone https://github.com/venom-2/load-balancer.git
```

---

## Move Into Project

```bash
cd load-balancer
```

---

## Build Spring Boot Applications

### Server 1

```bash
cd server1
mvn clean package
```

### Server 2

```bash
cd ../server2
mvn clean package
```

---

## Run Docker Compose

### Docker Compose V1

```bash
docker-compose up --build
```

### Docker Compose V2

```bash
docker compose up --build
```

---

# Access Application

Open browser:

```text
http://localhost:8080
```

Refresh multiple times to observe load balancing.

---

# Verify Containers

```bash
docker ps
```

Expected containers:

* spring-app1
* spring-app2
* nginx-lb

---

# NGINX Configuration

```nginx
events {}

http {

    upstream backend_servers {

        server app1:8081;
        server app2:8082;
    }

    server {

        listen 80;

        location / {

            proxy_pass http://backend_servers;

            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}
```

---

# CI/CD with GitHub Actions + Oracle Cloud

This project includes a simple **CI/CD Pipeline** using:

* GitHub Actions
* Oracle Cloud Infrastructure (OCI)
* SSH Deployment
* Docker Compose

Every push to the `main` branch automatically deploys the latest application to the OCI VM.

---

# CI/CD Architecture

```text
Developer
    |
git push
    |
GitHub Repository
    |
GitHub Actions Workflow
    |
SSH into OCI VM
    |
Pull Latest Code
    |
Build Spring Boot JARs
    |
Rebuild Docker Containers
    |
Deploy Updated Application
```

---

# GitHub Actions Workflow

Workflow file:

```text
.github/workflows/deploy.yml
```

---

# Workflow Configuration

```yaml
name: Deploy to OCI Instance

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:

      - name: Deploy to OCI Server
        uses: appleboy/ssh-action@master

        with:
          host: ${{ secrets.OCI_HOST }}
          username: ${{ secrets.OCI_USERNAME }}
          key: ${{ secrets.OCI_SSH_KEY }}

          script: |
            cd ~/load-balancer/load-balancer

            git pull origin main

            cd server1
            mvn clean package -DskipTests

            cd ../server2
            mvn clean package -DskipTests

            cd ..

            docker-compose down
            docker-compose up --build -d
```

---

# GitHub Secrets

The following GitHub repository secrets are configured:

| Secret Name  | Purpose                        |
| ------------ | ------------------------------ |
| OCI_HOST     | Oracle Cloud VM Public IP      |
| OCI_USERNAME | OCI VM Username                |
| OCI_SSH_KEY  | Private SSH Key for deployment |

---

# Automated Deployment Flow

1. Developer pushes code to GitHub

```bash
git push origin main
```

2. GitHub Actions workflow triggers automatically

3. Workflow connects to OCI VM using SSH

4. Latest code is pulled from GitHub

5. Spring Boot applications are rebuilt

6. Docker containers are rebuilt and restarted

7. Updated application becomes live automatically

---

# Oracle Cloud Deployment

The application is deployed on:

* Oracle Cloud Infrastructure (OCI)
* Ubuntu VM
* Dockerized Environment

---

# OCI Deployment Architecture

```text
                 INTERNET
                      |
                Public IP Address
                      |
               Oracle Cloud VM
                      |
        --------------------------------
        |              |              |
      NGINX         Spring App1    Spring App2
   Load Balancer
```

---

# Verify Deployment on OCI

Open browser:

```text
http://YOUR_PUBLIC_IP
```

Refresh multiple times to observe:

* responses from App1
* responses from App2

through NGINX round robin load balancing.

---

# Useful Docker Commands

## View Running Containers

```bash
docker ps
```

---

## View Logs

```bash
docker-compose logs -f
```

---

## Stop Containers

```bash
docker-compose down
```

---

## Restart Containers

```bash
docker-compose restart
```

---

## Rebuild Containers

```bash
docker-compose up --build -d
```

---

# Future Improvements

* Health Checks
* SSL/HTTPS
* Sticky Sessions
* Kubernetes Deployment
* HAProxy
* Monitoring with Prometheus & Grafana
* Auto Scaling
* API Gateway
* VPN Access
* Blue-Green Deployment
* Canary Releases

---

# Learning Outcomes

This project helps understand:

* Load Balancing
* Reverse Proxy
* Docker Networking
* Containerization
* Microservices Basics
* NGINX Upstreams
* Spring Boot Deployment
* Docker Compose Orchestration
* CI/CD Pipelines
* GitHub Actions
* Oracle Cloud Deployment
* Infrastructure Automation
* DevOps Fundamentals

---

# Author

Akshat Trivedi
