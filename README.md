# Docker Practice - Three-Tier Node.js Application

## Overview

This project is a simple **three-tier application** designed for learning Docker.

The application consists of:

* **Frontend** - Node.js web application
* **Backend** - Node.js REST API
* **Docker Compose** - Manages and runs multiple containers together

This repository is intended for practicing:

* Docker Images
* Docker Containers
* Dockerfile
* Docker Networking
* Docker Compose
* Multi-container Applications

---

# Project Structure

```text
.
├── backend
│   ├── server.js
│   ├── package.json
│   ├── Dockerfile
│   └── .dockerignore
│
├── frontend
│   ├── index.js
│   ├── Dockerfile
│   └── .dockerignore
│
└── docker-compose.yml
```

---

# Prerequisites

Before starting, ensure you have the following installed:

* Docker
* Docker Compose (included with Docker Desktop)

Verify installation:

```bash
docker --version
```

```bash
docker compose version
```

---

# Learning Objectives

By completing this lab, you will learn how to:

* Create Docker images
* Build images using Dockerfile
* Run containers
* Expose container ports
* Connect multiple containers
* Use Docker Compose
* View logs
* Stop and remove containers
* Verify

---

# Step 1 - Clone the Repository

```bash
git clone <repository-url>
```

```bash
cd <repository-folder>
```

---

# Step 2 - Build the Backend Image

Navigate to the backend folder.

```bash
cd backend
```

Build the Docker image.

```bash
docker build -t node-backend .
```

Verify the image.

```bash
docker images
```

---

# Step 3 - Run the Backend Container

```bash
docker run -d \
--name backend \
-p 5000:5000 \
node-backend
```

Check running containers.

```bash
docker ps
```

View logs.

```bash
docker logs backend
```

---

# Step 4 - Build the Frontend Image

Navigate to the frontend folder.

```bash
cd ../frontend
```

Build the image.

```bash
docker build -t node-frontend .
```

---

# Step 5 - Run the Frontend Container

```bash
docker run -d \
--name frontend \
-p 3000:3000 \
node-frontend
```

---

# Step 6 - Stop and Remove Containers

Stop containers.

```bash
docker stop frontend backend
```

Remove containers.

```bash
docker rm frontend backend
```

---

# Step 7 - Run Using Docker Compose

Return to the project root.

```bash
cd ..
```

Start all services.

```bash
docker compose up
```

Run in detached mode.

```bash
docker compose up -d
```

---

# Step 8 - Verify Running Containers

```bash
docker compose ps
```

or

```bash
docker ps
```

---

# Step 9 - View Logs

View logs for all services.

```bash
docker compose logs
```

Follow logs in real time.

```bash
docker compose logs -f
```

View logs for a specific service.

```bash
docker compose logs frontend
```

```bash
docker compose logs backend
```

---

# Step 10 - Stop the Application

Stop all running services.

```bash
docker compose down
```

To also remove volumes (if any):

```bash
docker compose down -v
```

---

# Useful Docker Commands

## List Images

```bash
docker images
```

## List Containers

```bash
docker ps
```

## List All Containers

```bash
docker ps -a
```

## Remove an Image

```bash
docker rmi <image-name>
```

## Remove a Container

```bash
docker rm <container-name>
```

## Execute a Command Inside a Container

```bash
docker exec -it <container-name> sh
```

---

# Expected Output

Once the application is running:

* Frontend: http://localhost:3000
* Backend API: http://localhost:5000

---

# Files Included

| File                  | Purpose                                           |
| --------------------- | ------------------------------------------------- |
| `backend/Dockerfile`  | Builds the backend image                          |
| `frontend/Dockerfile` | Builds the frontend image                         |
| `.dockerignore`       | Excludes unnecessary files from the build context |
| `docker-compose.yml`  | Starts and manages multiple containers            |

---

# Docker Concepts Covered

* Docker Images
* Docker Containers
* Dockerfile
* Docker Build
* Docker Run
* Port Mapping
* Docker Networking
* Docker Compose
* Container Logs
* Multi-Container Applications
* Docker Cleanup

---

## Happy Learning!

This project is intended for beginners who are getting started with Docker and Docker Compose. Experiment with the Dockerfiles, rebuild images after making changes, and explore Docker commands to strengthen your understanding of containerized applications.

## CI/CD Pipeline setup also Included
