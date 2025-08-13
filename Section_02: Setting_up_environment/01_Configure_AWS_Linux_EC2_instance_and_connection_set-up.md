# Study Notes: Setting Up Environment

## Why Use AWS for Linux Learning?
Not everyone has a Linux-based laptop or desktop. To simplify learning, we can use an **AWS EC2 instance** as our Linux virtual machine. This allows us to:
- Practice Linux commands in a cloud-based environment.
- Avoid modifying our local system.
- Use AWS resources just like in real-world cloud operations.

In this section, we will learn how to **set up an AWS Linux EC2 instance** and connect to it using different methods.

---

## Creating an AWS Linux EC2 Instance
AWS EC2 (Elastic Compute Cloud) allows users to rent virtual machines (VMs) in the cloud. Here’s how to create an EC2 instance:

1. **Log in to AWS** – Sign in to your AWS account as an IAM user.
2. **Navigate to EC2** – Open the **EC2 Dashboard**.
3. **Launch a New Instance**:
   - Click **Launch Instance**.
   - Provide a **name** for your instance.
   - Choose an **Amazon Machine Image (AMI)** – Select **Amazon Linux 2023 (kernel 6.12)** for the latest features and security updates.
   - Select **Instance Type** – Choose `t2.micro or t3.micro` (free-tier eligible, 1GB RAM).
   - Choose an **existing key pair** (or create a new one for secure access).
   - Configure Security Group
     - Allow SSH (port 22) from your IP for remote access.
     - Allow HTTP (port 80) from anywhere for web traffic.
       <img width="948" height="632" alt="Screenshot 2025-08-13 at 05 13 03" src="https://github.com/user-attachments/assets/6f0a15df-1244-472f-a143-8c68abc34409" />

   - Keep other settings default and **launch the instance**.
4. **Monitor the Instance**:
   - Go to the **Instances list**.
   - Wait until the **instance state** changes from `pending` to `running`.
   - Ensure the **status check** shows `2/2 checks passed`.

Once the instance is running, we can connect to it using different methods.

---

## Methods to Connect to an EC2 Instance
Once the instance is running, we need a way to access it. AWS provides four connection methods:

### 1. EC2 Instance Connect
- **Easiest method** to connect directly from the AWS Management Console.
- No additional software required.
- Works only for Amazon Linux and Ubuntu instances.
- Steps:
  - Select the EC2 instance.
  - Click **Connect** → **EC2 Instance Connect** → **Connect**.
  - A web-based terminal opens.
  - Run `whoami` to check the logged-in user (`ec2-user`).

### 2. AWS Systems Manager Session Manager
- Allows connection **without usernames, passwords, or SSH keys**.
- Provides secure access without opening inbound ports.
- Useful for enterprises needing **access control and auditing**.
- Features:
  - Logs all session activities for security auditing.
  - Manages **user permissions** to control access.
  - Secure connection via AWS CLI or Console.

### 3. SSH Client
- Traditional method using **Secure Shell (SSH)** from a local machine.
- Provides full control over the EC2 instance.
- Requires an SSH key (`.pem` file) for secure access.
- Steps:
  - Open a **terminal** (PowerShell, Linux Terminal, or macOS Terminal).
  - Navigate to the directory where the `.pem` file is stored.
  - Use the following syntax:
    ```bash
    ssh -i "your-key.pem" ec2-user@your-ec2-public-ip
    ```
  - Run `whoami` to confirm connection.

### 4. EC2 Serial Console
- A **backup connection method** when other connections fail.
- Used for troubleshooting network issues or misconfigurations.
- Provides **direct access to the instance console**.
- Helpful when an instance is unresponsive.

---

## Understanding SSH and Secure Connection
SSH (Secure Shell) enables secure remote access to Linux servers. It connects the **shell** of a local machine to the **shell** of a remote EC2 instance using a security key (`.pem` file). 

Here’s how SSH works:
- The **client (your local computer)** sends a request to the **server (EC2 instance)**.
- The request is **encrypted** using a private key (`.pem` file).
- The **server verifies** the private key before granting access.
- Once connected, users can **execute commands, transfer files, and manage applications**.

#### SSH Command Breakdown:
```bash
ssh -i "my-key.pem" ec2-user@54.123.45.67
```
- `ssh` – Secure Shell command.
- `-i "my-key.pem"` – Specifies the private key file.
- `ec2-user@54.123.45.67` – User and **public IP** of the EC2 instance.

#### Steps to Connect via SSH:
1. Open the terminal (Command Prompt, PowerShell, or macOS Terminal).
2. Navigate to the folder where your **key pair (.pem file)** is stored.
3. Run the SSH command.
4. If successful, you will be logged into your EC2 instance.
5. Run `whoami` to verify your user (`ec2-user`).

---

## Troubleshooting Connection Issues
If you are unable to connect to your EC2 instance:
1. **Check the instance status** – Ensure it is running and `2/2 checks passed`.
2. **Verify the security group rules** – Ensure **port 22 (SSH)** is open for your IP.
3. **Confirm the key pair** – Use the correct `.pem` file when connecting.
4. **Check file permissions** – The private key must have `chmod 400` permissions:
   ```bash
   chmod 400 my-key.pem
   ```
5. **Use EC2 Serial Console** – If the network is down, access the instance using the serial console.

---

## Summary
- We created a **Linux EC2 instance** in AWS.
- Learned **four ways to connect** to the instance: **EC2 Instance Connect, Session Manager, SSH, and EC2 Serial Console**.
- Understood how **SSH works** and the importance of the `.pem` file.
- Discussed **troubleshooting methods** for connection issues.

Now that our environment is ready, we can start exploring **Linux commands** in the next section.


