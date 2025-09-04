# Docker Learning Repository

This repository contains a comprehensive Docker learning guide and practical examples for containerization.

## Repository Contents

- **Docker-Learning-Guide.md** - Complete Docker reference guide with examples
- **staticwebsite/** - Sample static website for Docker practice
  - **index.html** - Main page
  - **page1.html** - Sample page 1
  - **page2.html** - Sample page 2

## Quick Start

### Running the Static Website with Docker

1. **Navigate to the staticwebsite directory:**

   ```bash
   cd staticwebsite
   ```

2. **Run the website using Docker:**

   ```bash
   docker run --name static-site -p 8080:80 -v "E:\Downloads\Chrome\staticwebsite:/usr/share/nginx/html" nginx
   ```

3. **Access your website:**
   - Open your browser and go to [http://localhost:8080](http://localhost:8080)
   - Navigate to [http://localhost:8080/page1.html](http://localhost:8080/page1.html) and [http://localhost:8080/page2.html](http://localhost:8080/page2.html)

### Managing the Container

```bash
# Stop the container
docker stop static-site

# Start the container again
docker start static-site

# Remove the container
docker rm static-site

# View container logs
docker logs static-site
```

## Learning Path

1. **Start with the basics** - Read through Docker-Learning-Guide.md
2. **Practice with examples** - Try the commands in the guide
3. **Experiment with the static website** - Modify HTML files and see changes
4. **Build your own containers** - Create Dockerfiles for your projects

## What You'll Learn

- Docker fundamentals and concepts
- Container lifecycle management
- Volume mounting and data persistence
- Port mapping and networking
- Dockerfile creation and best practices
- Troubleshooting common issues
- Real-world deployment scenarios

## Key Topics Covered

- **Container Basics**: Images, containers, and registries
- **Commands**: Essential Docker CLI commands
- **Volumes**: Data persistence and file sharing
- **Networking**: Port mapping and container communication
- **Dockerfiles**: Building custom images
- **Best Practices**: Security, performance, and resource management
- **Troubleshooting**: Common issues and debugging techniques

## Prerequisites

- Docker Desktop installed on your system
- Basic command line knowledge
- Text editor for modifying files

## Contributing

Feel free to:

- Add more examples
- Improve documentation
- Report issues
- Suggest enhancements

## Useful Resources

- [Official Docker Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Dockerfile Best Practices](https://docs.docker.com/develop/dev-best-practices/)

---

**Happy Learning!**

*Start your Docker journey with this comprehensive guide and hands-on examples.*
