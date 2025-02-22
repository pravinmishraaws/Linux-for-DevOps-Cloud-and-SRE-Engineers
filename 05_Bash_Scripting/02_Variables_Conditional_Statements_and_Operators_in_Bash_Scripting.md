### **Lesson 2: Variables, Conditional Statements, and Operators in Bash Scripting**  

Imagine you are a **DevOps/Cloud Engineer** managing a fleet of Azure Virtual Machines. In **Lesson 1**, you created a **basic system monitoring script** that logs hostname, uptime, and logged-in users.  

But what if you needed to:  
- **Check CPU load and take action if it's too high?**  
- **Log warnings when system uptime is too short (indicating frequent restarts)?**  
- **Send alerts when the number of logged-in users exceeds a threshold?**  

These tasks require the use of **variables, conditional statements, and operators** to make scripts more **dynamic and responsive**.

By the end of this lesson, you will be able to:  
✔ **Use variables to store and manipulate system data.**  
✔ **Implement `if-else` conditions to handle different scenarios.**  
✔ **Use comparison and logical operators for decision-making.**  
✔ **Enhance the previous script with dynamic monitoring.**  

This lesson builds on **Lesson 1**, adding **intelligent decision-making capabilities** to the existing script.

---

## **1. Understanding Variables in Bash**  

### **What is a Variable?**  
A **variable** is a way to store information that can change during script execution.  

### **Declaring and Using a Variable**  
```bash
MY_SERVER="Azure Virtual Machine"
echo "Monitoring system: $MY_SERVER"
```

### **Storing Command Output in a Variable**  
```bash
CURRENT_TIME=$(date)
echo "Script executed at: $CURRENT_TIME"
```

Now, let’s apply variables in our script.

---

## **2. Updating the System Monitoring Script with Variables**  

We will modify our **Lesson 1 script** to include:  
✔ **Variables to store system data dynamically**  
✔ **Conditional checks to log warnings**  

---

### **Updated `system_info.sh` Script**
```bash
#!/bin/bash
LOGFILE="/var/log/system_info.log"
HOSTNAME=$(hostname)
UPTIME=$(uptime -p)
LOGGED_USERS=$(who -q | head -n 1)
CPU_LOAD=$(uptime | awk '{print $(NF-2)}' | sed 's/,//')

echo "$(date) - System Information:" | tee -a $LOGFILE
echo "-------------------" | tee -a $LOGFILE
echo "Hostname: $HOSTNAME" | tee -a $LOGFILE
echo "Uptime: $UPTIME" | tee -a $LOGFILE
echo "Logged in users: $LOGGED_USERS" | tee -a $LOGFILE
echo "CPU Load: $CPU_LOAD" | tee -a $LOGFILE
```

✅ **What’s New?**  
- **Stored values in variables** (`HOSTNAME`, `UPTIME`, `CPU_LOAD`).  
- **Logs CPU load for additional monitoring**.  

---

## **3. Implementing Conditional Logic in the Script**  

### **Adding a Warning for Short Uptime**  
We’ll log a **warning** if the system has been up for **less than 5 minutes**.

```bash
#!/bin/bash
LOGFILE="/var/log/system_info.log"
UPTIME_SECONDS=$(awk '{print $1}' /proc/uptime | cut -d. -f1)

if [ "$UPTIME_SECONDS" -lt 300 ]; then
    echo "$(date) - WARNING: System uptime is less than 5 minutes!" | tee -a $LOGFILE
fi
```

✅ **Why?**  
If the server **restarts too frequently**, this warning helps identify the issue.

---

### **Adding CPU Load Monitoring**  
We’ll categorize CPU load as **Low, Moderate, or High**.

