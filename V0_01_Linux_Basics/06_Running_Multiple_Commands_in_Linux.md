# **Running Multiple Commands in Linux**  

As a Cloud engineer, **efficiency** is key. Running multiple commands **saves time, reduces manual work**, and ensures **automation** in server management, scripting, and troubleshooting. Below are practical **real-world use cases** for executing multiple commands in Linux.  

---

## **1️⃣ Sequential Execution (`;`)**
- **Executes multiple commands one after another, regardless of success or failure.**  

### **Use Case 1: Updating and Cleaning Up a Server**  
**Scenario:** You need to update all packages and remove unnecessary files **in one go**.  

✅ **Solution:**  
```bash
sudo apt update; sudo apt upgrade -y; sudo apt autoremove -y
```
### **Why It Matters?**  
- Ensures all updates are applied.  
- Cleans up unwanted packages to save space.  

---

### **Use Case 2: Restarting a Service After Editing Configuration**
**Scenario:** You edit the Nginx configuration and want to check for errors **before** restarting the service.  

✅ **Solution:**  
```bash
sudo nano /etc/nginx/nginx.conf; sudo nginx -t; sudo systemctl restart nginx
```
### **Why It Matters?**  
- Ensures no syntax errors before restarting.  
- Avoids **downtime** due to misconfiguration.  

---

## **2️⃣ Conditional Execution (`&&`)**  
- **Executes the next command **only if** the previous command succeeds.**  

### **Use Case 1: Deploying Code Only If Tests Pass**
**Scenario:** In a CI/CD pipeline, **run unit tests** before deploying code.  

✅ **Solution:**  
```bash
pytest tests/ && git push origin main
```
### **Why It Matters?**  
- Prevents deployment if **tests fail**.  
- Ensures stable code reaches production.  

---

### **Use Case 2: Creating and Entering a Directory Only If Creation Succeeds**
**Scenario:** You want to **create a project folder** and move inside it **only if** the creation is successful.  

✅ **Solution:**  
```bash
mkdir my_project && cd my_project
```
### **Why It Matters?**  
- Prevents navigation errors if the directory wasn’t created.  

---

## **3️⃣ Conditional Execution (`||`)**  
- **Executes the second command **only if** the first command fails.**  

### **Use Case 1: Restarting a Service Only If It's Not Running**
**Scenario:** You want to restart Nginx **only if it's not already running**.  

✅ **Solution:**  
```bash
systemctl is-active nginx || systemctl restart nginx
```
### **Why It Matters?**  
- Prevents unnecessary restarts.  
- Reduces **downtime** risk in production.  

---

### **Use Case 2: Checking If a Directory Exists Before Creating It**
**Scenario:** You need to create a `/backup` directory **only if it doesn't already exist**.  

✅ **Solution:**  
```bash
[ -d /backup ] || mkdir /backup
```
### **Why It Matters?**  
- Avoids unnecessary **overwrite errors**.  

---

## **4️⃣ Combining Operators (`&&` and `||`)**
- **Combining both operators ensures better control over execution flow.**  

### **Use Case 1: Running a Backup, But Only If Free Disk Space Is Available**
**Scenario:** A **backup script** should run **only if free space is more than 10GB**.  

✅ **Solution:**  
```bash
df -h | awk '$NF=="/"{if ($4+0 > 10) print "Enough space"; else print "Low space"}' && tar -czf backup.tar.gz /data || echo "Not enough space for backup"
```
### **Why It Matters?**  
- **Prevents backup failures** due to insufficient disk space.  

---

### **Use Case 2: Checking and Installing a Package If Not Found**
**Scenario:** Install `curl` **only if it's not installed**.  

✅ **Solution:**  
```bash
command -v curl >/dev/null 2>&1 && echo "Curl is installed" || sudo apt install -y curl
```
### **Why It Matters?**  
- Avoids redundant installations.  

---

## **Summary Table: Command Execution Methods and Their Uses**
| **Operator** | **Function** | **Example** | **Use Case** |
|-------------|-------------|-------------|--------------|
| `;` | Run commands sequentially | `apt update; apt upgrade` | **Update system & clean up** |
| `&&` | Run next command **only if** the first succeeds | `mkdir project && cd project` | **Create & enter directory safely** |
| `||` | Run next command **only if** the first fails | `systemctl is-active nginx || systemctl restart nginx` | **Restart service if down** |
| `&& ||` | Combine success & failure handling | `command -v curl && echo "Installed" || apt install curl` | **Check & install missing software** |

---

## **Hands-On Challenge for Students**
**Try these on your Linux VM:**  

✅ **Run multiple commands sequentially:**  
```bash
echo "Step 1"; echo "Step 2"; echo "Step 3"
```

✅ **Create a directory only if it doesn't exist:**  
```bash
[ -d /testdir ] || mkdir /testdir
```

✅ **Restart Apache only if it's not running:**  
```bash
systemctl is-active apache2 || systemctl restart apache2
```

---

By **practicing these commands**, you will **gain confidence** in automating system tasks and troubleshooting like real Cloud engineers.  
