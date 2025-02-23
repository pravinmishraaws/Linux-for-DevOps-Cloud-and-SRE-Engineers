### Firewall Management in Linux (Securing the Mini Finance Web Server)**  

#### **Building Upon the Previous Lesson**  

In **Lesson 2**, we successfully assigned **static private and public IPs** to our **Mini Finance web server** running on an **Azure VM**. Now, the website remains accessible even after VM reboots.  

However, having an **exposed public IP** poses a **security risk**. Malicious users can:  
- **Scan open ports** to exploit vulnerabilities.  
- **Attempt brute-force SSH attacks** to gain unauthorized access.  
- **Send malicious requests** to bring down our website.  

### **Why Do We Need a Firewall?**  
A **firewall** controls which incoming and outgoing connections are **allowed** or **blocked**, protecting the server from **unauthorized access and attacks**.  

By the end of this lesson, you will be able to:  

✔ **Set up and configure a firewall using UFW (Uncomplicated Firewall).**  
✔ **Allow only necessary services (HTTP, HTTPS, and SSH) while blocking all others.**  
✔ **Prevent brute-force SSH attacks using fail2ban.**  
✔ **Ensure Mini Finance remains secure while accessible to legitimate users.**  

---

## **1. Understanding Firewalls in Linux**  

A **firewall** filters **incoming and outgoing traffic** based on **predefined rules**. In Linux, we typically use:  

| **Firewall Tool** | **Description** |
|------------------|----------------|
| **UFW (Uncomplicated Firewall)** | A user-friendly interface for iptables. Recommended for most use cases. |
| **iptables** | A powerful but complex firewall tool for fine-grained rule customization. |
| **firewalld** | Default firewall manager in RHEL/CentOS. |

For this lesson, we will use **UFW**, as it simplifies firewall management while offering robust security.

---

## **2. Checking the Current Firewall Status**  

Before making changes, check if UFW is installed and running.  

```bash
sudo ufw status
```
Expected Output:
```
Status: inactive
```
If it’s inactive, we need to **enable and configure it**.

---

## **3. Allowing Essential Services**  

Since we are running a **public-facing web server**, we must allow only:  

✔ **SSH (Port 22)** – For remote access.  
✔ **HTTP (Port 80)** – To serve Mini Finance over HTTP.  
✔ **HTTPS (Port 443)** – For secure connections.  

### **Step 1: Enable UFW**  
```bash
sudo ufw enable
```

### **Step 2: Allow Necessary Ports**  
```bash
sudo ufw allow 22/tcp   # Allow SSH
sudo ufw allow 80/tcp   # Allow HTTP
sudo ufw allow 443/tcp  # Allow HTTPS
```

### **Step 3: Deny All Other Incoming Traffic**  
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### **Step 4: Check Firewall Rules**  
```bash
sudo ufw status verbose
```
Expected Output:
```
Status: active
To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
443/tcp                    ALLOW       Anywhere
Anywhere                   DENY        Anywhere (incoming)
```
This means:  
- **SSH, HTTP, and HTTPS traffic is allowed.**  
- **All other incoming connections are blocked.**  

---

## **4. Securing SSH Access (Preventing Unauthorized Logins)**  

By default, SSH is **open to everyone**, making it vulnerable to **brute-force attacks**. To prevent unauthorized access:  

### **Step 1: Limit SSH Access to a Specific IP**  
If you connect from a **fixed IP**, restrict SSH access:  
```bash
sudo ufw allow from <your-ip> to any port 22
```
For example, to allow SSH only from **192.168.1.100**:  
```bash
sudo ufw allow from 192.168.1.100 to any port 22
```

### **Step 2: Change the Default SSH Port**  
Modify the SSH configuration file:  
```bash
sudo vi /etc/ssh/sshd_config
```
Find this line:
```
#Port 22
```
Change it to a custom port (e.g., 2222):  
```
Port 2222
```
Restart SSH for changes to take effect:  
```bash
sudo systemctl restart sshd
```
Now, allow the new port in UFW:  
```bash
sudo ufw allow 2222/tcp
sudo ufw delete allow 22/tcp
```
Verify the firewall status:  
```bash
sudo ufw status
```

---

## **5. Protecting Against Brute-Force Attacks with Fail2Ban**  

Even with firewall rules, **attackers may repeatedly try logging in** using different passwords. To prevent this, we install **fail2ban**, which **blocks IPs after multiple failed login attempts**.

### **Step 1: Install Fail2Ban**  
```bash
sudo apt install fail2ban -y
```

### **Step 2: Configure SSH Protection**  
```bash
sudo vi /etc/fail2ban/jail.local
```
Add the following:  
```
[sshd]
enabled = true
maxretry = 3
bantime = 600
```
- **maxretry = 3** → Blocks IP after **3 failed login attempts**.  
- **bantime = 600** → Blocks the attacker for **10 minutes**.  

### **Step 3: Restart Fail2Ban**  
```bash
sudo systemctl restart fail2ban
```

### **Step 4: Check Banned IPs**  
```bash
sudo fail2ban-client status sshd
```

Expected Output:
```
Status for the jail: sshd
|- Number of failed attempts: 5
`- Currently banned IPs: 192.168.1.200
```
This confirms that Fail2Ban is **actively blocking unauthorized SSH login attempts**.

---

## **6. Testing the Firewall Setup**  

After configuring **UFW and Fail2Ban**, test the security setup:

### **Test 1: Verify Open Ports**  
From another machine, check open ports:  
```bash
nmap -p 22,80,443 <your-server-ip>
```
Expected Output:
```
22/tcp  open   ssh
80/tcp  open   http
443/tcp open   https
```
All other ports should be **closed**.

### **Test 2: Attempt SSH Brute-Force Attack**  
Simulate multiple failed SSH login attempts:  
```bash
ssh wronguser@your-server-ip
ssh wronguser@your-server-ip
ssh wronguser@your-server-ip
```
After three attempts, check if your IP is banned:  
```bash
sudo fail2ban-client status sshd
```

---

## **7. Hands-On Challenge**  

1. **Enable UFW and configure rules for Mini Finance.**  
```bash
sudo ufw enable
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw default deny incoming
```
2. **Restrict SSH access to your IP only.**  
```bash
sudo ufw allow from <your-ip> to any port 22
```
3. **Install and configure Fail2Ban to protect SSH.**  
```bash
sudo apt install fail2ban -y
sudo vi /etc/fail2ban/jail.local
```
4. **Restart services and test security settings.**  
```bash
sudo systemctl restart fail2ban
sudo ufw status verbose
```

---

## **Key Takeaways**  

✔ **Firewalls restrict access to only necessary services** (SSH, HTTP, HTTPS).  
✔ **Fail2Ban prevents brute-force attacks** by blocking IPs with failed login attempts.  
✔ **Changing the default SSH port enhances security** against automated attacks.  
✔ **UFW simplifies firewall management** while providing robust protection.  

---

## **What’s Next?**  

Now that the **Mini Finance web server** is **secure from unauthorized access**, the next step is:  

✔ **Troubleshooting network issues** to ensure smooth server operation.  

Let’s move to **Lesson 4: Troubleshooting Network Issues in Linux**.  
