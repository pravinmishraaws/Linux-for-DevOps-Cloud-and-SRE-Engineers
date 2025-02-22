# **Linux File Permissions & Ownership**  

In Linux, **file permissions** determine **who can read, write, or execute files and directories**. Understanding and managing permissions is critical for **security, automation, and DevOps workflows**.  

---

## **1. Why File Permissions Matter?**  

🔹 **Scenario:** Imagine you have an **important deployment script (`deploy.sh`)** on a production server. If permissions are not set correctly:  
✅ **Developers should execute it** but **not modify** it.  
❌ Unauthorized users should **not run or delete** it.  
💡 **File permissions ensure secure access control.**  

---

## **2. Types of File Permissions**  

Every file and directory has **three types of permissions**:  

| Permission | Symbol | Meaning |
|------------|--------|----------|
| **Read** | `r` | View file contents |
| **Write** | `w` | Modify or delete file |
| **Execute** | `x` | Run file as a program/script |

---

## **3. Permission Structure**  

🔹 **Check File Permissions**  
```bash
ls -l deploy.sh
```
📌 Output example:  
```
-rwxr-xr--  1 devopsuser devops  1024 Feb 20 10:00 deploy.sh
```

🔹 **Understanding the Output**  
| Position | Meaning | Example (`-rwxr-xr--`) |
|----------|---------|----------------|
| **1st Character** | File Type | `-` (file) or `d` (directory) |
| **2nd-4th** | **Owner’s Permissions** | `rwx` (read, write, execute) |
| **5th-7th** | **Group Permissions** | `r-x` (read, execute) |
| **8th-10th** | **Others’ Permissions** | `r--` (read-only) |

✅ **Example Use Case**  
**Scenario:** You want to know **who can access a deployment script**.  
```bash
ls -l /var/www/deploy.sh
```
📌 This shows whether developers have execution (`x`) permissions.

---

## **4. Numeric Representation of Permissions**  

Instead of using `rwx`, permissions can be **represented numerically** using:  
| Permission | Value |
|------------|--------|
| Read (r) | 4 |
| Write (w) | 2 |
| Execute (x) | 1 |

### **Common Permission Sets**  
| Numeric Value | Symbolic | Meaning |
|--------------|---------|-----------|
| `7` (4+2+1) | `rwx` | Full access (read, write, execute) |
| `5` (4+1) | `r-x` | Read and execute |
| `4` | `r--` | Read-only |
| `0` | `---` | No access |

✅ **Example Use Case**  
**Scenario:** You need to give a script full access to its owner, execution rights to the group, and read access to others.  
```bash
chmod 754 deploy.sh
```
📌 This sets:  
- `Owner (7)` → **Full access (rwx)**  
- `Group (5)` → **Read and execute (r-x)**  
- `Others (4)` → **Read-only (r--)**  

---

## **5. Changing File Permissions**  

🔹 **Modify File Permissions (Using Symbolic Mode)**  
```bash
chmod u+x deploy.sh  # Give execute permission to owner
chmod g-w deploy.sh  # Remove write permission from group
chmod o-r deploy.sh  # Remove read permission from others
```

🔹 **Modify File Permissions (Using Numeric Mode)**  
```bash
chmod 755 deploy.sh
```
📌 `755` means **Owner (rwx), Group (r-x), Others (r-x)**.  

✅ **Example Use Case**  
**Scenario:** You want to make `deploy.sh` **executable for developers but not writable**.  
```bash
chmod 750 deploy.sh
```
📌 Now, **developers can execute but not modify the script**.

---

## **6. Changing File Ownership**  

🔹 **Why Change Ownership?**  
- Files should be owned by the correct **user and group**.  
- Prevents **unauthorized modifications**.  

✅ **Check File Ownership**  
```bash
ls -l deploy.sh
```
📌 Example output:  
```
-rwxr-xr--  1 ubuntu devops 1024 Feb 20 10:00 deploy.sh
```
✅ **Change File Owner**  
```bash
sudo chown john deploy.sh
```
📌 Now, `john` owns the file.  

✅ **Change File Owner and Group**  
```bash
sudo chown john:devops deploy.sh
```
📌 `john` owns the file, and `devops` group members can access it.

---

# **Key Takeaways**  

✅ **File permissions control who can read, write, or execute files.**  
✅ **Permissions are represented in symbolic (rwx) or numeric (755) format.**  
✅ **Use `chmod` to change permissions and `chown` to change ownership.**  
✅ **Ensuring proper permissions prevents security risks.**  

🎯 **Next Step:** Now, let’s explore **practical examples of Linux file ownership and access control!** 🚀
