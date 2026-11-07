# Level 6: Pro - Connecting Next.js with Prisma/Drizzle & Docker Compose

In production Next.js apps, developers rarely write raw SQL strings in their React components. They use an **ORM (Object-Relational Mapper)** like **Prisma** or **Drizzle**.

---

## 🛠️ Step 1: Connecting with Prisma ORM

In your Next.js project:

```powershell
npm install @prisma/client
npm install -D prisma
npx prisma init
```

This creates a `prisma/schema.prisma` file:

```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String?
  posts     Post[]
  createdAt DateTime @default(now())
}

model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String?
  userId    Int
  user      User     @relation(fields: [userId], references: [id])
}
```

---

## 🔄 Step 2: Database Migrations

When you change your schema, you run a migration:

```powershell
npx prisma migrate dev --name init
```

Prisma automatically:
1. Generates the exact SQL DDL scripts.
2. Applies them to your PostgreSQL container.
3. Generates 100% type-safe TypeScript functions!

---

## ⚡ Step 3: Querying Inside a Next.js Server Component

In Next.js App Router (`app/users/page.tsx`):

```typescript
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

export default async function UsersPage() {
  // 100% Type-Safe Query with automatic relation loading!
  const users = await prisma.user.findMany({
    include: {
      posts: true,
    },
  });

  return (
    <div>
      <h1>Users & Their Posts</h1>
      {users.map((user) => (
        <div key={user.id}>
          <h3>{user.name} ({user.email})</h3>
          <p>Posts written: {user.posts.length}</p>
        </div>
      ))}
    </div>
  );
}
```

---

## 🐳 Step 4: The Production `docker-compose.yml`

Here is how Next.js, Prisma, and PostgreSQL run harmoniously together:

```yaml
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      # Connects using the service name 'db'!
      - DATABASE_URL=postgresql://user:password@db:5432/mydb
    depends_on:
      db:
        condition: service_healthy

  db:
    image: postgres:15-alpine
    restart: always
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user -d mydb"]
      interval: 5s
      timeout: 5s
      retries: 5

volumes:
  postgres_data:
```

---

## 🏆 You Have Mastered PostgreSQL!

You now know:
* How to run Postgres in Docker and manage it via `psql`.
* Writing fast, relational SQL queries (`SELECT`, `JOIN`, `GROUP BY`).
* Protecting data with Transactions (ACID) and Foreign Keys.
* Accelerating queries with Indexes.
* Connecting Next.js with Prisma ORM and Docker Compose.
Open folder: [3. redis-learning-guide](3.redis-learning-guide)
