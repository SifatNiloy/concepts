# Level 2: Data Structures & TTL (Auto-Expiration)

Redis is much more than a simple string store. It provides **5 fundamental data structures**.

---

## ⏳ 1. TTL (Time-To-Live / Automatic Expiration)

In caching, you don't want old data staying in RAM forever. Redis can automatically delete a key after a set number of seconds:

```text
# Store an OTP code that expires in 60 seconds (EX = seconds)
SET otp_code "982341" EX 60

# Check how many seconds remain before deletion
TTL otp_code
# Output: (integer) 54

# Set expiration on an existing key
EXPIRE user_session 3600 # Expires in 1 hour
```
Once the timer hits 0, Redis silently vanishes the key from RAM!

---

## 🗃️ 2. Hashes (Like JavaScript Objects)

Storing a user object with multiple fields:

```text
# Store a user object
HSET user:101 name "Sifat" email "sifat@example.com" role "admin"

# Get a specific field
HGET user:101 name
# Output: "Sifat"

# Get all fields and values
HGETALL user:101

# Increment a numerical field (super fast!)
HINCRBY user:101 visits 1
```

---

## 📋 3. Lists (Queues: FIFO & LIFO)

Lists in Redis are perfect for message queues and activity feeds:

```text
# Push items into a queue from the right (RPUSH)
RPUSH task_queue "send_welcome_email"
RPUSH task_queue "resize_avatar"
RPUSH task_queue "generate_invoice"

# Pop the first item from the left (LPUSH)
LPOP task_queue
# Output: "send_welcome_email"

# View all items in the list
LRANGE task_queue 0 -1
```

---

## 🎯 4. Sets & Sorted Sets (Unique Items & Leaderboards)

### Sets (Guaranteed unique, no duplicates):
```text
# Track unique online users:
SADD online_users "user_1" "user_2" "user_1"
SMEMBERS online_users
# Output: "user_1", "user_2" (duplicate was ignored!)
```

### Sorted Sets (ZSET - Ranked by Score):
Perfect for gaming leaderboards or trending articles:

```text
# Add players with their scores (ZADD key score member)
ZADD leaderboard 1500 "PlayerOne"
ZADD leaderboard 2400 "Sifat"
ZADD leaderboard 1800 "PlayerThree"

# Get the Top 3 players (highest score first)
ZREVRANGE leaderboard 0 2 WITHSCORES
# Output:
# 1) "Sifat" - 2400
# 2) "PlayerThree" - 1800
# 3) "PlayerOne" - 1500
```

---

## ✅ Level 2 Checklist

- [ ] I can set expiration times using `EX` and `TTL`.
- [ ] I can store objects using `HSET` and `HGETALL`.
- [ ] I can build queues with `RPUSH` and `LPOP`.
- [ ] I can create real-time leaderboards with Sorted Sets (`ZADD`).
