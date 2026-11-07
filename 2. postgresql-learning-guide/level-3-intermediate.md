# Level 3: Relationships, Foreign Keys & JOINs

The real power of a **Relational** Database is connecting tables together.

---

## 🔗 The 3 Types of Relationships

1. **One-to-One (1:1):** One User has One Profile.
2. **One-to-Many (1:N):** One User has Many Posts (The most common!).
3. **Many-to-Many (N:N):** Many Students take Many Courses (Uses a "Join Table").

---

## 🏗️ Creating a Related Table (`posts`)

Let's link blog posts to their author in `users`:

```sql
CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    content TEXT,
    -- FOREIGN KEY: Points back to users.id
    user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

> 💡 **What is `ON DELETE CASCADE`?**  
> If user #1 is deleted from the database, PostgreSQL will **automatically delete all of user #1's posts** so you don't leave orphaned data behind!

### Add some sample posts:
```sql
-- Assuming Sifat has id=1 and Rahim has id=2
INSERT INTO posts (title, content, user_id) VALUES
('Docker is Awesome', 'Learning containers today...', 1),
('Next.js with Postgres', 'How to connect databases...', 1),
('My First Post', 'Hello world from Rahim...', 2);
```

---

## 🤝 The Magic of JOINs

How do we get a list of posts showing the author's **name** instead of just their `user_id` number?  
**We JOIN the two tables!**

### 1. `INNER JOIN` (Matches only rows that exist in BOTH tables)
```sql
SELECT 
    posts.id, 
    posts.title, 
    users.name AS author, 
    users.email 
FROM posts
INNER JOIN users ON posts.user_id = users.id;
```

### 2. `LEFT JOIN` (Keeps ALL users, even if they have zero posts!)
If Ayesha has written 0 posts:
* `INNER JOIN` will **hide** Ayesha.
* `LEFT JOIN` will **include** Ayesha, showing `NULL` for the post title!

```sql
SELECT 
    users.name, 
    posts.title 
FROM users
LEFT JOIN posts ON users.id = posts.user_id;
```

---

## ✅ Level 3 Checklist

- [ ] I understand One-to-Many relationships and Foreign Keys (`REFERENCES`).
- [ ] I know what `ON DELETE CASCADE` does.
- [ ] I can write an `INNER JOIN` to combine related records.
- [ ] I know the difference between an `INNER JOIN` and a `LEFT JOIN`.
