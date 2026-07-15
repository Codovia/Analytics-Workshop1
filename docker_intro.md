# What is Docker?

Docker is a container management service. The keywords of Docker are **develop**, **ship**, and **run anywhere**. The whole idea of Docker is for developers to easily develop applications, ship them into containers which can then be deployed anywhere.

Docker was first released in March 2013 and since then has become an essential tool for modern software development, especially in Agile-based projects.

## Features of Docker

- **Reduced footprint** - Docker reduces the size of development by providing a smaller footprint of the operating system via containers.

- **Seamless collaboration** - With containers, it becomes easier for teams across different units (Development, QA, and Operations) to work seamlessly across applications.

- **Portable deployment** - You can deploy Docker containers anywhere, on any physical and virtual machines, and even on the cloud.

- **Scalability** - Since Docker containers are lightweight, they are very easily scalable.

## What is a Container?

A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI.

### Container Characteristics

- Isolated - Containers are isolated from each other and run their own software, binaries, and configurations
- Portable - Can be run on any OS
- Ephemeral - Can be started and stopped quickly
- Lightweight - Uses the host OS kernel, no full OS needed

## What is a Container Image?

When running a container, it uses an isolated filesystem. This custom filesystem is provided by a container image. Since the image contains the container's filesystem, it must contain everything needed to run an application:

- All dependencies
- Configuration files
- Scripts
- Binaries
- Environment variables
- Default command to run
- Other metadata

## Docker vs Virtual Machines

| Aspect | Docker Containers | Virtual Machines |
|--------|-------------------|------------------|
| OS | Shares host OS kernel | Full guest OS |
| Size | Small (MBs) | Large (GBs) |
| Startup | Seconds | Minutes |
| Performance | Near-native | Slight overhead |
| Isolation | Process-level | OS-level |

## Docker Architecture

```
┌─────────────────────────────────────────────────────────┐
│                     Docker Client                        │
│                    (CLI / API)                           │
└───────────────────────────┬─────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│                   Docker Daemon                         │
│              (Builds, runs, distributes)                │
└───────────────────────────┬─────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│                    Docker Registry                       │
│              (Docker Hub, private registry)              │
└─────────────────────────────────────────────────────────┘
```

## Common Use Cases

1. **Development** - Consistent development environments across teams

2. **CI/CD** - Build, test, and deploy in containers

3. **Microservices** - Each service in its own container

4. **Legacy Applications** - Containerize old apps for easier deployment

5. **Data Processing** - Isolated environments for data tasks

## Key Docker Concepts

| Concept | Description |
|---------|-------------|
| **Image** | Read-only template with instructions for creating a container |
| **Container** | Runnable instance of an image |
| **Dockerfile** | Text document with instructions to build an image |
| **Docker Compose** | Tool for defining and running multi-container apps |
| **Registry** | Storage and distribution system for named Docker images |

## Next Steps

- Learn [Docker Commands](docker_commands.md)
- Learn to [Build Custom Containers](build_container.md)
- Learn [Docker Compose](docker_compose.md)
