# **Working with System Information Commands in Linux**

## **Introduction**
System information commands in Linux help users gather details about their system, such as **operating system details, uptime, user sessions, disk usage, and memory consumption**. These commands are essential for monitoring system health, optimizing performance, and troubleshooting issues.

In this lesson, we will cover the following **system information commands**:
1. `uname` – Display system information  
2. `uptime` – Check system uptime  
3. `whoami` – Identify the current user  
4. `who` – Display logged-in users  
5. `df` – Check disk space usage  
6. `du` – Analyze disk usage by files/directories  
7. `free` – View memory usage  

---

## **1. Displaying System Information with `uname`**
The `uname` command displays details about the **operating system, kernel version, hardware architecture, and hostname**.

### **Syntax:**
```bash
uname [options]
```

### **Common Options**
| Option | Description |
|--------|-------------|
| `-a` | Display all system information |
| `-s` | Show only the operating system name |
| `-r` | Display kernel release version |
| `-m` | Show hardware architecture (e.g., `x86_64`) |
| `-n` | Display system hostname |

### **Examples**
```bash
uname -a
```
**Output:**
```bash
Linux ip-172-31-14-127 5.4.0-1045-aws x86_64 GNU/Linux
```
This output provides:
- **OS name:** `Linux`
- **Hostname:** `ip-172-31-14-127`
- **Kernel version:** `5.4.0-1045-aws`
- **Hardware architecture:** `x86_64`

To get only the **operating system name**:
```bash
uname -s
```
**Output:**
```bash
Linux
```

---

## **2. Checking System Uptime with `uptime`**
The `uptime` command shows **how long the system has been running**, the **number of logged-in users**, and the **system load averages**.

### **Syntax:**
```bash
uptime
```

### **Example Output:**
```bash
10:15:00 up 2 days, 4:30,  2 users,  load average: 0.05, 0.03, 0.02
```

### **Key Fields Explained**
| Field | Description |
|--------|-------------|
| **Current Time** | The system clock (`10:15:00`) |
| **Uptime** | How long the system has been running (`2 days, 4:30 hours`) |
| **Users** | Number of active users (`2 users`) |
| **Load Average** | CPU load over 1, 5, and 15 minutes (`0.05, 0.03, 0.02`) |

To display uptime in a **human-readable format**:
```bash
uptime -p
```
**Output:**
```bash
up 2 days, 4 hours, 30 minutes
```

---

## **3. Identifying the Current User with `whoami`**
The `whoami` command prints the username of the current logged-in user.

### **Syntax:**
```bash
whoami
```

### **Example Output:**
```bash
ec2-user
```
This indicates that the currently logged-in user is **`ec2-user`**.

---

## **4. Displaying Logged-In Users with `who`**
The `who` command shows all **currently logged-in users**.

### **Syntax:**
```bash
who
```

### **Example Output:**
```bash
ec2-user pts/0  2025-02-28 10:12 (192.168.1.10)
```

### **Key Fields Explained**
| Field | Description |
|--------|-------------|
| **User** | Name of the logged-in user (`ec2-user`) |
| **Terminal** | Terminal session (`pts/0`) |
| **Login Time** | When the user logged in (`2025-02-28 10:12`) |
| **Host** | Remote machine IP (`192.168.1.10`) |

To display a **summary of logged-in users**:
```bash
who -q
```
**Output:**
```bash
ec2-user
# users=1
```

---

## **5. Checking Disk Space Usage with `df`**
The `df` command shows **disk space usage** of file systems.

### **Syntax:**
```bash
df [options]
```

### **Common Options**
| Option | Description |
|--------|-------------|
| `-h` | Show output in human-readable format (`KB`, `MB`, `GB`) |
| `-t` | Display file system types |
| `--total` | Show total disk space usage |

### **Example**
```bash
df -h
```
**Output:**
```bash
Filesystem      Size  Used  Avail  Use%  Mounted on
/dev/xvda1      30G   12G   18G    40%   /
```

### **Key Fields Explained**
| Field | Description |
|--------|-------------|
| **Filesystem** | Name of storage device (`/dev/xvda1`) |
| **Size** | Total disk space (`30G`) |
| **Used** | Used space (`12G`) |
| **Available** | Free space (`18G`) |
| **Use%** | Percentage of disk used (`40%`) |
| **Mounted on** | Mount location (`/`) |

---

## **6. Analyzing Disk Usage with `du`**
The `du` (disk usage) command **analyzes the space consumed** by files and directories.

### **Syntax:**
```bash
du [options] <directory>
```

### **Common Options**
| Option | Description |
|--------|-------------|
| `-h` | Human-readable sizes (`KB`, `MB`, `GB`) |
| `-s` | Show **only** the total size |
| `-c` | Show cumulative total size |

### **Example:**
```bash
du -h /var/www/html
```
**Output:**
```bash
4.0K  /var/www/html/index.html
12M   /var/www/html/images
60M   /var/www/html
```
This shows:
- `4.0K` for `index.html`
- `12M` for `images/`
- `60M` total space used in `/var/www/html`

To show **only** the total space:
```bash
du -sh /var/www/html
```
**Output:**
```bash
60M  /var/www/html
```

---

## **7. Checking Memory Usage with `free`**
The `free` command displays **RAM and swap memory usage**.

### **Syntax:**
```bash
free [options]
```

### **Common Options**
| Option | Description |
|--------|-------------|
| `-h` | Show human-readable output (`KB`, `MB`, `GB`) |
| `-t` | Show total memory (RAM + swap) |

### **Example**
```bash
free -h
```
**Output:**
```bash
              total   used   free   shared  buff/cache  available
Mem:           8GB    2GB    5GB    100MB     1GB       5GB
Swap:          2GB    500MB  1.5GB
```

### **Key Fields Explained**
| Field | Description |
|--------|-------------|
| **Total** | Total RAM (`8GB`) |
| **Used** | Used RAM (`2GB`) |
| **Free** | Available RAM (`5GB`) |
| **Swap** | Virtual memory usage (`500MB` used) |

---

## **8. Summary of System Information Commands**
| Command | Purpose |
|---------|---------|
| `uname` | Display system/kernel information |
| `uptime` | Show system uptime and load average |
| `whoami` | Identify the current user |
| `who` | Display logged-in users |
| `df` | Show disk space usage |
| `du` | Analyze disk usage by files and directories |
| `free` | Check memory usage |

---

## **Next Steps**
Understanding **system information commands** helps in monitoring system health, diagnosing issues, and ensuring optimal resource usage. In the next section, we will dive into **Shell Scripting** to automate administrative tasks.
