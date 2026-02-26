## Docker Basics

### Container Management

#### Running Containers

```bash
# Run container
docker run <image_name>

# Run with name
docker run --name <container_name> <image_name>

# Run in detached mode (background)
docker run -d <image_name>

# Run with port mapping
docker run -p <host_port>:<container_port> <image_name>

# Run with environment variables
docker run -e "KEY=value" <image_name>

# Run with volume mount
docker run -v <host_path>:<container_path> <image_name>

# Run with interactive terminal
docker run -it <image_name> /bin/bash

# Example: SQL Server 2022
docker run -e 'ACCEPT_EULA=Y' \
  -e 'SA_PASSWORD=Asrofil123!' \
  -p 1433:1433 \
  --name sql2022 \
  -d mcr.microsoft.com/mssql/server:2022-latest
```

#### Container Lifecycle

```bash
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Start container
docker start <container_name>

# Stop container
docker stop <container_name>

# Restart container
docker restart <container_name>

# Pause container
docker pause <container_name>

# Unpause container
docker unpause <container_name>

# Remove container
docker rm <container_name>

# Force remove running container
docker rm -f <container_name>

# Remove all stopped containers
docker container prune
```

#### Container Information

```bash
# View container logs
docker logs <container_name>

# Follow logs (real-time)
docker logs -f <container_name>

# View last N lines of logs
docker logs --tail 100 <container_name>

# Inspect container
docker inspect <container_name>

# View container processes
docker top <container_name>

# View container stats
docker stats <container_name>

# View all container stats
docker stats
```

#### Executing Commands

```bash
# Execute command in running container
docker exec <container_name> <command>

# Interactive shell
docker exec -it <container_name> /bin/bash

# Execute as specific user
docker exec -u root -it <container_name> /bin/bash

# Example: MySQL client
docker exec -it mysql_container mysql -u root -p
```

---

## Docker Images

### Managing Images

```bash
# List images
docker images

# Pull image from registry
docker pull <image_name>

# Pull specific version
docker pull <image_name>:<tag>

# Build image from Dockerfile
docker build -t <image_name>:<tag> .

# Build with custom Dockerfile
docker build -f <dockerfile_path> -t <image_name> .

# Tag image
docker tag <image_id> <new_image_name>:<tag>

# Push image to registry
docker push <image_name>:<tag>

# Remove image
docker rmi <image_name>

# Force remove image
docker rmi -f <image_name>

# Remove unused images
docker image prune

# Remove all unused images
docker image prune -a
```

### Image Information

```bash
# View image history
docker history <image_name>

# Inspect image
docker inspect <image_name>

# View image layers
docker image inspect <image_name> --format='{{.RootFS.Layers}}'
```

---

## Docker Compose

### Basic Commands

```bash
# Start services
docker compose up

# Start in detached mode
docker compose up -d

# Build and start services
docker compose up --build

# Build and start in detached mode
docker compose up --build -d

# Stop services
docker compose down

# Stop and remove volumes
docker compose down -v

# View running services
docker compose ps

# View logs
docker compose logs

# Follow logs
docker compose logs -f

# Logs for specific service
docker compose logs <service_name>

# Restart services
docker compose restart

# Restart specific service
docker compose restart <service_name>

# Execute command in service
docker compose exec <service_name> <command>

# Scale service
docker compose up -d --scale <service_name>=3
```

### Advanced Compose

```bash
# Build services without starting
docker compose build

# Pull images for services
docker compose pull

# Validate docker-compose.yml
docker compose config

# View running processes
docker compose top

# Pause services
docker compose pause

# Unpause services
docker compose unpause

# Remove stopped containers
docker compose rm
```

---

## Docker Volumes

### Volume Management

```bash
# List volumes
docker volume ls

# Create volume
docker volume create <volume_name>

# Inspect volume
docker volume inspect <volume_name>

# Remove volume
docker volume rm <volume_name>

# Remove unused volumes
docker volume prune

# Remove all unused volumes
docker volume prune -a
```

### Using Volumes

```bash
# Run with named volume
docker run -v <volume_name>:<container_path> <image_name>

# Run with bind mount
docker run -v <host_path>:<container_path> <image_name>

# Read-only volume
docker run -v <volume_name>:<container_path>:ro <image_name>
```

---

## Docker Networks

### Network Management

```bash
# List networks
docker network ls

# Create network
docker network create <network_name>

# Create custom network with subnet
docker network create --subnet=172.18.0.0/16 <network_name>

# Inspect network
docker network inspect <network_name>

# Connect container to network
docker network connect <network_name> <container_name>

# Disconnect container from network
docker network disconnect <network_name> <container_name>

# Remove network
docker network rm <network_name>

# Remove unused networks
docker network prune
```

### Running with Networks

```bash
# Run container on specific network
docker run --network <network_name> <image_name>

# Run with network alias
docker run --network <network_name> --network-alias <alias> <image_name>
```

