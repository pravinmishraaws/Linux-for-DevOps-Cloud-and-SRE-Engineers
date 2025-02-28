### **Monitoring and Auditing Security Logs in Linux**  

---

## **Building Upon the Previous Lesson**  

In **Lesson 5**, we secured SSH and restricted root access to **harden the Mini Finance web server** against unauthorized access. Now, we focus on **monitoring and auditing security logs** to detect potential security breaches.  

By the end of this lesson, you will be able to:  
✔ **Identify and analyze security logs in Linux.**  
✔ **Monitor failed SSH login attempts and user activities.**  
✔ **Set up real-time log monitoring and alerts.**  
✔ **Use log auditing tools to track system changes.**  

---

## **1. Why is Security Log Monitoring Important?**  

### **Real-World Scenario: Preventing Unauthorized Access to Mini Finance**  

Your **Mini Finance web server** is publicly accessible, and users log in remotely to manage the application. You receive reports of **failed SSH login attempts** and need to determine:  

- **Who is attempting unauthorized access?**  
- **Are there suspicious activities from valid users?**  
- **Have system files been modified without approval?**  

To answer these, you must **analyze security logs** and **set up alerts** for unusual activity.

---

## **2. Understanding Linux Security Logs**  

Linux maintains several log files for **security monitoring**.  

| Log File | Description |
|----------|------------|
| `/var/log/auth.log` | Authentication attempts, SSH logins, sudo actions (Ubuntu/Debian). |
| `/var/log/secure` | Authentication logs for SSH and sudo (RHEL/CentOS). |
| `/var/log/syslog` | General system logs, including user activities. |
| `/var/log/faillog` | Failed login attempts and login failures. |
| `/var/log/wtmp` | Records all login attempts (view with `last`). |
| `/var/log/btmp` | Records failed login attempts (view with `lastb`). |

---

## **3. Monitoring SSH Login Attempts**  

To track SSH access, **filter authentication logs** for login activity.

### **Step 1: View Successful SSH Logins**  
```bash
sudo grep "Accepted" /var/log/auth.log
```
✅ **Example Output:**  
```
Feb 23 10:10:05 mini-finance-server sshd[1356]: Accepted publickey for azureuser from 192.168.1.50 port 45222
```

### **Step 2: View Failed SSH Login Attempts**  
```bash
sudo grep "Failed password" /var/log/auth.log
```
✅ **Example Output:**  
```
Feb 23 10:12:45 mini-finance-server sshd[1402]: Failed password for invalid user admin from 203.0.113.25 port 50934
```
This helps identify **brute-force attempts**.

### **Step 3: List Banned IPs (If Fail2Ban is Enabled)**  
```bash
sudo fail2ban-client status sshd
```
✅ **Example Output:**  
```
Status for the jail: sshd
|- Filter
|  |- Currently failed: 5
|  |- Total failed: 25
|  `- File list: /var/log/auth.log
`- Actions
   |- Currently banned: 2
   |- Total banned: 10
   `- Banned IP list: 192.168.1.102, 203.0.113.25
```
This confirms **Fail2Ban is actively blocking attackers**.

---

## **4. Tracking User Activity and Sudo Commands**  

To see who used `sudo` and what commands were executed:  

```bash
sudo cat /var/log/auth.log | grep "sudo"
```
✅ **Example Output:**  
```
Feb 23 11:03:15 mini-finance-server sudo: azureuser : TTY=pts/1 ; PWD=/home/azureuser ; COMMAND=/bin/cat /etc/passwd
```
This helps **detect unauthorized privilege escalations**.

---

## **5. Setting Up Real-Time Log Monitoring with `tail -f`**  

To monitor logs **in real-time**, use:  

```bash
sudo tail -f /var/log/auth.log
```
This will **continuously display** new login attempts as they happen.

---

## **6. Automating Security Alerts**  

Instead of manually checking logs, set up a **script that alerts you via email** when unusual activity occurs.

### **Step 1: Create a Security Alert Script**  
```bash
sudo vi /usr/local/bin/security_monitor.sh
```
Add the following:
```bash
#!/bin/bash
LOGFILE="/var/log/auth.log"
ALERTEMAIL="admin@mini-finance.com"
ATTEMPTS=$(grep "Failed password" $LOGFILE | wc -l)

if [ "$ATTEMPTS" -gt 5 ]; then
    echo "Warning: More than 5 failed SSH login attempts detected!" | mail -s "Security Alert" $ALERTEMAIL
fi
```
### **Step 2: Make it Executable**  
```bash
sudo chmod +x /usr/local/bin/security_monitor.sh
```
### **Step 3: Schedule with Cron to Run Every Hour**  
```bash
sudo crontab -e
```
Add this line:
```
0 * * * * /usr/local/bin/security_monitor.sh
```
This will **send an alert if 5+ failed login attempts are detected in an hour**.

---

## **7. Auditing System Changes with `auditd`**  

### **Step 1: Install and Enable Auditd**  
```bash
sudo apt install auditd -y
sudo systemctl enable --now auditd
```

### **Step 2: Track Changes to Critical Files**  
To **monitor changes** to `/etc/passwd`, add an audit rule:
```bash
sudo auditctl -w /etc/passwd -p wa -k user_changes
```
✅ **Verification Step:** View audit logs:
```bash
sudo ausearch -k user_changes --start today
```

---

## **Hands-On Challenges**  

1. **Check failed SSH login attempts on your Mini Finance server.**  
```bash
sudo grep "Failed password" /var/log/auth.log
```
2. **Enable real-time log monitoring using `tail -f`.**  
3. **Set up a cron job that detects unauthorized logins and alerts via email.**  
4. **Monitor changes to critical system files using `auditd`.**  

---

## **Key Takeaways**  

✔ **Linux logs store crucial security information.**  
✔ **Authentication logs help track login attempts and failed logins.**  
✔ **Real-time monitoring helps detect security threats faster.**  
✔ **Automated scripts can send alerts when unusual activity is detected.**  
✔ **Auditd helps track file modifications for compliance and security.**  

---

## **What’s Next?**  

Now that we’ve **monitored security logs**, the next step is:  

✔ **Final Project: Implementing a Secure and Optimized Linux Serves**
