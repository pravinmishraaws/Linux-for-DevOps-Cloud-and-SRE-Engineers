### **Lesson 4: Introduction to Systemd and Service Management**  
**(Building on Lesson 3 – Automating Tasks with Cron Jobs)**  

---

## **Why Should DevOps and Cloud Engineers Learn Systemd?**  

### **Real-World Problem Statement**  

Imagine you are a **DevOps/Cloud Engineer** managing a web application on an **Azure Virtual Machine**. You have scheduled **cron jobs** to automate tasks like log cleanup and backups. However, your team reports that the **web server (Nginx) occasionally crashes**, requiring a manual restart.  

You need a **more reliable way** to:  
- **Automatically restart services** if they crash.  
- **Ensure critical processes start on boot**.  
- **Monitor and control system services** in a structured way.  

This is where **systemd** comes in.

By the end of this lesson, you will be able to:  
✔ **Understand systemd and its role in managing services**.  
✔ **Start, stop, enable, and restart services using systemctl**.  
✔ **Create custom systemd services** to manage scripts and applications.  
✔ **Use systemd timers as an alternative to cron jobs**.  
✔ **Monitor service logs and troubleshoot failures**.  

This lesson will **build upon Lesson 3** by automating system services and improving reliability.

---

## **1. What is Systemd?**  

**Systemd** is the default **init system** on most modern Linux distributions, including **Ubuntu (Azure VMs)**. It is responsible for:  
- **Managing services** (starting, stopping, restarting).  
- **Handling system startup** (boot process).  
- **Monitoring system health** (logging and tracking failures).  

### **Why Use Systemd Instead of Cron?**  

| Feature | Cron Jobs | Systemd Services |
|---------|----------|-----------------|
| **For Running Commands at Intervals** | ✅ Yes | ✅ Yes (using timers) |
| **Automatically Restart on Failure** | ❌ No | ✅ Yes |
| **Runs on System Boot** | ❌ No (only if scheduled) | ✅ Yes |
| **Logging & Debugging** | ❌ Limited | ✅ Centralized logs (`journalctl`) |
| **Can Be Used for Background Services** | ❌ No | ✅ Yes |

Now, let’s start managing services with `systemctl`.

---

## **2. Managing System Services with systemctl**  

### **2.1 Checking the Status of a Service**  
To check if Nginx is running:  
```bash
sudo systemctl status nginx
```
✅ **Example Output (Active Service):**  
```
● nginx.service - A high performance web server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2025-02-24 14:00:12 UTC; 10min ago
```
✅ **Example Output (Failed Service):**  
```
● nginx.service - A high performance web server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: failed (Result: exit-code) since Mon 2025-02-24 14:05:45 UTC
```

---

### **2.2 Starting and Stopping Services**  
To start a service:  
```bash
sudo systemctl start nginx
```
To stop a service:  
```bash
sudo systemctl stop nginx
```

---

### **2.3 Restarting and Reloading Services**  
If a service crashes or needs reloading after configuration changes:  
```bash
sudo systemctl restart nginx
```
If you only need to **reload the configuration** without stopping the service:  
```bash
sudo systemctl reload nginx
```

---

### **2.4 Enabling and Disabling Services**  
To ensure a service **starts automatically on boot**:  
```bash
sudo systemctl enable nginx
```
To **disable** automatic startup:  
```bash
sudo systemctl disable nginx
```

---

### **2.5 Checking Failed Services**  
List all failed services:  
```bash
systemctl --failed
```

Now, let’s create a **custom systemd service**.

---

## **3. Creating a Custom Systemd Service**  

In the previous lesson, we scheduled `analyze_logs.sh` using **cron**. Now, we’ll create a **systemd service** to run the script and restart automatically if it fails.

---

### **3.1 Create a Custom Service File**  
```bash
sudo vi /etc/systemd/system/log_monitor.service
```

Add the following content:  
```ini
[Unit]
Description=Log Monitoring Service
After=network.target

[Service]
ExecStart=/home/azureuser/analyze_logs.sh
Restart=always
User=azureuser
WorkingDirectory=/home/azureuser
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

✅ **Explanation:**  
- **ExecStart** → The script that runs.  
- **Restart=always** → Restarts if it crashes.  
- **User=azureuser** → Runs as a non-root user.  

---

### **3.2 Reload Systemd and Start the Service**  
```bash
sudo systemctl daemon-reload
sudo systemctl start log_monitor.service
```

---

### **3.3 Enable the Service to Start on Boot**  
```bash
sudo systemctl enable log_monitor.service
```

✅ **Now, even if the system reboots or the script fails, it will restart automatically.**

---

## **4. Monitoring and Debugging Services**  

### **4.1 Viewing Service Logs**  
To check logs for a specific service:  
```bash
sudo journalctl -u log_monitor.service --no-pager
```

### **4.2 Viewing Logs in Real-Time**  
```bash
sudo journalctl -u log_monitor.service -f
```

---

## **5. Using Systemd Timers Instead of Cron Jobs**  

Instead of **cron jobs**, we can use **systemd timers** to **schedule tasks**.

### **5.1 Create a Timer Unit**  
```bash
sudo vi /etc/systemd/system/log_cleanup.timer
```
Add the following:  
```ini
[Unit]
Description=Run log cleanup script every day

[Timer]
OnCalendar=*-*-* 00:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

---

### **5.2 Create the Corresponding Service**  
```bash
sudo vi /etc/systemd/system/log_cleanup.service
```
```ini
[Unit]
Description=Log Cleanup Service

[Service]
ExecStart=/home/azureuser/cleanup_logs.sh
User=azureuser
```

---

### **5.3 Reload Systemd and Start the Timer**  
```bash
sudo systemctl daemon-reload
sudo systemctl enable log_cleanup.timer
sudo systemctl start log_cleanup.timer
```

✅ **Now, logs will be cleaned automatically at midnight without cron.**  

---

## **6. Real-World Practical Exercise**  

### **Task 1: Ensure Nginx Restarts If It Fails**  
1. Create a **custom systemd unit file** for Nginx with `Restart=always`.  
2. Test by stopping Nginx and verifying it restarts automatically.

### **Task 2: Automate Log Cleanup Using a Systemd Timer**  
1. Schedule `cleanup_logs.sh` using a **systemd timer** instead of cron.  
2. Verify the logs are cleaned up daily at midnight.

---

## **Key Takeaways**  

✔ **Systemd is the default service manager on modern Linux systems**.  
✔ **`systemctl` is used to start, stop, enable, and check services**.  
✔ **Custom systemd services** allow automation beyond cron jobs.  
✔ **Systemd timers provide a more robust alternative to cron jobs**.  
✔ **Logs and monitoring using `journalctl` help debug failing services**.  