---

## Dockerfile Best Practices

### Basic Dockerfile

```dockerfile
# Base image
FROM ubuntu:22.04

# Set working directory
WORKDIR /app

# Copy files
COPY . .

# Install dependencies
RUN apt-get update && apt-get install -y \
    package1 \
    package2 \
    && rm -rf /var/lib/apt/lists/*

# Expose port
EXPOSE 8080

# Set environment variables
ENV NODE_ENV=production

# Run command
CMD ["node", "app.js"]
```

### Multi-stage Build

```dockerfile
# Build stage
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM node:18-slim
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY package*.json ./
RUN npm install --production
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

---

## Docker System Commands

### System Information

```bash
# View Docker info
docker info

# View Docker version
docker version

# View disk usage
docker system df

# Detailed disk usage
docker system df -v
```

### System Cleanup

```bash
# Remove unused data (containers, networks, images)
docker system prune

# Remove all unused data (including volumes)
docker system prune -a --volumes

# Remove everything (use with caution!)
docker system prune -a -f
```

---

## Common Use Cases

### SQL Server

```bash
# Create SQL Server container
docker run -e 'ACCEPT_EULA=Y' \
  -e 'SA_PASSWORD=YourPassword123!' \
  -p 1433:1433 \
  --name sqlserver \
  -d mcr.microsoft.com/mssql/server:2022-latest

# Start SQL Server
docker start sqlserver

# Connect to SQL Server
docker exec -it sqlserver /opt/mssql-tools/bin/sqlcmd \
  -S localhost -U SA -P 'YourPassword123!'
```

### MySQL

```bash
# Create MySQL container
docker run --name mysql \
  -e MYSQL_ROOT_PASSWORD=root123 \
  -e MYSQL_DATABASE=mydb \
  -e MYSQL_USER=user \
  -e MYSQL_PASSWORD=pass123 \
  -p 3306:3306 \
  -d mysql:8.0

# Connect to MySQL
docker exec -it mysql mysql -u root -p
```

### PostgreSQL

```bash
# Create PostgreSQL container
docker run --name postgres \
  -e POSTGRES_PASSWORD=postgres123 \
  -e POSTGRES_DB=mydb \
  -p 5432:5432 \
  -d postgres:15

# Connect to PostgreSQL
docker exec -it postgres psql -U postgres
```

### Redis

```bash
# Create Redis container
docker run --name redis \
  -p 6379:6379 \
  -d redis:7

# Connect to Redis CLI
docker exec -it redis redis-cli
```

### MongoDB

```bash
# Create MongoDB container
docker run --name mongodb \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=admin123 \
  -p 27017:27017 \
  -d mongo:6

# Connect to MongoDB
docker exec -it mongodb mongosh -u admin -p admin123
```

### Nginx

```bash
# Create Nginx container
docker run --name nginx \
  -p 80:80 \
  -v $(pwd)/html:/usr/share/nginx/html:ro \
  -d nginx:alpine
```

---

## Docker Compose Example

### docker-compose.yml

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8080:8080"
    environment:
      - NODE_ENV=production
    volumes:
      - ./src:/app/src
    depends_on:
      - db
    networks:
      - app-network

  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: postgres123
      POSTGRES_DB: mydb
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - app-network

  redis:
    image: redis:7-alpine
    networks:
      - app-network

volumes:
  postgres-data:

networks:
  app-network:
    driver: bridge
```

---

## Troubleshooting

### Common Issues

#### Container won't start

```bash
# Check logs
docker logs <container_name>

# Check container details
docker inspect <container_name>

# Try running in foreground
docker run -it <image_name>
```

#### Port already in use

```bash
# Find process using port
sudo lsof -i :<port>
# or
sudo netstat -tulpn | grep :<port>

# Stop container using that port
docker stop $(docker ps -q --filter "publish=<port>")
```

#### Out of disk space

```bash
# Check disk usage
docker system df

# Clean up
docker system prune -a --volumes
```

#### Permission denied

```bash
# Add user to docker group
sudo usermod -aG docker $USER

# Restart session or run
newgrp docker
```

---

## Best Practices

✅ **Do:**

- Use official images when possible
- Tag your images with versions
- Use `.dockerignore` file
- Keep images small (use alpine variants)
- Use multi-stage builds
- Don't run containers as root
- Use environment variables for configuration
- Use named volumes for data persistence

❌ **Don't:**

- Store secrets in images
- Use `latest` tag in production
- Install unnecessary packages
- Leave containers running when not needed
- Ignore security updates

---

## Quick Reference

```bash
# Build and run with Docker Compose
docker compose up --build -d

# View logs
docker compose logs -f

# Stop all services
docker compose down

# Restart service
docker compose restart <service>

# Execute command
docker compose exec <service> bash

# Clean everything
docker system prune -a --volumes -f
```