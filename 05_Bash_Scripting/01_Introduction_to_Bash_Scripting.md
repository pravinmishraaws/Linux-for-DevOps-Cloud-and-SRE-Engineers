### **Lesson 1: Introduction to Bash Scripting**

---

## **Why Should DevOps and Cloud Engineers Learn Bash Scripting?**  

### **Real-World Problem Statement**  

Imagine you are a **DevOps/Cloud Engineer** managing a fleet of servers in an **Azure environment**. Your team depends on you to automate **server provisioning, log rotation, and system monitoring**. However, doing these tasks manually across multiple servers is **time-consuming and prone to errors**.  

Without **automation**, managing these tasks becomes overwhelming. This is where **Bash scripting** comes in.  

By the end of this lesson, you will be able to:  
- Understand **what Bash scripting is and why it is used**.  
- Write and execute your **first Bash script**.  
- Automate system tasks in an **Azure Virtual Machine**.  

This lesson lays the **foundation** for automation, and as we progress, we will **build real-world scripts** that DevOps and Cloud Engineers use in production.

---

## **1. What is Bash Scripting?**  

A **Bash script** is a text file containing a series of **commands** executed sequentially by the **Bash shell**. These commands can:  
- Automate repetitive system tasks.  
- Manage servers and cloud infrastructure.  
- Perform system monitoring and logging.  

### **Common Use Cases in DevOps & Cloud**  

| **Use Case** | **Example Task** |
|-------------|------------------|
| **Infrastructure Automation** | Provisioning servers, installing software. |
| **System Monitoring** | Checking disk space, monitoring CPU usage. |
| **Log Management** | Automating log rotation, compressing old logs. |
| **Backup and Recovery** | Scheduling automated backups. |
| **CI/CD Pipelines** | Running test suites, deploying applications. |

---

## **2. Why Use Bash Scripting?**  

Most Linux distributions **already include Bash**, making it:  
- **Lightweight** – No need for additional installations.  
- **Powerful** – Can interact with files, processes, and system resources.  
- **Universal** – Works across different cloud environments (**AWS, Azure, GCP**).  

For example, suppose you need to:  
- **Check if an application is running** and restart it if needed.  
- **Delete logs older than 7 days** to free up space.  
- **Create users and set permissions** automatically on a new server.  

Instead of performing these tasks manually, you can **write a Bash script once and let it run automatically**.

---

## **3. Setting Up Your Environment**  

### **Where Will You Run the Script?**  
For this course, we will run Bash scripts inside an **Azure Virtual Machine**. Ensure that:  
- You have an **Azure VM** running **Ubuntu 20.04 or later**.  
- You can access the VM using **SSH**.  

### **How to Connect to Your Azure VM?**  
If using **Linux/macOS**, open a terminal and connect via SSH:  
```bash
ssh azureuser@your-vm-ip
```
If using **Windows**, you can use **PowerShell** or **PuTTY**.

### **Do You Need Root Access?**  
- Some scripts require administrative privileges.  
- Use `sudo` before commands when necessary.  

---

## **4. Writing Your First Bash Script**  

Now, let’s create a simple **Bash script** that **displays system information**.

### **Step 1: Create a New Script File**  
```bash
vi system_info.sh
```
Press **`i`** to enter **insert mode**.

### **Step 2: Add the Shebang (`#!`)**  
The **shebang** (`#!`) tells the system which interpreter to use.  
```bash
#!/bin/bash
```

### **Step 3: Add Commands to the Script**  
```bash
#!/bin/bash
echo "System Information:"
echo "-------------------"
echo "Hostname: $(hostname)"
echo "Uptime: $(uptime -p)"
echo "Logged in users: $(who -q)"
```

### **Step 4: Save and Exit**  
1. Press **`Esc`** to exit **insert mode**.  
2. Type **`:wq`** and press **Enter** to save and exit.  

---

## **5. Executing a Bash Script**  

### **Make the Script Executable**  
```bash
chmod +x system_info.sh
```

### **Run the Script**  
```bash
./system_info.sh
```

Expected Output:  
```
System Information:
-------------------
Hostname: my-azure-vm
Uptime: up 2 hours, 5 minutes
Logged in users: user1, user2
```

Now, you have successfully created and executed your **first Bash script**.

---

## **6. Automating a Task with a Script**  

Let’s take a **real-world problem**: You are managing an **Azure Virtual Machine**, and you need to **monitor disk usage** regularly.

### **Step 1: Create a New Script File**  
```bash
vi check_disk.sh
```

### **Step 2: Add the Script Content**  
```bash
#!/bin/bash
THRESHOLD=80
USAGE=$(df -h / | grep / | awk '{print $5}' | sed 's/%//')

if [ "$USAGE" -gt "$THRESHOLD" ]; then
    echo "Warning: Disk usage is above $THRESHOLD%!"
else
    echo "Disk usage is under control."
fi
```

### **Step 3: Save and Make it Executable**  
```bash
chmod +x check_disk.sh
```

### **Step 4: Run the Script**  
```bash
./check_disk.sh
```

**Expected Output (if disk usage is high):**  
```
Warning: Disk usage is above 80%!
```

**Expected Output (if disk usage is normal):**  
```
Disk usage is under control.
```

Now, instead of manually checking disk space, you have a **script that automates it**.

---

## **7. Scheduling a Script to Run Automatically**  

Since manually running scripts is not ideal, we can **schedule them to run automatically** using **cron jobs**.

### **Step 1: Open the Crontab**
```bash
crontab -e
```

### **Step 2: Add the Following Line to Run the Script Every Hour**
```bash
0 * * * * /home/azureuser/check_disk.sh
```

Now, the script will **run every hour** and alert you if disk space is running low.

---

## **8. Best Practices for Bash Scripting**  

- **Always use the `#!/bin/bash` shebang** to specify the interpreter.  
- **Use comments (`#`)** to explain what each part of the script does.  
- **Validate input** to prevent errors (e.g., check if a file exists before deleting it).  
- **Test scripts in a safe environment** before running in production.  
- **Log important actions** to a file for future reference.  

Example: Adding Logging to the Disk Check Script  

```bash
#!/bin/bash
LOGFILE="/var/log/disk_usage.log"
THRESHOLD=80
USAGE=$(df -h / | grep / | awk '{print $5}' | sed 's/%//')

if [ "$USAGE" -gt "$THRESHOLD" ]; then
    echo "$(date) - Warning: Disk usage is above $THRESHOLD%!" | tee -a $LOGFILE
else
    echo "$(date) - Disk usage is under control." | tee -a $LOGFILE
fi
```

---

## **Key Takeaways**  

- **Bash scripting automates repetitive system tasks**, making it essential for DevOps and Cloud Engineers.  
- A **Bash script** is a text file containing a sequence of commands executed by the Bash shell.  
- Scripts can **monitor system health**, such as **disk space usage**, and **automate actions** when necessary.  
- **Scheduling scripts with cron** allows for automation without manual intervention.  

---

## **What’s Next?**  

Now that you know how to **write and execute basic Bash scripts**, the next step is to **add logic and decision-making** using **variables, conditionals, and operators**.

Let’s move forward and explore **Bash Variables, Conditionals, and Operators** in the next lesson.
