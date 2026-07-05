# 🚀 PostgreSQL Setup with Django (via Docker)

This guide explains how to set up PostgreSQL with Django using Docker.  
We’ll use descriptive container names like `db_local` (for development) and `db_prod` (for production) so that managing multiple databases is easy.

---

## 1. Install Docker

### Ubuntu / Debian
```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable --now docker
```

### macOS / Windows
- Download and install **Docker Desktop**:  
  👉 [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

---

## 2. Run PostgreSQL in Docker

Create and run a container for PostgreSQL:

```bash
docker run --name db_local \
  -e POSTGRES_USER=myuser \
  -e POSTGRES_PASSWORD=mypassword \
  -e POSTGRES_DB=mydatabase \
  -p 5432:5432 \
  -d postgres:16
```

when in production you might want to add auto-restart to always keep
db running

```bash
docker run --name db_prod \
  -e POSTGRES_USER=myuser \
  -e POSTGRES_PASSWORD=mypassword \
  -e POSTGRES_DB=mydatabase \
  -p 5432:5432 \
  --restart unless-stopped \
  -d postgres:16
```

### 🔹 Why use container names?
- `db_local` for development
- `db_prod` for production  
This naming makes it simple to manage and identify containers when running commands like `docker ps`, `docker start`, and `docker stop`.

---

## 3. Configure Django

Update your `settings.py`:

```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "NAME": "mydatabase",      # same as POSTGRES_DB
        "USER": "myuser",          # same as POSTGRES_USER
        "PASSWORD": "mypassword",  # same as POSTGRES_PASSWORD
        "HOST": "localhost",       # due to -p 5432:5432
        "PORT": "5432",
    }
}
```

Install the PostgreSQL driver for Django:

```bash
pip install psycopg2-binary
```

---

## 4. Apply Migrations

Run Django migrations to create tables in PostgreSQL:

```bash
python manage.py migrate
```

---

## 5. Managing the Database Container

### Stop the container
```bash
docker stop db_local
```

### Start the container again
```bash
docker start db_local
```

### Restart the container
```bash
docker restart db_local
```

---

✅ That’s it! You now have a PostgreSQL database running in Docker, connected to Django, with clear container naming to distinguish between local and production environments.
