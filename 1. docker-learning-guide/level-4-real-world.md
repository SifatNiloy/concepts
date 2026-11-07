# Level 4: Dockerizing Your Next.js App (Beginner Friendly Guide)

Welcome to Level 4! Up until now, we ran simple Express servers. Now let's tackle a **real-world production framework: Next.js**.

---

## 🧭 The Big Picture: What Are We Doing?

A Next.js app needs to:
1. Install dependencies (`npm install`).
2. Build the production bundle (`npm run build`).
3. Start the server (`npm run start`).

We are going to put all 3 steps inside a Docker recipe so anyone can run your Next.js app without needing Node.js installed on their computer!

```
[Your Computer] ──(docker build)──> [Next.js Docker Image] ──(docker run)──> [Live Website at :3000]
```

---

## Step 1: Create a Simple Next.js App (Or use an existing one)

If you already have a Next.js app, navigate inside it. If not, create one:

```powershell
npx create-next-app@latest my-nextjs-docker-app
```

When prompted in the terminal, you can choose:
* **TypeScript**: No (keep it simple for now)
* **ESLint**: Yes
* **Tailwind CSS**: No
* **src/ directory**: No
* **App Router**: Yes
* **Customize default import alias**: No

Now move into that project folder:
```powershell
cd my-nextjs-docker-app
```

---

## Step 2: Create `.dockerignore` (Crucial!)

Before creating the Dockerfile, create a file named `.dockerignore` in the project root.

> 💡 **What is `.dockerignore`?**  
> Just like `.gitignore` tells Git what files to ignore, `.dockerignore` tells Docker:  
> *"Do NOT copy these folders into the container."*  
> If you don't do this, Docker will copy your huge local `node_modules` folder, making the build super slow!

Create a file named `.dockerignore` (no extension):

```text
node_modules
.next
.git
*.md
.env*
```

---

## Step 3: Create the `Dockerfile`

In the root of your `my-nextjs-docker-app` folder, create a file named `Dockerfile` (capital `D`, no extension):

```dockerfile
# 1. Base Image: Use official Node 18 running on Alpine Linux (super lightweight!)
FROM node:18-alpine

# 2. Set the folder inside the container where our app will live
WORKDIR /app

# 3. Copy package files first (to take advantage of Docker layer caching)
COPY package*.json ./

# 4. Install all npm dependencies inside the container
RUN npm install

# 5. Copy the rest of our project files into the container
COPY . .

# 6. Build the Next.js production files (.next folder)
RUN npm run build

# 7. Document that our container listens on port 3000
EXPOSE 3000

# 8. Command that runs when the container starts up
CMD ["npm", "start"]
```

---

## Step 4: Build and Run Your Next.js Container

### 1. Build the image:
```powershell
docker build -t my-nextjs-app .
```
*(⚠️ Don't forget the space and dot ` .` at the end! It tells Docker to look in the current folder)*

> ⏳ *Note: The first build might take 1–3 minutes because Next.js builds its production pages inside the container.*

### 2. Run the container:
```powershell
docker run -d -p 3000:3000 --name my-nextjs-site my-nextjs-app
```

* `-d`: Run in background (detached mode) so your terminal stays free.
* `-p 3000:3000`: Map your computer's port `3000` to the container's port `3000`.
* `--name my-nextjs-site`: Give your container a friendly name.
* `my-nextjs-app`: The image blueprint to run.

### 3. Open your browser:

Your Next.js site is running 100% inside a Docker container! 🎉

---

## Step 5: How to Manage Your Container

Here are the most common commands you will use:

```powershell
# See your running container
docker ps

# View the real-time logs (see errors or console logs)
docker logs -f my-nextjs-site

# Stop the container
docker stop my-nextjs-site

# Start it again
docker start my-nextjs-site

# Delete the container when done
docker rm -f my-nextjs-site
```

---

## Step 6: Advanced - What is a "Multi-Stage Build"?

If you check the size of the image you just built with `docker images`, you might notice it's around **1 GB**! 

Why is it so big? Because it has:
* Node.js build tools
* Dev dependencies
* Source code + build artifacts

### 🍰 The Kitchen Analogy:
* **The Kitchen (Stage 1 - Builder):** You need big mixing bowls, spatulas, oven, and messy ingredients to bake a cake.
* **The Dining Table (Stage 2 - Runner):** When serving the cake to customers, you only bring the finished cake and a plate. You don't bring the oven and messy bowls to their table!

A **Multi-Stage Build** lets you use one image to build the app, and copy **only the final compiled files** to a fresh, clean, tiny image.

### How to use Multi-Stage in Next.js:

1. In your `next.config.js` (or `next.config.mjs`), add `output: 'standalone'`:
   ```javascript
   /** @type {import('next').NextConfig} */
   const nextConfig = {
     output: 'standalone',
   };

   module.exports = nextConfig;
   ```

2. Replace your `Dockerfile` with this multi-stage version:

```dockerfile
# -----------------------------
# STAGE 1: The Builder (Kitchen)
# -----------------------------
FROM node:18-alpine AS builder
WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

# -----------------------------
# STAGE 2: The Runner (Dining Table)
# -----------------------------
FROM node:18-alpine AS runner
WORKDIR /app

ENV NODE_ENV=production

# Copy ONLY the compiled standalone server and static assets from the builder stage
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

EXPOSE 3000
ENV PORT=3000

# Run Next.js directly using Node (no npm run start needed!)
CMD ["node", "server.js"]
```

> 🚀 **Result:** Your image size drops from **~1 GB** down to **~150 MB**!

---

## Passing Environment Variables

Next.js apps often need secrets (like API keys or Database URLs):

### Option A: Pass directly in the command
```powershell
docker run -d -p 3000:3000 -e DATABASE_URL="postgres://..." --name my-nextjs-site my-nextjs-app
```

### Option B: Use an `.env` file
```powershell
docker run -d -p 3000:3000 --env-file .env.local --name my-nextjs-site my-nextjs-app
```

---

## 🎯 Level 4 Practice Challenge

1. Open `app/page.js` in your editor and change the headline to:
   ```javascript
   <h1>Hello, Docker World! Built by Sifat.</h1>
   ```
2. Rebuild the image:
   ```powershell
   docker build -t my-nextjs-app .
   ```
3. Stop and remove the previous container:
   ```powershell
   docker rm -f my-nextjs-site
   ```
4. Run the new container:
   ```powershell
   docker run -d -p 3000:3000 --name my-nextjs-site my-nextjs-app
   ```
5. Refresh `http://localhost:3000` to see your change!

---

## Level 4 Checklist

- [ ] I understand what `.dockerignore` does and why `node_modules` shouldn't be copied.
- [ ] I built a Next.js Docker image with `docker build -t ... .`
- [ ] I ran the container with port mapping `-p 3000:3000`.
- [ ] I understand why a multi-stage build makes images much smaller (Kitchen vs Dining Table analogy).
- [ ] I know how to check logs using `docker logs -f <name>`.
