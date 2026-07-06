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

# CI/CD Pipeline Setup using GitHub Actions

## Architecture

```
Developer
     │
     │ git push
     ▼
GitHub Repository
     │
     │ GitHub Actions Trigger
     ▼
GitHub Actions Runner
     │
     │ SSH
     ▼
EC2 Instance
     │
     ├── git pull
     ├── docker compose build
     └── docker compose up -d
```

---

# Step 1: Create the GitHub Repository

* Create a new GitHub repository.
* Push your application code to the **main** branch.

---

# Step 2: Launch an EC2 Instance

* Launch an Ubuntu EC2 instance.
* Install:

  * Git
  * Docker
  * Docker Compose
* Clone your application repository.

```bash
git clone https://github.com/<username>/sample-nodejs-app.git
cd sample-nodejs-app
```

Verify the application runs manually.

```bash
docker compose up --build -d
```

---

# Step 3: Create GitHub Actions Workflow

Inside the repository create:

```
.github/
└── workflows/
    └── deploy.yml
```

Paste the following workflow:

```yaml
name: Deploy Application

on:
  push:
    branches:
      - main

jobs:

  deploy:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Deploy to EC2
        uses: appleboy/ssh-action@v1

        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USERNAME }}
          key: ${{ secrets.EC2_SSH_KEY }}

          script: |
            cd /home/ubuntu/sample-nodejs-app
            git pull origin main
            docker compose up --build -d
```

---

# Step 4: Configure GitHub Secrets

Open your GitHub repository.

**Settings → Secrets and Variables → Actions**

Create the following repository secrets.

| Secret Name  | Value                              |
| ------------ | ---------------------------------- |
| EC2_HOST     | Public IP of the EC2 instance      |
| EC2_USERNAME | ubuntu                             |
| EC2_SSH_KEY  | Contents of the `.pem` private key |

---

# Step 5: Copy the Required Information

You will need the following:

### EC2 Public IP

Example:

```
54.xx.xx.xx
```

### Username

```
ubuntu
```

### Private Key (.pem)

Open the `.pem` file using a text editor.

Copy the complete content:

```
-----BEGIN OPENSSH PRIVATE KEY-----
...
-----END OPENSSH PRIVATE KEY-----
```

Paste this entire content into the `EC2_SSH_KEY` GitHub secret.

---

# Step 6: Commit and Push the Workflow

```bash
git add .
git commit -m "Added GitHub Actions deployment pipeline"
git push origin main
```

---

# Step 7: Pipeline Trigger

Whenever code is pushed to the **main** branch:

1. GitHub detects the push.
2. GitHub Actions starts automatically.
3. The runner connects to the EC2 instance using SSH.
4. It navigates to the project directory.
5. Pulls the latest code.
6. Rebuilds Docker images.
7. Restarts the containers.

Deployment is now complete.

---

# Deployment Flow

```
Developer
    │
    │ git push
    ▼
GitHub Repository
    │
    ▼
GitHub Actions
    │
    │ SSH
    ▼
EC2 Server
    │
    ├── git pull
    ├── docker compose build
    └── docker compose up -d
    │
    ▼
Updated Application
```

---

# Advantages

* Fully automated deployment
* No manual SSH login required
* Consistent deployment process
* Faster delivery of application updates
* Reduced deployment errors
* Easy rollback using Git history

