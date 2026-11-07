# Level 1: What is Docker? Why Do People Use It?

## The Problem Docker Solves

Imagine this: You built a cool Next.js app on YOUR computer. It works perfectly! But when you send it to your friend, it doesn't work on THEIR computer. Why?

Maybe:
- Your computer has Node.js version 18, theirs has version 16
- Your computer has a database installed, theirs doesn't
- Your computer is Windows, theirs is Mac
- Some setting is different

This is called the **"It works on my machine"** problem. Every programmer has faced this!

## The Solution: Docker

Docker is like a **magic box** that contains EVERYTHING your app needs to run:
- The exact version of Node.js
- All the libraries your app uses
- The operating system settings
- Everything!

You can send this magic box to anyone, and your app will work EXACTLY the same on their computer.

## Real World Analogy

Think of Docker like a **lunch box**:
- Your app is the food
- The lunch box keeps everything together
- Anyone can open the lunch box and eat the same food
- It doesn't matter whose kitchen they're in

Or think of it like a **shipping container**:
- Before shipping containers, loading ships was chaos
- Containers made everything standard size
- Now anything fits on any ship, truck, or train
- Docker does the same for software!

## Key Words to Know

| Word | What it means |
|------|---------------|
| **Image** | A blueprint/template for your app (like a cookie cutter) |
| **Container** | A running instance of an image (like a cookie made from the cutter) |
| **Dockerfile** | A recipe that tells Docker how to build your image |
| **Docker Hub** | A website where people share their images (like GitHub for Docker) |

## Let's Check if Docker is Installed

Open your terminal and type:

```bash
docker --version
```

If you see something like `Docker version 24.0.7`, you're good!

If you get "command not found", you need to install Docker:
- **Windows**: Download Docker Desktop from https://www.docker.com/products/docker-desktop
- **Mac**: Download Docker Desktop from https://www.docker.com/products/docker-desktop
- **Linux**: Follow instructions at https://docs.docker.com/engine/install/

## Your First Docker Command

Let's run something simple:

```bash
docker run hello-world
```

What happens?
1. Docker looks for an image called "hello-world" on your computer
2. It doesn't find it, so it downloads it from Docker Hub
3. It creates a container from that image
4. The container runs and prints a welcome message

Type the command now and see what happens!

## What Just Happened?

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
...
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

- `Unable to find image locally` - You didn't have it yet
- `Pulling from library/hello-world` - Docker downloaded it
- `Hello from Docker!` - Your first container ran successfully!

## Try These Commands

```bash
# See all images on your computer
docker images

# See all running containers
docker ps

# See ALL containers (including stopped ones)
docker ps -a
```

## Level 1 Checklist

- [ ] I understand what Docker is (a magic box for apps)
- [ ] I understand why people use it (solves "works on my machine" problem)
- [ ] I know what an image is (a blueprint)
- [ ] I know what a container is (a running app)
- [ ] I have Docker installed
- [ ] I ran my first container (hello-world)
- [ ] I can list images and containers

## Next Step

When you're ready, open `level-2-basics.md` to learn how to run real applications!

---

**Remember**: Don't rush. Make sure you understand each level before moving on. Docker is a powerful tool, and learning it well will make you a much better programmer!
