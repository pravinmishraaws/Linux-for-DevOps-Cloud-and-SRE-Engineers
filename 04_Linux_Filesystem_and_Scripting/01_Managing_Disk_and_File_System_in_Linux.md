# **Managing Disk and Filesystem in Linux (Azure Edition)**  

## **Why Should DevOps and Cloud Engineers Learn Disk and Filesystem Management?**  

Imagine you are a **Cloud Engineer managing an Azure Virtual Machine**. Your team notifies you that **storage is running low**, and they need **additional disk space** to store logs and database files.  

As a **DevOps Engineer**, you might be working on **The EpicBook!**, a high-traffic online bookstore. The **database is growing rapidly**, and you need to **attach and configure additional storage** for better performance.  

To solve these problems, you need to:  
- **Attach a new Azure Managed Disk** to your VM.  
- **Format and mount the disk** so it becomes usable.  
- **Choose the right file system** based on workload requirements.  

By the end of this lesson, you will be able to:  
✔ Understand what a **disk** is and how storage works in Linux.  
✔ Learn about **file systems** and their role in organizing data.  
✔ Identify **common file systems** and their use cases.  
✔ **Attach, format, and mount a new Azure disk** in a VM.  

Let’s start with the basics.  

---

## **1. What is a Disk?**  

A **disk** is a storage device that holds data in blocks. In **Azure Virtual Machines**, storage is managed as:  

- **OS Disk** – The primary disk where the operating system is installed.  
- **Data Disks** – Additional disks used for application storage.  
- **Temporary Disk** – A non-persistent disk for swap space and temporary files.  

Each disk appears as a **device file** in Linux (`/dev/sdX` or `/dev/nvmeX`).  

---

### **Example: Attaching a New Disk to an Azure VM**  

When you add a **new Managed Disk** to an **Azure VM**, it does not automatically appear as a usable drive. You must:  
1. **Identify the new disk** using `lsblk`.  
2. **Create a file system** (`ext4`, `xfs`, or `btrfs`).  
3. **Mount the disk** to a directory (`/mnt/data`).  
4. **Ensure persistence** after a reboot.  

Now that we understand disks, let’s move on to **file systems**.

---

## **2. What is a Filesystem?**  

A **filesystem** is the way data is stored, organized, and retrieved on a disk. It provides:  
✔ **Structure** – Organizes data into files and directories.  
✔ **Permissions** – Controls who can access files.  
✔ **Metadata** – Stores information like timestamps and ownership.  

### **Why is a Filesystem Important?**  

- **Without a filesystem**, a disk is just **raw storage**.  
- The OS needs a filesystem to **store and access data efficiently**.  

---

## **3. Common File Systems in Linux (Used in Azure)**  

Different file systems have different use cases in **cloud environments**.

| Filesystem | Best Use Case |
|------------|-------------|
| **ext4** | Default for most Linux distributions, general-purpose use. |
| **XFS** | Optimized for **high-performance workloads** (databases, logs). |
| **Btrfs** | Supports **snapshots, compression, and advanced storage features**. |
| **NTFS** | Used when mounting Windows partitions. |

Azure **defaults to XFS** for high-performance workloads, but Btrfs is useful for **snapshot-based backups**.  

---

### **ext4 – The Standard Linux Filesystem**
- Used in **Ubuntu and Debian** as the default file system.  
- Works well for **general-purpose workloads**.  

#### **Format a disk with ext4**
```bash
sudo mkfs.ext4 /dev/sdc
```

---

### **XFS – Best for High-Performance Storage**
- **Recommended by Azure** for databases, logs, and high-write workloads.  
- Handles **large files efficiently** and scales well.  

#### **Format a disk with XFS**
```bash
sudo mkfs.xfs /dev/sdc
```

---

### **Btrfs – Best for Snapshots and Compression**
- Supports **snapshots, data integrity checks, and compression**.  
- Ideal for **backup systems, versioned data, and large-scale storage**.  

#### **Format a disk with Btrfs**
```bash
sudo mkfs.btrfs /dev/sdc
```

---

### **Choosing the Right File System in Azure**

| Use Case | Recommended File System |
|----------|------------------------|
| General-purpose data storage | ext4 |
| High-performance applications (databases, logs) | XFS |
| Snapshots, backups, and compression | Btrfs |
| Windows compatibility | NTFS |

---

## **Hands-On: Attaching and Formatting a Disk in Azure**  

### **Step 1: Attach a New Disk to Your Azure VM**  

Go to **Azure Portal → Virtual Machines → Your VM → Disks → Add Data Disk**.  

- **Choose size:** 20GB  
- **Host caching:** None  
- **Save and apply changes**  

---

### **Step 2: Verify the New Disk in Linux**  

SSH into your Azure VM:  
```bash
ssh azureuser@your-vm-ip
```
List all attached disks:  
```bash
lsblk
```
You should see a **new disk** (e.g., `/dev/sdc`).  

---

### **Step 3: Create a File System on the Disk**  

If the disk is **unformatted**, create a new **ext4**, **XFS**, or **Btrfs** file system:  

For **ext4**:  
```bash
sudo mkfs.ext4 /dev/sdc
```

For **XFS**:  
```bash
sudo mkfs.xfs /dev/sdc
```

For **Btrfs**:  
```bash
sudo mkfs.btrfs /dev/sdc
```

---

### **Step 4: Mount the Disk**  

Create a mount point:  
```bash
sudo mkdir -p /mnt/data
```
Mount the disk:  
```bash
sudo mount /dev/sdc /mnt/data
```
Check if it’s mounted:  
```bash
df -h
```

---

### **Step 5: Persist the Mount After Reboot**  

Edit `/etc/fstab` to ensure the disk mounts automatically:  
```bash
echo "/dev/sdc /mnt/data xfs defaults 0 0" | sudo tee -a /etc/fstab
```

---

## **Key Takeaways**  

- Azure VMs use **Managed Disks** that must be formatted before use.  
- The **ext4** filesystem is the default in Linux, while **XFS** is better for high-performance workloads.  
- **Btrfs** is useful for snapshots, compression, and backup systems.  
- After attaching a new disk, it must be **formatted and mounted** to be usable.  
- Adding the disk to **`/etc/fstab`** ensures it **persists after reboot**.  

---

## **What’s Next?**  

Now that you’ve **attached, formatted, and mounted a new disk**, the next step is:  

- **Listing and identifying partitions** on a Linux system.  
- **Using tools like `lsblk`, `fdisk`, and `df`** to analyze disk space.  
- **Understanding how Linux organizes storage devices**.  

Let’s move forward and **list disks and partitions in Linux**.  
