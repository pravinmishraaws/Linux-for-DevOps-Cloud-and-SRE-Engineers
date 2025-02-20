# **Practical Examples: Linux File Ownership & Access Control**

## **1️⃣ Checking File Permissions & Ownership**
### **Scenario:** A DevOps engineer needs to check access to a script before deploying an application.

#### **🔹 Check File Permissions & Ownership**
```bash
ls -l /var/www/deploy.sh
```
📌 **Example Output:**
```bash
-rwxr-xr--  1 devopsuser devops  1024 Feb 20 10:00 deploy.sh
```
### **🔹 Understanding the Output:**
| Position | Meaning | Example (-rwxr-xr--) |
|----------|---------|----------------------|
| **1st**  | File Type | `-` (file) or `d` (directory) |
| **2nd-4th** | Owner’s Permissions | `rwx` (read, write, execute) |
| **5th-7th** | Group Permissions | `r-x` (read, execute) |
| **8th-10th** | Others’ Permissions | `r--` (read-only) |

✅ **Use Case:** 
- Before running a deployment script, the DevOps team must ensure only authorized users can execute it.

---

## **2️⃣ Changing File Ownership**
### **Scenario:** The `deploy.sh` script is owned by `root`, but a DevOps engineer needs access to manage deployments.

#### **🔹 Check Current Owner**
```bash
ls -l deploy.sh
```
📌 **Example Output:**
```bash
-rwxr-xr--  1 root root  1024 Feb 20 10:00 deploy.sh
```
🚨 **Problem:** Only the `root` user has full control. The DevOps team (`devops` group) cannot modify it.

#### **🔹 Change File Owner to a Specific User**
```bash
sudo chown devopsuser deploy.sh
```
📌 Now, **devopsuser** owns the file.

#### **🔹 Change Both Owner and Group**
```bash
sudo chown devopsuser:devops deploy.sh
```
📌 **devopsuser** owns the file, and **devops group** members can access it.

✅ **Use Case:** 
- Assign proper ownership to files so the right team can manage deployments.

---

## **3️⃣ Changing File Permissions**
### **Scenario:** The DevOps team needs to run `deploy.sh`, but non-admin users should not modify it.

#### **🔹 Give Execute Permission to the Owner**
```bash
chmod u+x deploy.sh
```
📌 Now, the owner can execute the file.

#### **🔹 Remove Write Permission from the Group**
```bash
chmod g-w deploy.sh
```
📌 Now, the group **can read and execute**, but **cannot modify** the file.

#### **🔹 Remove Read Permission for Others**
```bash
chmod o-r deploy.sh
```
📌 Now, **only the owner and group** can see the file’s contents.

✅ **Use Case:** 
- Ensure sensitive scripts are **only executable** by authorized users.

---

## **4️⃣ Setting Permissions Using Numeric Mode**
### **Scenario:** A DevOps engineer wants to **set precise access levels** for a configuration file.

#### **🔹 Set Read, Write & Execute for Owner, Read-Execute for Group, Read-only for Others**
```bash
chmod 754 deploy.sh
```
📌 Breakdown:
| User | Permission | Value |
|------|-----------|-------|
| **Owner** | `rwx` (read, write, execute) | 7 (4+2+1) |
| **Group** | `r-x` (read, execute) | 5 (4+1) |
| **Others** | `r--` (read-only) | 4 (4) |

✅ **Use Case:** 
- Restrict script modifications while allowing controlled execution.

---

## **5️⃣ Managing Group-Based Access**
### **Scenario:** Multiple DevOps engineers need **access to log files**.

#### **🔹 Check the Group of a File**
```bash
ls -l /var/log/deploy.log
```
📌 **Example Output:**
```bash
-rw-r-----  1 root devops  2048 Feb 20 11:00 deploy.log
```
#### **🔹 Add a User to the `devops` Group**
```bash
sudo usermod -aG devops newuser
```
📌 Now, **newuser** can **read** `deploy.log`.

✅ **Use Case:** 
- Grant **team-wide access** to logs without exposing files to everyone.

---

## **6️⃣ Securely Storing SSH Private Keys**
### **Scenario:** A DevOps engineer needs to set **strict permissions** for an SSH private key.

#### **🔹 Set SSH Key to Owner-Only Read/Write**
```bash
chmod 600 ~/.ssh/id_rsa
```
📌 Breakdown:
| User | Permission | Value |
|------|-----------|-------|
| **Owner** | `rw-` (read, write) | 6 (4+2) |
| **Group** | `---` (no access) | 0 |
| **Others** | `---` (no access) | 0 |

✅ **Use Case:** 
- Protect SSH keys from unauthorized access.

---

## **7️⃣ Preventing Accidental File Deletion**
### **Scenario:** A critical log file should not be deleted.

#### **🔹 Make a File Immutable**
```bash
sudo chattr +i /var/log/deploy.log
```
📌 **Now, even root cannot delete or modify the file**.

#### **🔹 Remove the Immutable Attribute**
```bash
sudo chattr -i /var/log/deploy.log
```

✅ **Use Case:** 
- Prevent accidental or malicious modifications to critical logs.

---

## **8️⃣ Recursively Changing Ownership & Permissions**
### **Scenario:** A DevOps engineer needs to **secure a web directory**.

#### **🔹 Change Ownership for All Files in a Directory**
```bash
sudo chown -R www-data:www-data /var/www/html
```
📌 **All files and subdirectories** are now owned by `www-data`.

#### **🔹 Change Permissions for All Files & Folders**
```bash
sudo chmod -R 755 /var/www/html
```
📌 **All files are executable**, and directories remain accessible.

✅ **Use Case:** 
- Set up **secure web server file permissions**.

---

# **Final Thoughts**
✅ **What We Learned**
1. **Check file permissions & ownership** (`ls -l`).
2. **Change ownership** (`chown user:group file`).
3. **Modify permissions** (`chmod 750 file`).
4. **Set permissions using numbers** (`chmod 754 file`).
5. **Use groups for shared access** (`usermod -aG group user`).
6. **Secure sensitive files** (`chmod 600 id_rsa`).
7. **Prevent deletion of critical files** (`chattr +i file`).
8. **Apply changes recursively** (`chown -R user:group dir`).

🎯 **Next Steps**
- Try these commands in a cloud VM or local Linux machine.
- Practice permission management on your scripts and logs.
- Automate file ownership tasks using Ansible or Bash scripting.

🚀 **Hands-On Challenge:** Set up a web directory `/var/www/project` with:
- **www-data** as owner.
- **755** permissions for directories.
- **644** permissions for files.
