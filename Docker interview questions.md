
🐳 Top 30 Docker Interview Questions & Answers

## DevOps / SRE Interview Preparation

This guide covers the **top 30 Docker interview questions**, from basic concepts to advanced production troubleshooting scenarios.

---

# 🟢 Basic Docker Questions

## 1. What is Docker?

Docker is a containerization platform that packages an application along with its dependencies, libraries, configuration, and runtime into a portable unit called a **container**.

It helps ensure the application behaves consistently across development, testing, and production environments.

### Example

```text
Application + Dependencies
          ↓
       Docker Image
          ↓
       Container
```

---

## 2. What is a container?

A container is a lightweight, isolated environment used to run an application and its dependencies.

Unlike a VM, containers share the host operating system kernel.

```text
Host OS
  |
Docker Engine
  |
+-----------+-----------+
| Container | Container |
|    App    |    App    |
+-----------+-----------+
```

---

## 3. What is the difference between Docker and a Virtual Machine?

| Docker Container | Virtual Machine |
|---|---|
| Shares host kernel | Has its own guest OS |
| Lightweight | Heavy |
| Starts quickly | Takes longer |
| Uses fewer resources | Uses more resources |
| Container-level isolation | VM-level isolation |

### Interview Answer

> Containers share the host OS kernel, while VMs contain a complete guest operating system.

---

## 4. What is a Docker image?

A Docker image is a **read-only template** used to create containers.

It contains:

- Application
- Dependencies
- Libraries
- Configuration
- Runtime

### Example

```bash
docker pull nginx
docker run nginx
```

`nginx` is the image, and the running instance is the container.

---

## 5. What is the difference between an image and a container?

### Image

- Read-only
- Template
- Used to create containers

### Container

- Running instance of an image
- Has a writable container layer
- Can be started, stopped, and deleted

```text
Docker Image
     ↓
Container 1
Container 2
Container 3
```

---

## 6. What is Docker Hub?

Docker Hub is a public container image registry.

You can:

- Pull images
- Push images
- Store repositories
- Share images

### Example

```bash
docker pull nginx
docker push username/myapp:1.0
```

---

## 7. What is Docker Engine?

Docker Engine is the core technology that allows you to build and run containers.

It includes components such as:

```text
Docker CLI
    ↓
Docker API
    ↓
Docker Daemon
    ↓
Containers / Images / Networks / Volumes
```

---

## 8. What is Docker Daemon?

The Docker daemon (`dockerd`) is the background service responsible for managing Docker objects such as:

- Containers
- Images
- Networks
- Volumes

### Example

```bash
systemctl status docker
```

---

## 9. What is the Docker container lifecycle?

A typical container lifecycle is:

```text
Create
  ↓
Start
  ↓
Running
  ↓
Stop
  ↓
Restart
  ↓
Remove
```

### Commands

```bash
docker create
docker start
docker stop
docker restart
docker rm
```

---

## 10. What are the most commonly used Docker commands?

Important commands:

```bash
docker ps
docker ps -a
docker images
docker pull
docker run
docker exec
docker logs
docker inspect
docker stop
docker start
docker restart
docker rm
docker rmi
docker build
docker tag
docker push
```

---

# 🟡 Intermediate Docker Questions

## 11. What is a Dockerfile?

A Dockerfile is a text file containing instructions used to build a Docker image.

### Example

```dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### Build

```bash
docker build -t my-nginx:1.0 .
```

---

## 12. What is the difference between CMD and ENTRYPOINT?

Both define what runs when a container starts.

### CMD

Provides a default command that can easily be overridden.

```dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

### ENTRYPOINT

Defines the main executable.

```dockerfile
ENTRYPOINT ["python"]
```

For example:

```bash
docker run myimage app.py
```

With ENTRYPOINT:

```text
python app.py
```

### Interview Tip

A common pattern is to use `ENTRYPOINT` for the main executable and `CMD` for default arguments.

---

## 13. What is the difference between COPY and ADD?

Both can copy files into an image.

### COPY

Simpler and preferred for normal file copying.

```dockerfile
COPY app.jar /app/
```

### ADD

Has additional functionality, including handling local tar archives.

### Interview Answer

> I generally prefer `COPY` because it is simpler and more predictable. I use `ADD` only when its additional functionality is specifically required.

---

