# Level 1: Linux Filesystem & Core Commands (Beginner Friendly)

## 🐧 Why Linux?

Over **90% of the world's cloud servers** and **100% of modern containers** run on Linux.  
The good news: You already have real Linux running right inside your Windows machine through **WSL 2**!

---

## 📁 The Linux Tree: Everything is a File

In Windows, your drives are letters (`C:\`, `D:\`).  
In Linux, there are **no drive letters**. Everything begins from a single root slash: **`/`**

```
/ (Root of everything)
├── etc/          <-- Configuration files (like Nginx config, DNS settings)
├── var/          <-- Variable data (logs in /var/log, databases)
├── home/         <-- User folders (e.g. /home/sifat/ - like C:\Users\sifat)
├── bin/ & sbin/  <-- Executable programs (ls, docker, node)
└── tmp/          <-- Temporary files (deleted on reboot)
```

---

## 🧭 The Top 10 Commands You Will Use Daily

| Command | What it does | Example |
| :--- | :--- | :--- |
| `pwd` | **P**rint **W**orking **D**irectory (where am I?) | `pwd` $\rightarrow$ `/home/sifat` |
| `ls -la` | **L**i**s**t all files (including hidden `.env` files) with details | `ls -la` |
| `cd` | **C**hange **D**irectory | `cd /var/log` or `cd ..` (go up) |
| `mkdir -p` | **M**a**k**e **D**irectory (with parents) | `mkdir -p /app/backend` |
| `rm -rf` | **R**e**m**ove files or folders (be careful!) | `rm -rf old-folder` |
| `cp -r` | **C**o**p**y files or folders | `cp -r project backup-project` |
| `mv` | **M**o**v**e or rename | `mv old-name.txt new-name.txt` |
| `cat` / `less` | View the contents of a file | `cat /etc/os-release` |
| `nano` | Simple built-in terminal text editor | `nano Dockerfile` |
| `curl -I` | Fetch a website/API from terminal | `curl -I http://localhost:3000` |

---

## 🎯 Hands-On: Practice Inside WSL!

Open PowerShell and type:
```powershell
wsl
```
Your prompt will instantly change to a Linux prompt (e.g. `sifat@YourPC:~$`)!

Try these commands:
```bash
# 1. See what Linux version you are running:
cat /etc/os-release

# 2. See how much RAM and Disk you have:
free -h
df -h

# 3. Create a test project folder:
mkdir -p ~/linux-practice && cd ~/linux-practice
echo "Linux is awesome" > hello.txt
cat hello.txt

# 4. Exit back to Windows PowerShell:
exit
```

---

## ✅ Level 1 Checklist

- [ ] I understand the root `/` filesystem tree.
- [ ] I can navigate folders using `cd`, `ls -la`, and `pwd`.
- [ ] I can open WSL from PowerShell and run Linux commands.
