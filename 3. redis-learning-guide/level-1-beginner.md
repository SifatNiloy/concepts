# Level 1: What is Redis? (Beginner Friendly)

## 🗄️ The Desk vs Filing Cabinet Analogy

Imagine you are working in an office:

* **PostgreSQL (The Filing Cabinet in the Basement):**
  * Holds 10,000 files.
  * Very safe and permanent.
  * **The Problem:** Every time you need a document, you have to stand up, walk down to the basement, unlock the cabinet, search for the folder, and walk back up. This takes **15 milliseconds**.

* **Redis (Your Desk Surface / Sticky Notes):**
  * Holds only the 10 files you are actively working on today.
  * Sits directly in front of your eyes (in your computer's **RAM**).
  * Grabbing a document takes **0.1 milliseconds** (150x faster!).

---

## ⚡ Why is Redis So Fast?

1. **RAM vs Hard Drive (SSD):** RAM operates at nanosecond speeds. Reading from memory is orders of magnitude faster than reading from NVMe SSDs.
2. **Simple Key-Value Store:** Unlike SQL databases that calculate complex table joins, Redis lookups are $O(1)$ constant time: you ask for a key, and it gives you the value immediately.
3. **Single-Threaded Event Loop:** Redis avoids the overhead of thread-locking mechanisms.

---

## 🐳 Step 1: Launch Redis in Docker (10 Seconds)

Open PowerShell and run:

```powershell
docker run -d --name my-redis -p 6379:6379 redis:alpine
```

Verify:
```powershell
docker ps
```
*(Notice: The standard Redis port is **`6379`**)*

---

## 💻 Step 2: Open `redis-cli` (The Redis Terminal)

Walk inside the container and open the interactive CLI:

```powershell
docker exec -it my-redis redis-cli
```

Your prompt changes to:
```text
127.0.0.1:6379>
```

---

## 🔑 Step 3: Your First Redis Commands (`SET` & `GET`)

```text
# 1. Store a value
SET username "sifat"

# 2. Retrieve the value
GET username
# Output: "sifat"

# 3. Check if a key exists
EXISTS username
# Output: (integer) 1

# 4. Delete a key
DEL username
# Output: (integer) 1
```

Type `exit` to leave the CLI.

---

## ✅ Level 1 Checklist

- [ ] I understand the Desk vs Filing Cabinet analogy (RAM vs Disk).
- [ ] I launched Redis with Docker on port 6379.
- [ ] I connected to `redis-cli`.
- [ ] I stored and retrieved values using `SET` and `GET`.