## 14. What is a Docker image layer?

Docker images are built in layers.

### Example

```dockerfile
FROM ubuntu
RUN apt update
RUN apt install nginx
COPY index.html /var/www/html/
```

Each instruction can create an image layer.

### Benefits

- Layer caching
- Faster builds
- Reusability
- Reduced storage

---

## 15. What is `.dockerignore`?

`.dockerignore` specifies files and directories that should not be sent as part of the Docker build context.

### Example

```text
.git
node_modules
*.log
.env
```

### Benefits

- Smaller build context
- Faster builds
- Avoids unnecessary files
- Helps prevent accidental inclusion of sensitive files

---

## 16. How do you expose a Docker container to the host?

Use port mapping.

```bash
docker run -d -p 8080:80 nginx
```

This means:

```text
Host Port 8080
      ↓
Container Port 80
```

The application can then be accessed through port `8080` on the host.

---

## 17. What is Docker networking?

Docker networking allows containers to communicate with:

- Other containers
- The host
- External systems

### Common network types

```text
bridge
host
none
overlay
```

### Create a network

```bash
docker network create mynetwork
```

### Run a container

```bash
docker run -d \
  --network mynetwork \
  nginx
```

---

## 18. What is a Docker volume?

A Docker volume provides persistent storage for containers.

### Example

```bash
docker volume create mydata

docker run -d \
  -v mydata:/data \
  nginx
```

### Important Concept

Without persistent storage:

```text
Container deleted
       ↓
Container data lost
```

With a volume:

```text
Container
    ↓
Docker Volume
    ↓
Persistent Data
```

---

## 19. What is the difference between a volume and a bind mount?

### Volume

```bash
-v myvolume:/data
```

Docker manages the storage.

### Bind Mount

```bash
-v /host/path:/container/path
```

You specify the host filesystem location.

| Volume | Bind Mount |
|---|---|
| Docker-managed | User-managed |
| Easier portability | Direct host path |
| Common for persistent data | Useful for development/configuration |

---

## 20. What is Docker Compose?

Docker Compose is used to define and run multi-container applications using a YAML configuration file.

### Example

```yaml
services:

  web:
    image: nginx

  redis:
    image: redis

  db:
    image: mysql
```

### Start

```bash
docker compose up -d
```

### Stop

```bash
docker compose down
```

---

# 🔴 Advanced & Scenario-Based Questions

## 21. A container is running but you cannot access the application. How do you troubleshoot it?

I would troubleshoot in this order.

### Step 1 — Check container status

```bash
docker ps
```

### Step 2 — Check logs

```bash
docker logs <container>
```

### Step 3 — Check port mapping

```bash
docker port <container>
```

or:

```bash
docker ps
```

### Step 4 — Enter the container

```bash
docker exec -it <container> /bin/sh
```

### Step 5 — Check networking

```bash
docker network inspect <network>
```

### Verify

- Application process
- Listening port
- Docker port mapping
- Firewall/security group
- Network connectivity
- Application configuration
- Application logs

---

## 22. A container exits immediately after starting. What will you check?

First:

```bash
docker ps -a
```

Then:

```bash
docker logs <container>
```

Also check:

```bash
docker inspect <container>
```

### Possible causes

- Application startup error
- Incorrect CMD
- Incorrect ENTRYPOINT
- Missing configuration
- Missing environment variables
- Incorrect command
- Dependency failure

---

## 23. A container is continuously restarting. How do you troubleshoot?

Check:

```bash
docker ps
docker logs <container>
docker inspect <container>
```

Possible causes:

```text
Application crash
      ↓
Bad configuration
      ↓
Missing environment variable
      ↓
Health-check failure
      ↓
Dependency failure
```

Also check the configured restart policy.

---

## 24. How do you check container resource utilization?

Use:

```bash
docker stats
```

It displays:

- CPU usage
- Memory usage
- Network I/O
- Block I/O
- Process information

### Example

```bash
docker stats myapp
```

---

## 25. How do you limit CPU and memory for a container?

### Example

```bash
docker run -d \
  --name myapp \
  --memory=512m \
  --cpus=1 \
  nginx
```

This helps prevent a container from consuming unlimited host resources.

---

## 26. How do you reduce Docker image size?

Use the following techniques:

