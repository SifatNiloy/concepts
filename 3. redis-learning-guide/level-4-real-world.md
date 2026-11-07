# Level 4: Rate Limiting & Pub/Sub (Real-Time Chat)

Redis is an essential security tool and real-time engine.

---

## 🛑 1. API Rate Limiting (Preventing Scrapers & DDoS)

Imagine an attacker sends 1,000 requests per second to your `/api/login` endpoint to guess passwords.

Using Redis, you can implement a **Token Bucket** or **Sliding Window** rate limiter:  
*"Allow only 5 requests per minute per IP address."*

### Clean Next.js Rate Limiter Example:
```typescript
import redis from '@/lib/redis';

export async function isRateLimited(ip: string): Promise<boolean> {
  const key = `rate_limit:${ip}`;
  
  // INCR increments the counter by 1 atomically!
  const currentCount = await redis.incr(key);

  // If this is the user's first request, set the 60-second window
  if (currentCount === 1) {
    await redis.expire(key, 60);
  }

  // If they exceeded 5 requests in 60 seconds, block them!
  if (currentCount > 5) {
    return true; // Block!
  }

  return false; // Allow!
}
```

In your Next.js route:
```typescript
if (await isRateLimited(clientIp)) {
  return new Response("Too Many Requests! Calm down.", { status: 429 });
}
```

---

## 📢 2. Pub/Sub (Publish / Subscribe for Real-Time Apps)

In traditional web architecture, if Server #1 receives a chat message, how does Server #2 know to send it to users connected to it?  
**Redis Pub/Sub acts as an ultra-fast internal broadcast radio.**

```
[ User A ] ──▶ [ Server 1 ] ──▶ (PUBLISH "chat:room_1") ──▶ [ REDIS ]
                                                               │
                                                               ▼ (SUBSCRIBE)
[ User B ] ◀── [ Server 2 ] ◀───────────────────────────────────┘
```

### Try it in terminal!
Open two PowerShell windows:

#### Window 1 (The Subscriber):
```powershell
docker exec -it my-redis redis-cli
SUBSCRIBE sports_channel
```
*(It waits and listens...)*

#### Window 2 (The Publisher):
```powershell
docker exec -it my-redis redis-cli
PUBLISH sports_channel "Goal scored by Barcelona!"
```

Instantly, Window 1 prints the message in real time! ⚡

---

## ✅ Level 4 Checklist

- [ ] I can implement an atomic IP rate limiter with `INCR` and `EXPIRE`.
- [ ] I understand the 429 "Too Many Requests" HTTP status code.
- [ ] I understand how Redis Pub/Sub broadcasts messages between multiple servers.
