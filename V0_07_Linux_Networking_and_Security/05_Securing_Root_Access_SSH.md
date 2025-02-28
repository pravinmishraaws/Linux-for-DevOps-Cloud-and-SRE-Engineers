### **Securing Root Access & SSH in Linux**  

---

## **Building Upon the Previous Lesson**  

In **Lesson 4**, we focused on **troubleshooting network issues** for the **Mini Finance** web application by:  
- **Checking connectivity (IP addresses, firewall, DNS).**  
- **Diagnosing web server and service availability.**  
- **Monitoring network traffic to identify security threats.**  

Now, let’s take a **proactive approach** to security by **hardening SSH access and restricting root login** to prevent unauthorized access.  

---

## **Why is Securing SSH & Root Access Important?**  

### **Real-World Scenario: Protecting Mini Finance from Unauthorized Access**  

As a **DevOps Engineer**, your **Mini Finance web server** is exposed to the internet. You notice multiple **failed SSH login attempts** in your logs, indicating that attackers are trying to gain access.  

By default, SSH allows:  
✔ **Root login**, which is risky if an attacker guesses the password.  
✔ **Password authentication**, making brute-force attacks possible.  

To **secure SSH and root access**, you need to:  
✔ **Disable direct root login.**  
✔ **Enforce key-based authentication.**  
✔ **Restrict SSH to specific users and IPs.**  
✔ **Change the default SSH port.**  

---

## **1. Checking Current SSH Access**  

Before making changes, check how SSH is configured.

### **Step 1: Verify Root Login Access**  
```bash
sudo grep PermitRootLogin /etc/ssh/sshd_config
```
Expected Output:  
```
PermitRootLogin yes
```
If **"yes"**, root login is allowed (which is a security risk).

---

### **Step 2: Check Authentication Method**  
```bash
sudo grep PasswordAuthentication /etc/ssh/sshd_config
```
Expected Output:  
```
PasswordAuthentication yes
```
If **"yes"**, SSH allows password authentication (which is vulnerable to brute-force attacks).

---

## **2. Disabling Root Login Over SSH**  

To **prevent attackers** from logging in as root, disable direct root SSH access.

### **Step 1: Edit the SSH Configuration File**  
```bash
sudo vi /etc/ssh/sshd_config
```
Find the following line:
```
PermitRootLogin yes
```
Change it to:
```
PermitRootLogin no
```

Save the file (`Esc + :wq + Enter`).

### **Step 2: Restart SSH to Apply Changes**  
```bash
sudo systemctl restart sshd
```

✅ **Verification Step:**  
Try logging in as root:
```bash
ssh root@your-vm-ip
```
It should **deny access**.

---

## **3. Enforcing Key-Based Authentication**  

**Why?** Password authentication is vulnerable to brute-force attacks. Instead, use SSH keys for secure access.

### **Step 1: Generate an SSH Key Pair (On Your Local Machine)**  
```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/mini_finance_key
```
Press **Enter** for the default location and set a **passphrase** (optional but recommended).

### **Step 2: Copy the Public Key to the Server**  
```bash
ssh-copy-id -i ~/.ssh/mini_finance_key.pub azureuser@your-vm-ip
```

### **Step 3: Disable Password Authentication (On the Server)**  
```bash
sudo vi /etc/ssh/sshd_config
```
Find:
```
PasswordAuthentication yes
```
Change it to:
```
PasswordAuthentication no
```
Save and restart SSH:
```bash
sudo systemctl restart sshd
```

✅ **Verification Step:**  
Try SSH login with the key:
```bash
ssh -i ~/.ssh/mini_finance_key azureuser@your-vm-ip
```
If successful, password authentication is disabled.

---

## **4. Restricting SSH Access to Specific Users**  

To **limit who can access SSH**, explicitly allow only specific users.

### **Step 1: Edit SSH Configuration**  
```bash
sudo vi /etc/ssh/sshd_config
```
At the end of the file, add:
```
AllowUsers azureuser devops
```
Save and restart SSH:
```bash
sudo systemctl restart sshd
```

✅ **Verification Step:**  
Try logging in with an unauthorized user. It should be **denied**.

---

## **5. Changing the Default SSH Port**  

By default, SSH runs on **port 22**, which attackers often target. Changing the port **adds an extra layer of security**.

### **Step 1: Update the SSH Configuration**  
```bash
sudo vi /etc/ssh/sshd_config
```
Find:
```
#Port 22
```
Change it to:
```
Port 2222
```
Save and restart SSH:
```bash
sudo systemctl restart sshd
```

✅ **Verification Step:**  
Try connecting with the new port:
```bash
ssh -i ~/.ssh/mini_finance_key -p 2222 azureuser@your-vm-ip
```

**Note:** If using **UFW**, allow the new SSH port:
```bash
sudo ufw allow 2222/tcp
sudo ufw reload
```

---

## **6. Monitoring Failed SSH Login Attempts**  

### **Step 1: Check SSH Logs for Failed Login Attempts**  
```bash
sudo grep "Failed password" /var/log/auth.log | tail -10
```
This helps detect **brute-force attacks**.

### **Step 2: Block Repeated Failed Attempts with Fail2Ban**  
```bash
sudo apt install fail2ban -y
```
Create a **custom SSH protection rule**:
```bash
sudo vi /etc/fail2ban/jail.local
```
Add:
```
[sshd]
enabled = true
port = 2222
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 600
```
Save and restart Fail2Ban:
```bash
sudo systemctl restart fail2ban
```

✅ **Verification Step:**  
Check banned IPs:
```bash
sudo fail2ban-client status sshd
```
If an attacker **fails login 3 times**, their IP gets banned for **10 minutes**.

---

## **Hands-On Challenges**  

1. **Check if root login is enabled on your server.**  
```bash
sudo grep PermitRootLogin /etc/ssh/sshd_config
```
2. **Disable password authentication and enforce SSH key login.**  
3. **Restrict SSH access to a specific user.**  
4. **Change the SSH port and allow it in UFW.**  
5. **Install Fail2Ban and verify that it blocks failed login attempts.**  
```bash
sudo fail2ban-client status sshd
```

---

## **Key Takeaways**  

✔ **Disable direct root login to reduce attack surface.**  
✔ **Enforce SSH key-based authentication for stronger security.**  
✔ **Restrict SSH access to specific users to control access.**  
✔ **Change the default SSH port to prevent automated attacks.**  
✔ **Monitor and block repeated failed SSH login attempts using Fail2Ban.**  

---

## **What’s Next?**  

Now that SSH access is **hardened**, the next step is:  

✔ **Configuring Secure File Transfers and Remote Access** using SCP, SFTP, and Rsync.  

Let’s move forward to **Monitoring and Auditing Security Logs**.  
