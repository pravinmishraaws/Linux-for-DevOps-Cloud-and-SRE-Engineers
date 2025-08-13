# **Understanding Networking Commands in Linux**

## **Introduction**
Networking commands in Linux are essential for **configuring, diagnosing, and troubleshooting** network-related issues. They allow you to:
- **Check network configurations**
- **Monitor network activity**
- **Troubleshoot connectivity problems**
- **Manage remote access**

In this lesson, we will cover the following **networking commands**:
- `ifconfig`
- `ping`
- `netstat`
- `ssh`
- `wget`
- `dig`
- `host`
- `hostname`

---

## **1. Checking Network Interfaces with `ifconfig`**
The `ifconfig` command displays and configures **network interfaces** on your system. It helps you check:
- **IP address**
- **Subnet mask**
- **MAC address**
- **Packet transmission statistics**

### **Syntax:**
```bash
ifconfig
```

### **Example Output:**
```bash
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST> mtu 9001
    inet 172.31.14.127 netmask 255.255.240.0 broadcast 172.31.15.255
    inet6 fe80::1e2d:5cff:fe7a:9b80 prefixlen 64 scopeid 0x20<link>
    ether 02:3a:5c:7f:b1:33 txqueuelen 1000 (Ethernet)
    RX packets 12435  bytes 8745632 (8.7 MB)
    TX packets 8543  bytes 5123021 (5.1 MB)
```

### **Key Fields Explained**
| Field | Description |
|--------|-------------|
| **eth0** | Name of the network interface |
| **inet** | IPv4 address of the system |
| **netmask** | Subnet mask |
| **broadcast** | Broadcast address |
| **inet6** | IPv6 address |
| **ether** | MAC address (hardware address) |
| **RX packets** | Number of received packets |
| **TX packets** | Number of transmitted packets |

**Loopback Interface (`lo`)**:  
- The `lo` (loopback) interface is a **virtual** network interface.
- Used for **internal system communication**.
```bash
lo: flags=73<UP,LOOPBACK,RUNNING> mtu 65536
    inet 127.0.0.1  netmask 255.0.0.0
```

**New Names (ens5): Why the name changed**
The old names (eth0, eth1) were assigned in the order devices were detected, which could change between boots.
To make names persistent and predictable, systemd’s Predictable Network Interface Names policy was introduced.

Now names are based on:
- The physical slot on the motherboard (PCI location)
- The device’s MAC address
- Or the firmware-provided index

So you get names like:
- ens5 → Ethernet, slot 5
- enp0s3 → Ethernet, bus 0, slot 3

---

## **2. Checking Network Connectivity with `ping`**
The `ping` command tests **network connectivity** by sending small **ICMP echo requests** to a target host.

### **Syntax:**
```bash
ping <IP address or domain>
```

### **Example:**
```bash
ping 8.8.8.8
```
This sends packets to **Google's public DNS server (8.8.8.8)** and waits for a response.

### **Example Output:**
```bash
64 bytes from 8.8.8.8: icmp_seq=1 ttl=54 time=20.3 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=54 time=18.7 ms
```

### **Key Fields Explained**
| Field | Description |
|--------|-------------|
| **icmp_seq** | Sequence number of the packet |
| **ttl** | Time-to-Live (maximum hops before discarding) |
| **time** | Time taken for the packet round trip |

To **stop the ping process**, press **`CTRL + C`**.

---

## **3. Viewing Network Statistics with `netstat`**
The `netstat` command displays **network connections**, **routing tables**, and **interface statistics**.

### **Syntax:**
```bash
netstat [options]
```

### **Common Options**
| Option | Description |
|--------|-------------|
| `-a` | Show all network connections |
| `-t` | Display only TCP connections |
| `-u` | Display only UDP connections |
| `-n` | Show numerical addresses (instead of domain names) |
| `-r` | Show routing table |
| `-s` | Display network statistics |

### **Example:**
```bash
netstat -t
```
This lists all active **TCP connections**.

---

## **4. Securely Connecting to Remote Systems with `ssh`**
The `ssh` (Secure Shell) command allows **secure remote access** to another Linux system.

### **Syntax:**
```bash
ssh -i <key_file> <user>@<hostname>
```

### **Example:**
```bash
ssh -i mykey.pem ec2-user@54.123.45.67
```
This securely connects to an **AWS EC2 instance** using a **private key (`.pem` file)**.

---

## **5. Downloading Files from the Internet with `wget`**
The `wget` command **downloads files** from the internet.

### **Syntax:**
```bash
wget <URL>
```

### **Example:**
```bash
wget https://curl.se/download/curl-7.79.1.tar.gz
```
This downloads the **Curl application source code**.

### **Common Options**
| Option | Description |
|--------|-------------|
| `-O <file>` | Save file with a specific name |
| `-P <dir>` | Save file in a specific directory |
| `-c` | Resume an interrupted download |

---

## **6. Querying DNS Information with `dig`**
The `dig` (Domain Information Groper) command retrieves **DNS information**.

### **Syntax:**
```bash
dig <domain>
```

### **Example:**
```bash
dig google.com
```

### **Example Output:**
```bash
;; ANSWER SECTION:
google.com.  86400  IN  A  142.250.190.78
```

### **Key Fields Explained**
| Field | Description |
|--------|-------------|
| **QUESTION SECTION** | Shows the domain name queried |
| **ANSWER SECTION** | Displays the IP address |
| **QUERY TIME** | Time taken for response |

---

## **7. Performing DNS Lookups with `host`**
The `host` command retrieves **IP addresses of domain names**.

### **Syntax:**
```bash
host <domain>
```

### **Example:**
```bash
host google.com
```
**Output:**
```bash
google.com has address 142.250.190.78
google.com mail is handled by 40 smtp.google.com.
```
- Shows **A records** (IP addresses).
- Displays **MX records** (Mail servers).

---

## **8. Checking Hostname with `hostname`**
The `hostname` command displays the **system’s hostname**.

### **Syntax:**
```bash
hostname
```

### **Example Output:**
```bash
ip-172-31-14-127.ec2.internal
```
This is the **hostname of an AWS EC2 instance**.

---

## **9. Summary of Networking Commands**
| Command | Purpose |
|---------|---------|
| `ifconfig` | Displays network interface details |
| `ping` | Checks network connectivity |
| `netstat` | Displays network statistics |
| `ssh` | Secure remote access |
| `wget` | Downloads files from the internet |
| `dig` | Queries DNS information |
| `host` | Performs DNS lookups |
| `hostname` | Displays system hostname |

---

## **Next Steps**
You now have a strong foundation in **Linux networking commands**. Next, we will explore **system information commands** to gather insights about hardware, disk usage, and performance.
