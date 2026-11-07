# Level 6: Pro - Cron Jobs, Automated Backups & System Maintenance

Servers run 24/7/365. You need automated routines to clean up unused Docker containers, back up databases, and update security packages.

---

## ⏰ What is a Cron Job?

A **Cron Job** is an automated recurring scheduler built into Linux.  
Think of it like an alarm clock that triggers a bash script at specific times.

### The Cron Syntax (5 Stars):
```text
┌───────────── Minute (0 - 59)
│ ┌─────────── Hour (0 - 23)
│ │ ┌───────── Day of Month (1 - 31)
│ │ │ ┌─────── Month (1 - 12)
│ │ │ │ ┌───── Day of Week (0 - 6, Sunday is 0)
│ │ │ │ │
* * * * * command to execute
```

### Common Schedules:
* `0 2 * * *`: Every night at 2:00 AM.
* `0 0 * * 0`: Every Sunday at midnight.
* `*/15 * * * *`: Every 15 minutes.

---

## 💾 Hands-On: Automating PostgreSQL Backups

Remember our PostgreSQL database from `my-nextjs-docker-app`? Let's back it up automatically every night!

### Step 1: Create a backup script:
Create `/home/ubuntu/backup-db.sh`:
```bash
#!/bin/bash
BACKUP_DIR="/home/ubuntu/backups"
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
FILENAME="db_backup_$TIMESTAMP.sql.gz"

mkdir -p $BACKUP_DIR

# Dump database from the docker container and compress it
docker compose -f /app/my-nextjs-docker-app/docker-compose.yml exec -T db pg_dump -U user mydb | gzip > "$BACKUP_DIR/$FILENAME"

# Delete backups older than 7 days to save disk space
find $BACKUP_DIR -type f -name "*.sql.gz" -mtime +7 -delete

echo "Backup completed: $FILENAME"
```

Make it executable:
```bash
chmod +x /home/ubuntu/backup-db.sh
```

### Step 2: Schedule it in Crontab:
Open your user's crontab editor:
```bash
crontab -e
```

Add this line at the bottom:
```text
0 3 * * * /home/ubuntu/backup-db.sh >> /home/ubuntu/backups/backup.log 2>&1
```
*(Now your database is backed up every night at 3:00 AM without you lifting a finger!)*

---

## 🧹 Docker Maintenance: Preventing Disk Space Outages

Docker caches layers, old images, and stopped containers. Over months, this can fill up your server hard drive.

Add an automated Docker cleanup cron job:
```text
# Run Docker cleanup every Sunday at 4:00 AM:
0 4 * * 0 docker system prune -af --volumes
```

---

## 🏆 You Have Mastered Linux Server Administration!

You can now:
* Navigate and administer any Linux server.
* Secure it with SSH keys and firewalls.
* Run Docker containers in production.
* Schedule automated database backups and system maintenance.

👉 Next up: Let's attach custom domains and automatic HTTPS!  
Open folder: [4-networking-and-reverse-proxy](file:///d:/CODES/concepts/4-networking-and-reverse-proxy)
