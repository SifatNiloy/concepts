# Level 6: Pro - Redis Persistence (`RDB` vs `AOF`) & Docker Compose

Because Redis lives in RAM, **what happens if the server loses power?**  
Does all your cached session and queue data vanish into thin air?

Not if you configure **Persistence**!

---

## 💾 The Two Persistence Engines

Redis gives you two ways to save RAM data onto your hard drive:

### 1. RDB (Redis Database Snapshots)
* Takes a full snapshot of all data in RAM every $X$ minutes (e.g. *"every 15 minutes if at least 1 key changed"*).
* **Pros:** Compact single file (`dump.rdb`). Ultra-fast recovery on restarts.
* **Cons:** You could lose the last few minutes of data if power cuts between snapshots.

### 2. AOF (Append-Only File)
* Logs every single write command (`SET`, `HSET`, `LPUSH`) to a text log on disk in real time.
* **Pros:** Maximum safety! Zero data loss.
* **Cons:** Larger file size on disk.

> 🏆 **Production Best Practice:** **Turn ON both!** Redis will use AOF for safety and RDB for fast reboots.

---

## 🐳 Production `docker-compose.yml` (Next.js + Postgres + Redis)

Here is the holy trinity of modern full-stack web architectures:

```yaml
services:
  # 1. Next.js App
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/mydb
      - REDIS_URL=redis://redis:6379
    depends_on:
      - db
      - redis

  # 2. PostgreSQL Database (Disk-based persistent data)
  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data

  # 3. Redis In-Memory Store (Speed, Cache, Queues)
  redis:
    image: redis:alpine
    command: redis-server --appendonly yes --requirepass "myredispassword" --maxmemory 256mb --maxmemory-policy allkeys-lru
    volumes:
      - redis_data:/data
    ports:
      - "6379:6379"

volumes:
  postgres_data:
  redis_data:
```

### Breaking down the Redis flags:
* `--appendonly yes`: Enables AOF persistence so data survives container restarts.
* `--requirepass "myredispassword"`: Protects Redis with a password.
* `--maxmemory 256mb`: Caps Redis RAM so it doesn't eat your entire server memory!
* `--maxmemory-policy allkeys-lru`: **LRU (Least Recently Used)**: If Redis hits 256MB, it automatically deletes the oldest unused keys to make room for new ones!

---

## 🏆 You Have Mastered Redis!

You can now:
* Run and query Redis in Docker with `redis-cli`.
* Use Hashes, Lists, Sets, and Expirations (TTL).
* Boost your Next.js performance by 100x using the Cache-Aside pattern.
* Shield your APIs with atomic Rate Limiters.
* Offload heavy processing with background queues (BullMQ).
* Run production Redis safely with persistent storage and LRU memory policies.
Open folder: [4. cicd-learning-guide](file:///d:/CODES/concepts/4.%20cicd-learning-guide)
