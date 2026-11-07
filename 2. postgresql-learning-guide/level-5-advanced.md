# Level 5: Transactions (ACID) & JSONB (NoSQL in Postgres)

Two superpowers make PostgreSQL loved by architects: **Bulletproof Transactions** and **Native JSON support**.

---

## 🏦 1. Transactions & ACID (The Bank Transfer Analogy)

Imagine you transfer \$100 from Alice to Bob:
1. Step 1: Deduct \$100 from Alice's account.
2. Step 2: Add \$100 to Bob's account.

What if the server crashes right after Step 1? **Alice lost \$100, but Bob never received it!**

A **Transaction** groups multiple SQL commands into an **"All or Nothing"** unit:

```sql
BEGIN; -- Start the transaction

UPDATE accounts SET balance = balance - 100 WHERE user_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE user_id = 2;

COMMIT; -- Save both permanently!
```

If an error happens before `COMMIT`, you run:
```sql
ROLLBACK; -- Undoes everything completely, leaving balances untouched!
```

This is the **"A" in ACID (Atomicity)**: Either all operations succeed, or none do.

---

## 📦 2. JSONB: Using Postgres Like MongoDB!

Did you know PostgreSQL has full NoSQL capabilities?  
The `JSONB` data type lets you store raw JSON objects with full indexing and query support!

### Create a table with JSONB:
```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    attributes JSONB
);
```

### Insert JSON data:
```sql
INSERT INTO products (name, attributes) VALUES
('MacBook Pro', '{"brand": "Apple", "ram": 16, "colors": ["silver", "space gray"]}'),
('ThinkPad X1', '{"brand": "Lenovo", "ram": 32, "colors": ["black"]}');
```

### Querying inside the JSON object!
Use the arrow operators:
* `->` returns a JSON object.
* `->>` returns text.

```sql
-- Get products where brand is 'Apple':
SELECT name, attributes->>'brand' AS brand 
FROM products 
WHERE attributes->>'brand' = 'Apple';

-- Find products with 32GB RAM:
SELECT name FROM products 
WHERE (attributes->>'ram')::integer >= 32;
```

You get the strict safety of SQL **plus** the flexible schema of MongoDB in one database!

---

## ✅ Level 5 Checklist

- [ ] I understand the bank transfer problem and how `BEGIN`, `COMMIT`, and `ROLLBACK` solve it.
- [ ] I understand the ACID concept of Atomicity.
- [ ] I can store and query unstructured data using `JSONB` with the `->>` operator.
