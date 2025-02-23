### **Lesson 6: Advanced Bash Scripting – Practical Use Cases**  

## **Real-World Problem Statement**  

As a **DevOps Engineer**, you are responsible for ensuring smooth operations of your servers. However, **manual system maintenance** is inefficient. Your team has identified three critical tasks that should be automated:  

1. **Monitor disk usage** and trigger alerts if the space is low.  
2. **Automate log cleanup** to prevent old log files from filling up storage.  
3. **Schedule backups** of important files for disaster recovery.  

By the end of this lesson, you will be able to:  
✔ Write a **disk monitoring script** to alert if storage is running low.  
✔ Automate **log file cleanup** to free up disk space.  
✔ Create an **automated backup script** using Bash.  

Let’s get started by integrating everything we’ve learned.

---

## **1. Monitoring Disk Usage with Bash**  

### **Scenario:**  
Your servers handle **critical applications**, and **low disk space** could lead to failures. You need an automated script that:  
- Checks disk usage on **critical partitions**.  
- Sends a **warning message** if disk usage **exceeds a threshold**.  
- Logs the results for future reference.  

---

### **Step 1: Create the Disk Monitoring Script**  

```bash
vi monitor_disk.sh
```

### **Step 2: Add the Script Content**  

```bash
#!/bin/bash

LOGFILE="/var/log/disk_usage.log"
THRESHOLD=80
DISK_USAGE=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')

echo "$(date) - Disk usage: $DISK_USAGE%" | tee -a $LOGFILE

if [ "$DISK_USAGE" -ge "$THRESHOLD" ]; then
    echo "$(date) - WARNING: Disk usage is above $THRESHOLD%!" | tee -a $LOGFILE
fi
```

### **Step 3: Make the Script Executable**  

```bash
chmod +x monitor_disk.sh
```

### **Step 4: Test the Script**  

```bash
./monitor_disk.sh
```

✅ **Expected Output:**  
```
Tue Feb 27 10:30:00 UTC 2025 - Disk usage: 85%
Tue Feb 27 10:30:00 UTC 2025 - WARNING: Disk usage is above 80%!
```

Now, let’s **automate this task using cron jobs**.

---

## **2. Automating Log Cleanup**  

### **Scenario:**  
Your application generates **large log files** daily. To prevent storage issues, you need a script that:  
- Deletes logs **older than 7 days**.  
- Runs **automatically** every day.

---

### **Step 1: Create the Log Cleanup Script**  

```bash
vi cleanup_logs.sh
```

### **Step 2: Add the Script Content**  

```bash
#!/bin/bash

LOG_DIR="/var/log/myapp"
DAYS_TO_KEEP=7

echo "$(date) - Cleaning up logs older than $DAYS_TO_KEEP days..." | tee -a /var/log/cleanup.log
find $LOG_DIR -type f -mtime +$DAYS_TO_KEEP -exec rm -f {} \;
echo "$(date) - Cleanup complete." | tee -a /var/log/cleanup.log
```

### **Step 3: Make the Script Executable**  

```bash
chmod +x cleanup_logs.sh
```

### **Step 4: Test the Script**  

```bash
./cleanup_logs.sh
```

✅ **Expected Output:**  
```
Tue Feb 27 10:35:00 UTC 2025 - Cleaning up logs older than 7 days...
Tue Feb 27 10:35:00 UTC 2025 - Cleanup complete.
```

Now, let’s **schedule it with cron**.

---

### **Step 5: Automate Log Cleanup with Cron Jobs**  

Open the crontab:  
```bash
crontab -e
```

Add the following line to run the script every night at **midnight**:  
```bash
0 0 * * * /home/azureuser/cleanup_logs.sh
```

✅ **Verification:**  
Check scheduled jobs:  
```bash
crontab -l
```

Now, logs **older than 7 days will be automatically deleted**.

---

## **3. Automating Backups with Bash**  

### **Scenario:**  
You need to **back up critical files** every day to ensure **disaster recovery**. The script should:  
- Create a **timestamped archive**.  
- Store backups in a **designated directory**.  
- Automatically remove **old backups** after 30 days.  

---

### **Step 1: Create the Backup Script**  

```bash
vi backup_files.sh
```

### **Step 2: Add the Script Content**  

```bash
#!/bin/bash

BACKUP_DIR="/backup"
SOURCE_DIR="/home/azureuser/data"
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
BACKUP_FILE="$BACKUP_DIR/backup_$TIMESTAMP.tar.gz"

mkdir -p $BACKUP_DIR
tar -czf $BACKUP_FILE $SOURCE_DIR

echo "$(date) - Backup created: $BACKUP_FILE" | tee -a /var/log/backup.log

# Delete backups older than 30 days
find $BACKUP_DIR -type f -mtime +30 -exec rm -f {} \;
echo "$(date) - Old backups removed." | tee -a /var/log/backup.log
```

### **Step 3: Make the Script Executable**  

```bash
chmod +x backup_files.sh
```

### **Step 4: Test the Script**  

```bash
./backup_files.sh
```

✅ **Expected Output:**  
```
Tue Feb 27 10:40:00 UTC 2025 - Backup created: /backup/backup_20250227_104000.tar.gz
Tue Feb 27 10:40:00 UTC 2025 - Old backups removed.
```

Now, let’s **schedule it with cron**.

---

### **Step 5: Automate Backups with Cron Jobs**  

Open the crontab:  
```bash
crontab -e
```

Add the following line to run the backup script **every day at 2 AM**:  
```bash
0 2 * * * /home/azureuser/backup_files.sh
```

✅ **Verification:**  
Check scheduled jobs:  
```bash
crontab -l
```

Now, backups will **run automatically at 2 AM daily**.

---

## **Key Takeaways**  

- **Monitoring scripts** prevent system failures by detecting low disk space.  
- **Log cleanup scripts** remove old files, preventing disk overuse.  
- **Backup scripts** automate disaster recovery.  
- **Cron jobs** schedule and automate scripts without manual intervention.  

---

## **What’s Next?**  

Now that you have written **real-world Bash scripts**, the next step is:  
✔ **Integrating scripts into CI/CD pipelines.**  
✔ **Using Bash for cloud automation (Terraform, Ansible).**  
✔ **Optimizing and troubleshooting Bash scripts.**  

Let’s move forward and explore **advanced Bash automation techniques!**
