# Level 3: Building Your Own Docker Images

Now we learn how to create our own Docker images using a **Dockerfile**.

## What is a Dockerfile?

A Dockerfile is a **recipe** that tells Docker:
- What base image to use
- What files to copy
- What commands to run
- What port to open
- What command to start the app

## Let's Build a Simple Image

Create a new folder for this lesson:

```bash
mkdir my-first-docker-image
cd my-first-docker-image
```

Create a file called `Dockerfile` (no extension!):

```dockerfile
# Use Node.js as the base image
FROM node:18-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy package.json first (for caching)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy all other files
COPY . .

# Open port 3000
EXPOSE 3000

# Start the app
CMD ["npm", "start"]
```

Now create a simple `package.json`:

```json
{
  "name": "my-first-docker-image",
  "version": "1.0.0",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.0"
  }
}
```

Create a simple `server.js`:

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello from Docker! I am running inside a container!');
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

## Build the Image

```bash
docker build -t my-first-app .
```

- `docker build` - Build an image
- `-t my-first-app` - Tag it with a name
- `.` - Look for Dockerfile in current directory

This will take a while the first time. Docker is downloading Node.js and installing your dependencies!

## Run Your Image

```bash
docker run -d -p 3000:3000 my-first-app
```

Now open `http://localhost:3000` in your browser!

You should see: "Hello from Docker! I am running inside a container!"

## Understanding the Dockerfile

Let's break down each line:

```dockerfile
FROM node:18-alpine
```
- Start with Node.js version 18
- "alpine" means a small, lightweight version

```dockerfile
WORKDIR /app
```
- Inside the container, go to the /app folder
- If it doesn't exist, create it

```dockerfile
COPY package*.json ./
```
- Copy package.json and package-lock.json
- The `./` means "to the working directory (/app)"

```dockerfile
RUN npm install
```
- Run a command inside the container
- This installs your dependencies

```dockerfile
COPY . .
```
- Copy everything else from your folder to the container

```dockerfile
EXPOSE 3000
```
- Tell Docker this app uses port 3000
- (This is just documentation, doesn't actually open the port)

```dockerfile
CMD ["npm", "start"]
```
- The command to run when the container starts
- This is what keeps the container alive

## Useful Build Commands

```bash
# List all images
docker images

# Remove an image
docker rmi my-first-app

# Build with a specific tag
docker build -t my-first-app:v1 .

# Build without cache (force rebuild)
docker build --no-cache -t my-first-app .
```

## Try This Challenge

1. Change the message in server.js
2. Rebuild the image
3. Run it again
4. See the new message

```bash
# Edit server.js, then:
docker build -t my-first-app:v2 .
docker run -d -p 3001:3000 my-first-app:v2
```

## Level 3 Checklist

- [ ] I understand what a Dockerfile is
- [ ] I can write a basic Dockerfile
- [ ] I can build an image from a Dockerfile
- [ ] I can run my own image
- [ ] I understand FROM, WORKDIR, COPY, RUN, EXPOSE, CMD

## Next Step

Ready for Level 4? Open `level-4-real-world.md` to learn how to Dockerize your Next.js app!
