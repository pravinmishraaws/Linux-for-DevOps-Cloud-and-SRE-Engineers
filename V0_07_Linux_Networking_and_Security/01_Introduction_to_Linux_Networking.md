# **Lesson 1: Introduction to Linux Networking (Mini Finance Web Server)**  

## **Prerequisites**  

Before starting this lesson, ensure you have completed the previous module where we **deployed the Mini Finance static website** on an **Azure Virtual Machine using Nginx**. If you haven’t, follow these steps first:  

You can follow steps [here from the last module](https://github.com/pravinmishraaws/Linux-for-Cloud-Engineers-A-Complete-Project-Based-Learning/blob/main/06_Linux_Automation_and_Text_Processing/01_Searching_Files_in_Linux.md)

1. **Set up an Azure VM running Ubuntu 20.04 or later**.  
2. **Deploy the Mini Finance static website in the `/var/www/mini_finance/` directory**.  
3. **Configure Nginx to serve the website**.  

If the Mini Finance website is already running, proceed with this lesson.  

---

## **Why Should DevOps and Cloud Engineers Learn Linux Networking?**  

### **Real-World Problem Statement**  

You are a **DevOps Engineer** responsible for hosting and maintaining **Mini Finance**, a **static website** running on **an Azure Virtual Machine** with **Nginx**. Everything seems fine—until your team reports:  

- The website **randomly goes offline**, and you need to **troubleshoot connectivity issues**.  
- The site is **accessible via IP but not by domain**, meaning **DNS resolution is broken**.  
- The public IP **keeps changing** after VM restarts, requiring **a static IP configuration**.  

These are all **real-world networking problems** DevOps Engineers face daily. If you don’t understand **Linux networking**, troubleshooting these issues will be **time-consuming and frustrating**.  

By the end of this lesson, you will be able to:  

✔ **Understand networking basics** and how they affect cloud-hosted applications.  
✔ **View and analyze network configurations** on an Azure VM.  
✔ **Troubleshoot common connectivity problems** to keep your site online.  

---

## **1. Why Networking Matters for Mini Finance?**  

Your Mini Finance website is a **real-world web application** running on an **Azure VM**. Without proper networking knowledge:  

- **Users won't be able to access the site** (if the web server is not reachable).  
- **DNS might not work** (if domain name resolution is misconfigured).  
- **Security vulnerabilities** might expose the site to attacks (if firewall rules are not correctly set).  

---

## **2. Understanding Basic Networking Concepts**  

To diagnose networking issues, you first need to **understand how Linux handles network communication**.

### **2.1 Public vs. Private IP Addresses**  

Your **Azure VM** has two types of IP addresses:  

- **Public IP** → Users access your Mini Finance website through this.  
- **Private IP** → Used for internal communication between services within Azure.  

#### **Check Your VM’s Public IP (This is what your users see)**  
```bash
curl -s ifconfig.me
```
Expected Output:  
```
20.199.1.55
```
This is the **external IP** users use to visit `http://20.199.1.55`.  

#### **Check Your VM’s Private IP (Used inside Azure’s network)**  
```bash
ip a | grep inet
```
Expected Output:  
```
inet 10.0.0.4/24 brd 10.0.0.255 scope global dynamic eth0
```
- **10.0.0.4** → Internal network communication (e.g., between a database and web server).  

### **Why Does This Matter for Mini Finance?**  
If your **public IP changes** frequently, the website might **become unreachable**. You will need a **static IP** (covered in Lesson 2).  

---

## **3. Checking if Your Web Server is Reachable**  

Now that you understand **IP addresses**, let’s ensure your **Mini Finance website** is actually **accessible**.

#### **Step 1: Test Network Connectivity to Your VM**  
```bash
ping -c 4 google.com
```
Expected Output:  
```
64 bytes from 142.250.182.46: icmp_seq=1 ttl=118 time=21.3 ms
```
If this **fails**, your VM **does not have internet access**.

#### **Step 2: Check if the Mini Finance Website is Running**  
```bash
curl -I http://20.199.1.55
```
Expected Output (If Nginx is running):  
```
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
```
**If this fails, your site is down!** We will troubleshoot in the next section.

---

## **4. Checking DNS Resolution for Mini Finance**  

If you configured a **custom domain** (e.g., `mini-finance.com`), it must point to your **Azure VM’s Public IP**.

#### **Check If Your Domain Resolves to the Correct IP**  
```bash
nslookup mini-finance.com
```
Expected Output:  
```
Server:  8.8.8.8
Address:  8.8.8.8#53

Non-authoritative answer:
Name: mini-finance.com
Address: 20.199.1.55
```
If the **IP does not match**, update your **DNS records** in your domain registrar.

### **Why Does This Matter for Mini Finance?**  
If **DNS is misconfigured**, your website might **work via IP but not via domain name**.  

---

## **5. Checking If Nginx is Listening on the Correct Ports**  

If Mini Finance is not accessible, check whether **Nginx is running** and listening on **port 80**.

#### **Check If Nginx is Active**  
```bash
sudo systemctl status nginx
```
Expected Output:  
```
Active: active (running)
```
If it’s **inactive**, restart it:  
```bash
sudo systemctl restart nginx
```

#### **Check Open Ports**  
```bash
sudo netstat -tulnp | grep nginx
```
Expected Output:  
```
tcp  0  0 0.0.0.0:80  0.0.0.0:* LISTEN  1234/nginx
```
If **port 80 is missing**, **Nginx is not listening**, meaning **users cannot access your site**.

---

## **6. Hands-On Challenge**  

1. **Find Your VM’s Public and Private IP Addresses**  
```bash
curl -s ifconfig.me
ip a | grep inet
```
2. **Ensure Your Website is Reachable**  
```bash
curl -I http://your-vm-ip
```
3. **Test DNS Resolution for Your Domain**  
```bash
nslookup mini-finance.com
```
4. **Check Open Ports on Your Web Server**  
```bash
sudo netstat -tulnp | grep nginx
```

---

## **Key Takeaways**  

✔ **Public and private IPs determine how your Mini Finance server communicates.**  
✔ **Network interfaces allow data transmission and must be correctly configured.**  
✔ **DNS resolution is critical for users to access the website via a domain name.**  
✔ **Troubleshooting tools like `ping`, `netstat`, and `curl` help diagnose connectivity issues.**  

---

## **What’s Next?**  

Now that we understand **networking basics**, the next step is to:  

✔ **Configure network interfaces** to ensure the Mini Finance website has a **static IP** and remains accessible after reboots.  

