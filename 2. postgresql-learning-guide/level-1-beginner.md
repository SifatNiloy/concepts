# Level 1: What is a Relational Database? (Beginner Friendly)

## 📊 The Spreadsheet Analogy

Imagine an Excel spreadsheet called **`users.xlsx`**:
* **Columns (Headers):** `id`, `name`, `email`, `created_at`
* **Rows (Records):** Each individual person signed up on your app.

A **Relational Database Management System (RDBMS)** like PostgreSQL is like a super-powered, lightning-fast Excel workbook that:
* Can hold **billions of rows** without lagging.
* Enforces strict data types (you can't put letters in an `age` column).
* Links different sheets together (e.g. linking `orders` to `users`).
* Lets thousands of users read and write simultaneously without corrupting files.

---

## 🐳 Step 1: Launch PostgreSQL in Docker (30 Seconds!)

You don't need to install heavy Windows database installers. Run Postgres in Docker:

```powershell
docker run -d --name my-postgres -p 5432:5432 -e POSTGRES_PASSWORD=mysecretpassword -e POSTGRES_DB=devdb postgres:15-alpine
```

Verify it is running:
```powershell
docker ps
```

---

## 💻 Step 2: Open `psql` (The PostgreSQL Terminal)

Let's walk inside the container and open the SQL prompt:

```powershell
docker exec -it my-postgres psql -U postgres -d devdb
```

Your prompt changes to:
```text
devdb=#
```
You are now inside PostgreSQL! 🎉

---

## 🧱 Step 3: Your First Table & Data Types

In SQL, every column must have a **Data Type**:
* `SERIAL PRIMARY KEY`: An auto-incrementing unique ID (1, 2, 3...).
* `VARCHAR(100)`: Text up to 100 characters.
* `TEXT`: Unlimited length text (blog posts, descriptions).
* `INTEGER`: Whole numbers (age, quantity).
* `BOOLEAN`: `TRUE` or `FALSE`.
* `TIMESTAMP`: Dates and times.

### Create your first table:
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    age INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Useful `psql` Shortcuts:
* `\dt`: List all tables in the database.
* `\d users`: Inspect the columns and structure of the `users` table.
* `\q`: Exit `psql` back to PowerShell.

---

## ✅ Level 1 Checklist

- [ ] I understand the spreadsheet analogy of tables, columns, and rows.
- [ ] I launched a PostgreSQL container with Docker.
- [ ] I connected to `psql` using `docker exec -it my-postgres psql ...`.
- [ ] I created my first table using `CREATE TABLE`.
