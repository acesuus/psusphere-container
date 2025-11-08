# PSUSphere

## Initial Setup

### Prerequisites
- **Docker**: [Download Docker](https://www.docker.com/get-started)
- **Docker Compose**: Included with Docker Desktop

Verify installation:
```bash
docker --version
docker-compose --version
```

---

## Option 1: Pull and Run Existing Image

### Pull the Image from Docker Hub
```bash
docker pull acesus/psusphere:latest
```

### Run the Container
```bash
docker run -d \
  -p 8000:8000 \
  -e DEBUG=1 \
  -e USE_SQLITE=true \
  -e DJANGO_ALLOWED_HOSTS="localhost 127.0.0.1 [::1]" \
  --name psusphere \
  acesus/psusphere:latest
```

### Access the Application
Open your browser and go to:
```
http://localhost:8000
```

### Stop and Remove Container
```bash
docker stop psusphere
docker rm psusphere
```

---

## Option 2: Build and Run with Docker Compose

### Update docker-compose.yml
Modify the compose file to use the Docker Hub image:
```yaml
version: "3.8"
services:
  web:
    image: acesus/psusphere:latest  # Pull from Docker Hub
    ports:
      - "8000:8000"
    volumes:
      - .:/app
    environment:
      - DEBUG=1
      - USE_SQLITE=true
      - DJANGO_ALLOWED_HOSTS=localhost 127.0.0.1 [::1]
```

### Run with Docker Compose
```bash
docker-compose up
```

---

## Development Workflow

### 1. Make Code Changes
Edit your project files locally as needed.

### 2. Build New Image Locally
```bash
docker build -t acesus/psusphere:latest .
```

### 3. Test Locally
```bash
docker-compose up
```
or
```bash
docker run -p 8000:8000 acesus/psusphere:latest
```

### 4. Login to Docker Hub
```bash
docker login
```
Enter your Docker Hub username and password.

### 5. Push New Image to Docker Hub
```bash
docker push acesus/psusphere:latest
```

### 6. Tag Versions (Optional but Recommended)
```bash
# Build with version tag
docker build -t acesus/psusphere:v1.0.0 .
docker tag acesus/psusphere:v1.0.0 acesus/psusphere:latest

# Push both tags
docker push acesus/psusphere:v1.0.0
docker push acesus/psusphere:latest
```

---

## Complete Development Workflow Example

```bash
# 1. Make your code changes
# ... edit files ...

# 2. Build the new image
docker build -t acesus/psusphere:latest .

# 3. Test locally
docker-compose up

# 4. If everything works, login to Docker Hub
docker login

# 5. Push to Docker Hub
docker push acesus/psusphere:latest

# 6. Others can now pull your updated image
docker pull acesus/psusphere:latest
```

---

## Useful Commands

### View Running Containers
```bash
docker ps
```

### View All Containers (including stopped)
```bash
docker ps -a
```

### View Images
```bash
docker images
```

### Remove Old Images
```bash
docker image prune
```

### View Container Logs
```bash
docker logs psusphere
```

### Execute Commands in Running Container
```bash
docker exec -it psusphere python manage.py createsuperuser
```

### Stop All Containers
```bash
docker stop $(docker ps -q)
```

### Remove All Stopped Containers
```bash
docker container prune
```

---

## Quick Reference

| Task | Command |
|------|---------|
| Pull image | `docker pull acesus/psusphere:latest` |
| Build image | `docker build -t acesus/psusphere:latest .` |
| Run container | `docker run -p 8000:8000 acesus/psusphere:latest` |
| Push image | `docker push acesus/psusphere:latest` |
| Start with compose | `docker-compose up` |
| Stop with compose | `docker-compose down` |
| View logs | `docker logs psusphere` |

---
