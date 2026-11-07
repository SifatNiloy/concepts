# Level 6: Pro - Continuous Deployment (CD) to a Live Server

Welcome to the ultimate goal of CI/CD: **Continuous Deployment**.

When you type:
```powershell
git commit -m "Add new landing page"
git push origin main
```
You want your live production server (whether a \$0 Oracle Cloud instance or a VPS) to automatically pull the new Docker container and restart it with **zero manual typing** on the server!

---

## 🏗️ The End-to-End CD Architecture

```
[Developer Laptop]
       │
       ▼ (git push)
[GitHub Repository]
       │
       ▼ (Triggers GitHub Actions)
[GitHub Runner (Ubuntu)]
       │
       ├─▶ 1. Run Tests & Linter
       ├─▶ 2. Build Docker Image
       ├─▶ 3. Push to GHCR (ghcr.io)
       │
       ▼ (SSH into production server)
[Live Server / VPS]
       │
       ├─▶ 1. Pull new image from GHCR
       ├─▶ 2. docker compose up -d --remove-orphans
       └─▶ 3. Live website updated in seconds! 🚀
```

---

## 🔑 How GitHub Connects to Your Server Securely (SSH)

To deploy to a server, GitHub Actions needs an **SSH Key**:
1. You generate an SSH key pair (`ssh-keygen`).
2. You place the **Public Key** on your server (`~/.ssh/authorized_keys`).
3. You save the **Private Key** into **GitHub Secrets** as `SERVER_SSH_KEY`.
4. You save the server IP as `SERVER_HOST` (e.g. `123.45.67.89`).
5. You save the username as `SERVER_USER` (e.g. `ubuntu`).

---

## 📜 The Complete Production CI/CD Workflow (`deploy.yml`)

Here is the exact real-world workflow file used by software engineers:

```yaml
name: Deploy Production

on:
  push:
    branches: [ main ]

jobs:
  # -------------------------------------------------------------
  # STAGE 1: TEST & BUILD
  # -------------------------------------------------------------
  build-and-push:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to GitHub Container Registry
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build and Push Docker Image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ghcr.io/${{ github.repository }}:latest
          cache-from: type=gha
          cache-to: type=gha,mode=max

  # -------------------------------------------------------------
  # STAGE 2: DEPLOY TO SERVER (Runs only if Stage 1 succeeds!)
  # -------------------------------------------------------------
  deploy:
    needs: build-and-push
    runs-on: ubuntu-latest

    steps:
      - name: Deploy via SSH
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SERVER_SSH_KEY }}
          script: |
            # 1. Log in to GHCR on the server
            echo "${{ secrets.GITHUB_TOKEN }}" | docker login ghcr.io -u ${{ github.actor }} --password-stdin

            # 2. Pull the latest image
            docker pull ghcr.io/${{ github.repository }}:latest

            # 3. Restart the container using Docker Compose
            cd /app/my-nextjs-docker-app
            docker compose up -d --force-recreate
```

---

## 🏆 You Have Mastered CI/CD!

With this setup:
* You never have to manually log in to servers to deploy updates.
* If your code contains a syntax error, CI prevents broken code from ever being deployed.
* The entire process takes less than 2 minutes from `git push` to live production.

---

## 🗺️ Next Steps in the Master Path

Now that you know how automated deployment connects to a server, you need to understand the server itself!
