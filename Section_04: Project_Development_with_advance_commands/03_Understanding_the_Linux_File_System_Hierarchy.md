# Study Notes: Understanding the Linux File System Hierarchy

## What is the Linux File System Hierarchy?
The **Linux file system hierarchy** is a structured way to organize files and directories within a Linux operating system. It ensures a consistent approach to storing, accessing, and managing different types of files.

The **root directory (`/`)** is the top-level directory in Linux. All other directories, files, and devices are stored under it.

---

## Key Directories in the Linux File System
Each directory in the Linux file system serves a specific purpose:

### `/` (Root Directory)
- The **starting point** of the Linux file system.
- All other directories and files exist within this directory.

### `/bin` (Binaries)
- Contains **essential executable files** required by users and the system.
- Commands like `ls`, `cp`, `mkdir`, etc., are stored here.

### `/boot` (Boot Files)
- Stores **files needed for system startup**, including the Linux kernel and bootloader.

### `/dev` (Devices)
- Contains **device files** that represent hardware components like hard drives, USB devices, and printers.

### `/etc` (Configuration Files)
- Stores **system-wide configuration files**.
- Includes settings for networking, authentication, and startup scripts.

### `/home` (User Directories)
- Each user has a **home directory** under `/home`, providing private storage for personal files and configurations.
- Example: `/home/ec2-user` is the home directory for the `ec2-user`.

### `/lib` (Libraries)
- Contains **shared libraries** used by programs in `/bin` and `/sbin`.

### `/media` (Removable Media)
- Used as a **mount point** for removable storage devices like USB drives and CDs.

### `/mnt` (Temporary Mounts)
- Used for manually mounting external file systems and network shares.

### `/opt` (Optional Software)
- Used for **third-party applications** that don’t follow the default Linux file system hierarchy.

### `/proc` (Process Information)
- A **virtual file system** that provides real-time information about running processes and system resources.

### `/root` (Root User Home Directory)
- The **home directory for the root user**, who has administrative access.

### `/sbin` (System Binaries)
- Contains **essential system administration commands**.
- Used for managing network settings and system backups.

### `/tmp` (Temporary Files)
- Stores **temporary data** used by applications.
- Data in this directory is usually cleared upon system reboot.

### `/usr` (User Programs)
- Contains **user-related programs and files**.
- Includes subdirectories like `/usr/bin`, `/usr/sbin`, `/usr/lib`, and `/usr/share`.

### `/var` (Variable Data)
- Stores **files that frequently change**, such as log files, email data, and cached files.

---

## Locating Nginx HTML Files in the File System
The default location for **HTML files** served by the **Nginx web server** is:
```bash
/var/www/
```
However, in **Amazon Linux 2**, the default Nginx HTML directory is:
```bash
/usr/share/nginx/html
```

To navigate to this directory, use:
```bash
cd /usr/share/nginx/html
ls
```
This command lists the contents of the HTML directory, which includes the default Nginx welcome page (`index.html`).

---

## Viewing File Contents with `cat`
The **`cat` (Concatenate) command** is used to display the contents of files directly in the terminal.

#### Syntax:
```bash
cat <filename>
```

#### Example:
```bash
cat index.html
```
This command displays the **source code** of the Nginx default welcome page.

### Additional `cat` Options:
| Option | Description |
|--------|-------------|
| `-n`   | Displays line numbers. |
| `-b`   | Displays line numbers only for non-empty lines. |
| `-s`   | Suppresses multiple empty lines. |
| `-E`   | Displays a `$` at the end of each line. |
| `-T`   | Displays tabs as `^I`. |

Example to display line numbers:
```bash
cat -n index.html
```

To view multiple files at once:
```bash
cat index.html 404.html
```
This command displays the contents of both `index.html` and `404.html`.

---

## Summary
- The **Linux file system hierarchy** organizes files and directories efficiently.
- Each directory has a **specific purpose** (binaries, logs, configurations, user files, etc.).
- The **default Nginx HTML directory** is `/usr/share/nginx/html`.
- The **`cat` command** is used to display the contents of files and supports additional options.

Now that we understand the **file system hierarchy**, we are ready to **move our website files to the correct directory and configure Nginx**.


