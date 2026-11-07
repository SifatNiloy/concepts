# Level 3: Setting Up Nginx with Docker Compose

Nginx is the battle-tested king of web servers, powering over a third of the entire web.

---

## 🛠️ The Architecture

We will run **Nginx in its own Docker container**.  
* Nginx listens on port `80`.
* When traffic hits port `80`, Nginx proxies (forwards) the request internally to your `app` container at port `3000`.

---

## 📝 Step 1: The Nginx Configuration File (`nginx.conf`)

Create a file called `nginx.conf`:

```nginx
events {
    worker_connections 1024;
}

http {
    # Define an upstream server (points to our Docker service name!)
    upstream nextjs_app {
        server app:3000;
    }

    server {
        listen 80;
        server_name localhost;

        location / {
            # Forward all requests to our Next.js container:
            proxy_pass http://nextjs_app;
            
            # Pass client headers so Next.js knows the visitor's real IP:
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

---

## 🐳 Step 2: The `docker-compose.yml`

Notice how your `app` container **no longer needs port 3000 exposed to the host**! Only Nginx opens port 80:

```yaml
services:
  # 1. Reverse Proxy (The Front Door)
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - app

  # 2. Next.js App (Hidden in internal Docker network!)
  app:
    build: .
    # Notice: NO ports: section! Nginx talks to it internally at app:3000
    expose:
      - "3000"
```

---

## 🚀 Run and Test

```powershell
docker compose up -d
```

Now open:

Nginx intercepted port 80 and routed your request directly to your Next.js app!

---

## ✅ Level 3 Checklist

- [ ] I understand `proxy_pass http://app:3000;`.
- [ ] I know why we only expose port 80 on Nginx and keep `app` hidden internally.
- [ ] I can mount `nginx.conf` into an Nginx container using volumes.
