# Level 2: Running Your First Real Containers

Now let's run actual applications in Docker!

## Running a Website in Docker

Let's run a simple website (nginx - a web server):

```bash
docker run nginx
```

Wait... it seems like it's hanging. That's because nginx runs forever (it's a web server!).

Open a NEW terminal window and try:

```bash
docker ps
```

You'll see nginx running! The container is active.

Now let's stop it:

```bash
docker stop <container-id>
```

You can find the container ID from the `docker ps` output.

## Better Way: Run in Background

```bash
docker run -d nginx
```

The `-d` means "detached" - it runs in the background so you can keep using your terminal.

Now try:

```bash
docker ps
```

You'll see it running. But how do we see the website?

## Port Mapping

Containers are like separate computers. They have their own "doors" (ports). We need to connect your computer's door to the container's door:

```bash
docker run -d -p 8080:80 nginx
```

This means: "Take port 8080 on MY computer and connect it to port 80 inside the container"

Now open your browser and go to: `http://localhost:8080`

You'll see the nginx welcome page!

## Useful Commands to Try

```bash
# See logs from a container
docker logs <container-id>

# See resource usage
docker stats

# Stop a container
docker stop <container-id>

# Start a stopped container
docker start <container-id>

# Remove a container
docker rm <container-id>

# Remove an image
docker rmi nginx
```

## Run a Fun Example

Let's run a game in Docker!

```bash
docker run -d -p 8080:8080 docker/getting-started
```

Wait a few seconds, then open `http://localhost:8080` in your browser.

## Understanding What Happened

When you run `docker run -d -p 8080:80 nginx`:

1. Docker checks if you have the nginx image
2. If not, it downloads it from Docker Hub
3. It creates a container from that image
4. It maps port 8080 on your computer to port 80 in the container
5. It starts the container in the background (-d)

## Common Options for docker run

| Option | What it does | Example |
|--------|--------------|---------|
| `-d` | Run in background | `docker run -d nginx` |
| `-p` | Map a port | `docker run -p 8080:80 nginx` |
| `--name` | Give it a name | `docker run --name my-nginx nginx` |
| `-e` | Set environment variable | `docker run -e KEY=value nginx` |
| `-v` | Mount a folder | `docker -v /host:/container nginx` |
| `--rm` | Delete when stopped | `docker run --rm nginx` |

## Try This Challenge

1. Run nginx on port 9090
2. Visit it in your browser
3. Check the logs
4. Stop it
5. Remove it

```bash
# Step 1
docker run -d -p 9090:80 nginx

# Step 2 - open http://localhost:9090 in browser

# Step 3
docker logs <container-id>

# Step 4
docker stop <container-id>

# Step 5
docker rm <container-id>
```

## Level 2 Checklist

- [ ] I can run a container in the background
- [ ] I understand port mapping
- [ ] I can see container logs
- [ ] I can stop and remove containers
- [ ] I ran a web server in Docker

## Next Step

Ready for Level 3? Open `level-3-intermediate.md` to learn how to build your own Docker images!
