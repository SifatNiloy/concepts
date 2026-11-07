# Level 3: The Cache-Aside Pattern in Next.js / Node.js

The **Cache-Aside Pattern** is the universal pattern used by Facebook, Amazon, and Netflix to make websites feel instantaneous while shielding their SQL database from melting under heavy traffic.

---

## 🔄 The Cache-Aside Flow

```
[ Next.js User Request ]
          │
          ▼
   1. Check Redis Cache
          │
    ┌─────┴─────┐
    ▼           ▼
[CACHE HIT]   [CACHE MISS]
 (In Redis)   (Not in Redis)
    │           │
    │           ├─▶ 2. Fetch from slow PostgreSQL Database (takes 100ms)
    │           ├─▶ 3. Save result into Redis with TTL = 60s
    │           │
    ▼           ▼
[ Return response to user! ] (Next request takes 1ms!)
```

---

## 💻 Hands-On: Implementing it with `ioredis`

In your Next.js / Node project:

```powershell
npm install ioredis
```

### 1. Create a Redis Client (`lib/redis.ts`):
```typescript
import Redis from 'ioredis';

// Connects to Docker Redis at localhost:6379 (or 'redis:6379' in Docker Compose)
const redis = new Redis(process.env.REDIS_URL || 'redis://localhost:6379');

export default redis;
```

### 2. Implement Cached Query (`app/api/products/route.ts`):
```typescript
import { NextResponse } from 'next/server';
import redis from '@/lib/redis';
import prisma from '@/lib/prisma'; // Our PostgreSQL client

export async function GET() {
  const cacheKey = 'all_products';

  // 1. Try to read from Redis first!
  const cachedData = await redis.get(cacheKey);

  if (cachedData) {
    console.log('⚡ Cache Hit! Served from Redis in 1ms');
    return NextResponse.json(JSON.parse(cachedData));
  }

  // 2. Cache Miss: Query the slow PostgreSQL database
  console.log('🐢 Cache Miss! Fetching from PostgreSQL...');
  const products = await prisma.product.findMany();

  // 3. Store in Redis for 60 seconds (EX 60)
  await redis.set(cacheKey, JSON.stringify(products), 'EX', 60);

  return NextResponse.json(products);
}
```

---

## ⚡ Cache Invalidation: What if a Product Changes?

If an admin updates a product price, you must **invalidate** (bust) the cache so users don't see old prices:

```typescript
export async function POST(req: Request) {
  const body = await req.json();
  
  // 1. Create product in Postgres
  const newProduct = await prisma.product.create({ data: body });

  // 2. Bust the cache!
  await redis.del('all_products');

  return NextResponse.json(newProduct);
}
```

---

## ✅ Level 3 Checklist

- [ ] I understand the Cache Hit vs Cache Miss lifecycle.
- [ ] I can connect Node.js to Redis using `ioredis`.
- [ ] I know how to store JSON in Redis with expiration (`EX`).
- [ ] I know how and when to invalidate cached keys using `redis.del()`.
