### **Troubleshooting Network Issues in Linux (Ensuring Mini Finance Availability)**  

---

## **Building Upon the Previous Lesson**  

In **Lesson 3**, we secured the **Mini Finance web server** by:  
- **Configuring UFW** to allow only SSH, HTTP, and HTTPS.  
- **Preventing brute-force attacks** with Fail2Ban.  
- **Restricting SSH access** to specific IPs.  

Now, imagine this scenario:  

Your team reports that the **Mini Finance website is down**. Users are unable to access it. You need to **quickly diagnose and fix the issue**.  

### **Why is Network Troubleshooting Important?**  
As a **DevOps or Cloud Engineer**, you need to:  
✔ **Identify connectivity issues** when applications fail.  
✔ **Check network configurations** and ensure proper routing.  
✔ **Diagnose and resolve DNS and firewall-related problems.**  

By the end of this lesson, you will be able to:  
✔ **Use Linux commands to diagnose network problems.**  
✔ **Fix connectivity, firewall, and DNS issues.**  
✔ **Ensure Mini Finance remains accessible.**  

---

## **1. Understanding Common Network Issues**  

When troubleshooting, always ask:  

- **Is the server accessible?** (Check connectivity)  
- **Are the required ports open?** (Check firewall rules)  
- **Is the domain resolving properly?** (Check DNS)  
- **Are external services reachable?** (Check routing)  

| **Issue Type** | **Possible Causes** |
|--------------|--------------------|
| **Server Unreachable** | Network interface down, incorrect IP configuration. |
| **Website Not Loading** | Web server crash, firewall blocking traffic. |
| **DNS Resolution Issues** | Incorrect DNS records, external DNS issues. |
| **Slow Network Performance** | High bandwidth usage, packet loss. |

---

## **2. Checking Basic Connectivity**  

Before debugging, confirm that the server is **reachable**.

### **Step 1: Check Network Interface**  
```bash
ip a
```
Expected Output:
```
2: eth0: <UP,BROADCAST,RUNNING,MULTICAST> mtu 1500
    inet 20.199.1.55/24 brd 20.199.1.255 scope global dynamic eth0
```
- Ensure the interface is **UP**.  
- The **public IP (20.199.1.55)** should match your Mini Finance VM.  

If missing, restart the network interface:  
```bash
sudo systemctl restart networking
```

---

### **Step 2: Check Server’s Reachability with Ping**  
```bash
ping -c 4 google.com
```
Expected Output:
```
64 bytes from 142.250.72.78: icmp_seq=1 ttl=113 time=20.3 ms
```
If ping fails:  
- The server **may not have internet access**.  
- **Check the default gateway**:  
  ```bash
  ip route
  ```
- Restart the network service:  
  ```bash
  sudo systemctl restart networking
  ```

---

## **3. Checking Firewall Rules (UFW)**  

If the Mini Finance website is **not accessible**, check if the firewall is blocking traffic.  

### **Step 1: Check Open Ports**  
```bash
sudo ufw status verbose
```
Expected Output:
```
To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       192.168.1.100
80/tcp                     ALLOW       Anywhere
443/tcp                    ALLOW       Anywhere
```
- If **port 80 or 443 is missing**, allow them:  
  ```bash
  sudo ufw allow 80/tcp
  sudo ufw allow 443/tcp
  sudo systemctl restart ufw
  ```
- If **SSH is blocked**, re-enable it:  
  ```bash
  sudo ufw allow 22/tcp
  ```

---

## **4. Checking Web Server Status**  

If firewall rules are correct but Mini Finance **still doesn’t load**, check if **Nginx is running**.

### **Step 1: Check Nginx Service**  
```bash
sudo systemctl status nginx
```
Expected Output (if running):  
```
● nginx.service - A high performance web server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2025-02-23 10:30:12 UTC
```
If Nginx is **not running**, restart it:  
```bash
sudo systemctl restart nginx
```

---

## **5. Checking DNS Resolution Issues**  

If Mini Finance is accessible via **IP** but not via **domain name**, the issue may be **DNS-related**.

### **Step 1: Check DNS Resolution**  
```bash
nslookup mini-finance.com
```
Expected Output:
```
Server:         8.8.8.8
Address:        8.8.8.8#53
Non-authoritative answer:
Name:   mini-finance.com
Address: 20.199.1.55
```
- If the **IP doesn’t match the Azure VM**, check DNS settings in the **domain registrar**.
- If DNS is misconfigured, update the correct IP.

### **Step 2: Flush the DNS Cache**  
```bash
sudo systemd-resolve --flush-caches
```

---

## **6. Checking External Connectivity Issues**  

If Mini Finance depends on **external APIs or services**, test their availability.

### **Step 1: Check if an API is Reachable**  
```bash
curl -I https://api.paymentgateway.com
```
Expected Output:
```
HTTP/2 200
content-type: application/json
```
If this fails:  
- The **external API may be down**.  
- Check firewall egress rules:
  ```bash
  sudo ufw status
  ```
- Check outbound connectivity:  
  ```bash
  traceroute api.paymentgateway.com
  ```

---

## **7. Monitoring Network Traffic (Optional for Advanced Debugging)**  

If the issue is **intermittent**, monitor real-time network traffic.

### **Step 1: Install Tcpdump**  
```bash
sudo apt install tcpdump -y
```

### **Step 2: Capture HTTP Traffic**  
```bash
sudo tcpdump -i eth0 port 80 -nn -A
```
- This helps debug incoming **requests to Mini Finance**.
- If no requests appear, users are not reaching the server.

---

## **8. Hands-On Challenge**  

1. **Check if the Mini Finance VM has internet access.**  
```bash
ping -c 4 google.com
```
2. **Verify that firewall rules allow HTTP/HTTPS.**  
```bash
sudo ufw status
```
3. **Restart Nginx if the website is not loading.**  
```bash
sudo systemctl restart nginx
```
4. **Check if Mini Finance is accessible via its domain name.**  
```bash
nslookup mini-finance.com
```
5. **Run a network packet capture to debug intermittent issues.**  
```bash
sudo tcpdump -i eth0 port 80 -nn -A
```

---

## **Key Takeaways**  

✔ **Network troubleshooting is critical to maintaining uptime for production applications.**  
✔ **Start by checking connectivity (IP address, firewall, DNS).**  
✔ **Verify that the web server (Nginx) is running correctly.**  
✔ **Use nslookup to diagnose DNS resolution issues.**  
✔ **Monitor network traffic using tcpdump for deeper debugging.**  

---

## **What’s Next?**  

Now that the **Mini Finance web server is secured and stable**, the next step is:  

✔ **Logging and Monitoring Network Activity** to detect security threats and performance issues.  

Let’s move to **Lesson 5: Logging and Monitoring Network Traffic in Linux**.  
