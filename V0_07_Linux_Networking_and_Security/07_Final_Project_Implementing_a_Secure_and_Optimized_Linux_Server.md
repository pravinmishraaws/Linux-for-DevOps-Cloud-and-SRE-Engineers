### **Final Project: Implementing a Secure and Optimized Linux Server for Mini Finance**  

---

## **Project Overview**  

In this final project, you will **apply all the concepts** learned in this module to **harden and optimize the Mini Finance web server** running on your Azure VM. By the end of this project, you will have a **secure, well-optimized, and automated Linux server** that follows industry best practices for **networking, security, and performance tuning**.  

✔ **Project Goal:**  
Build a **secure and optimized Mini Finance web server** using Linux security and networking best practices.  

✔ **Skills Applied:**  
- **Networking:** Configuring IP addresses, DNS, and firewall rules.  
- **Security:** Securing SSH, managing user permissions, and configuring a firewall.  
- **Monitoring & Auditing:** Tracking logs, detecting anomalies, and setting up alerts.  
- **Automation:** Using cron jobs and scripting for security & maintenance tasks.  

---

## **Scenario: Hardening the Mini Finance Web Server**  

### **Business Requirement:**  
Your company, Mini Finance, has launched a **public-facing financial website** hosted on an **Azure Virtual Machine**. Security analysts have flagged potential security risks, and performance testing has identified **network bottlenecks**.  

You are responsible for:  
✔ **Securing remote access to the server** to prevent unauthorized SSH logins.  
✔ **Configuring firewall rules** to restrict access to only necessary services.  
✔ **Monitoring and logging** security events to detect potential attacks.  
✔ **Automating security updates and system maintenance tasks.**  

---

## **Project Steps**  

### **Step 1: Network Configuration & Security**  

**1.1 Verify Network Settings**  
- Check public and private IPs:  
  ```bash
  ip a
  ```

**1.2 Configure the Firewall**  
- Allow only SSH, HTTP, and HTTPS traffic:  
  ```bash
  sudo ufw allow OpenSSH
  sudo ufw allow 80/tcp
  sudo ufw allow 443/tcp
  sudo ufw enable
  ```

---

### **Step 2: Securing SSH Access**  

**2.1 Disable Root SSH Access**  
Edit SSH configuration:  
```bash
sudo vi /etc/ssh/sshd_config
```
Change:
```
PermitRootLogin no
```
Restart SSH:  
```bash
sudo systemctl restart ssh
```

**2.2 Enable SSH Key Authentication (If Not Already Done)**  
- Ensure SSH keys are required:  
  ```bash
  sudo vi /etc/ssh/sshd_config
  ```
  Change:
  ```
  PasswordAuthentication no
  ```
  Restart SSH:  
  ```bash
  sudo systemctl restart ssh
  ```

---

### **Step 3: Monitoring & Log Auditing**  

**3.1 Track Failed SSH Logins**  
```bash
sudo grep "Failed password" /var/log/auth.log
```

**3.2 Set Up Real-Time Security Monitoring**  
Create a script to detect failed SSH login attempts:  
```bash
sudo vi /usr/local/bin/security_alert.sh
```
Add:
```bash
#!/bin/bash
LOGFILE="/var/log/auth.log"
ALERTEMAIL="admin@mini-finance.com"
ATTEMPTS=$(grep "Failed password" $LOGFILE | wc -l)

if [ "$ATTEMPTS" -gt 5 ]; then
    echo "Warning: More than 5 failed SSH login attempts detected!" | mail -s "Security Alert" $ALERTEMAIL
fi
```
Make it executable:
```bash
sudo chmod +x /usr/local/bin/security_alert.sh
```
Schedule it to run every hour:
```bash
sudo crontab -e
```
Add:
```
0 * * * * /usr/local/bin/security_alert.sh
```

---

### **Step 4: Optimizing the Server for Performance**  

**4.1 Enable HTTP/2 for Faster Page Loads**  
Modify the Nginx config:  
```bash
sudo vi /etc/nginx/sites-available/mini_finance
```
Change:  
```
listen 443 ssl http2;
```
Restart Nginx:
```bash
sudo systemctl restart nginx
```

**4.2 Enable Gzip Compression for Web Content**  
```bash
sudo vi /etc/nginx/nginx.conf
```
Add:
```
gzip on;
gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
```
Restart Nginx:
```bash
sudo systemctl restart nginx
```

---

### **Step 5: Automating Security & Maintenance**  

**5.1 Set Up Automatic Security Updates**  
```bash
sudo apt install unattended-upgrades -y
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

**5.2 Schedule Log Cleanup with Cron Jobs**  
```bash
sudo vi /usr/local/bin/log_cleanup.sh
```
Add:
```bash
#!/bin/bash
find /var/log -name "*.log" -mtime +7 -exec rm -f {} \;
echo "Log cleanup completed on $(date)" >> /var/log/log_cleanup.log
```
Make it executable:
```bash
sudo chmod +x /usr/local/bin/log_cleanup.sh
```
Schedule it to run daily:
```bash
sudo crontab -e
```
Add:
```
0 0 * * * /usr/local/bin/log_cleanup.sh
```

---

## **Final Verification**  

✔ **Check Network Connectivity**  
```bash
ping -c 3 google.com
```

✔ **Verify Firewall Rules**  
```bash
sudo ufw status
```

✔ **Test SSH Security**  
Try logging in from an unauthorized IP and check logs:
```bash
sudo tail -f /var/log/auth.log
```

✔ **Check if Automated Tasks are Running**  
List active cron jobs:
```bash
crontab -l
```

---

## **Project Completion**  

Congratulations! You have successfully **secured and optimized** the Mini Finance web server by:  
✔ Implementing **firewall rules** to restrict access.  
✔ **Hardening SSH** to prevent unauthorized access.  
✔ Setting up **real-time monitoring and alerts** for security events.  
✔ Automating **log cleanup and security updates** for long-term stability.  

