# Docker Learning Guide 

A comprehensive reference guide for learning Docker fundamentals, best practices, and troubleshooting.

## Table of Contents
- [What is Docker?](#what-is-docker)
- [Installation](#installation)
- [Basic Concepts](#basic-concepts)
- [Essential Commands](#essential-commands)
- [Working with Images](#working-with-images)
- [Container Management](#container-management)
- [Volume Mounting](#volume-mounting)
- [Port Mapping & Networking](#port-mapping--networking)
- [Dockerfile Basics](#dockerfile-basics)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Real-World Examples](#real-world-examples)

## What is Docker?

Docker is a containerization platform that allows you to package applications and their dependencies into lightweight, portable containers. Containers ensure that your application runs consistently across different environments.

### Key Benefits:
- **Consistency**: Same environment across development, testing, and production
- **Isolation**: Applications run in isolated environments
- **Portability**: Run anywhere Docker is installed
- **Scalability**: Easy to scale applications horizontally
- **Resource Efficiency**: Lightweight compared to virtual machines

## Installation

### Windows
1. Download Docker Desktop from [docker.com](https://www.docker.com/products/docker-desktop)
2. Install and restart your computer
3. Verify installation: docker --version

### macOS
`ash
# Using Homebrew
brew install --cask docker

# Or download from docker.com
`

### Linux (Ubuntu/Debian)
`ash
# Update package index
sudo apt update

# Install Docker
sudo apt install docker.io

# Start and enable Docker
sudo systemctl start docker
sudo systemctl enable docker

# Add user to docker group (optional)
sudo usermod -aG docker 
`

## Basic Concepts

### Images vs Containers
- **Image**: A read-only template used to create containers (like a class in programming)
- **Container**: A running instance of an image (like an object in programming)

### Key Components
- **Dockerfile**: Text file with instructions to build an image
- **Registry**: Repository for storing and sharing images (Docker Hub, etc.)
- **Volume**: Persistent storage for containers
- **Network**: Communication between containers

## Essential Commands

### System Information
`ash
# Check Docker version
docker --version
docker version

# System information
docker info

# Show disk usage
docker system df
`

### Image Commands
`ash
# List local images
docker images
docker image ls

# Pull an image from registry
docker pull nginx
docker pull ubuntu:20.04

# Remove an image
docker rmi nginx
docker image rm nginx

# Remove unused images
docker image prune
`

### Container Commands
`ash
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Run a container
docker run nginx

# Run container in background (detached)
docker run -d nginx

# Run container with name
docker run --name my-container nginx

# Stop a container
docker stop container-name

# Start a stopped container
docker start container-name

# Remove a container
docker rm container-name

# Remove all stopped containers
docker container prune
`

## Working with Images

### Pulling Images
`ash
# Pull latest version
docker pull nginx

# Pull specific version
docker pull nginx:1.21

# Pull from different registry
docker pull registry.example.com/my-image
`

### Building Images
`ash
# Build from Dockerfile in current directory
docker build .

# Build with tag
docker build -t my-app:latest .

# Build with specific Dockerfile
docker build -f Dockerfile.prod -t my-app:prod .
`

### Image Management
`ash
# Inspect image details
docker inspect nginx

# View image history
docker history nginx

# Save image to file
docker save nginx > nginx.tar

# Load image from file
docker load < nginx.tar
`

## Container Management

### Running Containers
`ash
# Basic run
docker run nginx

# Run with custom command
docker run ubuntu echo "Hello World"

# Run interactively
docker run -it ubuntu bash

# Run with environment variables
docker run -e MYSQL_ROOT_PASSWORD=secret mysql

# Run with resource limits
docker run --memory=512m --cpus=1 nginx
`

### Container Lifecycle
`ash
# Create container without starting
docker create nginx

# Start existing container
docker start container-name

# Restart container
docker restart container-name

# Pause container
docker pause container-name

# Unpause container
docker unpause container-name

# Kill container (force stop)
docker kill container-name
`

### Container Interaction
`ash
# Execute command in running container
docker exec -it container-name bash

# Copy files to/from container
docker cp file.txt container-name:/path/to/destination
docker cp container-name:/path/to/file.txt ./

# View container logs
docker logs container-name

# Follow logs in real-time
docker logs -f container-name
`

## Volume Mounting

Volumes allow you to persist data and share files between the host and container.

### Types of Volumes
1. **Bind Mounts**: Mount host directory into container
2. **Named Volumes**: Docker-managed volumes
3. **Anonymous Volumes**: Temporary volumes

### Bind Mounts
`ash
# Mount host directory to container
docker run -v /host/path:/container/path nginx

# Windows example (absolute path)
docker run -v "C:\Users\Username\Documents:/app" nginx

# Current directory mount
docker run -v E:\Downloads\Chrome\staticwebsite:/app nginx

# Read-only mount
docker run -v /host/path:/container/path:ro nginx
`

### Named Volumes
`ash
# Create named volume
docker volume create my-volume

# Use named volume
docker run -v my-volume:/data nginx

# List volumes
docker volume ls

# Inspect volume
docker volume inspect my-volume

# Remove volume
docker volume rm my-volume
`

### Volume Management
`ash
# List all volumes
docker volume ls

# Remove unused volumes
docker volume prune

# Backup volume data
docker run --rm -v my-volume:/data -v E:\Downloads\Chrome\staticwebsite:/backup ubuntu tar czf /backup/backup.tar.gz -C /data .
`

## Port Mapping & Networking

### Port Mapping
`ash
# Map host port to container port
docker run -p 8080:80 nginx

# Map to specific host interface
docker run -p 127.0.0.1:8080:80 nginx

# Map random host port
docker run -P nginx

# Map multiple ports
docker run -p 8080:80 -p 3306:3306 nginx
`

### Network Types
`ash
# List networks
docker network ls

# Create custom network
docker network create my-network

# Run container in specific network
docker run --network my-network nginx

# Connect container to network
docker network connect my-network container-name

# Disconnect container from network
docker network disconnect my-network container-name
`

### Container Communication
`ash
# Run containers in same network
docker run --name web --network my-network nginx
docker run --name db --network my-network mysql

# Containers can communicate using container names
# From web container: curl http://db:3306
`

## Dockerfile Basics

### Basic Structure
`dockerfile
# Use official base image
FROM ubuntu:20.04

# Set working directory
WORKDIR /app

# Copy files
COPY . .

# Install dependencies
RUN apt-get update && apt-get install -y python3

# Set environment variables
ENV PYTHONPATH=/app

# Expose port
EXPOSE 8000

# Define default command
CMD ["python3", "app.py"]
`

### Common Instructions
- FROM: Base image
- WORKDIR: Set working directory
- COPY: Copy files from host to container
- ADD: Copy files with additional features (URLs, tar extraction)
- RUN: Execute commands during build
- ENV: Set environment variables
- EXPOSE: Document port (doesn't actually expose)
- CMD: Default command when container starts
- ENTRYPOINT: Fixed command that always runs
- USER: Set user for subsequent instructions
- VOLUME: Create mount point

### Multi-stage Builds
`dockerfile
# Build stage
FROM node:16 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
`

## Best Practices

### Security
`ash
# Run as non-root user
USER 1000

# Use specific image tags, not 'latest'
FROM nginx:1.21

# Scan images for vulnerabilities
docker scan nginx

# Use multi-stage builds to reduce image size
`

### Performance
`ash
# Use .dockerignore file
echo "node_modules" >> .dockerignore
echo "*.log" >> .dockerignore

# Optimize layer caching
# Copy package files first, then install dependencies
COPY package*.json ./
RUN npm install
COPY . .

# Use alpine images for smaller size
FROM node:16-alpine
`

### Resource Management
`ash
# Set memory limits
docker run --memory=512m nginx

# Set CPU limits
docker run --cpus=1 nginx

# Set restart policy
docker run --restart=unless-stopped nginx
`

## Troubleshooting

### Common Issues

#### Container Won't Start
`ash
# Check container logs
docker logs container-name

# Run container interactively to debug
docker run -it nginx bash

# Check if port is already in use
netstat -tulpn | grep :8080
`

#### Volume Mount Issues
`ash
# Check if path exists
ls -la /host/path

# Verify permissions
docker run -v /host/path:/container/path nginx

# Use absolute paths on Windows
docker run -v "C:\full\path:/app" nginx
`

#### Network Issues
`ash
# Check if port is accessible
curl http://localhost:8080

# Inspect container network
docker inspect container-name | grep NetworkMode

# Test container connectivity
docker exec container-name ping google.com
`

### Debugging Commands
`ash
# Inspect container details
docker inspect container-name

# View container processes
docker top container-name

# Monitor resource usage
docker stats container-name

# Check system events
docker events
`

## Real-World Examples

### Static Website (Nginx)
`ash
# Create directory structure
mkdir static-website
cd static-website

# Create HTML files
echo "<h1>Hello World</h1>" > index.html

# Run nginx container
docker run --name website \
  -p 8080:80 \
  -v "E:\Downloads\Chrome\staticwebsite:/usr/share/nginx/html" \
  nginx
`

### Node.js Application
`dockerfile
FROM node:16-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
`

`ash
# Build and run
docker build -t my-node-app .
docker run -p 3000:3000 my-node-app
`

### Database with Persistent Storage
`ash
# Run MySQL with persistent volume
docker run --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=myapp \
  -v mysql-data:/var/lib/mysql \
  -p 3306:3306 \
  mysql:8.0
`

### Multi-Container Application (Docker Compose)
`yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
    environment:
      - DATABASE_URL=mysql://user:password@db:3306/myapp
  
  db:
    image: mysql:8.0
    environment:
      - MYSQL_ROOT_PASSWORD=secret
      - MYSQL_DATABASE=myapp
    volumes:
      - mysql-data:/var/lib/mysql

volumes:
  mysql-data:
`

## Useful Commands Reference

### Quick Reference
`ash
# Essential commands
docker run -d -p 8080:80 --name my-app nginx
docker ps
docker logs my-app
docker exec -it my-app bash
docker stop my-app
docker rm my-app

# Cleanup commands
docker system prune -a  # Remove all unused resources
docker volume prune     # Remove unused volumes
docker network prune    # Remove unused networks
`

### Environment-Specific Commands
`ash
# Development
docker run -it --rm -v E:\Downloads\Chrome\staticwebsite:/app node:16 bash

# Production
docker run -d --restart=unless-stopped -p 80:80 nginx

# Testing
docker run --rm -v E:\Downloads\Chrome\staticwebsite:/app -w /app node:16 npm test
`

## Next Steps

1. **Docker Compose**: Learn to manage multi-container applications
2. **Docker Swarm**: Container orchestration
3. **Kubernetes**: Advanced container orchestration
4. **CI/CD Integration**: Automate Docker builds and deployments
5. **Monitoring**: Implement logging and monitoring solutions

## Resources

- [Official Docker Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Dockerfile Best Practices](https://docs.docker.com/develop/dev-best-practices/)
- [Docker Security Best Practices](https://docs.docker.com/engine/security/)

---

**Happy Containerizing! **

*This guide covers the fundamentals of Docker. Practice with real projects to solidify your understanding.*
