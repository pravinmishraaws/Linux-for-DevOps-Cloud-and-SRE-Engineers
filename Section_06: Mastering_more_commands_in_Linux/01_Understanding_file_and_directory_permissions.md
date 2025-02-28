# **Understanding File and Directory Permissions in Linux**

## **Introduction**
File and directory permissions are a fundamental part of the Linux file system. They control **who** can access a file and **what actions** they can perform. Linux provides a structured permission system to manage security and privacy in a multi-user environment.

In this lesson, we will explore:
1. **What Linux permissions are**
2. **How to view file and directory permissions**
3. **How to modify permissions using different methods**
4. **How to change file ownership**
5. **How to use Access Control Lists (ACLs) for more fine-grained control**

Let's start by understanding **how Linux defines permissions** for files and directories.

---

## **1. Linux File Permissions: The Basics**
Each file and directory in Linux has **three permission levels**:
- **Read (r)** → Allows viewing the file content or listing a directory.
- **Write (w)** → Allows modifying the file content or adding/removing files in a directory.
- **Execute (x)** → Allows executing a file as a program or accessing a directory.

These permissions apply to **three categories of users**:
1. **Owner (User - `u`)** → The user who created the file.
2. **Group (`g`)** → A collection of users with shared access.
3. **Others (`o`)** → Everyone else on the system.

Now that we know how permissions work, let’s see how to check them.

---

## **2. Viewing File and Directory Permissions**
To check the permissions of files and directories, use:
```bash
ls -l
```
This command lists all files and displays their permissions.

### **Example Output**
```
-rw-r--r-- 1 ec2-user ec2-user 1024 Feb 28 12:00 myfile.txt
```
Breaking it down:
| Symbol  | Meaning |
|---------|---------|
| `-` | Regular file (`d` for directory) |
| `rw-` | Owner has **read (r) and write (w)** permission |
| `r--` | Group has **read-only (r)** permission |
| `r--` | Others have **read-only (r)** permission |

The first character **(- or d)** represents whether it is a file (`-`) or directory (`d`). The next **nine characters** define the **owner, group, and others** permissions.

---

## **3. Modifying Permissions with `chmod`**
The `chmod` command is used to change **file and directory permissions**.

Linux provides **two ways** to modify permissions:
1. **Absolute Mode (Using Numbers)**
2. **Symbolic Mode (Using Letters)**

Let's explore both methods.

---

### **A. Absolute Mode (Numeric)**
Each permission type is assigned a number:
- `Read (r) = 4`
- `Write (w) = 2`
- `Execute (x) = 1`
- **Sum the values** to set permissions.

#### **Example: Changing Permissions**
1. **Give the owner read/write (6), group read-only (4), others no access (0)**:
```bash
chmod 640 myfile.txt
```
2. **Give full permissions to owner (7), read/execute to group (5), and others (5)**:
```bash
chmod 755 myscript.sh
```

---

### **B. Symbolic Mode (Using Letters)**
In **symbolic mode**, permissions are modified using letters.

**Syntax:**
```bash
chmod [User] [Operator] [Permission] filename
```
**User Types:**
- `u` → Owner
- `g` → Group
- `o` → Others
- `a` → All (Owner, Group, Others)

**Operators:**
- `+` → Add a permission
- `-` → Remove a permission
- `=` → Set exact permissions

#### **Example: Modifying Permissions**
1. **Grant execute (`x`) permission to the owner**:
```bash
chmod u+x myfile.txt
```
2. **Remove write (`w`) permission from the group**:
```bash
chmod g-w myfile.txt
```
3. **Set exact permissions for all**:
```bash
chmod a=r myfile.txt
```

---

### **C. Changing Directory Permissions Recursively**
If you need to modify **permissions for all files inside a directory**, use the `-R` option.
```bash
chmod -R 644 mydir
```
This applies **read/write permissions to the owner and read-only permissions to others** for all files inside `mydir`.

---

## **4. Changing File Ownership with `chown`**
The `chown` command allows changing **the owner of a file or directory**.

#### **Syntax:**
```bash
chown new_owner filename
```
#### **Example: Change File Owner**
```bash
chown ec2-user myfile.txt
```

#### **Changing Both Owner and Group**
```bash
chown new_owner:new_group myfile.txt
```

### **Changing Ownership Recursively**
To apply ownership change to all files inside a directory:
```bash
chown -R new_owner:new_group mydir
```

---

## **5. Changing Group Ownership with `chgrp`**
The `chgrp` command changes **group ownership**.

#### **Syntax:**
```bash
chgrp new_group filename
```
#### **Example: Change Group Ownership**
```bash
chgrp developers myfile.txt
```

---

## **6. Using Access Control Lists (ACLs) for Advanced Permissions**
Sometimes, the standard **owner/group/others** model is not enough. **ACLs (Access Control Lists)** allow assigning **specific** permissions to multiple users.

### **Viewing ACLs**
To check ACL permissions of a file or directory, use:
```bash
getfacl myfile.txt
```

### **Setting ACLs**
To **grant specific permissions** to a user:
```bash
setfacl -m u:ec2-user:rw myfile.txt
```
- `-m` → Modify ACL
- `u:ec2-user:rw` → **Grant read & write** to `ec2-user`

### **Removing ACL Entries**
```bash
setfacl -x u:ec2-user myfile.txt
```

### **Applying ACL Changes Recursively**
To apply changes to **all files inside a directory**:
```bash
setfacl -R -m u:ec2-user:rw mydir
```

---

## **7. Summary**
✔ **File permissions** define who can read, write, or execute files and directories.  
✔ **View permissions** with `ls -l`.  
✔ **Modify permissions** using `chmod` (absolute mode & symbolic mode).  
✔ **Change ownership** using `chown`.  
✔ **Modify group ownership** with `chgrp`.  
✔ **Use ACLs** for advanced access control.  

---

## **Next Steps**
Now that you understand **file and directory permissions**, the next lesson will cover **user and group management in Linux**.
