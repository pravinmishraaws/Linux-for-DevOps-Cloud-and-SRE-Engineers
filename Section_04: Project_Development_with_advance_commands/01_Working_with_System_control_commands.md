# Study Notes: Project Development with Advanced Commands

## Working with System Control Commands
Managing system services is an essential part of administering a Linux system. The **systemctl** command allows us to control background processes known as **daemons**, ensuring they function properly.

### What is `systemctl`?
The `systemctl` command is a **service management tool** used to control **system services (daemons)**. It allows us to:
- Start, stop, restart, enable, and disable services.
- Check the status of a service.

Daemons are **background processes** that continuously run without user interaction, helping the system function smoothly.

### `systemctl` Syntax
The syntax of the command follows a simple structure:
```bash
systemctl <action> <service-name>
```
Where:
- `<action>` is the operation to perform (e.g., start, stop, restart, enable, disable, status).
- `<service-name>` is the name of the service to manage.

---

## Checking the Status of the Nginx Server
Nginx is a web server, which is considered a **service** in Linux. We use `systemctl` to manage this service. 

To check if Nginx is running:
```bash
systemctl status nginx
```
**Expected Output:**
- `Active: inactive (dead)` → The service is not running.
- `Active: active (running)` → The service is running.

If the service is inactive, we need to start it.

### Starting the Nginx Service
To start Nginx, use:
```bash
systemctl start nginx
```
Now, check the status again:
```bash
systemctl status nginx
```
If successful, the output should indicate that Nginx is **active and running**.

---

## Accessing the Nginx Web Server
Once Nginx is running, we can verify it using the **public IP address** of our EC2 instance.

### Steps to Verify Nginx:
1. Go to **AWS Management Console**.
2. Open the **EC2 Dashboard**.
3. Select your running EC2 instance.
4. Copy the **Public IP address**.
5. Open a browser and enter the copied IP in the address bar.

If everything is set up correctly, the Nginx default welcome page should load.

---

## Additional `systemctl` Actions
Apart from starting and checking the status, here are some other useful commands:

### Stop a Service
To stop a running service:
```bash
systemctl stop nginx
```
This terminates the service until it is restarted manually.

### Restart a Service
To stop and start the service again:
```bash
systemctl restart nginx
```
This is useful when applying configuration changes.

### Enable a Service at Boot
To ensure Nginx starts automatically when the system boots:
```bash
systemctl enable nginx
```
This is similar to applications that start automatically when a computer is turned on.

### Disable a Service at Boot
To prevent the service from starting automatically:
```bash
systemctl disable nginx
```
This does not stop the service immediately but prevents it from launching on boot.

### Check if a Service is Enabled
To verify whether a service starts at boot:
```bash
systemctl is-enabled nginx
```

---

## Downloading a Static Website Template
Instead of creating a website from scratch, we will use a **ready-made static website template**.

### Steps to Download a Template
1. Visit a free **[website template provider](https://www.tooplate.com/)**.
2. Select a preferred template.
3. Download the **ZIP file** to your local computer.
4. Extract the ZIP file to access the website files.

Mini Finance download link **[here]**(https://www.tooplate.com/download/2135_mini_finance).

### Moving the Website to the Nginx Web Server
Once downloaded, we need to transfer these files to our **Linux EC2 instance**, placing them inside the Nginx web server directory.

In the next session, we will learn how to **upload and configure these website files** to be hosted on our Linux server.

---

## Summary
- The `systemctl` command is used to manage system services.
- We checked the **status** of Nginx and learned how to **start, stop, restart, enable, and disable** services.
- We verified Nginx by accessing the **public IP** of our EC2 instance.
- We downloaded a **static website template** to be hosted on our server.

In the next session, we will move the **website files** to the Nginx web server and finalize our blog website setup.


