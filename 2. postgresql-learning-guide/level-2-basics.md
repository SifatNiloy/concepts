# Level 2: CRUD Operations & Filtering

**CRUD** stands for **C**reate, **R**ead, **U**pdate, and **D**elete. This represents 90% of all SQL queries you will write in your career.

---

## ➕ 1. CREATE (`INSERT INTO`)

Add rows into your `users` table:

```sql
-- Insert a single user
INSERT INTO users (name, email, age) 
VALUES ('Sifat', 'sifat@example.com', 24);

-- Insert multiple users at once
INSERT INTO users (name, email, age) VALUES 
('Rahim', 'rahim@example.com', 28),
('Karim', 'karim@example.com', 21),
('Ayesha', 'ayesha@example.com', 25);
```

> 💡 **Tip:** Add `RETURNING *;` to the end of any `INSERT` query to get back the newly created row with its generated `id`!
> ```sql
> INSERT INTO users (name, email) VALUES ('John', 'john@test.com') RETURNING id, created_at;
> ```

---

## 🔍 2. READ (`SELECT`)

Fetching data from your tables:

```sql
-- Fetch all columns from all rows
SELECT * FROM users;

-- Fetch only specific columns (Best practice for performance!)
SELECT name, email FROM users;

-- Filtering with WHERE
SELECT * FROM users WHERE age >= 25;

-- Multiple conditions with AND / OR
SELECT * FROM users WHERE age > 20 AND name = 'Sifat';

-- Pattern matching with LIKE / ILIKE (ILIKE is case-insensitive in Postgres!)
SELECT * FROM users WHERE email ILIKE '%@example.com';

-- Sorting results (ORDER BY)
SELECT * FROM users ORDER BY age DESC; -- Highest age first

-- Pagination (LIMIT and OFFSET)
SELECT * FROM users ORDER BY id ASC LIMIT 2 OFFSET 0; -- Page 1 (first 2 users)
SELECT * FROM users ORDER BY id ASC LIMIT 2 OFFSET 2; -- Page 2 (next 2 users)
```

---

## ✏️ 3. UPDATE

Modifying existing rows:

```sql
-- Update Karim's age
UPDATE users 
SET age = 22 
WHERE email = 'karim@example.com';
```

> ⚠️ **CRITICAL WARNING:** **ALWAYS include a `WHERE` clause!**  
> If you run `UPDATE users SET age = 22;` without `WHERE`, **EVERY user in the entire database will have their age changed to 22!**

---

## ❌ 4. DELETE

Removing rows:

```sql
-- Delete a specific user
DELETE FROM users WHERE email = 'john@test.com';
```

> ⚠️ **CRITICAL WARNING:** Same rule applies! Running `DELETE FROM users;` without `WHERE` will erase your entire table.

---

## ✅ Level 2 Checklist

- [ ] I can insert single and multiple records with `INSERT INTO`.
- [ ] I can filter data using `WHERE`, `AND`, `OR`, and `ILIKE`.
- [ ] I can paginate data using `LIMIT` and `OFFSET`.
- [ ] I know why omitting `WHERE` in `UPDATE` or `DELETE` is dangerous.
