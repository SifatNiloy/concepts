# Level 3: Processes, Services & Systemd

When you close your terminal, what keeps your app running? **Systemd**.

---

## ⚙️ What is `systemd`?

`systemd` is the **Master Controller** of modern Linux (Ubuntu, Debian, RedHat).  
It starts during machine bootup, launches background services (like Docker, SSH, databases), monitors them, and **restarts them automatically if they crash**.

---

## 🛠️ The 5 Commands to Control Any Service

Every server admin uses `systemctl`:

```bash
# 1. Check if Docker is running:
sudo systemctl status docker

# 2. Start a service:
sudo systemctl start docker

# 3. Stop a service:
sudo systemctl stop docker

# 4. Restart a service (after editing configs):
sudo systemctl restart docker

# 5. Enable on boot (make it start automatically when server restarts):
sudo systemctl enable docker
```

---

## 🔍 How to Read Server Logs (`journalctl`)

When something crashes or fails to start, don't guess! Read the system logs:

```bash
# View recent logs from the Docker service:
sudo journalctl -u docker -n 50

# Follow logs in real-time (live stream):
sudo journalctl -u docker -f
```

---

## 📊 Monitoring Processes: `htop` & `ps`

To see what programs are eating your CPU and RAM:

```bash
# View active processes:
ps aux | grep node

# Interactive task manager (install with: sudo apt install htop):
htop
```
*(Press `q` to quit `htop`)*

---

## ✅ Level 3 Checklist

- [ ] I can check service health with `systemctl status`.
- [ ] I know how to start, stop, and restart background services.
- [ ] I know how to read live service logs using `journalctl -u <service> -f`.
- [ ] I can inspect CPU and RAM usage with `htop`.