1. Use an appropriate minimal base image.
2. Use multi-stage builds.
3. Remove unnecessary packages.
4. Use `.dockerignore`.
5. Avoid unnecessary files in the image.
6. Install only production dependencies.
7. Keep the final runtime image focused on what the application needs.

### Example

```dockerfile
FROM node:alpine
```

For production, choose the base image based on compatibility, support, and security — not size alone.

---

## 27. What is a multi-stage Docker build?

Multi-stage builds allow you to use multiple stages in a Dockerfile and copy only the required artifacts into the final image.

### Example

```dockerfile
FROM maven:3.9 AS build

WORKDIR /app

COPY pom.xml .
COPY src ./src

RUN mvn clean package

FROM eclipse-temurin:21-jre

COPY --from=build /app/target/app.jar /app/app.jar

CMD ["java", "-jar", "/app/app.jar"]
```

### Benefits

- Smaller production image
- Build tools excluded
- Better security
- Cleaner deployment image

---

## 28. How would you integrate Docker with Jenkins?

A typical pipeline is:

```text
GitHub
   ↓
Jenkins
   ↓
Checkout
   ↓
Build
   ↓
Test
   ↓
SonarQube
   ↓
Docker Build
   ↓
Docker Image
   ↓
Docker Registry
   ↓
Deploy
```

### Example

```bash
docker build -t myapp:$BUILD_NUMBER .

docker tag myapp:$BUILD_NUMBER \
  myrepo/myapp:$BUILD_NUMBER

docker push myrepo/myapp:$BUILD_NUMBER
```

Then the deployment server can pull the required version.

---

## 29. How do you secure Docker containers?

Important practices:

- Run containers as a non-root user.
- Use trusted and appropriate base images.
- Scan images for vulnerabilities.
- Keep images and runtime components updated.
- Never hard-code secrets into Dockerfiles.
- Use appropriate secrets management.
- Avoid unnecessary `--privileged`.
- Apply CPU and memory limits.
- Use read-only filesystems where appropriate.
- Restrict network access.
- Protect access to the Docker daemon/socket.

### Interview Answer

> I follow least privilege, use non-root containers, scan images, avoid secrets in images, minimize the runtime image, restrict resources and network access, and keep base images and Docker components patched.

---

## 30. Explain a real-time Docker production troubleshooting scenario.

### ⭐ Sample Interview Answer

> Suppose an application container suddenly becomes unavailable. First, I check `docker ps` to verify whether the container is running. If it is stopped or restarting, I check `docker ps -a` and `docker logs` for the failure reason.
>
> If the container is running, I verify the port mapping and application listening port. Then I use `docker exec` to check the application process and connectivity from inside the container.
>
> I check `docker inspect` and `docker network inspect` for configuration and networking issues. I also check `docker stats` for CPU and memory problems.
>
> Finally, I verify host-level issues such as disk space, firewall/security rules, dependencies, and recent deployments.
>
> After identifying the root cause, I restore the service and document the RCA.

---

# ⭐ Top 10 Questions to Prepare Extremely Well

If you have limited preparation time, focus heavily on these:

1. Docker architecture
2. Image vs Container
3. Dockerfile
4. CMD vs ENTRYPOINT
5. COPY vs ADD
6. Docker networking
7. Docker volumes
8. Docker Compose
9. Docker troubleshooting
10. Docker + Jenkins CI/CD

---

# 🎯 DevOps/SRE Interview Preparation Strategy

Don't just memorize these 30 answers.

For every question:

### 1. Understand

Learn what the feature does.

### 2. Practice

Run the commands yourself.

### 3. Break It

Intentionally create problems:

- Wrong port
- Wrong image
- Wrong Dockerfile command
- Missing environment variable
- Stop a dependency
- Remove a volume
- Use an incorrect network

### 4. Troubleshoot

Use:

```bash
docker ps
docker ps -a
docker logs
docker inspect
docker exec
docker stats
docker network inspect
```

### 5. Explain

Practice explaining the issue as if you are answering an interviewer.

---

# 🏆 Final Goal

For a DevOps/SRE role, your Docker knowledge should go beyond commands.

You should be able to explain and demonstrate:

```text
Build
  ↓
Package
  ↓
Test
  ↓
Scan
  ↓
Push
  ↓
Deploy
  ↓
Monitor
  ↓
Troubleshoot
  ↓
RCA
```
