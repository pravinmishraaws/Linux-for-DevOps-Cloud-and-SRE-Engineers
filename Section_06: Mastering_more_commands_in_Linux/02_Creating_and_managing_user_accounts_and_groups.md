# **Creating and Managing User Accounts and Groups in Linux**

## **Introduction**
In Linux, **user and group management** is a crucial aspect of system administration. It helps in controlling access to files, directories, and system resources. In this lecture, we will learn:
- **What users and groups are in Linux**
- **How to create, modify, and delete user accounts**
- **How to create and manage groups**
- **How to assign users to groups**
- **How permissions work in Linux user management**

By the end of this lesson, you will have a solid understanding of how user accounts and groups function in Linux.

---

## **1. Understanding Users and Groups**
Before we dive into commands, let's first understand what **users** and **groups** are in Linux.

### **Users**
A **user** is an individual who can log into a Linux system. Each user has:
- **A unique username**
- **A user ID (UID)**
- **A home directory** (e.g., `/home/username`)
- **A default shell** (e.g., `/bin/bash`)

For example, on a Linux system:
```
/home/aj
/home/vijay
```
Here, **Aj** and **Vijay** are different users, each with a **personal home directory**.

### **Groups**
A **group** is a collection of users with shared permissions. Groups make it easier to manage file permissions across multiple users.

Example:
- A **developers group** can include multiple users (`aj`, `vijay`), allowing them to share files and access common directories.

Now, let’s move on to **creating and managing user accounts**.

---

## **2. Creating a New User**
To create a user in Linux, use the **`useradd`** command.

#### **Syntax:**
```bash
useradd username
```

#### **Example: Creating a user named "aj"**
```bash
sudo useradd aj
```
Since this action modifies system files, **sudo privileges** are required.

### **Setting a Password for a New User**
After creating a user, set a password using the `passwd` command.

#### **Example:**
```bash
sudo passwd aj
```
The system will prompt you to **enter and confirm** the new password.

> **Note:** The `useradd` command **only creates** the user; it does not assign a password. The password must be set separately.

---

## **3. Checking User Account Properties**
Linux stores **user account details** in `/etc/passwd`.

To view details of a specific user, use the **`grep`** command:
```bash
grep aj /etc/passwd
```
### **Example Output:**
```
aj:x:1002:1003::/home/aj:/bin/bash
```
Breaking this down:
- **`aj`** → Username
- **`x`** → Password placeholder (stored in `/etc/shadow`)
- **`1002`** → User ID (UID)
- **`1003`** → Group ID (GID)
- **`/home/aj`** → Home directory
- **`/bin/bash`** → Default shell

To check **password-related details**, use:
```bash
sudo grep aj /etc/shadow
```

---

## **4. Switching Between Users**
To **switch to another user**, use the **`su`** command.

#### **Example: Switching to "aj"**
```bash
su aj
```
After entering the password, you will be logged in as **aj**.

To return to the original user, type:
```bash
exit
```

---

## **5. Modifying User Accounts**
The `usermod` command allows modifying user properties.

### **A. Changing a Username**
To rename a user from **aj** to **rohit**:
```bash
sudo usermod -l rohit aj
```

### **B. Changing a User's Home Directory**
To change **rohit's** home directory to `/home/developer`:
```bash
sudo usermod -d /home/developer -m rohit
```
The `-m` option moves existing files to the new directory.

---

## **6. Deleting a User**
To remove a user:
```bash
sudo userdel rohit
```
This **deletes the user but not their home directory**.  
To **delete the user along with the home directory**:
```bash
sudo userdel -r rohit
```
To confirm that the user was deleted:
```bash
grep rohit /etc/passwd
```

---

## **7. Managing Groups in Linux**
Just like users, Linux allows **group management** for better access control.

### **A. Creating a Group**
To create a group:
```bash
sudo groupadd developers
```
Linux assigns **a unique Group ID (GID)** automatically.

To check the group details:
```bash
grep developers /etc/group
```

---

## **8. Adding Users to a Group**
To add **aj** to the **developers** group:
```bash
sudo usermod -aG developers aj
```
To verify:
```bash
groups aj
```
**Output:**
```
aj : aj developers
```
This means **aj** is now part of the developers group.

---

## **9. Removing Users from a Group**
To remove a user from a group:
```bash
sudo gpasswd -d aj developers
```

---

## **10. Modifying a Group Name**
To rename a group:
```bash
sudo groupmod -n dev_team developers
```

---

## **11. Deleting a Group**
To remove a group:
```bash
sudo groupdel developers
```
To verify that the group was deleted:
```bash
grep developers /etc/group
```

---

## **12. Viewing Available Options**
To explore **all available options** for user and group management commands, use:
```bash
man useradd
man usermod
man groupadd
```
This opens the **manual pages**, which provide detailed descriptions and usage.

---

## **13. Summary of Commands**
| Task | Command |
|------|---------|
| Create a user | `sudo useradd username` |
| Set user password | `sudo passwd username` |
| View user details | `grep username /etc/passwd` |
| Switch to another user | `su username` |
| Modify user properties | `sudo usermod options username` |
| Delete a user | `sudo userdel username` |
| Create a group | `sudo groupadd groupname` |
| Add user to a group | `sudo usermod -aG groupname username` |
| Remove user from a group | `sudo gpasswd -d username groupname` |
| Rename a group | `sudo groupmod -n newgroupname oldgroupname` |
| Delete a group | `sudo groupdel groupname` |

---

## **Next Steps**
We have now covered **creating, managing, and deleting users and groups** in Linux. The next topic will explore **compression and archiving files in Linux**.
