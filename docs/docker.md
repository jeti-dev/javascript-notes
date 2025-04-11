---
title: Docker
layout: default
---

# Docker
## From a tutorial - TBD
[Notes of this video](https://www.youtube.com/watch?v=RqTEHSBrYFw&t=165s)

- container image: like a class
- container: like the object from a class
    - containers share the kernel with the host system
- 3 linux building blocks
    - **namespace**: make a part of a global resource appear to a process that the process thinks it owns the whole resource
        - e.g. using the User namespace the user of the container can be an admin in the container BUT on the host system that user is mapped to a non-admin user
    - **cgroup**: organize processess to hierarchical groups -> the resources these groups use can then be limited and monitored e.g. 30% CPU for group1 and 20% for group2
    - **union mount filesystem (overlayfs)**: virtual file system: if there are files at multiple levels, they can appear as they were in on the same level -> images can share stuff
- storing data:
    - by default, everything is cleared on shutdown
    - dependency (we know the data at build time): build into the image e.g. node
    - data generated: use a (named or anonymous) Volume to store it outside of the container -> will be stored on the virtual machine docker is running (volume mount, preferred) e.g. db file, log file
    - mount a specific folder on your machine to a folder in the container: bind mount e.g. source code
- dockerfile: what and how to build
    - build context: other files included in the build
    - .dockerignore: what you don't want to build into the image

## Nodejs development container example
### Dockerfile
``` yaml
# Use a Node.js base image
FROM node:23-alpine

# Set working directory inside the container
WORKDIR /app

# Copy only package files first to leverage caching
COPY package*.json ./

# Install dependencies
RUN npm install

# Expose the port Express will listen on
EXPOSE 3000

# Start the app in development mode: it runs when the container is RUN!
CMD ["npm", "run", "dev"]
```

### .dockerignore
``` yaml
# It won't be built into the image
node_modules
```
### Start script
``` shell
docker run -it --rm  -v ${PWD}:/app  -v /app/node_modules -p 3000:3000  express-dev
```

| Param | Function |
|-------|----------|
|-it|interactive mode|
|--rm|remove the container after stopping it|
|-v ${PWD}:/app|bind mount our current directory to the app directory in the container -> mounting our source code|
|-v /app/node_modules|create an anonymous volume for the node_modules|
|-p 3000:3000|port of the host and container|
| express-dev|name of the image|

### docker-compose
```yaml
version: "3.8"

services:
  express:
    build:
      context: .
      dockerfile: Dockerfile
    working_dir: /app
    volumes:
      - .:/app # Mount your local code into the container
      - /app/node_modules # Ensure node_modules are isolated
    ports:
      - "3000:3000" # Expose port for Express app
    command: ["npm", "run", "dev"] # Use npm to start in dev mode
    environment:
      - NODE_ENV=development # Set environment variable for dev

```


## 🟢 Beginner Docker Concepts

| Concept             | Description                                                                 |
|---------------------|-----------------------------------------------------------------------------|
| `Dockerfile`        | A script with instructions to build a Docker image.                         |
| `docker build`      | Builds an image from a Dockerfile.                                          |
| `docker run`        | Runs a container based on an image.                                         |
| `docker ps`         | Lists running containers.                                                   |
| `docker images`     | Lists available images.                                                     |
| `docker stop`       | Stops a running container.                                                  |
| `docker rm`         | Removes a stopped container.                                                |
| `docker rmi`        | Removes an image.                                                           |
| `docker pull`       | Downloads an image from Docker Hub.                                        |
| `docker exec`       | Runs a command inside a running container.                                  |
| `.dockerignore`     | File listing paths to exclude from the image build context.                 |
| `docker-compose.yml`| Defines multi-container applications.                                       |

---

## 🟡 Intermediate Docker Concepts

| Concept                    | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| Multi-stage builds         | Optimize Dockerfile by using multiple `FROM` statements for smaller images.|
| Docker volumes             | Persist or share data between containers using `-v` flag or `volumes`.     |
| Named and anonymous volumes| Named volumes persist beyond container lifecycle; anonymous do not.        |
| Networking                 | Custom bridges allow inter-container communication by name.                |
| Environment variables      | Use `-e VAR=value` or `env_file` to inject env vars into containers.       |
| Docker Compose             | Define and manage multi-container apps with `docker-compose`.              |
| Healthchecks               | Specify a command in the Dockerfile to check container health.             |
| Port binding               | Map host ports to container ports with `-p`.                               |
| `.env` in Compose          | Load environment variables into Compose files.                             |
| `docker logs`              | View container stdout/stderr logs.                                         |

---

## 🔴 Advanced Docker Concepts

| Concept                     | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| Dockerfile best practices   | Minimize layers, pin versions, clean cache to reduce image size.           |
| BuildKit                    | Enhanced image build engine with better caching and features.              |
| Layer caching               | Docker caches unchanged layers during builds for speed.                   |
| Container orchestration     | Tools like Docker Swarm or Kubernetes manage many containers.              |
| Security scanning           | Use `docker scan` to detect vulnerabilities in images.                     |
| Image signing and provenance| Use tools like Notary or `cosign` to sign and verify images.              |
| Secrets management          | Store sensitive data securely outside of images using tools like `docker secrets`. |
| Docker context              | Work with remote Docker hosts using different contexts.                    |
| Custom networks             | Isolate and control communication between containers.                      |
| Resource limits             | Use `--memory`, `--cpus` to constrain resource usage.                      |
