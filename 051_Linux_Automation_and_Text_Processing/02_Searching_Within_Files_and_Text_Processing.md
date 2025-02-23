### **Lesson 2: Searching Within Files and Text Processing**  
**(Building on Lesson 1 – Searching Files in Linux)**  

---

### **Real-World Problem Statement**  

Imagine you are a **DevOps/Cloud Engineer** managing an **Azure Virtual Machine** that hosts a **static website**. Your team reports that:  

- The website is **loading slowly** due to **500 Internal Server Errors**.  
- Unauthorized users are **attempting failed SSH logins**, indicating a **possible security breach**.  
- The website configuration file contains **old IP addresses**, which need to be updated across multiple instances.  

In **Lesson 1**, we learned how to **find files in Linux**. But **finding a file is just the beginning**—now we need to **search inside files** for specific patterns and process the text.  

By the end of this lesson, you will be able to:  
✔ **Search within large files using `grep` and `less`**.  
✔ **Use regular expressions (`grep -E`) for advanced searches**.  
✔ **Filter and format logs using `awk`**.  
✔ **Modify configuration files using `sed`**.  
✔ **Automate log analysis with a Bash script**.  

This lesson will **build upon Lesson 1** to help you efficiently **analyze logs, troubleshoot errors, and modify configurations**.  

---

## **1. Searching Within Files Using `grep`**  

The **`grep`** command searches for **specific text patterns** inside files.  

### **1.1 Basic `grep` Usage**  
Search for a keyword (`error`) inside the **Nginx error log**:  

```bash
grep "error" /var/log/nginx/error.log
```

✅ **Example Output**  
```
[error] 1234#1234: *1 open() failed (2: No such file or directory) while reading response header
```

### **1.2 Case-Insensitive Search**  
Use `-i` to **ignore case** differences:  
```bash
grep -i "error" /var/log/nginx/error.log
```

### **1.3 Search for Failed SSH Login Attempts**  
```bash
grep "Failed password" /var/log/auth.log
```

✅ **Example Output**  
```
Feb 23 10:15:02 azure-vm sshd[1234]: Failed password for invalid user admin from 192.168.1.50
```

Now that we can search logs, let’s refine searches using **regular expressions**.

---

## **2. Advanced Searching with Regular Expressions (`grep -E`)**  

### **2.1 Searching for Multiple Keywords**  
Use the `-E` flag to match **multiple patterns**:  
```bash
grep -E "error|failed|denied" /var/log/nginx/error.log
```

✅ **Example Output**  
```
[error] 1234#1234: *1 access denied due to invalid credentials
```

### **2.2 Find All Requests with a 500 Status Code**  
Search for HTTP 500 errors in **Nginx access logs**:  
```bash
grep " 500 " /var/log/nginx/access.log
```

✅ **Example Output**  
```
192.168.1.30 - - [23/Feb/2025:11:00:45 +0000] "GET /index.html HTTP/1.1" 500 1023
```

Now, let’s learn how to **interactively search large log files**.

---

## **3. Searching Large Files Interactively with `less`**  

For large log files, `grep` is useful but sometimes **scrolling through output is better**.  

### **3.1 Use `less` to Open a Large Log File**  
```bash
less /var/log/nginx/error.log
```

**Navigation Inside `less`:**  
- **`/error`** → Search for the word “error”.  
- **`n`** → Jump to the **next match**.  
- **`Shift + G`** → Jump to the **end of the file**.  
- **`q`** → **Exit** `less`.  

Now, let’s extract **specific columns** from logs using `awk`.

---

## **4. Filtering and Extracting Data with `awk`**  

`awk` is used for **column-based filtering** in log files.  

### **4.1 Extract Only IP Addresses from Nginx Logs**  
```bash
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head
```

✅ **Example Output**  
```
25 192.168.1.50
18 192.168.1.30
10 192.168.1.20
```

Now, let’s modify configuration files using `sed`.

---

## **5. Modifying Configuration Files with `sed`**  

`sed` is a powerful tool for **finding and replacing text** in files.

### **5.1 Replace an Outdated IP Address in a Configuration File**  
```bash
sudo sed -i 's/192.168.1.10/192.168.1.100/g' /etc/nginx/sites-available/mini_finance
```

✅ **Explanation**:  
- `s/old-text/new-text/g` → Replaces all occurrences of `192.168.1.10` with `192.168.1.100`.  
- `-i` → **Modifies the file in place** without creating a backup.  

### **5.2 Delete Lines Matching a Pattern**  
Remove all lines containing `debug` from `nginx.conf`:  
```bash
sudo sed -i '/debug/d' /etc/nginx/nginx.conf
```

---

## **6. Automating Log Analysis with a Bash Script**  

Instead of manually running these searches, let’s **automate log analysis**.

### **6.1 Create a Script to Check for Errors and Failed Logins**  
```bash
vi analyze_logs.sh
```

### **6.2 Add the Following Code**  
```bash
#!/bin/bash

LOGFILE="/var/log/nginx/error.log"
AUTHLOG="/var/log/auth.log"

echo "Checking for recent Nginx errors..."
grep -E "error|failed|denied" $LOGFILE | tail -5

echo "Checking for failed SSH login attempts..."
grep "Failed password" $AUTHLOG | awk '{print $1, $2, $3, $9, $11}' | tail -5

echo "Top IPs accessing the website:"
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head -5
```

### **6.3 Save and Execute the Script**  
```bash
chmod +x analyze_logs.sh
./analyze_logs.sh
```

✅ **Expected Output:**  
```
Checking for recent Nginx errors...
[error] 1234#1234: *1 open() failed (2: No such file or directory)

Checking for failed SSH login attempts...
Feb 23 10:15:02 invalid admin 192.168.1.50

Top IPs accessing the website:
25 192.168.1.50
18 192.168.1.30
```

Now, we can **automate log monitoring** and **detect security threats**.

---

## **Key Takeaways**  

✔ **`grep` searches inside files** for specific words or patterns.  
✔ **Regular expressions (`grep -E`) enhance searches** for multiple keywords.  
✔ **`less` helps navigate large logs** interactively.  
✔ **`awk` extracts specific columns** from logs, like IP addresses.  
✔ **`sed` modifies text inside configuration files** without manual editing.  
✔ **A Bash script can automate log analysis** for troubleshooting and security.  

---

## **What’s Next?**  

Now that you know how to **search and process text**, the next step is to **automate recurring tasks with cron jobs**.

Let’s move forward and **automate tasks using cron jobs!**