```bash
if (( $(echo "$CPU_LOAD < 1.5" | bc -l) )); then
    echo "$(date) - CPU Load is Low ($CPU_LOAD)" | tee -a $LOGFILE
elif (( $(echo "$CPU_LOAD >= 1.5 && $CPU_LOAD < 3.5" | bc -l) )); then
    echo "$(date) - CPU Load is Moderate ($CPU_LOAD)" | tee -a $LOGFILE
else
    echo "$(date) - WARNING: High CPU Load detected! ($CPU_LOAD)" | tee -a $LOGFILE
fi
```

✅ **What’s New?**  
- **Classifies CPU load into different levels**.  
- **Logs warnings when CPU load is too high**.  

---

### **Final Updated `system_info.sh` Script with Conditionals**
```bash
#!/bin/bash
LOGFILE="/var/log/system_info.log"
HOSTNAME=$(hostname)
UPTIME=$(uptime -p)
LOGGED_USERS=$(who -q | head -n 1)
CPU_LOAD=$(uptime | awk '{print $(NF-2)}' | sed 's/,//')
UPTIME_SECONDS=$(awk '{print $1}' /proc/uptime | cut -d. -f1)

echo "$(date) - System Information:" | tee -a $LOGFILE
echo "-------------------" | tee -a $LOGFILE
echo "Hostname: $HOSTNAME" | tee -a $LOGFILE
echo "Uptime: $UPTIME" | tee -a $LOGFILE
echo "Logged in users: $LOGGED_USERS" | tee -a $LOGFILE
echo "CPU Load: $CPU_LOAD" | tee -a $LOGFILE

# Check uptime
if [ "$UPTIME_SECONDS" -lt 300 ]; then
    echo "$(date) - WARNING: System uptime is less than 5 minutes!" | tee -a $LOGFILE
fi

# Check CPU Load
if (( $(echo "$CPU_LOAD < 1.5" | bc -l) )); then
    echo "$(date) - CPU Load is Low ($CPU_LOAD)" | tee -a $LOGFILE
elif (( $(echo "$CPU_LOAD >= 1.5 && $CPU_LOAD < 3.5" | bc -l) )); then
    echo "$(date) - CPU Load is Moderate ($CPU_LOAD)" | tee -a $LOGFILE
else
    echo "$(date) - WARNING: High CPU Load detected! ($CPU_LOAD)" | tee -a $LOGFILE
fi
```

---

## **4. Running the Script and Viewing Logs**  

### **Make the Script Executable**
```bash
chmod +x system_info.sh
```

### **Run the Script**
```bash
./system_info.sh
```

### **View Logs**
```bash
cat /var/log/system_info.log
```

✅ **Example Log Output**
```
Sat Feb 22 14:30:05 UTC 2025 - System Information:
-------------------
Hostname: my-azure-vm
Uptime: up 3 minutes
Logged in users: user1, user2
CPU Load: 4.2
Sat Feb 22 14:30:05 UTC 2025 - WARNING: System uptime is less than 5 minutes!
Sat Feb 22 14:30:05 UTC 2025 - WARNING: High CPU Load detected! (4.2)
```

---

## **5. Automating the Script Execution**  

To **run this script every hour**, add it to **cron**.

### **Step 1: Open Crontab**
```bash
crontab -e
```

### **Step 2: Add the Following Line**
```bash
0 * * * * /home/azureuser/system_info.sh
```

Now, the script **logs system data every hour and triggers warnings** when needed.

---

## **Key Takeaways**  

- **Bash variables store dynamic data** used within scripts.  
- **If-else statements allow scripts to make decisions.**  
- **Case statements help classify system metrics (CPU load).**  
- **Logical operators (`&&`, `||`) enable multi-condition checks.**  
- **Automating the script using cron ensures continuous monitoring.**  

---

## **What’s Next?**  

Now that we’ve introduced **variables and conditionals**, the next step is:  

- **Using loops to iterate over system data.**  
- **Processing user input dynamically in scripts.**  
- **Enhancing system automation with more advanced Bash techniques.**  

Let’s move forward and explore **Bash Loops and User Input in Lesson 3**.
