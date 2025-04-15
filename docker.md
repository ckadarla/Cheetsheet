## Docker Basics

### 1. What is Docker?
Docker is an open-source platform for automating deployment, scaling, and management of applications using containerization. It allows applications to run in isolated environments (containers) ensuring consistency and portability.

### 2. What is a Docker container?
A Docker container is a lightweight, standalone executable package with all needed components: code, runtime, system tools, libraries, and dependencies. It ensures consistent behavior across environments.

### 3. What are the benefits of using Docker?
- Easy and consistent deployment across environments
- Efficient resource usage
- Rapid scalability and horizontal scaling
- Isolation between apps and dependencies
- Simplified dependency management
- Easier collaboration and sharing of applications

### 4. How does Docker differ from virtual machines (VMs)?
- Docker containers share the host OS kernel, making them lightweight and fast
- VMs emulate full OS, leading to slower start times and higher resource use
- Docker provides isolation at the app level; VMs do so at the hardware level

### 5. What is a Docker image?
A Docker image is a read-only template to create containers. It contains all necessary files and instructions to set up the environment and is built using a Dockerfile.

### 6. What is a Docker registry?
A Docker registry stores and distributes Docker images. Docker Hub is the default public registry, but private registries can be used within organizations.

### 7. How do you build a Docker image?
Create a `Dockerfile` and run:
```bash
docker build -t imagename .
```
This builds the image based on instructions in the Dockerfile.

### 8. How do you run a Docker container?
Use:
```bash
docker run [options] image_name
```
Options can include ports (`-p`), environment variables (`-e`), volumes (`-v`), etc.

### 9. How do you manage Docker containers?
Useful commands:
- `docker ps`: list running containers
- `docker start` / `stop` / `restart`: control containers
- `docker logs`: view logs
- `docker rm`: remove containers

---

Let me know if you'd like this content saved to a `.md` file!
