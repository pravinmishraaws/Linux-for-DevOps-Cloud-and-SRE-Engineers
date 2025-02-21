# **Linux File Ownership & Access Control for DevOps Engineers**

## **What You Will Learn**
By the end of this guide, you will be able to:  
✔️ Check and understand **file permissions & ownership** using `ls -l`  
✔️ Modify file permissions using `chmod` (symbolic and numeric modes)  
✔️ Change file ownership using `chown` and manage group-based access  
✔️ Secure sensitive files such as **SSH keys & deployment scripts**  
✔️ Prevent accidental file deletion using `chattr +i`  
✔️ Recursively change ownership & permissions for **web servers & log files**  
✔️ Apply real-world **DevOps use cases** related to **scripts, logs, and automation**  

---

## **Prerequisites**
Before we begin, ensure your system has the required files, users, and groups.

### **1. Ensure the DevOps User and Group Exist**
Check if `devopsuser` exists:
```bash
id devopsuser
```
If it does not exist, create it:
```bash
sudo useradd -m -s /bin/bash devopsuser
sudo passwd devopsuser
```
Now, check if the `devops` group exists:
```bash
getent group devops
```
If it does not exist, create it and add `devopsuser`:
```bash
sudo groupadd devops
sudo usermod -aG devops devopsuser
```

### **2. Create a Deployment Script**
```bash
sudo mkdir -p /var/www
sudo touch /var/www/deploy.sh
echo -e '#!/bin/bash\necho "Deploying application..."' | sudo tee /var/www/deploy.sh > /dev/null
sudo chmod 000 /var/www/deploy.sh
sudo chown root:root /var/www/deploy.sh
```

Now, check the file:
```bash
ls -l /var/www/deploy.sh
```
✅ **Expected Output:**
```
---------- 1 root root 44 Feb 21 05:31 /var/www/deploy.sh
```

---

# **Scenario 1: Checking File Permissions & Ownership**
### **Why is this important?**
Before executing `deploy.sh`, the DevOps team must verify **who has access**.

📌 **Example: yes, this is example to understand:**
```
-rwxr-xr--  1 xxuser xxgroup  1024 Feb 20 10:00 deploy.sh
```

### **Understanding File Permission Structure**
| Position | Meaning | Example (-rwxr-xr--) |
|----------|---------|----------------------|
| **1st**  | File Type | `-` (file) or `d` (directory) |
| **2nd-4th** | Owner’s Permissions | `rwx` (read, write, execute) |
| **5th-7th** | Group Permissions | `r-x` (read, execute) |
| **8th-10th** | Others’ Permissions | `r--` (read-only) |

✅ **Takeaway:** If the script is not executable, the deployment process may fail.

---

# **Scenario 2: Changing File Ownership**
### **Why is this important?**
By default, files created by `root` can only be modified by `root`. A DevOps engineer needs access to `deploy.sh`.

### **Check Current Ownership**
```bash
ls -l /var/www/deploy.sh
```

### **Change File Ownership to a DevOps User**
```bash
sudo chown devopsuser /var/www/deploy.sh
```

### **Change Both Owner and Group**
```bash
sudo chown devopsuser:devops /var/www/deploy.sh
```

✅ **Expected Output:**
```
-rwxr-xr--  1 devopsuser devops  1024 Feb 20 10:00 /var/www/deploy.sh
```
Now, **devopsuser** owns the script, and **devops group members** can access it.

---

# **Scenario 3: Changing File Permissions**

## **Why is This Important?**
**Understanding File Permissions and Adjusting Them for Secure Execution** is essential for DevOps engineers.  
Before executing a deployment script, a DevOps engineer must ensure the **correct permissions** are in place:

✔️ The **owner** should be able to **execute** the script.  
✔️ **Group members** should have **limited access**.  
✔️ **Others should have NO access** for security reasons.

---

## **Current File Permissions**
Run the following command to check permissions:
```bash
ls -l /var/www/deploy.sh
```
✅ **Example Output:**
```
---------- 1 root root 44 Feb 21 05:31 /var/www/deploy.sh
```

