# **Real-World Examples: Linux File Ownership & Access Control**
## **Scenario 1: Setting Permissions for a Deployment Script**
### **Background:**  
A **DevOps team** maintains a deployment script (`deploy.sh`) on a production server.  
**Goal:** Developers should execute it, but only the DevOps lead should modify it.

### **Step 1: Check Current Permissions**
```bash
ls -l /var/www/deploy.sh
```
**Example Output:**
```bash
-rw-r--r--  1 devopslead devops  1024 Feb 20 10:00 deploy.sh
```
**Problem:**  
- Anyone can read it (`r--`), but only the **owner** can modify (`rw-`).
- **Developers** need **execute** (`x`) permission.

### **Step 2: Grant Execute Permission to Developers**
```bash
chmod g+x /var/www/deploy.sh
```
**Updated Permissions:**
```bash
-rw-r-xr--  1 devopslead devops  1024 Feb 20 10:00 deploy.sh
```
Now, developers in the `devops` group can execute the script but not modify it.

---

## **Scenario 2: Assigning Log Files to a Specific Team**
### **Background:**  
System logs (`/var/log/app.log`) should be readable **only** by the `logteam`.  
**Goal:**  
- Give `logteam` read access.  
- Prevent all other users from accessing logs.

### **Step 1: Check Current Permissions**
```bash
ls -l /var/log/app.log
```
**Example Output:**
```bash
-rw-r--r--  1 root root  2048 Feb 20 11:00 app.log
```
### **Step 2: Assign `logteam` as Group Owner**
```bash
sudo chown root:logteam /var/log/app.log
```
**Updated Output:**
```bash
-rw-r--r--  1 root logteam  2048 Feb 20 11:00 app.log
```
### **Step 3: Restrict Access to Only the Group**
```bash
chmod 640 /var/log/app.log
```
**Final Permissions:**
```bash
-rw-r-----  1 root logteam  2048 Feb 20 11:00 app.log
```
Now, only `root` and `logteam` members can read logs.

---

## **Scenario 3: Securing SSH Private Keys**
### **Background:**  
An **SSH private key** (`~/.ssh/id_rsa`) should only be readable & writable by **its owner**.  
**Goal:** Prevent unauthorized access.

### **Step 1: Check Current Permissions**
```bash
ls -l ~/.ssh/id_rsa
```
**Example Output:**
```bash
-rw-r--r--  1 devopsuser devops  2048 Feb 20 11:00 id_rsa
```
Problem: The key is **readable by all users**, which is a **security risk**.

### **Step 2: Restrict Access**
```bash
chmod 600 ~/.ssh/id_rsa
```
**Final Permissions:**
```bash
-rw-------  1 devopsuser devops  2048 Feb 20 11:00 id_rsa
```
Now, only the key owner can access it, securing SSH authentication.

---

## **Scenario 4: Preventing Accidental Deletion of Important Files**
### **Background:**  
The **nginx.conf** file should never be deleted or modified accidentally.  
**Goal:** Make the file **immutable**.

### **Step 1: Check File Attributes**
```bash
lsattr /etc/nginx/nginx.conf
```
**Example Output:**
```bash
------------- /etc/nginx/nginx.conf
```
### **Step 2: Make the File Immutable**
```bash
sudo chattr +i /etc/nginx/nginx.conf
```
### **Step 3: Verify**
```bash
lsattr /etc/nginx/nginx.conf
```
**Updated Output:**
```bash
----i-------- /etc/nginx/nginx.conf
```
Now, even `root` cannot modify or delete the file.  
To remove immutability, use:
```bash
sudo chattr -i /etc/nginx/nginx.conf
```
Use Case: Prevent accidental deletion of critical configuration files.

---

## **Scenario 5: Recursively Assigning Permissions to a Web Directory**
### **Background:**  
A web server’s files (`/var/www/html`) should be owned by **www-data** for security.  
**Goal:**  
- Set ownership to `www-data`.  
- Ensure **read & execute** for group members.

### **Step 1: Change Ownership for All Files**
```bash
sudo chown -R www-data:www-data /var/www/html
```
### **Step 2: Set Correct Permissions**
```bash
sudo chmod -R 755 /var/www/html
```
**Final Permissions:**
- **Directories:** `rwxr-xr-x` (755)
- **Files:** `rw-r--r--` (644)

Now, web server processes can access the files securely.

---

## **Scenario 6: Managing Permissions for Shared Team Directories**
### **Background:**  
A DevOps team collaborates on Terraform configurations in `/opt/terraform`.  
**Goal:**  
- All team members should have **read & write** access.

### **Step 1: Create a Shared Group**
```bash
sudo groupadd terraform-team
```
### **Step 2: Add Users to the Group**
```bash
sudo usermod -aG terraform-team alice
sudo usermod -aG terraform-team bob
```
### **Step 3: Assign Ownership**
```bash
sudo chown -R root:terraform-team /opt/terraform
```
### **Step 4: Set Group Write Permissions**
```bash
sudo chmod -R 770 /opt/terraform
```
**Final Permissions:**
```bash
drwxrwx---  1 root terraform-team  4096 Feb 20 10:00 terraform
```
Now, only `terraform-team` members can modify Terraform configurations.

---

## **Scenario 7: Securely Storing API Credentials**
### **Background:**  
A **DevOps engineer** needs to store API keys securely in `/etc/secrets/api.key`.  
**Goal:** Prevent unauthorized users from reading or modifying the file.

### **Step 1: Restrict Permissions**
```bash
chmod 600 /etc/secrets/api.key
```
Now, only the file owner can access it.

### **Step 2: Assign the Correct Owner**
```bash
sudo chown devopsuser:devops /etc/secrets/api.key
```
Use Case: Secure sensitive credentials.

---

## **Scenario 8: Using ACLs for Fine-Grained Permissions**
### **Background:**  
A DevOps team needs to **grant specific users** access to a log directory.  
**Goal:** Give `bob` read access to `/var/logs/custom`.

### **Step 1: Enable ACLs**
```bash
sudo setfacl -m u:bob:r /var/logs/custom
```
### **Step 2: Verify ACLs**
```bash
getfacl /var/logs/custom
```
The output will show `bob` has read access.

Use Case: Set **custom permissions** without modifying groups.

---

# **Key Takeaways**
- Use `chmod` to modify **read, write, execute** permissions.  
- Use `chown` to **change ownership** of files & directories.  
- Use `setfacl` for **fine-grained permissions** without changing groups.  
- Use `chattr +i` to **prevent accidental deletion**.  
- Use **recursive permission changes** (`chmod -R`) for large directories.  

---

## **Next Steps:**  
- Try these scenarios on a cloud VM  
- Experiment with different permission settings  
- Automate permission management using Ansible
