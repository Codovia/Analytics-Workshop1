# Expose Ports, Volume Mounts, Networks, and Resource Limits

## Exposing Ports

### What is a Port?

A port is a communication endpoint. If the URL is the address of a building, the port is a particular door.

Only one application can use a specific port at a time.

**Syntax:** `URL:PORT`

**Example:** `127.0.0.1:5000`

Here `127.0.0.1` is the URL and `5000` is the port.

### Why Expose Ports?

By default, a Docker container does not publish any ports to the host machine. Services running inside the container are not accessible from the host unless we explicitly expose the ports.

### How to Expose Ports

#### 1. Using `docker run`

We use the `-p` flag with the `hostport:containerport` format:

```bash
docker run -p 8000:8080 <IMAGE>
```

This maps port `8000` on the host to port `8080` in the container.

#### 2. Using Docker Compose

Specify the port mapping under the `ports` key in `docker-compose.yaml` file:

```yaml
services:
  web:
    image: nginx
    ports:
      - "8000:80"
```

---

## Volume Mounts

### Why Use Volumes?

By default, all files created inside a container are stored in a writable container layer. The data doesn't persist when the container is removed - it gets deleted when the container stops.

Volumes provide:
- **Persistent data** that survives container removal
- **Shared data** between host and container
- **Live code reloading** during development

### How to Use Volumes

#### 1. Using `docker run`

We use the `-v` flag with the `host_path:container_path` format:

```bash
docker run -v /home/workshop/Downloads:/home/usr/Downloads alpine
```

#### 2. Using Docker Compose

Specify volumes under the `volumes` key in `docker-compose.yaml` file:

```yaml
services:
  web:
    image: nginx
    volumes:
      - /home/workshop/Downloads:/home/usr/Downloads
```

### Named Volumes

Named volumes are managed by Docker and are useful for persisting database data:

```yaml
version: "3.8"

services:
  db:
    image: postgres:16-alpine
    container_name: app_database
    ports:
      - '5432:5432'
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=flask_db
    volumes:
      - app-db:/var/lib/postgresql/data

volumes:
  app-db:
```

In this example:
- `app-db` is a named volume managed by Docker
- Data persists even if you remove the container with `docker compose down -v`

### Volume Types

| Type | Syntax | Purpose |
|------|--------|---------|
| Bind mount | `/host/path:/container/path` | Share host files with container |
| Named volume | `volume-name:/container:path` | Persistent data managed by Docker |
| Anonymous volume | `/container/path` | Temporary container storage |

---

## Docker Networks

### Network Types

#### 1. Bridge (Default) - Isolation

This is the default network type. Containers on the same bridge network can communicate with each other.

**Demo:**

Start two Alpine containers:
```bash
docker run -dit --name alpine1 alpine ash
docker run -dit --name alpine2 alpine ash
```

Inspect the bridge network:
```bash
docker network inspect bridge
```

Exec into one container and ping the other:
```bash
docker exec -it alpine1 sh
/ # ping -c 3 alpine2
```

#### 2. Host - No Isolation

The container shares the host's network stack. The `--network=host` parameter is used:

```bash
docker run --network=host nginx
```

Use this when you need full network access from the container.

#### 3. None - No Network

The container has no network access whatsoever:

```bash
docker run --network=none alpine
```

Use this for highly secure, isolated containers.

#### 4. Custom Networks

Create your own isolated networks:

```bash
docker network create my-network
docker run --network=my-network my-app
```

---

## Resource Limits

### Why Limit Resources?

By default, a container can use as much resources as the host's kernel scheduler allows. This can cause problems - one container might use excessive resources and starve other processes.

Docker allows us to control CPU and memory usage.

### How to Limit Resources

#### 1. Using `docker run`

**For memory:**
```bash
docker run -m=100m alpine
```

**For CPU:**
```bash
docker run --cpus=1.5 alpine
```

This limits the container to 1.5 CPU cores.

**Combined example:**
```bash
docker run -m=512m --cpus=1.5 --name my-app my-image
```

#### 2. Using Docker Compose

Specify limits under the `deploy` key in `docker-compose.yaml` file:

```yaml
version: "3.8"

services:
  web:
    image: nginx
    deploy:
      resources:
        limits:
          cpus: '0.50'
          memory: 50M
        reservations:
          cpus: '0.25'
          memory: 20M
```

### Resource Commands

| Command | Description |
|---------|-------------|
| `docker stats` | Live resource usage of all containers |
| `docker stats <container>` | Resource usage for specific container |
| `-m <size>` | Memory limit (b, k, m, g) |
| `--cpus=<value>` | CPU limit (e.g., 1.5 = 1.5 cores) |
| `--cpuset-cpus=<range>` | Specific CPUs to use (e.g., 0-3) |

---

## Complete Example

Here's a complete `docker-compose.yaml` showing all concepts:

```yaml
version: "3.8"

services:
  db:
    image: postgres:16-alpine
    container_name: app_database
    ports:
      - '5432:5432'            # Expose PostgreSQL port
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=flask_db
    volumes:
      - app-db:/var/lib/postgresql/data  # Named volume for persistence
    deploy:
      resources:
        limits:
          cpus: '1.0'
          memory: 512M
    networks:
      - app-network

  web:
    build: ./app
    ports:
      - "5000:5000"            # Expose Flask port
    container_name: app_frontend
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=flask_db
      - FLASK_ENV=development
    volumes:
      - ./app:/app             # Bind mount for live reload
    depends_on:
      - db
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 256M
    networks:
      - app-network

networks:
  app-network:

volumes:
  app-db:
```

## Quick Reference

| Feature | Docker Run Flag | Compose Key |
|---------|-----------------|-------------|
| Port mapping | `-p 8000:80` | `ports: ["8000:80"]` |
| Bind mount | `-v /host:/container` | `volumes: ["/host:/container"]` |
| Named volume | `-v vol:/path` | `volumes: ["vol:/path"]` |
| CPU limit | `--cpus=1.5` | `deploy.resources.limits.cpus: '1.5'` |
| Memory limit | `-m=512m` | `deploy.resources.limits.memory: 512M` |
| Network | `--network=net` | `networks: [net]` |
