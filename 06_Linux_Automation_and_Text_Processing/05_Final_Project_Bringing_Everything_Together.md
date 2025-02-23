### **Final Project: Automated Log Management System**  
**Bringing Everything Together: File Searching, Text Processing, and Automation**  

---

## **Scenario: Automating Log Management in a Linux Server**  

### **Real-World Problem Statement**  

You are a **DevOps/Cloud Engineer** managing an **Azure Virtual Machine** running multiple applications. Each application generates logs that can quickly consume disk space if not managed properly.  

Your challenge is to **automate log management** to:  
✔ **Find and filter logs for critical errors.**  
✔ **Process logs to remove unnecessary information.**  
✔ **Automate log cleanup to save disk space.**  

By the end of this project, you will have a **fully automated log management system** running on your Azure VM.

---

## **Project Overview**  

| **Task** | **Description** |
|----------|---------------|
| **Locate Log Files** | Use `find` to identify old and large log files. |
| **Extract Critical Errors** | Use `grep` and `awk` to filter logs for important messages. |
| **Modify Log Formats** | Use `sed` to clean up logs and reformat entries. |
| **Automate Cleanup** | Use `cron` or `systemd timers` to delete old logs regularly. |

---

## **Step 1: Identify Log Files to Manage**  

### **1.1 Finding Log Files in the System**  
To begin, search for **all log files** in `/var/log/`:  
```bash
find /var/log -type f -name "*.log"
```
To find **log files larger than 100MB**:  
```bash
find /var/log -type f -name "*.log" -size +100M
```

✅ **Example Output:**  
```
/var/log/nginx/access.log
/var/log/nginx/error.log
/var/log/syslog
/var/log/auth.log
```

---

## **Step 2: Extract Critical Errors from Logs**  

### **2.1 Using grep to Filter Errors**  
Extract **critical errors** from system logs:  
```bash
grep -i "error" /var/log/syslog
```
✅ **Example Output:**  
```
Feb 23 14:05:12 server kernel: [12345.678] ERROR: Disk I/O failure on /dev/sdb
Feb 23 14:10:01 server app[5678]: ERROR: Database connection lost
```

To **search for multiple patterns**, use `-E` (Extended Regex):  
```bash
grep -E "error|failed|critical" /var/log/syslog
```

---

### **2.2 Using awk to Extract Specific Fields**  
To extract **timestamps and error messages only**:  
```bash
awk '{print $1, $2, $3, $NF}' /var/log/syslog | grep -E "error|failed|critical"
```
✅ **Example Output:**  
```
Feb 23 14:05:12 ERROR:
Feb 23 14:10:01 ERROR:
```

---

## **Step 3: Modify Log Formats for Better Readability**  

### **3.1 Using sed to Clean and Reformat Logs**  
Remove unnecessary information and **standardize log format**:  
```bash
sed 's/\[.*\]//g' /var/log/syslog | grep -E "error|failed|critical"
```
✅ **Before:**  
```
Feb 23 14:05:12 server kernel: [12345.678] ERROR: Disk I/O failure on /dev/sdb
```
✅ **After:**  
```
Feb 23 14:05:12 ERROR: Disk I/O failure on /dev/sdb
```

Replace IP addresses with `[REDACTED]`:  
```bash
sed -E 's/[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+/[REDACTED]/g' /var/log/auth.log
```

---

## **Step 4: Automating Log Cleanup**  

### **4.1 Deleting Old Logs with find**  
To delete logs **older than 7 days**:  
```bash
find /var/log -type f -name "*.log" -mtime +7 -exec rm -f {} \;
```

---

### **4.2 Automating Log Cleanup with a Script**  
Create a script `log_cleanup.sh`:  
```bash
vi /home/azureuser/log_cleanup.sh
```
Add the following:  
```bash
#!/bin/bash
LOG_DIR="/var/log"
find $LOG_DIR -type f -name "*.log" -mtime +7 -exec rm -f {} \;
echo "$(date) - Deleted old logs" >> /var/log/log_cleanup.log
```
Save and **make it executable**:  
```bash
chmod +x /home/azureuser/log_cleanup.sh
```

---

### **4.3 Scheduling with Cron Job**  
Open the crontab editor:  
```bash
crontab -e
```
Add the following line to **run the script every day at midnight**:  
```bash
0 0 * * * /home/azureuser/log_cleanup.sh
```

✅ **Now, log cleanup is fully automated.**

---

## **Step 5: Advanced Automation with Systemd Timers**  

Instead of `cron`, we can use **systemd timers**.

### **5.1 Create a Timer File**  
```bash
sudo vi /etc/systemd/system/log_cleanup.timer
```
Add:  
```ini
[Unit]
Description=Run log cleanup daily

[Timer]
OnCalendar=*-*-* 00:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

---

### **5.2 Create a Service File**  
```bash
sudo vi /etc/systemd/system/log_cleanup.service
```
```ini
[Unit]
Description=Log Cleanup Service

[Service]
ExecStart=/home/azureuser/log_cleanup.sh
User=azureuser
```

---

### **5.3 Enable and Start the Timer**  
```bash
sudo systemctl daemon-reload
sudo systemctl enable log_cleanup.timer
sudo systemctl start log_cleanup.timer
```

✅ **Now, logs will be automatically cleaned every day at midnight.**

---

## **Final Verification: Testing the System**  

### **Check Log Extraction**
```bash
/home/azureuser/analyze_logs.sh
```

### **Check Scheduled Cleanup**
```bash
sudo systemctl list-timers
```

---

## **Project Summary**  

| **Step** | **Task** | **Command Used** |
|----------|---------|-----------------|
| **1** | Locate old logs | `find /var/log -type f -name "*.log" -mtime +7` |
| **2** | Extract critical errors | `grep -E "error|failed|critical" /var/log/syslog` |
| **3** | Format logs | `sed -E 's/[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+/[REDACTED]/g'` |
| **4** | Automate log deletion | `find /var/log -type f -name "*.log" -mtime +7 -exec rm -f {} \;` |
| **5** | Automate with cron | `crontab -e` |
| **6** | Automate with systemd | `systemctl enable log_cleanup.timer` |

---

## **Key Takeaways**  

✔ **Log management is critical for system reliability and disk space optimization.**  
✔ **Find, grep, awk, and sed help process and extract meaningful data from logs.**  
✔ **Cron jobs automate tasks at scheduled intervals.**  
✔ **Systemd timers provide a more robust alternative to cron.**  
✔ **Automation ensures logs are monitored and managed without manual intervention.**  
