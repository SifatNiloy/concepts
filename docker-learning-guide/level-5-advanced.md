# Level 5: Docker Compose - Running Multiple Containers

Real apps often need more than one container. For example:
- Your Next.js app
- A database (like PostgreSQL)
- A cache (like Redis)

Docker Compose lets you run ALL of them with ONE command!

## What is Docker Compose?

Instead of running:
```bash
docker run -d postgres
docker run -d redis
docker run -d -e DATABASE_URL=postgres://... my-nextjs-app
```

You write a `docker-compose.yml` file and run:
```bash
docker compose up
```

That's it! All containers start together.

## Create a Docker Compose File

In your Next.js project, create `docker-compose.yml`:

```yaml
version: '3.8'

services:
  # Your Next.js app
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/mydb
    depends_on:
      - db

  # PostgreSQL database
  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

# Volumes persist data even when containers stop
volumes:
  postgres_data:
```

## Understanding the File

```yaml
services:
  app:                    # Name of the service
    build: .             # Build from Dockerfile in current folder
    ports:               # Port mapping
      - "3000:3000"
    environment:         # Environment variables
      - DATABASE_URL=...
    depends_on:          # Start db BEFORE app
      - db

  db:                     # Another service
    image: postgres:15-alpine  # Use existing image
    volumes:            # Persistent storage
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:          # Define the volume
```

## Run Everything

```bash
# Start all containers
docker compose up

# Start in background
docker compose up -d

# See logs
docker compose logs

# Stop everything
docker compose down

# Stop and delete volumes (WARNING: deletes data!)
docker compose down -v
```

## Useful Docker Compose Commands

```bash
# Rebuild images
docker compose build

# Restart a service
docker compose restart app

# See running containers
docker compose ps

# Execute command in a container
docker compose exec db psql -U user -d mydb

# Scale a service (run multiple instances)
docker compose up --scale app=3
```

## Try This Challenge

1. Add Redis to your docker-compose.yml
2. Start everything
3. Check that all containers are running

```yaml
# Add to services:
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
```

```bash
docker compose up -d
docker compose ps
```

## Level 5 Checklist

- [ ] I understand what Docker Compose is
- [ ] I can write a docker-compose.yml file
- [ ] I can run multiple containers together
- [ ] I understand volumes for persistent data
- [ ] I can use depends_on to control startup order

## Next Step

Ready for Level 6? Open `level-6-pro.md` to learn deployment and best practices!
