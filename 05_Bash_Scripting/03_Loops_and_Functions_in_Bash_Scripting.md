### **Lesson 3: Loops and Functions in Bash Scripting**  

---

Imagine you are a **DevOps/Cloud Engineer** managing multiple **Azure Virtual Machines**. You already have a script that collects system information, but now you need to:  

- **Monitor multiple system metrics dynamically** (CPU, Memory, Disk).  
- **Check multiple services and restart them if needed.**  
- **Make the script modular so you can add new monitoring checks without rewriting code.**  

Instead of **writing the same logic multiple times**, **loops** allow us to **iterate** over multiple values, and **functions** allow us to **reuse code efficiently**.

By the end of this lesson, you will be able to:  
✔ **Use `for`, `while`, and `until` loops to automate repetitive tasks.**  
✔ **Write functions to improve script modularity and reusability.**  
✔ **Enhance the `system_info.sh` script to dynamically check system health.**  

This lesson **builds on Lesson 2**, making the script **more scalable and efficient**.

---

## **1. Using Loops in Bash**  

Loops help execute commands multiple times, **reducing manual effort**.  

### **Types of Loops in Bash**  

| **Loop Type** | **Usage** |
|--------------|----------|
| `for` loop | Iterates over a list of values. |
| `while` loop | Runs while a condition is **true**. |
| `until` loop | Runs until a condition becomes **true**. |

---

## **2. Improving System Monitoring with Loops**  

Our **existing script** only checks basic system information **once**. Let's improve it to **monitor multiple metrics dynamically**.

### **Example: Using a `for` Loop to Check Multiple Services**
```bash
#!/bin/bash

LOGFILE="/var/log/system_health.log"

log_message() {
    echo "$(date) - $1" | tee -a $LOGFILE
}

check_services() {
    SERVICES=("nginx" "apache2" "mysql")

    for SERVICE in "${SERVICES[@]}"; do
        if systemctl is-active --quiet "$SERVICE"; then
            log_message "$SERVICE is running."
        else
            log_message "WARNING: $SERVICE is NOT running!"
            log_message "Attempting to restart $SERVICE..."
            sudo systemctl restart "$SERVICE"
            if systemctl is-active --quiet "$SERVICE"; then
                log_message "$SERVICE successfully restarted."
            else
                log_message "ERROR: Failed to restart $SERVICE!"
            fi
        fi
    done
}
```

✅ **What’s New?**  
- The script now **iterates over multiple services** and restarts them if needed.  
- Logs all actions in **`/var/log/system_health.log`**.  

---

## **3. Using a `while` Loop to Continuously Monitor System Health**  

Instead of **running the script manually**, let’s make it **run continuously** every 10 seconds.

```bash
monitor_system() {
    while true; do
        log_message "Checking system health..."
        check_services
        sleep 10
    done
}
```

✅ **What’s New?**  
- The script now **monitors system health continuously**.  
- It **logs events** at regular intervals.  

---

## **4. Using Functions for Modular Code**  

Now, let’s **refactor the existing `system_info.sh`** script to use **functions and loops**.

```bash
#!/bin/bash

LOGFILE="/var/log/system_health.log"

log_message() {
    echo "$(date) - $1" | tee -a $LOGFILE
}

check_system_info() {
    log_message "Collecting System Information..."
    log_message "-----------------------------"
    log_message "Hostname: $(hostname)"
    log_message "Uptime: $(uptime -p)"
    log_message "CPU Load: $(uptime | awk '{print $(NF-2)}' | sed 's/,//')"
    log_message "Memory Usage: $(free -h | awk '/Mem:/ {print $3 "/" $2}')"
    log_message "Disk Usage: $(df -h / | awk 'NR==2 {print $5}')"
}

check_services() {
    SERVICES=("nginx" "apache2" "mysql")

    for SERVICE in "${SERVICES[@]}"; do
        if systemctl is-active --quiet "$SERVICE"; then
            log_message "$SERVICE is running."
        else
            log_message "WARNING: $SERVICE is NOT running!"
            log_message "Attempting to restart $SERVICE..."
            sudo systemctl restart "$SERVICE"
            if systemctl is-active --quiet "$SERVICE"; then
                log_message "$SERVICE successfully restarted."
            else
                log_message "ERROR: Failed to restart $SERVICE!"
            fi
        fi
    done
}

monitor_system() {
    while true; do
        log_message "Checking system health..."
        check_system_info
        check_services
        sleep 10
    done
}

# Execute the monitoring function
monitor_system
```

✅ **What’s New?**  
- **Refactored script into functions** for better readability.  
- Added **CPU, memory, and disk usage checks**.  
- **Loop runs continuously** every 10 seconds to monitor system health.  

---

## **5. Running and Testing the Script**  

### **Make the Script Executable**
```bash
chmod +x system_info.sh
```

### **Run the Script**
```bash
./system_info.sh
```

✅ **View Logs**
```bash
cat /var/log/system_health.log
```

✅ **Example Log Output**
```
Sat Feb 22 15:10:00 UTC 2025 - Checking system health...
Sat Feb 22 15:10:00 UTC 2025 - Collecting System Information...
Sat Feb 22 15:10:00 UTC 2025 - Hostname: my-azure-vm
Sat Feb 22 15:10:00 UTC 2025 - Uptime: up 5 hours
Sat Feb 22 15:10:00 UTC 2025 - CPU Load: 4.2
Sat Feb 22 15:10:00 UTC 2025 - Memory Usage: 3.5G/8G
Sat Feb 22 15:10:00 UTC 2025 - Disk Usage: 78%
Sat Feb 22 15:10:00 UTC 2025 - Checking Services...
Sat Feb 22 15:10:00 UTC 2025 - WARNING: nginx is NOT running!
Sat Feb 22 15:10:00 UTC 2025 - Attempting to restart nginx...
Sat Feb 22 15:10:00 UTC 2025 - nginx successfully restarted.
```

---

## **6. Scheduling the Script to Run at Startup**  

If you want this script to **start automatically on system reboot**, add it to **crontab**.

### **Open Crontab**
```bash
crontab -e
```

### **Add the Following Line**
```bash
@reboot /home/azureuser/system_info.sh
```

Now, the script will **start monitoring automatically when the server reboots**.

---

## **Key Takeaways**  

- **Loops automate repetitive tasks**, reducing manual work.  
- **Functions improve script readability and reusability.**  
- **Using loops, we can check multiple system metrics efficiently.**  
- **Refactoring scripts with functions makes them modular and maintainable.**  
- **Scheduling the script with cron ensures automated execution.**  

---

## **What’s Next?**  

Now that you understand **loops and functions**, the next step is:  

- **Handling user input dynamically in Bash scripts.**  
- **Reading and processing command-line arguments.**  
- **Creating interactive scripts for system automation.**  

Let’s move forward and explore **User Input and Command-Line Arguments in Lesson 4**.
