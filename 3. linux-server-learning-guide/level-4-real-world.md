# Level 4: SSH, Firewalls (UFW) & Server Security

A public cloud server is exposed to the entire internet. Automated bots scan every server every few seconds. Here is how to make your server practically unhackable.

---

## 🔑 SSH Keys: Passwords are Outdated

Never log into a server with an account password. Passwords can be brute-forced.  
Always use **SSH Key Pairs** (Cryptography):

* **Private Key (`id_ed25519`)**: Stays on your personal laptop. Never share it with anyone!
* **Public Key (`id_ed25519.pub`)**: Goes onto the server in `~/.ssh/authorized_keys`.

### Step 1: Generate an SSH Key on your Windows laptop:
Open PowerShell:
```powershell
ssh-keygen -t ed25519 -C "sifat-laptop"
```
*(Hit Enter to accept default location and optional passphrase)*

### Step 2: Connect to a server using your key:
```powershell
ssh -i ~/.ssh/id_ed25519 ubuntu@<SERVER_IP>
```

---

## 🧱 The Firewall: UFW (Uncomplicated Firewall)

A firewall blocks all incoming internet traffic except the doors (ports) you specifically approve.

### ⚠️ Golden Rule of Firewalls:
**ALWAYS allow SSH (port 22) BEFORE enabling the firewall**, otherwise you will lock yourself out of your server forever!

```bash
# 1. Allow SSH (so you don't get kicked out!):
sudo ufw allow 22/tcp

# 2. Allow Web traffic (HTTP and HTTPS):
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# 3. Turn ON the firewall:
sudo ufw enable

# 4. Check status:
sudo ufw status verbose
```

Now, any hacker trying to connect to your database port (`5432`) directly from the outside world is instantly blocked!

---

## 🛡️ Production Hardening: Disable Password Authentication

Once your SSH key works, edit `/etc/ssh/sshd_config`:
```bash
sudo nano /etc/ssh/sshd_config
```

Change:
```text
PasswordAuthentication no
PermitRootLogin prohibit-password
```

Restart the SSH service:
```bash
sudo systemctl restart ssh
```
Now, nobody on Earth can log in without your physical private key!

---

## ✅ Level 4 Checklist

- [ ] I understand the difference between public and private SSH keys.
- [ ] I know why we use Ed25519 keys instead of passwords.
- [ ] I know how to configure `ufw` to allow ports 22, 80, and 443.
- [ ] I know how to disable password login for maximum security.

👉 Open [level-5-advanced.md](file:///d:/CODES/concepts/3-linux-server-learning-guide/level-5-advanced.md) to set up your **100% Free Forever Cloud Server**!
