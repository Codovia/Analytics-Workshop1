# Flask - PostgreSQL Application

## About This App

This is a basic Flask web application with a PostgreSQL database backend. It demonstrates:

- A web form that takes **First Name**, **Last Name**, **Age**, and **Phone Number** as inputs
- Upon clicking "Submit", the data is inserted into the PostgreSQL database
- Data in the database is displayed on top of the page
- Users can delete records by entering the ID

## Tech Stack

- **Flask** - Python web framework
- **PostgreSQL** - Relational database
- **psycopg2** - PostgreSQL adapter for Python
- **Docker** - Containerization platform
- **Docker Compose** - Multi-container orchestration

## File Structure

```
app/
├── app.py              # Flask application code
├── Dockerfile          # Container build instructions
├── requirements.txt    # Python dependencies
└── templates/
    └── base.html       # HTML template for the web interface
```

## Running the Application

### Option 1: Using Docker Compose (Recommended)

From the root of the repository:

```bash
docker compose up
```

The application will be available at `http://localhost:5000`

### Option 2: Using Docker Build

```bash
cd app
docker build -t flask-app .
docker run -p 5000:5000 --env POSTGRES_USER=postgres --env POSTGRES_PASSWORD=postgres --env POSTGRES_DB=flask_db flask-app
```

### Option 3: Local Development (requires Python and PostgreSQL)

1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

2. Set up PostgreSQL database:
   ```bash
   createdb flask_db
   ```

3. Set environment variables:
   ```bash
   export POSTGRES_USER=postgres
   export POSTGRES_PASSWORD=postgres
   export POSTGRES_DB=flask_db
   ```

4. Run the application:
   ```bash
   flask run
   ```

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `POSTGRES_USER` | postgres | Database user |
| `POSTGRES_PASSWORD` | postgres | Database password |
| `POSTGRES_DB` | flask_db | Database name |
| `FLASK_ENV` | development | Flask environment |
| `FLASK_RUN_HOST` | 0.0.0.0 | Host to bind to |
| `FLASK_RUN_PORT` | 5000 | Port to bind to |

## Database Schema

**Table: students**

| Column | Type | Description |
|--------|------|-------------|
| id | serial | Primary key, auto-increment |
| fname | varchar(30) | First name (required) |
| lname | varchar(30) | Last name (optional) |
| age | integer | Age (required) |
| phone | varchar(10) | Phone number (required) |

## Features

- ✅ Add new student records
- ✅ View all student records
- ✅ Delete student records by ID
- ✅ Persistent data storage (PostgreSQL)
- ✅ Responsive web interface
- ✅ Live code reloading with Docker Compose

## Troubleshooting

### Database Connection Errors

If you see connection errors:
1. Ensure PostgreSQL container is running: `docker ps`
2. Check environment variables are set correctly
3. Verify database exists: check logs with `docker compose logs db`

### Port Already in Use

If port 5000 is already in use:
- Either stop the conflicting service
- Or change the port mapping in `docker-compose.yaml`

## License

See [LICENSE](../LICENSE) file in the repository root.
