# Level 2: Domains, DNS Records & Web Ports

Before setting up a reverse proxy, you need to understand how the internet connects words (`google.com`) to numbers (`142.250.190.46`).

---

## 📖 The Internet Phonebook: DNS (Domain Name System)

Computers only understand IP addresses. When a user types `sifat.dev` into Chrome:

1. Chrome asks a **DNS Server** (like Cloudflare `1.1.1.1` or Google `8.8.8.8`): *"What IP belongs to sifat.dev?"*
2. The DNS server checks its records and responds: *"It is at `123.45.67.89`!"*
3. Chrome connects directly to that server IP.

---

## 📋 The 3 DNS Records You Must Know

When you buy a domain (from Namecheap, Cloudflare, or Porkbun), you will configure these:

| Record Type | What it points to | Example | Use Case |
| :--- | :--- | :--- | :--- |
| **`A` Record** | Directly to an **IPv4 Address** | `@` $\rightarrow$ `123.45.67.89` | Connect your root domain (`mysite.com`) to your server IP |
| **`AAAA` Record** | To an **IPv6 Address** | `@` $\rightarrow$ `2606:4700:...` | IPv6 equivalent of an A record |
| **`CNAME` Record** | To **Another Domain Name** | `www` $\rightarrow$ `mysite.com` | Aliasing subdomains (e.g. `api.mysite.com` or `blog.mysite.com`) |

---

## 🚪 The Two Magical Ports: 80 and 443

When you visit any website:
* `http://example.com` $\rightarrow$ The browser automatically talks to **Port 80**.
* `https://example.com` $\rightarrow$ The browser automatically talks to **Port 443** (Encrypted SSL/TLS).

Because browsers default to these two ports, **your reverse proxy must always listen on ports 80 and 443** so visitors don't need to specify a port number in their URL!

---

## ✅ Level 2 Checklist

- [ ] I understand how DNS maps domain names to server IPs.
- [ ] I know when to use an `A` record vs a `CNAME` record.
- [ ] I understand why ports 80 (HTTP) and 443 (HTTPS) are standard for the web.
