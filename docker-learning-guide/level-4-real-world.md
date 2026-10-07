# Level 4: Dockerizing Your Next.js App

This is where it gets exciting! Let's put your Next.js app inside Docker.

## Create a Simple Next.js App

If you don't have one, let's create a simple one:

```bash
npx create-next-app@latest my-nextjs-docker-app
cd my-nextjs-docker-app
```

Choose these options:
- TypeScript: No (keep it simple)
- ESLint: Yes
- Tailwind: No
- src directory: No
- App Router: Yes
- import alias: No

## Create a Dockerfile for Next.js

In your project root, create a file called `Dockerfile`:

```dockerfile
# Use Node.js 18
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy all files
COPY . .

# Build the Next.js app
RUN npm run build

# Open port 3000
EXPOSE 3000

# Start the app
CMD ["npm", "start"]
```

## Create a .dockerignore File

This tells Docker what NOT to copy (makes builds faster):

```
node_modules
.next
.git
*.md
.env*
```

## Build and Run

```bash
# Build the image
docker build -t my-nextjs-app .

# Run it
docker run -d -p 3000:3000 my-nextjs-app
```

Wait a minute... then open `http://localhost:3000`

Your Next.js app is now running inside Docker!

## Better Dockerfile (Multi-Stage Build)

The Dockerfile above works, but it's not optimal. Here's a better version:

```dockerfile
# Stage 1: Build the app
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Stage 2: Run the app (smaller image!)
FROM node:18-alpine AS runner
WORKDIR /app

# Copy only what we need from builder
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

EXPOSE 3000
CMD ["node", "server.js"]
```

This is called a **multi-stage build**. It makes your final image much smaller because it doesn't include all the build tools.

## Environment Variables

Your app might need environment variables (like API keys):

```bash
docker run -d -p 3000:3000 -e DATABASE_URL=my-database-url my-nextjs-app
```

Or use a file:

```bash
# Create .env file
echo "DATABASE_URL=my-database-url" > .env

# Run with env file
docker run -d -p 3000:3000 --env-file .env my-nextjs-app
```

## Useful Commands for Next.js + Docker

```bash
# See logs from your app
docker logs <container-id>

# Follow logs in real-time
docker logs -f <container-id>

# Execute a command inside the running container
docker exec -it <container-id> sh

# See resource usage
docker stats
```

## Try This Challenge

1. Add a new page to your Next.js app
2. Rebuild the Docker image
3. Run it and see your new page

```bash
# Create a new page
echo '<h1>Hello from Docker!</h1>' > app/hello/page.tsx

# Rebuild
docker build -t my-nextjs-app:v2 .

# Run
docker run -d -p 3000:3000 my-nextjs-app:v2

# Visit http://localhost:3000/hello
```

## Level 4 Checklist

- [ ] I can create a Dockerfile for Next.js
- [ ] I understand .dockerignore
- [ ] I can build and run a Next.js app in Docker
- [ ] I understand multi-stage builds
- [ ] I can pass environment variables

## Next Step

Ready for Level 5? Open `level-5-advanced.md` to learn Docker Compose and more!
