# Study Notes: Working with Text Editors, Package Managers, and Sudo Commands

## **Introduction**
We have successfully completed our **static website project** using various Linux commands. Now, let’s continue learning more essential Linux commands that are widely used for **editing text files, managing software packages, and handling user privileges**.

To ensure a structured approach, we will cover:
1. **Text Editors (Nano, Vi, Vim)**
2. **Package Management (Yum, APT)**
3. **The Sudo Command (Superuser Privileges)**

If you haven’t already, it’s highly recommended to maintain **a notebook** where you document all commands and their functions.

---

## **1. Working with Text Editors**
### **Overview**
Text editors in Linux are used for **creating, modifying, and managing text files**. The three most commonly used editors are:
- **Nano** – Simple and beginner-friendly.
- **Vi** – A classic Unix text editor.
- **Vim** – An improved version of Vi.

Before using these editors, let's check if they are already installed.

### **Checking Installed Text Editors**
Run the following commands:
```bash
nano --version
vi --version
vim --version
```
If any editor is missing, install it using:
```bash
sudo yum install nano -y  # For RHEL-based Linux (Amazon Linux 2)
sudo apt install nano -y  # For Debian-based Linux (Ubuntu)
```

---

### **Nano Text Editor**
Nano is the easiest text editor to use.

#### **Opening a File in Nano**
```bash
nano index.html
```
#### **Editing and Saving in Nano**
- Modify the file directly.
- **Save changes**: `CTRL + O`, then `Enter`
- **Exit Nano**: `CTRL + X`

#### **Example: Editing a Static Website Title**
1. Open `index.html` in Nano.
2. Modify the **title** tag.
3. Save changes using `CTRL + O`, then press `Enter`.
4. Exit Nano using `CTRL + X`.

To verify the change, **refresh the website in the browser**.

---

### **Vi Text Editor**
Vi is a traditional Unix text editor. It operates in **two modes**:
1. **Insert Mode** – For text editing (`i` key).
2. **Command Mode** – For executing commands (`Esc` key to switch back).

#### **Opening a File in Vi**
```bash
vi index.html
```
#### **Editing in Vi**
- Enter **Insert Mode**: Press `i`
- Modify the file.
- Exit Insert Mode: Press `Esc`
- Save and exit: Type `:wq` then `Enter`
- Exit without saving: Type `:q!` then `Enter`

To check if the changes are applied, **refresh the website** in the browser.

---

### **Vim Text Editor**
Vim (**Vi Improved**) is an advanced version of Vi with additional features.

#### **Opening a File in Vim**
```bash
vim index.html
```
Vim works similarly to Vi, with **Insert Mode** (`i` to enter) and **Command Mode** (`Esc` to exit).

- Modify the **title** tag.
- Press `Esc`, then type `:wq` to **save and exit**.

After making changes, **refresh the website in the browser** to verify the update.

---

## **2. Package Management**
In Linux, software is managed using **package managers**. These tools allow us to **install, update, and remove software packages** efficiently.

### **Why Package Managers Are Important**
Each Linux distribution uses a specific package manager:
- **YUM** – For **RHEL-based distributions** (Amazon Linux 2, CentOS).
- **APT** – For **Debian-based distributions** (Ubuntu, Debian).

In our project, we used **YUM** to install Nginx because **Amazon Linux 2 is RHEL-based**.

---

### **Using the YUM Package Manager**
#### **Updating the System**
```bash
sudo yum update -y
```
#### **Installing a Package (Example: Nano)**
```bash
sudo yum install nano -y
```
#### **Removing a Package**
```bash
sudo yum remove nano -y
```

---

### **Using the APT Package Manager**
APT is used for **Ubuntu and Debian-based distributions**.

#### **Updating the System**
```bash
sudo apt update
sudo apt upgrade -y
```
#### **Installing a Package (Example: Apache Web Server)**
```bash
sudo apt install apache2 -y
```
#### **Removing a Package**
```bash
sudo apt remove nano -y
```

---

## **3. The Sudo Command**
### **Why Do We Use Sudo?**
Some system tasks require **elevated privileges**. To execute commands as a **superuser**, we use `sudo`.

### **Running a Command with Sudo**
```bash
sudo yum install nginx -y
```
If a user **does not have sudo access**, Linux **denies permission**.

---

### **Managing User Privileges with Sudo**
The **sudoers file** determines which users have administrative rights.

### **Checking If a User Has Sudo Access**
```bash
id
```
If the output contains:
```bash
groups=1000(ec2-user), 27(wheel)
```
It means the user belongs to the **wheel group**, which has sudo privileges.

---

### **Editing the Sudoers File Safely**
To modify user permissions, use `visudo`:
```bash
sudo visudo
```
Locate:
```bash
%wheel  ALL=(ALL)       ALL
```
Users in the **wheel group** can execute **all commands** as a root user.

### **Adding a User to the Sudo Group**
To **grant full root privileges**, add the user in the **sudoers file**:
```bash
ec2-user  ALL=(ALL)  ALL
```
Save and exit (`Esc`, then `:wq`).

---

### **Verifying Sudo Privileges**
To confirm a user's sudo access:
```bash
sudo -l
```
This lists all **commands the user can execute** as a superuser.

---

## **Summary**
### **In this session, we covered:**
✔ **Text Editors**: Nano (simple), Vi (classic), and Vim (advanced).  
✔ **Package Managers**: Yum for RHEL-based systems, APT for Debian-based systems.  
✔ **The Sudo Command**: Used for executing commands with administrative privileges.  
✔ **Modifying the Sudoers File**: Granting root access using `visudo`.  

By mastering these tools, we can efficiently **edit files, install software, and manage user privileges** in Linux.

---

### **Next Steps**
We will now explore **more advanced Linux commands** for system administration.