### **Breaking Down the Current Permissions**
| **Position** | **Meaning** | **Current Value (`----------`)** |
|-------------|------------|--------------------------------|
| **1st**  | File Type | `-` (regular file) |
| **2nd-4th** | Owner’s Permissions | `---` (no read, write, or execute) |
| **5th-7th** | Group Permissions | `---` (no read, write, or execute) |
| **8th-10th** | Others’ Permissions | `---` (no read, write, or execute) |

### **🚨 Problem:**
- The **owner, group, and others have NO permissions at all**.  
- The script **cannot be read, modified, or executed** by anyone.

## **1️⃣ Switch to Root User (Required)**
Since the file is **owned by `root`**, only **root can modify permissions**.

Run the following command:
```bash
sudo su -
```
Now, confirm you are the **root user**:
```bash
whoami
```
✅ **Expected Output:**
```
root
```

## **2️⃣ Adjusting File Permissions**

### **1️⃣ Grant Execute Permission to the Owner**
```bash
chmod u+x /var/www/deploy.sh
```
✅ **Now, the owner (`root`) can execute the script.**

#### **Check Permission**
```bash
ls -l /var/www/deploy.sh
```


### **2️⃣ Grant Read and Execute Permission to the Group**
Since the **group currently has no permissions (`---`)**, we need to allow **read (`r`) and execute (`x`)** access:
```bash
chmod g+rx /var/www/deploy.sh
```
✅ **Now, group members can read and execute the script.**

#### **Check Permission**
```bash
ls -l /var/www/deploy.sh
```

### **3️⃣ Ensure Others Have No Permissions**
Since **others (`o`) already have no permissions (`---`)**, this is optional. However, to explicitly **restrict all access** for others, run:
```bash
chmod o-rwx /var/www/deploy.sh
```
✅ **This ensures others have no read, write, or execute permissions.**

### **✅ Final Permission Check**
```bash
ls -l /var/www/deploy.sh
```
✅ **Expected Output:**
```
---xr-x--- 1 devopsuser devops 44 Feb 21 05:31 /var/www/deploy.sh
```

### ** Breakdown of Updated Permissions**
| Position | Meaning | Updated Value (`-rwxr-x---`) |
|----------|---------|----------------------|
| **1st**  | File Type | `-` (regular file) |
| **2nd-4th** | Owner (`root`) | `__x` (execute) |
| **5th-7th** | Group (`root`) | `r-x` (read & execute) |
| **8th-10th** | Others | `---` (no access) |

---

### **🚀 Final Takeaways**
✔ **The owner (`root`) can execute the script.**  
✔ **Group members can read & execute the script but cannot modify it.**  
✔ **Others have no access for security.**  


---

# **Scenario 4: Setting Permissions Using Numeric Mode**

# **Setting Permissions Using Numeric Mode**

## **Why is This Important?**
Numeric mode provides **precise control** over file permissions, making it easier to manage access in a structured way. Unlike symbolic mode (`chmod u+x`), numeric mode assigns **specific permission values** to the owner, group, and others in a single command.

## **Current File Permissions**
Before making changes, let's check the current permissions of the file:

```bash
ls -l /var/www/deploy.sh
```

✅ **Example Output:**
```
---xr-x--- 1 root root 44 Feb 21 05:31 /var/www/deploy.sh
```

### **Breaking Down the Current Permissions**
| **User Type** | **Current Permission** | **Meaning** |
|--------------|-----------------|------------------------------|
| **Owner (`root`)** | `--x` | execute |
| **Group (`root`)** | `r-x` | Read & execute only |
| **Others** | `---` | No access |

🚨 **Problem:**
- Currently, **others (`---`) have no access** to the script.
- If we want **others to have read access**, we need to modify permissions.

## **1️⃣ Understanding Numeric Permissions**
Each permission (read, write, execute) has a numeric value:

| Permission | Symbol | Numeric Value |
|------------|--------|--------------|
| **Read**   | `r`    | `4`          |
| **Write**  | `w`    | `2`          |
| **Execute**| `x`    | `1`          |

