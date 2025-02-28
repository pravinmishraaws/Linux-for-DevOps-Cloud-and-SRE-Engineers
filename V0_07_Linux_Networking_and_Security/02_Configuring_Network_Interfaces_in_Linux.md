# Configuring Network Interfaces in Linux (Mini Finance Web Server)

In Lesson 1, we learned how to view network configurations and diagnose connectivity issues for our Mini Finance web server hosted on an Azure Virtual Machine. However, we identified a critical problem—our public IP keeps changing every time the VM restarts.

To ensure Mini Finance remains accessible, we need to:
✔ Assign a static private IP for internal communication.
✔ Configure a static public IP in Azure to avoid frequent changes.
✔ Ensure network settings persist across reboots.



## **1. Why Do Network Interfaces Matter?**  

### **Real-World Problem Statement**  

You are managing the **Mini Finance web server** on an **Azure VM**. Everything works fine until one day, after a scheduled VM restart, your team reports:  

- **The website is down**, even though the VM is running.  
- The **public IP has changed**, breaking the DNS configuration.  
- The **database running on a private IP is unreachable**, causing backend failures.  

This happens because **network interfaces** are dynamically assigned unless explicitly configured.  

By the end of this lesson, you will be able to:  

✔ **Modify network interface settings on your Azure VM**.  
✔ **Set up a static private IP for persistent internal communication**.  
✔ **Ensure Mini Finance remains accessible after reboots with a static public IP**.  

---

## **2. Understanding Network Interfaces**  

A **network interface** is a virtual or physical adapter that connects a machine to a network.  

### **Azure VM Network Interfaces**  

Your **Azure VM** has:  
- **A public IP** for external access.  
- **A private IP** for internal communication within the Azure network.  

#### **List Available Network Interfaces**  
```bash
ip a
```
Expected Output:
```
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    inet 10.0.0.4/24 brd 10.0.0.255 scope global eth0
```
- **eth0** is the primary network interface.  
- **10.0.0.4** is the private IP assigned by Azure.  

---

## **3. Setting a Static Private IP Address**  

By default, Azure assigns **dynamic private IPs**, which can change after VM restarts. To ensure **Mini Finance’s database server** remains accessible, we need to assign a **static private IP**.

### **Step 1: Open the Network Configuration File**  
```bash
sudo vi /etc/netplan/50-cloud-init.yaml
```
Locate this section:
```yaml
network:
    ethernets:
        eth0:
            dhcp4: true
```
### **Step 2: Change DHCP to a Static IP**  
Modify it as follows:
```yaml
network:
    ethernets:
        eth0:
            dhcp4: no
            addresses:
                - 10.0.0.10/24
            gateway4: 10.0.0.1
            nameservers:
                addresses:
                    - 8.8.8.8
                    - 8.8.4.4
```
- **10.0.0.10** → Static private IP for Mini Finance web server.  
- **10.0.0.1** → Default gateway.  
- **8.8.8.8 & 8.8.4.4** → Google DNS for name resolution.  

### **Step 3: Apply the Changes and Restart Networking**  
```bash
sudo netplan apply
```
Verify the new IP:  
```bash
ip a show eth0
```
Expected Output:  
```
inet 10.0.0.10/24 brd 10.0.0.255 scope global eth0
```

---

## **4. Assigning a Static Public IP in Azure**  

A **changing public IP** means that users cannot reliably access **Mini Finance**. We need to configure a **static public IP** in Azure.  

### **Step 1: Assign a Static Public IP via Azure Portal**  
1. Open **Azure Portal** → Go to **Virtual Machines**.  
2. Select your VM → Click **Networking**.  
3. Under **Public IP Address**, click **Detach**.  
4. Click **Create a New Public IP** → Set it to **Static**.  
5. Click **Save** and restart the VM.  

### **Step 2: Verify the New Public IP**  
```bash
curl -s ifconfig.me
```
Expected Output:
```
20.199.1.100
```
Now, your Mini Finance website remains accessible at **http://20.199.1.100** even after a reboot.  

---

## **5. Ensuring Network Settings Persist After Reboot**  

Even with static IPs, we must **ensure settings persist**.  

### **Step 1: Check Network Configuration on Boot**  
```bash
cat /etc/netplan/50-cloud-init.yaml
```
Ensure the configuration matches what we set earlier.  

### **Step 2: Restart the VM and Test Connectivity**  
Reboot your VM:  
```bash
sudo reboot
```
After the reboot, verify:  
```bash
curl -I http://20.199.1.100
```
Expected Output:
```
HTTP/1.1 200 OK
```
If the website is reachable, networking is correctly configured.  

---

## **6. Hands-On Challenge**  

1. **List network interfaces and identify their IPs**  
```bash
ip a
```
2. **Modify the network configuration to set a static private IP**  
```bash
sudo vi /etc/netplan/50-cloud-init.yaml
```
3. **Apply the changes and verify**  
```bash
sudo netplan apply
ip a show eth0
```
4. **Assign a static public IP via the Azure Portal**  
5. **Reboot the VM and confirm Mini Finance is accessible**  
```bash
curl -I http://your-static-public-ip
```

---

## **Key Takeaways**  

✔ **Static IPs prevent connectivity issues for cloud-hosted applications**.  
✔ **Private IPs ensure reliable internal communication** between services.  
✔ **Public IPs must be static** to prevent domain resolution failures.  
✔ **Netplan is used in Ubuntu** to configure network interfaces persistently.  

---

## **What’s Next?**  

Now that our **Mini Finance web server** has a **reliable network configuration**, the next step is to:  

✔ **Secure network access with firewall rules** to protect the website from attacks.  

Let’s move to **Lesson 3: Firewall Management in Linux**.  
