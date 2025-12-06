---
title: Docker Quick Reference
layout: default
parent: Reference
nav_order: 4
---

# Docker Quick Reference

Essential Docker commands and examples for containers and images.

{: .fs-6 .fw-300 }

---

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Container Management

### Running Containers

```bash
# Run container from image
docker run nginx

# Run in detached mode
docker run -d nginx

# Run with custom name
docker run --name my-nginx nginx

# Run with port mapping
docker run -p 8080:80 nginx

# Run with environment variables
docker run -e "ENV=production" nginx

# Run with volume mount
docker run -v $(pwd):/app nginx

# Run interactively
docker run -it ubuntu bash

# Run and remove on exit
docker run --rm nginx
```

### Container Commands

```bash
# List running containers
docker ps

# List all containers
docker ps -a

# Start container
docker start container-name

# Stop container
docker stop container-name

# Restart container
docker restart container-name

# Pause container
docker pause container-name

# Unpause container
docker unpause container-name

# Remove container
docker rm container-name

# Force remove running container
docker rm -f container-name
```

### Container Inspection

```bash
# View container logs
docker logs container-name

# Follow logs in real-time
docker logs -f container-name

# Show last N lines
docker logs --tail 50 container-name

# Inspect container details
docker inspect container-name

# Show resource usage statistics
docker stats

# Show running processes
docker top container-name

# Show container port mappings
docker port container-name
```

### Execute Commands in Container

```bash
# Execute command
docker exec container-name ls -la

# Interactive shell
docker exec -it container-name bash

# Execute as different user
docker exec -u root container-name whoami

# Set working directory
docker exec -w /app container-name pwd
```

---

## Image Management

### Image Commands

```bash
# List images
docker images

# Pull image from registry
docker pull nginx:latest

# Pull specific version
docker pull nginx:1.21

# Build image from Dockerfile
docker build -t myapp:1.0 .

# Build with build arguments
docker build --build-arg VERSION=1.0 -t myapp .

# Tag image
docker tag myapp:1.0 myapp:latest

# Push image to registry
docker push myapp:1.0

# Remove image
docker rmi image-name

# Force remove image
docker rmi -f image-name

# Remove unused images
docker image prune
```

### Image Inspection

```bash
# Show image history
docker history image-name

# Inspect image details
docker inspect image-name

# Show image layers
docker image inspect --format='{{.RootFS.Layers}}' image-name
```

---

## Dockerfile Examples

### Basic Dockerfile (Python)

```dockerfile
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Copy requirements
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application
COPY . .

# Expose port
EXPOSE 8000

# Run application
CMD ["python", "app.py"]
```

### Multi-Stage Build (Go)

```dockerfile
# Build stage
FROM golang:1.21 AS builder

WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 go build -o main .

# Run stage
FROM alpine:latest

RUN apk --no-cache add ca-certificates
WORKDIR /root/

COPY --from=builder /app/main .

EXPOSE 8080
CMD ["./main"]
```

### Node.js Application

```dockerfile
FROM node:18-alpine

WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm ci --only=production

# Copy application
COPY . .

# Create non-root user
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nodejs -u 1001
USER nodejs

EXPOSE 3000

CMD ["node", "server.js"]
```

---

## Docker Compose

### Basic docker-compose.yml

```yaml
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    environment:
      - NGINX_HOST=localhost
    networks:
      - frontend

  app:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/mydb
    networks:
      - frontend
      - backend

  db:
    image: postgres:15
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
      - POSTGRES_DB=mydb
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend

networks:
  frontend:
  backend:

volumes:
  db-data:
```

### Docker Compose Commands

```bash
# Start services
docker-compose up

# Start in detached mode
docker-compose up -d

# Build images
docker-compose build

# Start specific service
docker-compose up web

# Stop services
docker-compose down

# Stop and remove volumes
docker-compose down -v

# View logs
docker-compose logs

# Follow logs
docker-compose logs -f web

# Execute command in service
docker-compose exec web bash

# List running services
docker-compose ps

# Restart services
docker-compose restart
```

---

## Networking

### Network Commands

```bash
# List networks
docker network ls

# Create network
docker network create my-network

# Create bridge network with subnet
docker network create --driver bridge --subnet 172.18.0.0/16 my-network

# Inspect network
docker network inspect my-network

# Connect container to network
docker network connect my-network container-name

# Disconnect container from network
docker network disconnect my-network container-name

# Remove network
docker network rm my-network

# Remove unused networks
docker network prune
```

### Container Networking

