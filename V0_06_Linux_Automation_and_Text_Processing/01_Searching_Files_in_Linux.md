### **Lesson 1: Searching Files in Linux (with Real-World Use Case)**  

---

## **Why Should DevOps and Cloud Engineers Learn File Search in Linux?**  

### **Real-World Problem Statement**  

Imagine you are a **DevOps/Cloud Engineer** managing an **Azure VM** hosting a web application. Your team reports issues:  

- Users are seeing errors when accessing the website.  
- The latest updates to the website **are not reflecting**.  
- Suspicious IPs are **attempting unauthorized access**.  

You need to:  
1. **Find website configuration files** to ensure they are set up correctly.  
2. **Search log files** for errors and security threats.  
3. **Automate system checks** to identify problems before they escalate.  

To achieve this, we will deploy a **static website** on an **Azure VM** and use **Linux file search tools** to troubleshoot and monitor the system.

---

## **Step 1: Understanding Linux File Search Commands**  

Before automating searches, you need to understand the core file location commands:

| **Command** | **Use Case** |
|------------|-------------|
| **find** | Search for files based on name, size, and modification date. |
| **locate** | Quickly search files using a pre-built database. |
| **which** | Find the absolute path of an executable program. |
| **whereis** | Locate binaries, source code, and manual pages. |
| **grep** | Search inside files for patterns (logs, configs, etc.). |

Each of these commands plays a key role in **file and log management**. We will now apply them in a **real-world scenario**.

---

## **Step 2: Deploying the Static Website on an Azure VM**  

Before searching for files, we need **real files** to work with. We will deploy a **static website** using **Nginx** on our **Azure Virtual Machine**.

### **2.1 Create a Directory for the Website**  
```bash
sudo mkdir -p /var/www/mini_finance
```

### **2.2 Download the Website Files**  
```bash
cd /var/www/mini_finance
sudo wget -O mini_finance.zip https://www.tooplate.com/download/2135_mini_finance
```

### **2.3 Install `unzip` and Extract Files**  
```bash
sudo apt update && sudo apt install unzip -y
sudo unzip mini_finance.zip -d /var/www/mini_finance
```

After extraction, the website files are inside `/var/www/mini_finance/2135_mini_finance`.

---

### **2.4 Set Correct File & Directory Permissions**  
Ensure Nginx can access the website files:  
```bash
sudo chown -R www-data:www-data /var/www/mini_finance
sudo chmod -R 755 /var/www/mini_finance
```

---

### **2.5 Install and Configure Nginx**  
```bash
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

### **2.6 Configure Nginx to Serve the Website**  

Open the **Nginx configuration file**:  
```bash
sudo vi /etc/nginx/sites-available/mini_finance
```

Update the content to match the **correct root directory**:
```nginx
server {
    listen 80;
    server_name your-vm-ip;

    root /var/www/mini_finance/2135_mini_finance;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Save the file (`ESC + :wq + ENTER`).

---

### **2.7 Enable the Site and Restart Nginx**  
```bash
sudo ln -s /etc/nginx/sites-available/mini_finance /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

✅ **Verification Step:** Open a browser and visit:  
```
http://your-vm-ip
```
You should now see the **Mini Finance** website running.  

---

## **Step 3: Using Linux Search Commands in a Real-World Scenario**  

Now that we have a **real website** running, let's **search for configurations, logs, and security issues**.

---

### **3.1 Find the Nginx Configuration File for the Website**  

#### **Scenario:** Your site is misconfigured, and you need to locate the correct **Nginx configuration file**.

```bash
sudo find /etc/nginx -type f -name "mini_finance"
```

✅ **Expected Output**:
```
/etc/nginx/sites-available/mini_finance
/etc/nginx/sites-enabled/mini_finance
```

---

### **3.2 Locate the Website’s Root Directory**  

#### Installing locate on Ubuntu/Debian

```bash
sudo apt update
sudo apt install mlocate -y
sudo updatedb
```

#### **Scenario:** You need to confirm where the website files are stored.

```bash
sudo locate mini_finance
```

✅ **Expected Output**:
```
/var/www/mini_finance/2135_mini_finance/index.html
/var/www/mini_finance/2135_mini_finance/css/style.css
/var/www/mini_finance/2135_mini_finance/js/main.js
```

---

### **3.3 Find the Nginx Executable and Config Paths**  

#### **Scenario:** You need to modify **Nginx settings**, but you’re unsure where its files are located.

```bash
which nginx
whereis nginx
```

✅ **Expected Output**:
```
/usr/sbin/nginx
nginx: /usr/sbin/nginx /etc/nginx /usr/share/nginx
```

---

### **3.4 Search Nginx Logs for Errors**  

#### **Scenario:** Users report that the website is **not loading properly**. You need to check **error logs**.

```bash
sudo grep "error" /var/log/nginx/error.log
```

✅ **Expected Output**:
```
[error] 1156#1156: *1 open() "/var/www/mini_finance/2135_mini_finance/index.html" failed (2: No such file or directory)
```

---

### **3.5 Detect Unauthorized Access Attempts**  

#### **Scenario:** You suspect that someone is **trying to access restricted areas**.

```bash
sudo grep "403" /var/log/nginx/access.log
```

✅ **Expected Output**:
```
192.168.1.20 - - [23/Feb/2025:10:15:45 +0000] "GET /admin HTTP/1.1" 403 1023
```

Now, you know that **someone tried accessing `/admin`**, which is restricted.

---

## **Step 4: Automating Searches with a Bash Script**  

Instead of manually running these commands every time, let’s automate **log checks and file searches**.

#### **4.1 Create a Bash Script**
```bash
vi check_website_status.sh
```

#### **4.2 Add the Following Code**
```bash
#!/bin/bash

CONFIG_FILE="/etc/nginx/sites-available/mini_finance"
ROOT_DIR="/var/www/mini_finance/2135_mini_finance"
ERROR_LOG="/var/log/nginx/error.log"
ACCESS_LOG="/var/log/nginx/access.log"

echo "Checking if Nginx configuration file exists..."
if [ -f "$CONFIG_FILE" ]; then
    echo "Found: $CONFIG_FILE"
else
    echo "Warning: Nginx configuration file missing!"
fi

echo "Checking if website root directory exists..."
if [ -d "$ROOT_DIR" ]; then
    echo "Found: $ROOT_DIR"
else
    echo "Warning: Website root directory missing!"
fi

echo "Checking for recent Nginx errors..."
grep "error" $ERROR_LOG | tail -5

echo "Checking for unauthorized access attempts..."
grep "403" $ACCESS_LOG | awk '{print $1, $4, $7}' | sort | uniq -c | sort -nr | head
```

---

## **Key Takeaways**
✔ **Deploying a real website** on an **Azure VM** makes file searches practical.  
✔ **Using `find`, `locate`, `which`, `whereis`, and `grep`** helps locate **website configurations and logs**.  
✔ **Automating searches with Bash scripts** saves time in managing cloud infrastructure.  
