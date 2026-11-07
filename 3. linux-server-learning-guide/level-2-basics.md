# Level 2: Permissions, Users & Sudo

Security in Linux is strict. Every file and process belongs to a **User** and a **Group**.

---

## 🔒 Understanding `ls -l` Output

When you type `ls -l`, you see lines like this:

```text
-rw-r--r--  1  sifat  developers  4096  Nov 7 20:00  app.js
```

Let's dissect the first 10 characters: `-rw-r--r--`

```
 -       rw-          r--          r--
[Type]  [Owner]      [Group]      [Others]
File    Read+Write   Read-Only    Read-Only
```

### The 3 Permissions:
* **`r`** (Read) = Value 4
* **`w`** (Write) = Value 2
* **`x`** (Execute - run as script/program) = Value 1

---

## 🔢 The Magic of `chmod 755` vs `chmod 600`

You will often see tutorials saying `chmod 755` or `chmod 600`. Here is the math:

* `7` = 4 + 2 + 1 = **Read + Write + Execute** (Full access)
* `6` = 4 + 2 = **Read + Write**
* `5` = 4 + 1 = **Read + Execute**
* `0` = No permissions at all

### Common Rules of Thumb:
1. **`chmod 755 script.sh`**: You can edit and run it; others can only read and run it. (Standard for scripts/executables).
2. **`chmod 600 id_rsa`**: ONLY you can read and write; everyone else is blocked. **Required by SSH for private keys!**
3. **`chmod +x deploy.sh`**: Quick shortcut to make any script executable.

---

## 👑 What is `sudo`?

`sudo` stands for **SuperUser DO**.  
* In Windows, it's equivalent to: *"Right Click $\rightarrow$ Run as Administrator"*.
* If you run a command and get: `Permission denied`, you typically prefix it with `sudo`:
  ```bash
  sudo apt update
  sudo systemctl restart docker
  ```

---

## 👤 Changing Ownership: `chown`

If you want a folder to be owned by your user instead of `root`:
```bash
sudo chown -R sifat:sifat /var/www/my-app
```
*(Now `sifat` can edit files without needing `sudo` every time!)*

---

## ✅ Level 2 Checklist

- [ ] I understand `r`, `w`, and `x` permissions.
- [ ] I know why SSH private keys require `chmod 600`.
- [ ] I know what `sudo` does and when to use it.
- [ ] I know how to change file ownership with `chown`.

👉 Open [level-3-intermediate.md](file:///d:/CODES/concepts/3-linux-server-learning-guide/level-3-intermediate.md) to manage background processes and `systemd`!
