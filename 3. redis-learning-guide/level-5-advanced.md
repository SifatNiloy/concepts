# Level 5: Background Queues & Workers with BullMQ

What happens when a user clicks **"Sign Up"** and your server needs to:
1. Create user in Postgres (5ms).
2. Generate an invoice PDF (2,000ms).
3. Send a verification email via SendGrid (1,500ms).

If you make the user wait for steps 2 and 3, your website feels sluggish (3.5+ seconds).  
Even worse: if SendGrid's API fails, the entire sign-up crashes!

---

## 📬 The Solution: Background Queues (BullMQ + Redis)

* Your web API only does Step 1, drops a "ticket" into a **Redis Queue**, and instantly tells the user: *"Success!"* (10ms).
* A separate background container (**Worker**) picks up tickets from the Redis Queue and sends the emails quietly in the background!

```
[ User Clicks Sign Up ]
           │
           ▼
   [ Next.js API ] ──▶ 1. Saves User to Postgres
           │       ──▶ 2. Adds Job to Redis Queue ("send_email", { email: "sifat@test.com" })
           ▼
[ Returns 200 OK! ] (In 10ms! ⚡)

           ... Meanwhile in the background ...

   [ Redis Queue ] ──▶ [ Background Worker Container ] ──▶ Sends Email to SendGrid
```

---

## 🛠️ Implementing with BullMQ

In your Node/TypeScript project:

```powershell
npm install bullmq ioredis
```

### 1. Producer: Add Job to Queue (`queue.ts`):
```typescript
import { Queue } from 'bullmq';

const connection = { host: 'localhost', port: 6379 };

export const emailQueue = new Queue('email-queue', { connection });

// In your API route:
await emailQueue.add('welcome-email', {
  to: 'sifat@example.com',
  name: 'Sifat',
}, {
  attempts: 3, // Automatically retry 3 times if it fails!
  backoff: { type: 'exponential', delay: 1000 },
});
```

### 2. Consumer: The Background Worker (`worker.ts`):
```typescript
import { Worker } from 'bullmq';

const connection = { host: 'localhost', port: 6379 };

const worker = new Worker('email-queue', async (job) => {
  console.log(`Processing job ${job.id} for ${job.data.to}...`);
  
  // Simulate sending email:
  await new Promise((resolve) => setTimeout(resolve, 2000));
  
  console.log(`✓ Email sent to ${job.data.to}!`);
}, { connection });

worker.on('failed', (job, err) => {
  console.error(`Job ${job?.id} failed:`, err.message);
});
```

---

## ✅ Level 5 Checklist

- [ ] I understand why heavy tasks (emails, PDFs, video encoding) belong in background workers.
- [ ] I can produce jobs to a queue using BullMQ.
- [ ] I can process jobs asynchronously with automatic retries and exponential backoff.
