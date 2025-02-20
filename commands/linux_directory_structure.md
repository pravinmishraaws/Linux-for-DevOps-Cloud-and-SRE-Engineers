# **Linux Directory Structure - A Cloud Engineer’s Guide**  
A well-organized directory structure is a key aspect of Linux, helping Cloud engineers efficiently manage system files, configurations, applications, and user data. This guide explains **major Linux directories**, their **purpose, commands, and hands-on exercises**.

---

## **1. Root Directory (`/`)**
### **What is the Root Directory?**
- The **Root Directory (`/`)** is the **starting point of the entire Linux filesystem**.  
- All files and directories **branch out from here**, similar to how **C:** is the root in Windows.  
- It contains **system-critical files, configurations, and subdirectories**.  
- **Superuser (root) permission** is required to modify its contents.

### **Important Characteristics:**
✅ Represented as a **single forward slash (`/`)**.  
✅ Contains **all system-critical files** and **subdirectories**.  
✅ Requires **superuser permissions** for modifications.  

### **Try it yourself!** 🔥  
```bash
ls -l /
```
This command **lists all directories under `/`**, giving an overview of the system layout.

---

## **2. System Binaries**
### **What are System Binaries?**
- System binaries are **executable programs** that allow **users and system administrators** to **interact with the operating system, perform management tasks, and execute commands**.
- Commands like `ls`, `cp`, `reboot`, and `git` belong to system binaries.

### **Purpose of System Binaries:**
- **`/bin`** → Essential binaries for all users.
- **`/sbin`** → System binaries (mainly for root users).
- **`/usr/bin`** → Non-essential binaries for regular users.

| **Directory** | **Purpose** | **Who Uses It?** | **Example Commands** |
|-------------|------------|------------------|----------------------|
| `/bin` | Essential binaries for all users | All users | `ls`, `cp`, `rm`, `echo` |
| `/sbin` | System admin commands | Root users | `fdisk`, `reboot`, `shutdown` |
| `/usr/bin` | Additional user commands | Regular users | `vim`, `wget`, `git` |

### **Try it yourself!** 🔥  
```bash
ls -l /bin | head -10       # List first 10 commands in /bin
ls -l /sbin | head -10      # List first 10 commands in /sbin
ls -l /usr/bin | head -10   # List first 10 commands in /usr/bin
```
Want to check where **nginx** is installed? Use:
```bash
which nginx
```
It should be in `/usr/sbin/` or `/usr/bin/` if installed.

---

## **3. Configuration Files**
### **What are Configuration Files?**
Configuration files control **system-wide settings for services, networking, users, applications, and security policies**.

### **Purpose of Configuration Files:**
- **`/etc`** → Contains system-wide configuration files.
- **`/etc/default`** → Config files for non-user-installed applications.
- **`/etc/sysconfig`** → Dynamic config files for running applications.

| **Directory** | **Purpose** | **Who Uses It?** | **Example Files** |
|-------------|------------|------------------|----------------------|
| `/etc` | System-wide configuration files | Root users | `/etc/passwd`, `/etc/hosts` |
| `/etc/default` | Config files for system-installed apps | System admins | `/etc/default/grub` |
| `/etc/sysconfig` | Dynamic config files | System services | `/etc/sysconfig/network` |

### **Try it yourself!** 🔥  
```bash
cat /etc/passwd          # View user accounts
cat /etc/hostname        # Show system hostname
cat /etc/os-release      # Display Linux version info
```

---

## **4. Home & User Files**
### **What are Home & User Files?**
Linux stores user-related files under the **home directory**. Each user has a separate directory under `/home/username`, which contains personal files, settings, and preferences.

### **Purpose of Home & User Files:**
- **`/home/username`** → Personal files for users.
- **`/root`** → Superuser's home directory.

| **Directory** | **Purpose** | **Who Uses It?** | **Example Files** |
|-------------|------------|------------------|----------------------|
| `/home` | User directories | Regular users | `/home/user/.bash_history` |
| `/root` | Admin user home | Root users | `/root/.bashrc` |

### **Try it yourself!** 🔥  
```bash
ls -la /home      # List all user home directories
ls -la /root      # List root user’s files (only for sudo users)
```

---

## **5. System Logs**
### **What are System Logs?**
Logs track **system events, user activity, and application actions**. These files are crucial for **debugging, security auditing, and troubleshooting**.

### **Purpose of System Logs:**
- **`/var/log`** → Stores system and application logs.
- **`/var/log/apache2/`** → Web server logs.
- **`/var/log/syslog`** → System logs.

| **Directory** | **Purpose** | **Who Uses It?** | **Example Files** |
|-------------|------------|------------------|----------------------|
| `/var/log` | Stores logs | Root users | `/var/log/syslog` |
| `/var/log/apache2` | Web server logs | System admins | `/var/log/apache2/access.log` |

### **Try it yourself!** 🔥  
```bash
tail -f /var/log/syslog      # View real-time system logs
tail -f /var/log/auth.log    # Track user authentication logs
```

---

## **6. Temporary & Mount Points**
### **What are Temporary & Mount Points?**
Temporary files and mount points allow Linux to **store temporary data and connect external storage like USB drives or network shares**.

### **Purpose of Temporary & Mount Points:**
- **`/tmp`** → Temporary files (cleared on reboot).
- **`/mnt`** → Temporary mount points.
- **`/media`** → Auto-mounted external devices.

| **Directory** | **Purpose** | **Who Uses It?** | **Example Files** |
|-------------|------------|------------------|----------------------|
| `/tmp` | Temporary files | All users | `/tmp/testfile` |
| `/mnt` | Mount points | Root users | `/mnt/usb` |
| `/media` | Auto-mounted devices | System | `/media/cdrom` |

### **Try it yourself!** 🔥  
```bash
ls /tmp           # View temporary files
mount | grep /mnt # Check mounted devices
```

---

## **7. Boot Files**
### **What are Boot Files?**
Boot files are essential for **starting the Linux system**. The bootloader initializes the OS and loads the kernel.

### **Purpose of Boot Files:**
- **`/boot`** → Kernel and bootloader files.
- **`/efi`** → Boot files for EFI systems.

| **Directory** | **Purpose** | **Who Uses It?** | **Example Files** |
|-------------|------------|------------------|----------------------|
| `/boot` | Kernel & bootloader | Root users | `/boot/vmlinuz` |
| `/efi` | Boot files for EFI | Root users | `/efi/bootx64.efi` |

### **Try it yourself!** 🔥  
```bash
ls /boot          # View boot files
ls /efi           # Check EFI boot directory
```