To **set precise access**, we sum the values:

| **User**  | **Required Permission** | **Value Calculation** | **Numeric Value** |
|-----------|------------------------|---------------------|---------------|
| **Owner** | `rwx` (execute) | `0+0+1` | **1** |
| **Group** | `r-x` (read, execute) | `4+0+1` | **5** |
| **Others** | `---` (read-only) | `0+0+0` | **4** |

Thus, to set **Owner: Full Access**, **Group: Read & Execute**, **Others: Read-Only**, we use:

```bash
chmod 754 /var/www/deploy.sh
```

## **2️⃣ Applying Numeric Permissions**
### **Run the following command:**
```bash
chmod 754 /var/www/deploy.sh
```

✅ **What This Does:**
✔ **Owner (`7`)** → Full access (read, write, execute)  
✔ **Group (`5`)** → Read & execute only  
✔ **Others (`4`)** → Read-only  

## **3️⃣ Verify Updated Permissions**
Run:
```bash
ls -l /var/www/deploy.sh
```
✅ **Expected Output:**
```
-rwxr-xr-- 1 root root 44 Feb 21 05:31 /var/www/deploy.sh
```

### **Updated Permission Breakdown**
| **User**  | **Updated Permission** | **Numeric Value** |
|-----------|------------------------|---------------|
| **Owner (`root`)** | `rwx` (read, write, execute) | **7** |
| **Group (`root`)** | `r-x` (read, execute) | **5** |
| **Others** | `r--` (read-only) | **4** |

✅ **This ensures:**
✔ **The owner can modify and execute the script.**  
✔ **Group members can execute but not modify the script.**  
✔ **Others can only read the script.**  

## **🚀 Final Takeaways**
✔ **Numeric mode (`chmod 754`) simplifies permission management.**  
✔ **It’s faster than symbolic mode when managing multiple files.**  
✔ **Use `ls -l` to verify permissions after applying changes.**  

---

# **Scenario 5: Managing Group-Based Access**
### **Why is this important?**
If multiple engineers need access to `deploy.sh`, they should **not modify it accidentally**.

### **Add a User to the DevOps Group**
```bash
sudo usermod -aG devops newuser
```

✅ Now, `newuser` **can execute the script** without modifying it.

---

# **Scenario 6: Securing Sensitive Files (SSH Private Keys)**
### **Why is this important?**
SSH keys should **never be readable by others**.

### **Restrict SSH Key Access**
```bash
chmod 600 ~/.ssh/id_rsa
```

📌 **Now:**
- **Only the owner** can read/write.

✅ **This prevents unauthorized SSH access.**

---

# **Scenario 7: Preventing Accidental File Deletion**
### **Why is this important?**
Critical log files should **never be deleted accidentally**.

### **Make a File Immutable**
```bash
sudo chattr +i /var/log/deploy.log
```
📌 Now, **even `root` cannot delete it**.

### **Remove the Immutable Attribute**
```bash
sudo chattr -i /var/log/deploy.log
```

✅ **This protects log files from unintended deletions.**

---

# **Scenario 8: Recursively Changing Ownership & Permissions**
### **Why is this important?**
Web servers need correct permissions for all files and directories.

### **Change Ownership for an Entire Directory**
```bash
sudo chown -R www-data:www-data /var/www/html
```

### **Apply Secure Permissions to All Files & Folders**
```bash
sudo chmod -R 755 /var/www/html
```

✅ **Expected Result:**  
- **Files** are readable and executable.  
- **Directories** remain accessible.

---

## **Final Takeaways**
✔️ **Check permissions** before executing scripts.  
✔️ **Use `chown`** to assign proper ownership.  
✔️ **Set strict permissions** on sensitive files.  
✔️ **Use groups** to manage shared access.  
✔️ **Protect critical logs from accidental deletion.**  

---

## **Hands-On Challenge**
Set up `/var/www/project` with:
- **www-data** as the owner.
- **755** for directories.
- **644** for files.