```bash
# Run container on specific network
docker run --network my-network nginx

# Run with custom hostname
docker run --hostname web-server nginx

# Link containers (legacy)
docker run --link db:database app

# Expose all ports
docker run -P nginx

# Publish specific port
docker run -p 8080:80 nginx

# Publish to specific IP
docker run -p 127.0.0.1:8080:80 nginx
```

---

## Volumes

### Volume Commands

```bash
# List volumes
docker volume ls

# Create volume
docker volume create my-volume

# Inspect volume
docker volume inspect my-volume

# Remove volume
docker volume rm my-volume

# Remove unused volumes
docker volume prune

# Remove all unused volumes
docker volume prune -a
```

### Using Volumes

```bash
# Mount named volume
docker run -v my-volume:/data nginx

# Mount host directory
docker run -v $(pwd):/app nginx

# Read-only mount
docker run -v my-volume:/data:ro nginx

# Anonymous volume
docker run -v /data nginx

# Mount with specific permissions
docker run -v $(pwd):/app:rw nginx
```

---

## System Commands

### Cleanup

```bash
# Remove stopped containers
docker container prune

# Remove unused images
docker image prune

# Remove unused networks
docker network prune

# Remove unused volumes
docker volume prune

# Remove everything unused
docker system prune

# Remove everything including volumes
docker system prune -a --volumes

# Show disk usage
docker system df
```

### System Information

```bash
# Show Docker info
docker info

# Show Docker version
docker version

# Show events
docker events

# Show system-wide information
docker system df

# Show detailed disk usage
docker system df -v
```

---

## Best Practices

### Dockerfile Optimization

```dockerfile
# ✅ Good: Specific base image version
FROM python:3.11-slim

# ❌ Bad: Using latest tag
FROM python:latest

# ✅ Good: Multi-stage build
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html

# ✅ Good: Layer caching - copy dependencies first
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .

# ✅ Good: Use .dockerignore
# Create .dockerignore file
node_modules
.git
*.md
.env

# ✅ Good: Non-root user
RUN useradd -m appuser
USER appuser

# ✅ Good: Clean up in same layer
RUN apt-get update && \
    apt-get install -y package && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
```

### Security Best Practices

```bash
# Scan image for vulnerabilities
docker scan myapp:1.0

# Use official images
docker pull nginx:alpine

# Don't run as root
USER nonroot

# Limit container resources
docker run --cpus=".5" --memory="512m" myapp

# Read-only root filesystem
docker run --read-only myapp

# Drop capabilities
docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE nginx
```

---

## Common Patterns

### Development Environment

```yaml
# docker-compose.dev.yml
version: '3.8'

services:
  app:
    build:
      context: .
      target: development
    volumes:
      - .:/app
      - /app/node_modules
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
    command: npm run dev
```

### Health Checks

```dockerfile
# Dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost/ || exit 1
```

```yaml
# docker-compose.yml
services:
  web:
    image: nginx
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

### Environment Variables

```bash
# .env file
DB_HOST=localhost
DB_PORT=5432
DB_NAME=myapp
```

```yaml
# docker-compose.yml
services:
  app:
    env_file: .env
    environment:
      - API_KEY=${API_KEY}
```

---

## Troubleshooting

### Common Issues

#### Container Won't Start

```bash
# Check logs
docker logs container-name

# Check with error output
docker logs --details container-name

# Inspect container
docker inspect container-name
```

#### Port Already in Use

```bash
# Find process using port
lsof -i :8080

# Change port mapping
docker run -p 8081:80 nginx
```

#### Out of Disk Space

```bash
# Check disk usage
docker system df

# Clean up
docker system prune -a

# Remove specific volumes
docker volume rm $(docker volume ls -qf dangling=true)
```

#### Cannot Connect to Docker Daemon

```bash
# Start Docker service
sudo systemctl start docker

# Add user to docker group
sudo usermod -aG docker $USER
newgrp docker
```

---

## Quick Reference

| Command | Description |
|---------|-------------|
| `docker run <image>` | Run container |
| `docker ps` | List containers |
| `docker stop <id>` | Stop container |
| `docker rm <id>` | Remove container |
| `docker images` | List images |
| `docker pull <image>` | Download image |
| `docker build -t <name> .` | Build image |
| `docker logs <id>` | View logs |
| `docker exec -it <id> bash` | Shell into container |
| `docker-compose up -d` | Start services |

---

## Additional Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Dockerfile Best Practices](https://docs.docker.com/develop/dev-best-practices/)
- [Play with Docker](https://labs.play-with-docker.com/)

---

## Related Topics

- [Configuration Examples]({% link docs/reference/config-examples.md %})
- [Python Snippets]({% link docs/reference/python-snippets.md %})
