### **Lesson 1: Introduction to Bash Scripting**

---

## **Why Should DevOps and Cloud Engineers Learn Bash Scripting?**

### **Real-World Problem Statement**

Imagine you are a **DevOps/Cloud Engineer** managing a fleet of servers in an **Azure environment**. Your team depends on you to automate **server provisioning, log rotation, and system monitoring**. However, doing these tasks manually across multiple servers is **time-consuming and prone to errors**.

In an enterprise environment with hundreds of virtual machines, manual checks are impractical. **Bash scripting ensures efficiency, consistency, and reliability**, allowing you to scale operations seamlessly.

By the end of this lesson, you will be able to:
- Understand **what Bash scripting is and why it is used**.
- Write and execute your **first Bash script**.
- Automate system tasks in an **Azure Virtual Machine**.
- Implement **basic logging** to capture script execution details.

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
- You create a **dedicated scripts directory**:
  ```bash
  mkdir -p ~/scripts && cd ~/scripts
  ```

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

---

## **6. Automating a Task with a Script**

### **Adding Logging to the Script**
To capture execution logs, modify the script to log output to a file.
```bash
#!/bin/bash
LOGFILE="/var/log/system_info.log"
echo "$(date) - System Information:" | tee -a $LOGFILE
echo "-------------------" | tee -a $LOGFILE
echo "Hostname: $(hostname)" | tee -a $LOGFILE
echo "Uptime: $(uptime -p)" | tee -a $LOGFILE
echo "Logged in users: $(who -q)" | tee -a $LOGFILE
```
This ensures that each execution is logged.

---

## **7. What’s Next?**

Right now, our scripts execute basic commands. But what if we need **custom inputs, dynamic thresholds, or conditional execution**?

In the next lesson, we will **enhance our scripts with variables, conditionals, and operators**, making them more flexible and intelligent.

Let’s move forward and explore **Bash Variables, Conditionals, and Operators** in the next lesson.

