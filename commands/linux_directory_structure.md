# **Linux Directory Structure – A Cloud Engineer’s Guide**  
*A practical approach to mastering Linux directories with real-world examples and hands-on exercises.*

---

## **1. Root Directory (`/`)**  
### **What is the Root Directory?**  
- The **Root Directory (`/`)** is the **starting point of the entire Linux filesystem**.  
- It is similar to **C:\** in Windows, but in Linux, everything (files, directories, and mounted devices) is organized **under `/`**.  
- This is where **all other directories and files branch from**, making it **the backbone of the Linux system**.

### **Why does this matter for Cloud?**  
- **Docker containers** and **Kubernetes nodes** have a minimal filesystem starting from `/`.
- **Terraform**, when provisioning servers, often references paths like `/etc/` for configurations.
- **Ansible playbooks** frequently manipulate files under `/var/` or `/etc/`.

### **Try it yourself!** 🔥  
```bash
ls -l /
```
This command **lists all major directories** inside `/` and gives a high-level overview of the system.

---

## **2. System Binaries (`/bin`, `/sbin`, `/usr/bin`)**  
### **What are System Binaries?**  
System binaries are **essential programs that allow users and system administrators to interact with Linux**.

### **Why does this matter for Cloud?**  
- **Terraform, Ansible, Kubernetes binaries** are often stored in `/usr/bin/` or `/usr/local/bin/`.
- **CI/CD tools like Jenkins and GitLab CI/CD** execute binaries from these locations.
- Debugging a system? **Common commands like `ls`, `cp`, `rm`, `wget`, `systemctl`** live in these directories.

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

**Want to check where Terraform is installed?** Use:
```bash
which terraform
```
If installed, it should be in `/usr/local/bin/terraform`.

---

## **3. Configuration Files (`/etc`)**  
### **What are Configuration Files?**  
These files **store system-wide settings for networking, users, applications, and security policies**.

### **Why does this matter for Cloud?**  
- **Nginx, Apache, MySQL, PostgreSQL configuration files** are stored in `/etc/`.
- **Kubernetes kubeconfig file (`~/.kube/config`)** helps manage clusters.
- **Ansible inventories** often reference configuration files in `/etc/ansible/`.

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
cat /etc/hostname        # Show system hostname
cat /etc/os-release      # Display Linux version info
```

**Want to check the Nginx configuration?**
```bash
cat /etc/nginx/nginx.conf
```

---

## **4. Home & User Files (`/home`, `/root`)**  
### **What are Home & User Files?**  
These directories store **personal data, user preferences, and shell configurations**.

### **Why does this matter for Cloud?**  
- When **managing multiple users in Linux**, home directories help in organizing their files.
- CI/CD pipelines sometimes need access to user directories (e.g., **Jenkins home directory** `/var/lib/jenkins`).
- **SSH keys (`~/.ssh/authorized_keys`)** are stored inside user home directories.

### **Purpose of Home & User Files:**  
- **`/home/username/`** → Personal files for users.
- **`/root/`** → The root user’s home directory.

| **Directory** | **Purpose** | **Who Uses It?** | **Example Files** |
|-------------|------------|------------------|----------------------|
| `/home` | User home directories | Regular users | `~/.bashrc`, `~/.ssh/authorized_keys` |
| `/root` | Root user’s home directory | Root users | `/root/.bashrc`, `/root/.ssh/` |

### **Try it yourself!** 🔥  
```bash
ls -la /home
ls -la /root
```

---

## **5. System Logs (`/var/log`)**  
### **What are System Logs?**  
Logs track **system events, user activity, and application actions**.

### **Why does this matter for Cloud?**  
- If a **Docker container** crashes, check logs in `/var/log/`.
- **CI/CD tools (Jenkins, GitLab CI/CD)** generate logs stored in `/var/log/jenkins.log`.
- **Web servers (Nginx, Apache)** store access and error logs in `/var/log/apache2/`.

### **Purpose of System Logs:**
- **`/var/log`** → Stores system and application logs.
- **`/var/log/apache2/`** → Web server logs.
- **`/var/log/syslog`** → System logs.

| **Directory** | **Purpose** | **Who Uses It?** | **Example Files** |
|-------------|------------|------------------|----------------------|
| `/var/log` | Stores logs | Root users | `/var/log/syslog`, `/var/log/auth.log` |

### **Try it yourself!** 🔥  
```bash
tail -f /var/log/syslog      # View real-time system logs
tail -f /var/log/auth.log    # Track user authentication logs
```

---

## **6. Temporary & Mount Points (`/tmp`, `/mnt`, `/media`)**  
### **What are Temporary & Mount Points?**  
These directories manage **temporary data and mounted external storage**.

### **Why does this matter for Cloud?**  
- `/tmp` is often used for **temporary caching during Ansible playbook execution**.
- `/mnt` and `/media` help attach external storage (e.g., AWS EBS volumes).

### **Purpose of Temporary & Mount Points:**
- **`/tmp`** → Temporary files (cleared on reboot).
- **`/mnt`** → Temporary mount points.
- **`/media`** → Auto-mounted external devices.

| **Directory** | **Purpose** | **Who Uses It?** | **Example Files** |
|-------------|------------|------------------|----------------------|
| `/tmp` | Temporary files (cleared on reboot) | Root users | `/tmp/swapfile` |
| `/mnt` | Manual mount points | System admins | `/mnt/external_drive` |
| `/media` | Auto-mounted external devices | Regular users | `/media/usb_drive` |

### **Try it yourself!** 🔥  
```bash
df -h     # Check mounted filesystems
ls -l /tmp
```

---

## **7. Boot Files (`/boot`, `/efi`)**  
### **What are Boot Files?**  
These files are **essential for starting the Linux system**.

### **Why does this matter for Cloud?**  
- If a **Linux server fails to boot**, checking `/boot/` can help diagnose issues.
- **Cloud VM images** rely on `/boot/vmlinuz` for kernel loading.

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
journalctl -b | tail -20  # View boot logs
```
