# Docker Commands

Docker is a containerization system that packages and runs applications with their dependencies inside containers. There are several Docker commands you must know when working with Docker.

## Docker Post-Installation Setup

### Run Docker as Non-Root User

To create the docker group and add your user:

1. Create the docker group:
   ```bash
   sudo groupadd docker
   ```

2. Add your user to the docker group:
   ```bash
   sudo usermod -aG docker $USER
   ```

3. Activate the changes to groups:
   ```bash
   newgrp docker
   ```

4. Verify that you can run docker commands without sudo:
   ```bash
   docker images
   ```

---

## Essential Docker Commands

### 1. Docker Version

To find the installed Docker version:

```bash
docker --version
```

Example output:
```
Docker version 27.3.1, build ce12230
```

### 2. Downloading an Image

To work with any Docker image, we need to download it first:

```bash
docker pull <IMAGE>
```

Example of pulling Alpine image:
```bash
docker pull alpine:latest
```

### 3. List All Docker Images

To list all images available locally on your host machine:

```bash
docker images
```

Example output:
```
REPOSITORY   TAG      IMAGE ID       CREATED        SIZE
alpine       latest   c059bfaa849c   6 weeks ago    7.39MB
postgres     16       abc123xyz456   2 days ago     229MB
```

### 4. Run Docker Image

The `docker run` command creates a writeable container layer over the specified image and starts it:

```bash
docker run [options] <IMAGE>
```

Common options:
- `-d` - Run in detached mode (background)
- `-it` - Interactive mode with terminal
- `-p` - Publish container port to host
- `-v` - Mount volume
- `--name` - Assign a name to the container
- `--rm` - Automatically remove container when it exits

> Explore all options [here](https://docs.docker.com/engine/reference/run/)

Example of running Alpine image with interactive terminal:
```bash
docker run -it alpine:latest
# or
docker run -it alpine:latest sh
```

### 5. List Running Containers

The `docker ps` command lists only running containers:

```bash
docker ps
```

Example output:
```
CONTAINER ID   IMAGE          COMMAND       CREATED        STATUS        PORTS     NAMES
8973c7347905   alpine:latest  "/bin/sh"     2 minutes ago  Up 2 minutes            ecstatic_jang
```

To list all containers including stopped ones:
```bash
docker ps -a
```

### 6. Access a Running Container

The `docker exec` command runs a new command in a running container:

```bash
docker exec [options] <CONTAINER_ID> <COMMAND>
```

> Explore options [here](https://docs.docker.com/engine/reference/commandline/exec/)

Example: Execute into a running Alpine container and create files:
```bash
# First, get container ID from docker ps
docker ps

# Then exec into it
docker exec -it 8973c7347905 sh

# Inside container:
/ # mkdir demo
/ # cd demo
/demo # touch helloworld.txt
/demo # ls
helloworld.txt
/demo # exit
```

Commands used inside container:
- `mkdir` - Create directory
- `cd` - Change directory
- `touch` - Create empty file
- `ls` - List files

### 7. Stop a Container

Stop a running container:

```bash
docker stop [OPTIONS] <CONTAINER_ID>
```

> Explore options [here](https://docs.docker.com/engine/reference/commandline/stop/)

Example:
```bash
docker stop 8973c7347905
```

Once stopped, the container still exists locally but is not running.

### 8. Start a Stopped Container

Start a stopped container:

```bash
docker start [OPTIONS] <CONTAINER_ID>
```

> Explore options [here](https://docs.docker.com/engine/reference/commandline/start/)

Example:
```bash
# First list all containers to find the stopped one
docker ps -a

# Start it
docker start 8973c7347905
```

### 9. Remove a Container

Remove one or more containers:

```bash
docker rm [OPTIONS] <CONTAINER_ID>
```

> Explore options [here](https://docs.docker.com/engine/reference/commandline/rm/)

To remove a running container, use the `-f` (force) flag:
```bash
docker rm -f 8973c7347905
```

### 10. Remove an Image

Remove local images:

```bash
docker rmi [OPTIONS] <IMAGE_ID> or <IMAGE_NAME>
```

Example:
```bash
docker rmi c059bfaa849c
# or by name
docker rmi alpine:latest
```

To force removal:
```bash
docker rmi -f alpine:latest
```

### 11. View Container Logs

View logs from a container:

```bash
docker logs <CONTAINER_ID>
```

Follow logs in real-time:
```bash
docker logs -f <CONTAINER_ID>
```

### 12. Remove All Stopped Containers

Clean up all stopped containers:
```bash
docker container prune
```

### 13. Remove All Unused Images

Clean up all unused images:
```bash
docker image prune -a
```

## Quick Reference Card

| Command | Description |
|---------|-------------|
| `docker --version` | Show Docker version |
| `docker pull <img>` | Download an image |
| `docker images` | List local images |
| `docker run <img>` | Run a container |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `docker exec <id> <cmd>` | Execute command in container |
| `docker stop <id>` | Stop a container |
| `docker start <id>` | Start a stopped container |
| `docker rm <id>` | Remove container |
| `docker rmi <img>` | Remove image |
| `docker logs <id>` | Show container logs |
| `docker system prune` | Remove unused data |
