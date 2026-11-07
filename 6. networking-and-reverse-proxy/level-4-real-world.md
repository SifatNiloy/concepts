# Level 4: Caddy — 100% Automatic Free SSL Certificates

While Nginx is great, configuring SSL certificates (Let's Encrypt / Certbot) with Nginx requires 40 lines of config and renewal cron jobs.

Meet **Caddy**: The modern, blazing-fast web server written in Go that does **automatic SSL out of the box with zero configuration**!

---

## ⚡ The Miracle of Caddy

With Caddy, getting HTTPS on a real domain requires just **3 lines** in a file called `Caddyfile`:

```text
mysite.com {
    reverse_proxy app:3000
}
```

That is literally it! Caddy will automatically:
1. Contact Let's Encrypt / ZeroSSL.
2. Prove you own the domain.
3. Issue a free SSL certificate.
4. Enable HTTPS on port 443 with modern HTTP/2 and HTTP/3.
5. Auto-renew the certificate every 60 days forever!

---

## 🐳 Running Caddy with Docker Compose

Here is how you set up Caddy in Docker:

### 1. Create a `Caddyfile`:
```text
# For local development testing:
localhost {
    reverse_proxy app:3000
}

# In production on a real server, just replace localhost with your domain:
# sifat.dev {
#     reverse_proxy app:3000
# }
```

### 2. Update `docker-compose.yml`:
```yaml
services:
  caddy:
    image: caddy:2-alpine
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - caddy_data:/data       # Crucial: Saves your SSL certificates!
      - caddy_config:/config
    depends_on:
      - app

  app:
    build: .
    expose:
      - "3000"

volumes:
  caddy_data:
  caddy_config:
```

> ⚠️ **Important:** The `caddy_data` volume is essential because it stores your SSL certificates so Caddy doesn't re-request new certificates every time you restart the container (which could hit Let's Encrypt rate limits).

---

## ✅ Level 4 Checklist

- [ ] I understand why Caddy is preferred by modern developers for automated SSL.
- [ ] I can write a 3-line `Caddyfile`.
- [ ] I know why the `caddy_data` volume is necessary to persist SSL certificates.
