# Level 4: Aggregations, GROUP BY & Indexes (Query Optimization)

When your app grows to 100,000+ rows, running basic queries can start taking seconds instead of milliseconds. Here is how to calculate statistics and make your database lightning fast.

---

## 📈 Aggregations: `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`

Calculate statistics across rows:

```sql
-- Total number of users
SELECT COUNT(*) FROM users;

-- Average age of users
SELECT AVG(age) FROM users;

-- Youngest and oldest user
SELECT MIN(age) AS youngest, MAX(age) AS oldest FROM users;
```

---

## 👥 Grouping Data: `GROUP BY` & `HAVING`

How many posts has each user written?

```sql
SELECT 
    users.name, 
    COUNT(posts.id) AS total_posts
FROM users
LEFT JOIN posts ON users.id = posts.user_id
GROUP BY users.id, users.name;
```

### Filtering Groups with `HAVING`:
* `WHERE` filters individual rows **before** grouping.
* `HAVING` filters the calculated groups **after** aggregation:

```sql
-- Find only active users who have written 2 or more posts:
SELECT 
    users.name, 
    COUNT(posts.id) AS total_posts
FROM users
INNER JOIN posts ON users.id = posts.user_id
GROUP BY users.id, users.name
HAVING COUNT(posts.id) >= 2;
```

---

## ⚡ Indexes: The Secret to 100x Faster Queries

### The Book Index Analogy:
* **Without an index:** If you ask a reader: *"Find the page mentioning 'Docker'"*, they must flip through all 500 pages one by one (**Sequential Scan / Table Scan**). Slow!
* **With an index:** The reader turns to the back of the book, finds "Docker: page 42", and jumps directly to it (**Index Scan**). Instant!

### Creating an Index:
```sql
-- Speed up lookups by email:
CREATE INDEX idx_users_email ON users(email);

-- Speed up finding posts by a specific user:
CREATE INDEX idx_posts_user_id ON posts(user_id);
```

### 🔬 How to Test Performance: `EXPLAIN ANALYZE`
Prefix any SQL query with `EXPLAIN ANALYZE` to see exactly how Postgres executes it:

```sql
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'sifat@example.com';
```
Postgres will output the exact execution plan and execution time in fractions of a millisecond!

---

## ✅ Level 4 Checklist

- [ ] I can use `COUNT`, `SUM`, and `AVG`.
- [ ] I can group records using `GROUP BY` and filter them with `HAVING`.
- [ ] I understand the book index analogy and how `CREATE INDEX` accelerates searches.
- [ ] I can inspect query execution speed using `EXPLAIN ANALYZE`.
