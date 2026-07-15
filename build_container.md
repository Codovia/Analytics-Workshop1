# Build the App's Container Image

In order to build the application, we need to use a Dockerfile. A Dockerfile is a text-based script of instructions that is used to create a container image.

## 1. Get the App from Git

```bash
https://github.com/UniCourt/Analytics-Workshop1
```

Clone this repo or download the zip file. Unzip the file and cd into the app folder.

## 2. Dockerfile Overview

The Dockerfile is already provided in the workshop folder. Here's what it contains:

```dockerfile
FROM python:3.13-alpine

ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /app

COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt

COPY . /app

EXPOSE 5000

CMD ["flask", "run", "--host=0.0.0.0", "--port=5000"]
```

### Dockerfile Contents Explained:

a. **Get the base image**: `FROM python:3.13-alpine`
   - This pulls the official Python 3.13 image based on Alpine Linux for a minimal size

b. **Environment variables**: Set Flask configuration
   - `FLASK_APP`: Specifies the application entry point
   - `FLASK_RUN_HOST`: Sets the host to listen on all interfaces

c. **Set working directory**: `WORKDIR /app`
   - Sets the working directory for subsequent instructions

d. **Copy and install dependencies**: Better layer caching
   - Copy `requirements.txt` first for better Docker layer caching
   - Install Python dependencies

e. **Copy application code**: `COPY . /app`
   - Copy the rest of the application code

f. **Expose port**: `EXPOSE 5000`
   - Documents that the container listens on port 5000

g. **Default command**: `CMD ["flask", "run"...]`
   - Command to run when the container starts

## 3. Requirements File

The `requirements.txt` file contains:

```
Flask==3.0.3
psycopg2-binary==2.9.9
```

This installs Flask (web framework) and psycopg2-binary (PostgreSQL adapter).

## 4. Build the Container Image

Open a terminal, go to the app directory with the Dockerfile, and build the container image:

```bash
cd app
docker build -t flask-app .
```

The `-t` flag tags the image with a name (`flask-app`).

## 5. Start the Container

Start your container using the docker run command:

```bash
docker run -p 5000:5000 flask-app
```

## Note: Expected Error

Do not panic if you see an error similar to:

```
psycopg2.OperationalError: could not connect to server: Connection refused
```

or

```
NameError: name 'db' is not defined
```

This is expected because we haven't set up the database yet. The application needs PostgreSQL to connect to, which we'll set up in the Docker Compose section.

## 6. View the Running Container

To see running containers:

```bash
docker ps
```

This will list all running containers.

## 7. Stop the Running Container

To stop the container:

```bash
docker stop <container_id>
```

Replace `<container_id>` with the actual container ID you see from `docker ps`.
