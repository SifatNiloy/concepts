# Level 6: Pro - Deployment and Best Practices

Now you're ready for real-world Docker usage!

## Where is Docker Used?

Docker is used EVERYWHERE:
- **Companies**: Google, Amazon, Netflix, Spotify, Uber
- **Websites**: Every major website uses containers
- **Apps**: Your favorite apps probably run on Docker
- **Cloud**: AWS, Google Cloud, Azure all use containers

## Deploying Your App

### Option 1: Deploy to the Cloud

Popular platforms that use Docker:

| Platform | What it is |
|----------|------------|
| **Railway** | Easy deployment, great for beginners |
| **Render** | Simple, free tier available |
| **Vercel** | Made by Next.js team, perfect for Next.js |
| **AWS** | Amazon's cloud, very powerful |
| **Google Cloud** | Google's cloud |
| **DigitalOcean** | Simple and affordable |

### Option 2: Deploy to a VPS (Virtual Private Server)

You can rent a server from:
- DigitalOcean ($4/month)
- Linode ($5/month)
- Hetzner (cheap, in Europe)

Then:
```bash
# On your server
git clone your-repo
cd your-repo
docker compose up -d
```

## Best Practices

### 1. Keep Images Small

```dockerfile
# BAD: Large image
FROM node:18

# GOOD: Small image
FROM node:18-alpine
```

### 2. Use .dockerignore

Always create one to exclude:
```
node_modules
.next
.git
*.log
```

### 3. Don't Run as Root

```dockerfile
FROM node:18-alpine
USER node  # Run as non-root user
```

### 4. Use Specific Versions

```dockerfile
# BAD
FROM node:latest

# GOOD
FROM node:18.17.0-alpine
```

### 5. Multi-Stage Builds

Always use multi-stage builds for production (you learned this in Level 4!)

## Security Tips

```bash
# Scan images for vulnerabilities
docker scan my-app

# Don't put secrets in Dockerfiles
# BAD:
ENV PASSWORD=secret123

# GOOD: Use environment variables at runtime
docker run -e PASSWORD=secret123 my-app
```

## Real-World Example: Complete Next.js Setup

Here's a production-ready setup:

**Dockerfile:**
```dockerfile
# Build stage
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine AS runner
WORKDIR /app
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static
USER nextjs
EXPOSE 3000
ENV NODE_ENV=production
CMD ["node", "server.js"]
```

**docker-compose.yml:**
```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/mydb
      - REDIS_URL=redis://redis:6379
    depends_on:
      - db
      - redis
    restart: unless-stopped

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: unless-stopped

  redis:
    image: redis:7-alpine
    restart: unless-stopped

volumes:
  postgres_data:
```

## Useful Production Commands

```bash
# View resource usage
docker stats

# Set resource limits
docker run -d --memory="512m" --cpus="1.0" my-app

# Auto-restart on failure
docker run -d --restart=always my-app

# Health checks
docker run -d --health-cmd="curl -f http://localhost:3000 || exit 1" my-app
```

## Common Problems and Solutions

| Problem | Solution |
|---------|----------|
| Port already in use | Change the port: `-p 3001:3000` |
| Container stops immediately | Check logs: `docker logs <id>` |
| Can't connect to database | Use service name, not localhost |
| Image too big | Use alpine images, multi-stage builds |
| Changes not showing | Rebuild image: `docker compose build` |

## Level 6 Checklist

- [ ] I know where Docker is used
- [ ] I can deploy to cloud platforms
- [ ] I understand best practices
- [ ] I know security tips
- [ ] I can troubleshoot common problems

## Congratulations!

You've learned Docker from zero to pro! Here's what you can now do:

- Run any app in Docker
- Build your own images
- Dockerize your Next.js apps
- Run multiple containers with Docker Compose
- Deploy to production
- Follow best practices

## Keep Learning

- **Kubernetes**: For running hundreds of containers
- **CI/CD**: Automate building and deploying
- **Monitoring**: Watch your containers in production
- **Networking**: Advanced container networking

## Final Challenge

Deploy your Next.js app to a real server and share it with friends!

---

**You did it!** You're now a Docker developer. Keep practicing and building cool things!
