# Study Notes: Project Initialization

## Setting Up for the Project
Now that we have successfully connected our local computer to the **Linux EC2 instance** via SSH, it’s time to begin learning **Linux commands**.

Before jumping into specific commands, let's first categorize them based on their functionality. This structured approach will help us understand Linux more effectively while working on our project.

---

## Categories of Linux Commands
Linux commands can be grouped into different categories:

1. **System Information** – Commands to check system details such as hostname, logged-in users, and disk space.
2. **Navigation** – Commands to move between directories and manage files.
3. **File Operations** – Commands to create, edit, and manage files.
4. **File Permissions** – Commands to change and manage file permissions.
5. **User and Group Management** – Commands to add, delete, and manage users and groups.
6. **Text Processing** – Commands like `grep`, `awk`, and `sort` for text manipulation.
7. **Process Management** – Commands to monitor and control running processes.
8. **Package Management** – Commands for installing and managing software.
9. **Networking** – Commands to manage network connections.
10. **Compression and Archiving** – Commands for file compression and extraction.
11. **System Control** – Commands to manage system services and configurations.

Each category plays a role in efficiently managing a Linux-based system. We will learn and execute these commands **while working on our project**.

---

## Project: Managing a Blog Website on a Linux Server
We will use a **Linux EC2 instance** to simulate a **real-world scenario** where we manage a blog website. The tasks in this project will help us practice essential Linux commands required to:

- Manage **files and directories**.
- Search and analyze **data**.
- Handle **user and group management**.
- Monitor **system resources**.

This approach ensures that we learn **Linux through practical application**.

### Resources Needed:
To host a **static website** on a Linux machine, we need:

1. **A Web Server** – A web server stores and delivers web pages to users over the Internet. In this project, we will use **Nginx**, a popular web server known for its speed and reliability.
2. **Linux EC2 Instance** – We will use an **Amazon Linux 2 EC2 instance** in AWS as our virtual machine.
3. **Local Computer (CLI Access)** – We will access the EC2 instance using **Windows PowerShell** or a Linux/macOS terminal.

---

## Hosting a Static Website on a Linux Server
A **static website** consists of fixed content that remains the same for every visitor. Unlike dynamic websites, it does not have interactive elements or databases.

### What is a Web Server?
A **web server** is software that stores and delivers websites over the Internet. When users request a webpage through a browser (e.g., Chrome, Firefox), the web server retrieves and serves the required files.

For this project, we will use **Nginx** because:
- It is **fast and efficient**, capable of handling many users at once.
- It is widely used in **cloud environments** for hosting websites.
- It is **lightweight** and does not consume too many system resources.

Now, let’s proceed with setting up our environment and installing **Nginx**.

---

## Installing Nginx on an EC2 Instance
Before installing Nginx, let’s first check our **current user status** and **working directory**.

### Step 1: Verify Logged-In User
To check the user currently logged into the EC2 instance, use:
```bash
whoami
```
**Output:**
```bash
ec2-user
```
This confirms that we are logged in as the default EC2 user.

### Step 2: Check the Current Working Directory
To check our location in the file system, use:
```bash
pwd
```
**Output:**
```bash
/home/ec2-user
```
This command shows that we are in the `home` directory of the `ec2-user`.

### Step 3: Switch to Root User
Some administrative tasks require **root privileges**. To switch to the root user, use:
```bash
sudo su
```
You will notice that the prompt changes from `$` to `#`, indicating **root access**. Be cautious when using root privileges, as mistakes can affect the entire system.

### Step 4: Verify Directory After Switching to Root
Check if the working directory has changed after switching to root:
```bash
pwd
```
If the output remains `/home/ec2-user`, it means the directory is unchanged.

### Step 5: Install Nginx
Nginx is stored in an **online software repository**. To download and install it, use:
```bash
yum install nginx -y
```
- `yum` is the package manager for **Amazon Linux**.
- `install nginx` specifies the package to install.
- `-y` automatically confirms the installation.

If the installation is successful, you will see a message confirming that Nginx has been installed.

### Handling Installation Errors
Sometimes, the `yum install nginx` command may fail due to missing repositories. If you see an error, follow the instructions provided in the error message to enable the necessary repository and retry the command.

---

## Summary
In this session, we:
- Categorized **Linux commands** based on their functionality.
- Introduced our **project: Managing a Blog Website on a Linux Server**.
- Understood **why Nginx is used** for hosting static websites.
- Set up a **Linux EC2 instance** for our project.
- Learned **how to check the current user and working directory**.
- **Switched to the root user** to gain administrative privileges.
- **Installed Nginx** to serve our website.

With Nginx installed, we are ready to configure it and launch our blog website. In the next session, we will **set up our web server and host our first web page**.


