### **Lesson 3: Automating Tasks with Cron Jobs**  
**(Building on Lesson 2 – Searching Within Files and Text Processing)**  

---

## **Why Should DevOps and Cloud Engineers Learn Cron Jobs?**  

### **Real-World Problem Statement**  

Imagine you are a **DevOps/Cloud Engineer** managing an **Azure Virtual Machine** hosting a **static website**. You’ve set up **log monitoring** in the previous lesson to identify failed login attempts and HTTP errors. However, running these scripts manually is inefficient.  

Your team has requested the following automation:  
- **Daily log cleanup at midnight** to prevent logs from consuming disk space.  
- **Automated backups every Sunday at 3 AM** to ensure data safety.  
- **Periodic execution of the log analysis script** to detect security threats in real-time.  

Instead of **manually running these tasks**, we can **schedule them using cron jobs**.

By the end of this lesson, you will be able to:  
✔ **Understand cron jobs and their syntax**.  
✔ **Schedule recurring and one-time tasks** using `crontab`.  
✔ **Manage and debug cron jobs** efficiently.  
✔ **Automate file cleanup, log rotation, and backups**.  
✔ **Ensure smooth server operations with minimal manual intervention**.  

This lesson will **build upon Lesson 2** by automating log analysis, backups, and maintenance tasks.

---

## **1. What Are Cron Jobs?**  

A **cron job** is a scheduled task that runs at predefined intervals on a Linux system.  

- The cron service checks for scheduled jobs and **executes them automatically**.  
- Jobs are **stored in crontab files** (`crontab -e`).  
- Each job runs based on a **specific time pattern** (minute, hour, day, etc.).  

Now, let’s understand **cron syntax**.

---

## **2. Understanding Cron Job Syntax**  

A cron job follows this format:  

```bash
* * * * * command_to_execute
```

| Field | Description | Example |
|--------|------------|---------|
| **Minute** | (0 - 59) | `0` = At the start of the hour |
| **Hour** | (0 - 23) | `3` = 3 AM |
| **Day of Month** | (1 - 31) | `15` = Runs on the 15th |
| **Month** | (1 - 12) | `6` = Runs in June |
| **Day of Week** | (0 - 7) (Sunday = 0 or 7) | `0` = Runs on Sunday |

✅ **Example Cron Jobs**  

- **Run a script every day at midnight:**  
  ```bash
  0 0 * * * /home/azureuser/cleanup_logs.sh
  ```
- **Run a script every Sunday at 3 AM:**  
  ```bash
  0 3 * * 0 /home/azureuser/backup_script.sh
  ```
- **Run a script every 30 minutes:**  
  ```bash
  */30 * * * * /home/azureuser/analyze_logs.sh
  ```

---

## **3. Managing Cron Jobs**  

### **3.1 Listing Existing Cron Jobs**  
To see all scheduled cron jobs for the current user:  
```bash
crontab -l
```

### **3.2 Editing the Crontab File**  
To add, modify, or remove cron jobs:  
```bash
crontab -e
```
This opens the **cron editor** where you can define scheduled tasks.

### **3.3 Removing All Cron Jobs**  
```bash
crontab -r
```
Use this with caution—it **deletes all scheduled jobs** for the user.

Now, let’s automate **log cleanup**.

---

## **4. Automating Log Cleanup with Cron**  

Old logs can quickly **consume disk space**, so let’s schedule **daily cleanup**.

### **4.1 Create a Log Cleanup Script**  
```bash
vi /home/azureuser/cleanup_logs.sh
```

### **4.2 Add the Following Code**  
```bash
#!/bin/bash

LOG_DIR="/var/log/nginx"
DAYS_TO_KEEP=7

find $LOG_DIR -type f -name "*.log" -mtime +$DAYS_TO_KEEP -exec rm -f {} \;

echo "$(date) - Deleted logs older than $DAYS_TO_KEEP days" >> /var/log/cleanup.log
```

### **4.3 Make the Script Executable**  
```bash
chmod +x /home/azureuser/cleanup_logs.sh
```

### **4.4 Schedule the Script to Run Daily at Midnight**  
```bash
crontab -e
```
Add the following line:  
```bash
0 0 * * * /home/azureuser/cleanup_logs.sh
```

✅ **Now, logs older than 7 days will be deleted automatically**.

---

## **5. Automating Backups with Cron**  

To ensure data safety, let’s schedule **weekly backups**.

### **5.1 Create a Backup Script**  
```bash
vi /home/azureuser/backup_script.sh
```

### **5.2 Add the Following Code**  
```bash
#!/bin/bash

BACKUP_DIR="/var/backups"
SOURCE_DIR="/var/www/mini_finance"
BACKUP_FILE="$BACKUP_DIR/backup_$(date +%F).tar.gz"

mkdir -p $BACKUP_DIR
tar -czf $BACKUP_FILE $SOURCE_DIR

echo "$(date) - Backup created: $BACKUP_FILE" >> /var/log/backup.log
```

### **5.3 Make the Script Executable**  
```bash
chmod +x /home/azureuser/backup_script.sh
```

### **5.4 Schedule the Backup Job**  
```bash
crontab -e
```
Add the following line:  
```bash
0 3 * * 0 /home/azureuser/backup_script.sh
```

✅ **Now, backups will be created every Sunday at 3 AM**.

---

## **6. Debugging Cron Jobs**  

If a cron job doesn’t run as expected, use these debugging techniques.

### **6.1 Check Cron Logs**  
```bash
sudo cat /var/log/syslog | grep CRON
```

### **6.2 Redirect Output to a Log File**  
Modify a cron job to log its output:  
```bash
0 0 * * * /home/azureuser/cleanup_logs.sh >> /var/log/cron_output.log 2>&1
```

### **6.3 Run the Script Manually**  
```bash
/home/azureuser/cleanup_logs.sh
```
If there are issues, **fix errors before adding to cron**.

---

## **7. Real-World Practical Exercise**  

### **Task 1: Schedule a Cron Job to Delete Temporary Files**  
1. Create a script to **delete all `.tmp` files** in `/tmp`.  
2. Schedule it to run **every day at midnight**.  

✅ **Solution**  
```bash
vi /home/azureuser/cleanup_tmp.sh
```
```bash
#!/bin/bash
find /tmp -type f -name "*.tmp" -mtime +1 -exec rm -f {} \;
```
```bash
chmod +x /home/azureuser/cleanup_tmp.sh
```
```bash
crontab -e
```
```bash
0 0 * * * /home/azureuser/cleanup_tmp.sh
```

### **Task 2: Automate Log Analysis Every Hour**  
1. Schedule `analyze_logs.sh` to **run every hour**.  
```bash
crontab -e
```
```bash
0 * * * * /home/azureuser/analyze_logs.sh
```

✅ **Now, security monitoring runs every hour automatically**.

---

## **Key Takeaways**  

✔ **Cron jobs automate recurring system tasks**, reducing manual effort.  
✔ **Crontab syntax** defines execution time with minute, hour, day, month, and weekday.  
✔ **Managing cron jobs** includes listing (`crontab -l`), editing (`crontab -e`), and deleting (`crontab -r`).  
✔ **Real-world automation includes log cleanup, backups, and security monitoring**.  
✔ **Debugging cron jobs ensures reliability** (`sudo cat /var/log/syslog | grep CRON`).  
