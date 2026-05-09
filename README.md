# Java Load Balancer Practice using NGINX + Spring Boot + Docker

This project demonstrates a simple **Load Balancer Architecture** using:

- Java Spring Boot Applications
- NGINX Load Balancer
- Docker & Docker Compose

The setup distributes incoming requests across multiple backend servers using **Round Robin Load Balancing**.

---

# Architecture Diagram

<img width="860" height="778" alt="image" src="https://github.com/user-attachments/assets/c787acdc-6ba2-43e0-a778-3235bd20c296" />


# Features

- Reverse Proxy using NGINX
- Round Robin Load Balancing
- Multiple Spring Boot backend servers
- Dockerized applications
- Docker Compose orchestration
- Internal Docker networking

---

# Tech Stack

- Java 17
- Spring Boot
- NGINX
- Docker
- Docker Compose

---

# Project Structure

```text
load-balancer-practice/
│
├── docker-compose.yml
├── README.md
├── .gitignore
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
   - app1:8081
   - app2:8082

4. Requests are distributed using:
   - Round Robin Algorithm

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

### App1

```bash
cd server1
mvn clean package
```

### App2

```bash
cd ../server2
mvn clean package
```

---

## Run Docker Compose

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

- spring-app1
- spring-app2
- nginx-lb

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

# Future Improvements

- Health Checks
- SSL/HTTPS
- Sticky Sessions
- Kubernetes Deployment
- HAProxy
- Monitoring with Prometheus & Grafana
- Auto Scaling
- API Gateway

---

# Learning Outcomes

This project helps understand:

- Load Balancing
- Reverse Proxy
- Docker Networking
- Containerization
- Microservices Basics
- NGINX Upstreams
- Spring Boot Deployment
- Docker Compose Orchestration

---

# Author

Akshat Trivedi
