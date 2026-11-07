# Level 4: Building & Pushing Docker Images with GitHub Actions

This is where your **Docker knowledge** and **CI/CD knowledge** unite!

Instead of manually running:
```powershell
docker build -t my-app .
docker push my-app
```
We will configure GitHub to build your multi-stage `Dockerfile` and publish the image to a container registry automatically whenever you push to `main`.

---

## 📦 What is a Container Registry?

Just like GitHub is a registry for your **source code**, a Container Registry is a cloud warehouse for your **built Docker images**:

1. **Docker Hub** (`docker.io`): The most famous public/private registry.
2. **GHCR (GitHub Container Registry)** (`ghcr.io`): Built right inside GitHub, free, fast, and no extra passwords needed!

We will use **GHCR** because you already have a GitHub account and need zero third-party setups!

---

## 🔒 GitHub Secrets: Never Hardcode Passwords!

Before pushing anything to a registry, the runner needs permissions.  
GitHub provides a built-in magic secret called:
```yaml
${{ secrets.GITHUB_TOKEN }}
```
This is a temporary security token automatically created for every job and destroyed when the job finishes. You don't need to configure any passwords!

---

## 🛠️ The Real-World Workflow: Build & Push to GHCR

Create `.github/workflows/docker-publish.yml`:

```yaml
name: Build & Push Docker Image

on:
  push:
    branches: [ main ]
    tags: [ 'v*.*.*' ] # Also triggers when you make a version tag like v1.0.0

# Grant permissions to write packages to GitHub Container Registry
permissions:
  contents: read
  packages: write

jobs:
  docker-build-and-push:
    runs-on: ubuntu-latest

    steps:
      - name: 1. Checkout repository
        uses: actions/checkout@v4

      # Sets up Docker Buildx (extended build capabilities)
      - name: 2. Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      # Log in to GitHub Container Registry using the automatic GITHUB_TOKEN
      - name: 3. Log in to GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      # Automatically generate tags (e.g. :latest, :sha-12345, :v1.0.0)
      - name: 4. Extract Docker Metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ghcr.io/${{ github.repository }}
          tags: |
            type=raw,value=latest
            type=sha,format=short

      # Build and push using multi-layer caching (ultra fast!)
      - name: 5. Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          file: ./Dockerfile
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
          cache-from: type=gha # Use GitHub Actions cache for Docker layers!
          cache-to: type=gha,mode=max
```

---

## 🌟 What Happens When This Runs?

1. You push a commit to `main`.
2. GitHub starts a runner, sets up Docker Buildx.
3. It builds your Next.js multi-stage `Dockerfile`.
4. It pushes the completed image to:
   `ghcr.io/<your-username>/my-nextjs-docker-app:latest`
5. You can now pull and run that image on **any server in the world** with:
   ```bash
   docker run -d -p 3000:3000 ghcr.io/<your-username>/my-nextjs-docker-app:latest
   ```

---

## ✅ Level 4 Checklist

- [ ] I understand what a Container Registry (Docker Hub, GHCR) is.
- [ ] I understand what `${{ secrets.GITHUB_TOKEN }}` is and why we never hardcode tokens.
- [ ] I know how `docker/build-push-action` automates the build and push steps.
- [ ] I know where to find my published package in my GitHub profile under "Packages".
