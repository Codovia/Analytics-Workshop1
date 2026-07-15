# Docker Compose - Run Multiple Services

## What is Docker Compose?

Developing applications using Docker can become challenging when juggling multiple services and containers.

Docker Compose is the tool that helps you run multi-container application environments.

An application can consist of multiple containers running different services. It can be tedious to start and manage containers manually, so Docker created a useful tool - Docker Compose.

Docker Compose works by applying rules defined in a `docker-compose.yaml` file. The YAML file configures the application's services and includes rules specifying how you want them to run. With the file in place, you can start, stop, or rebuild all services using a single command.

## Docker Compose Basic Commands

| Command | Description |
|---------|-------------|
| `docker compose --help` | Show help and usage instructions |
| `docker compose build` | Build images for services containing `build:` |
| `docker compose run` | Run a one-time command against a service |
| `docker compose up` | Build, (re)create, start, and attach to containers |
| `docker compose -f <file>` | Specify custom compose file location |
| `docker compose start` | Start existing containers |
| `docker compose stop` | Stop running containers (without removing) |
| `docker compose pause` | Pause running containers |
| `docker compose unpause` | Unpause paused containers |
| `docker compose down` | Stop and remove containers, networks, volumes |
| `docker compose ps` | List containers in the compose project |
| `docker compose images` | List images used by created containers |
| `docker compose ls` | List running Compose projects |

> **Note:** Modern Docker uses `docker compose` (space) not `docker-compose` (hyphen). The hyphen version refers to the standalone tool which is now deprecated.

## docker-compose.yaml File

The `docker-compose.yaml` file is already provided in the root of the workshop repository:

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
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

  web:
    build: ./app
    ports:
      - "5000:5000"
    container_name: app_frontend
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=flask_db
      - FLASK_ENV=development
    volumes:
      - ./app:/app
    depends_on:
      db:
        condition: service_healthy
    command: ["flask", "run", "--host=0.0.0.0", "--port=5000"]

volumes:
  app-db:
```

### File Contents Explained:

- **version**: Compose file format version
- **services**: Define your services (db and web)
- **db service**: PostgreSQL database with health check
- **web service**: Flask application that depends on db
- **volumes**: Persistent storage for the database

## Build Using Docker Compose

```bash
docker compose build
```

This command uses the Dockerfile to build a new container image. You might notice that many "layers" were downloaded. This is because we instructed the builder to start from the `python:3.13-alpine` image and other packages.

## Running the Application

From the directory where `docker-compose.yaml` is present, run:

```bash
docker compose up
```

This will bring up both containers. The app will be accessible at `http://localhost:5000`

To run in detached mode (background):

```bash
docker compose up -d
```

## Live Code Reloading

The `docker-compose.yaml` includes a volume mount:
```yaml
volumes:
  - ./app:/app
```

This mounts your local `app` directory into the container at `/app`. Whenever you make changes to the code on your local machine, they will be reflected immediately in the running container.

### Test Live Reload

1. With the application running, open `app/templates/base.html`
2. Find line 63:
   ```html
   <p> <label for="phone">Phone : </label> <input type="number" name="phone" placeholder="Phone"> </p>
   ```
3. Change "Phone" to "Phone Number":
   ```html
   <p> <label for="phone">Phone Number : </label> <input type="number" name="phone" placeholder="Phone"> </p>
   ```
4. Save the file and refresh your browser
5. You should see the updated label immediately

## Stopping the Application

To stop all services:

```bash
docker compose down
```

This stops and removes all containers, networks, and volumes created by `docker compose up`.

## Viewing Logs

To view logs from all services:

```bash
docker compose logs
```

To follow logs in real-time:

```bash
docker compose logs -f
```

To view logs for a specific service:

```bash
docker compose logs web
docker compose logs db
```

## Rebuilding After Changes

If you make changes to the Dockerfile or requirements.txt:

```bash
docker compose up --build
```

Or to rebuild without cache:

```bash
docker compose build --no-cache
docker compose up
```
