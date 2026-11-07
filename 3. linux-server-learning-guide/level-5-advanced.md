# Level 5: Getting a 100% Free Forever Cloud Server & Installing Docker

You don't need a credit card bill to practice on a real cloud server. 

---

## ☁️ The Best Free Cloud Tier in the World: Oracle Cloud Always Free

While AWS and Google Cloud only give 12 months free, **Oracle Cloud has an "Always Free" tier that never expires**.

### What you get for \$0.00:
* **4 Ampere ARM CPU Cores**
* **24 GB of RAM**
* **200 GB of NVMe SSD Storage**
* **Static Public IP Address**
* **Ubuntu Linux 22.04 or 24.04**

*(This is stronger than a \$40/month Droplet on DigitalOcean!)*

---

## 🛠️ Step-by-Step Setup:

### 1. Sign up for Oracle Cloud Free Tier
* Go to: `oracle.com/cloud/free`
* Select **Home Region** closest to you (e.g., Singapore, Frankfurt, Tokyo).
* *(Requires a debit/credit card for identity verification, but charges \$0.00)*.

### 2. Launch an Instance
* Compute $\rightarrow$ Instances $\rightarrow$ **Create Instance**.
* Image: **Ubuntu 24.04 LTS**.
* Shape: **Ampere ARM** (Select 2 to 4 cores, 12 to 24 GB RAM).
* Add SSH Keys: Paste your `id_ed25519.pub` public key that you created in Level 4!
* Click **Create**.

### 3. Connect via PowerShell:
```powershell
ssh -i ~/.ssh/id_ed25519 ubuntu@<YOUR_PUBLIC_IP>
```

You are now inside your real remote cloud server!

---

## 🐳 Installing Docker on Ubuntu (The Clean Script)

Once logged into your server, run the official Docker setup:

```bash
# 1. Update packages
sudo apt update && sudo apt upgrade -y

# 2. Install prerequisites
sudo apt install -y curl ca-certificates

# 3. Download and run Docker's official installation script:
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# 4. Allow your user to run Docker without typing "sudo" every time:
sudo usermod -aG docker $USER
newgrp docker

# 5. Verify it works!
docker run hello-world
```

---

## ✅ Level 5 Checklist

- [ ] I understand how Oracle Cloud's Always Free tier works.
- [ ] I connected to a remote Ubuntu server via SSH.
- [ ] I installed Docker and Docker Compose on Ubuntu.
- [ ] I ran `docker run hello-world` on a remote cloud server!

👉 Open [level-6-pro.md](file:///d:/CODES/concepts/3-linux-server-learning-guide/level-6-pro.md) for automated backups and Cron jobs!
