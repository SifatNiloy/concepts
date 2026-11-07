# Level 5: Multi-Domain Routing & Docker Internal Networks

What if you have **3 different websites** running on one single \$5 VPS?

* `mysite.com` $\rightarrow$ Your Next.js portfolio (`app1`)
* `api.mysite.com` $\rightarrow$ Your Node backend API (`app2`)
* `analytics.mysite.com` $\rightarrow$ Your Umami or Plausible analytics (`app3`)

You don't buy 3 servers! You run all 3 on **one server** and let your Reverse Proxy route them by domain name!

---

## 🎯 Multi-Domain Routing in `Caddyfile`

Caddy makes this effortlessly simple:

```text
# Domain 1: Next.js Frontend
mysite.com {
    reverse_proxy frontend:3000
}

# Domain 2: Express / NestJS Backend API
api.mysite.com {
    reverse_proxy backend:4000
}

# Domain 3: Database Admin UI (e.g. pgAdmin or Prisma Studio)
admin.mysite.com {
    # Add simple HTTP Basic Auth so hackers can't see your admin panel!
    basicauth {
        admin $2a$14$Zkx19XLiW6... # Hashed password
    }
    reverse_proxy pgadmin:80
}
```

Caddy will automatically request **3 separate SSL certificates** for all 3 subdomains!

---

## 🔒 Custom Docker Networks for Isolation

By default, Docker Compose puts all containers in one shared network. In production, you often want isolation:
* Your public reverse proxy should talk to the frontend and backend.
* But the public proxy should **never** have a network route to the PostgreSQL database!

```yaml
services:
  caddy:
    image: caddy:2-alpine
    networks:
      - web_net # Only in web network

  app:
    build: .
    networks:
      - web_net # Can talk to Caddy
      - db_net  # Can talk to Database

  db:
    image: postgres:15-alpine
    networks:
      - db_net  # Completely isolated from Caddy and the internet!

networks:
  web_net:
  db_net:
```

Now, even if a vulnerability was found in Caddy, an attacker cannot even ping the database!

---

## ✅ Level 5 Checklist

- [ ] I can host multiple domains/subdomains on a single server using a Reverse Proxy.
- [ ] I can protect sensitive URLs with HTTP Basic Auth.
- [ ] I understand how segmented Docker networks isolate databases from web traffic.

👉 Open [level-6-pro.md](file:///d:/CODES/concepts/4-networking-and-reverse-proxy/level-6-pro.md) to integrate Cloudflare & WebSockets!
