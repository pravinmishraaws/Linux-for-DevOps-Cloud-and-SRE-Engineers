# **Managing Disk and Filesystem in Linux**  

## **Why Should DevOps and Cloud Engineers Learn Disk and Filesystem Management?**  

Imagine you are a **DevOps or Cloud Engineer** setting up an **AWS EC2 instance** or a **Google Cloud VM** for a database server. Your application team reports that **disk space is running out**, and you need to **add a new volume** to the server.  

To resolve this, you must:  
- **Attach a new disk** to the server.  
- **Format and mount the disk** so the system can use it.  
- **Choose the right file system** to optimize performance and reliability.  

Understanding disk and filesystem management is essential to:  
- Ensure efficient storage utilization.  
- Format and mount new disks in cloud environments.  
- Choose the right file system for performance and stability.  

By the end of this lesson, you will be able to:  
- Understand what a disk is and how storage works in Linux.  
- Learn about file systems and their role in organizing data.  
- Identify common file systems and their use cases.  

Let’s begin by understanding what a disk is.

---

## **1. What is a Disk?**  

A disk is a physical or virtual storage device that stores data in blocks. In a Linux system, disks can be:  

- **Physical disks** (HDDs, SSDs, USB drives).  
- **Virtual disks** (EBS volumes in AWS, Persistent Disks in Google Cloud).  
- **Network storage** (NFS, EFS, or SAN drives).  

Each disk is divided into partitions, and each partition is formatted with a file system.

### **Example: Adding a New Disk in a Cloud Server**  

If you create an **EC2 instance** on AWS, the root disk may be `/dev/xvda`, and additional attached volumes might be `/dev/xvdb` or `/dev/nvme1n1`. These disks must be formatted and mounted before they can be used by the operating system.  

Now that we understand disks, let’s move on to file systems.

---

## **2. What is a Filesystem?**  

A filesystem is a way to store, organize, and retrieve files on a disk. It manages:  

- How data is stored and accessed.  
- Permissions and metadata (ownership, timestamps).  
- Data integrity and performance.  

### **Why is a Filesystem Important?**  

- Without a file system, a disk is just raw storage.  
- A file system structures the disk so that the operating system can read and write data efficiently.  

### **Example: Formatting a New Disk for Application Data**  

If you add a 100GB EBS volume to an AWS EC2 instance, it appears as unformatted storage (`/dev/xvdb`). Before using it, you must:  

1. Create a file system (e.g., `ext4` or `xfs`).  
2. Mount the disk to a directory (`/mnt/data`).  
3. Ensure it persists after a reboot.  

Now, let’s explore common file systems in Linux.

---

## **3. Common File Systems in Linux**  

Different file systems are optimized for different workloads.

| Filesystem | Use Case | Best For |
|------------|---------|----------|
| ext4 | Default for most Linux distributions | General-purpose use |
| xfs | High-performance journaling file system | Large-scale storage, databases |
| btrfs | Advanced features like snapshots and RAID | Data integrity, backups |
| NTFS | Windows file system | Dual-boot with Windows |
| vfat / exFAT | Universal compatibility | USB drives, external storage |
| NFS | Networked file system | Shared storage across multiple servers |

Let’s go over when to use each file system.

---

### **ext4 – The Standard Filesystem**
- Most Linux distributions use `ext4` as the default.  
- Reliable and good for general workloads.  

#### **Format a disk with ext4**
```bash
sudo mkfs.ext4 /dev/xvdb
```

---

### **XFS – Best for Large-Scale Storage**
- Used in high-performance environments (e.g., AWS default for RHEL).  
- Handles large files and fast writes well.  

#### **Format a disk with XFS**
```bash
sudo mkfs.xfs /dev/xvdb
```

---

### **btrfs – Best for Snapshots and RAID**
- Supports snapshots and built-in RAID.  
- Used in backup systems and advanced storage needs.  

#### **Format a disk with btrfs**
```bash
sudo mkfs.btrfs /dev/xvdb
```

---

### **NFS – Networked File System**
- Allows multiple servers to share storage.  
- Used in cloud environments (AWS EFS, Google Filestore).

#### **Mount an NFS Share**
```bash
sudo mount -t nfs 192.168.1.10:/data /mnt/nfs
```

---

### **Choosing the Right File System for Different Use Cases**

| Use Case | Recommended File System |
|----------|------------------------|
| General use (home, office) | ext4 |
| High-performance databases | XFS |
| File versioning & snapshots | btrfs |
| Dual-boot with Windows | NTFS |
| Cloud-based shared storage | NFS |

---

## **Hands-On Challenge**  

1. Check the file system of your root partition.  
```bash
df -T /
```

2. Format a new disk with `ext4` and mount it.  
```bash
sudo mkfs.ext4 /dev/xvdb
sudo mount /dev/xvdb /mnt/data
```

3. Determine which file system is best for an application server.  
```bash
df -Th
```

---

## **Key Takeaways**  

- A disk is a storage device that holds partitions and file systems.  
- A file system organizes and structures data on a disk.  
- ext4 is the most commonly used file system in Linux.  
- XFS is optimized for high-performance workloads (like databases).  
- NFS allows multiple servers to share a single storage system.  

---

## **What’s Next?**  

Now that you understand disks and file systems, the next step is:  

- Listing and identifying disks in a Linux system.  
- Creating, formatting, and mounting partitions.  
- Managing disk storage efficiently in cloud environments.  

Let’s move forward and explore how to list disks and partitions in Linux.
