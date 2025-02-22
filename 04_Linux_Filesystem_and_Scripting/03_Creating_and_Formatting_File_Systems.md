# **Creating and Formatting File Systems in Azure Virtual Machines**  

## **Why Should DevOps and Cloud Engineers Learn File System Management?**  

Imagine you are a **DevOps Engineer** managing a cloud-based application, and your logs are filling up the disk rapidly. You need to **attach a new disk, create a filesystem, and efficiently store logs** in a dedicated location.  

Or, as a **Cloud Engineer**, you need to **format and mount additional storage volumes** to store application data, ensuring optimal performance and persistence.  

In **Azure Virtual Machines**, disks are **not automatically formatted** when attached, so you need to **create and format file systems manually** before using them.

By the end of this lesson, you will be able to:  
✔ **Understand different types of file systems and their use cases.**  
✔ **Format and mount a new disk with a file system.**  
✔ **Ensure persistence after reboot using `/etc/fstab`.**  
✔ **Monitor disk usage and optimize storage.**  

Let’s start by understanding **what a file system is and why it matters.**  

---

## **1. What Is a File System?**  

A **file system** is the method by which **data is stored, organized, and retrieved** on a disk. It acts as an **indexing system**, ensuring files are efficiently placed and accessed.

### **Common File Systems in Linux**
| File System | Description | Best Use Case |
|-------------|------------|---------------|
| **ext4** | Default Linux file system with journaling support. | General-purpose workloads. |
| **xfs** | High-performance file system, optimized for large files. | High-speed databases & logs. |
| **btrfs** | Supports snapshots and compression. | Advanced storage needs. |
| **vfat/exFAT** | Compatible with Windows and macOS. | USB drives and external storage. |

Now, let’s move on to **creating and formatting a new file system in Azure VMs**.

---

## **2. Creating and Formatting a File System on an Azure VM**  

### **Step 1: List Available Disks**  

Before formatting a disk, check which disks are **available but unformatted**.

```bash
lsblk
```

✅ **Example Output:**
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  30G  0 disk /
sdb      8:16   0  50G  0 disk 
```
Here, **`/dev/sdb`** is an **unformatted disk**.

---

### **Step 2: Partition the New Disk Using `fdisk`**  
Launch `fdisk` to create a partition on the new disk:

```bash
sudo fdisk /dev/sdb
```

In the **interactive menu**, follow these steps:  
1. Press **`n`** → Create a **new partition**.  
2. Press **`p`** → Select **Primary partition**.  
3. Press **Enter** → Accept the **default start sector**.  
4. Press **Enter** → Accept the **default end sector** (entire disk).  
5. Press **`w`** → Write the partition table and exit.

✅ **Verification Step:**  
Check the partition table:

```bash
lsblk
```
✅ **Example Output:**
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sdb      8:16   0  50G  0 disk 
└─sdb1   8:17   0  50G  0 part  
```
Now, **`/dev/sdb1`** is ready to be formatted.

---

### **Step 3: Format the Partition**  

#### **Option 1: Format as EXT4** (Recommended for general workloads)
```bash
sudo mkfs.ext4 /dev/sdb1
```

#### **Option 2: Format as XFS** (Recommended for high-performance workloads)
```bash
sudo mkfs.xfs /dev/sdb1
```

✅ **Verification Step:**  
Check the file system type:

```bash
sudo blkid /dev/sdb1
```
✅ **Example Output:**
```
/dev/sdb1: UUID="e1f3a8c5-5b34-4a62-bde7-ef75b8a6f8dc" TYPE="ext4"
```
The partition is now **formatted and ready to be mounted**.

---

## **3. Mounting the New File System**  

### **Step 1: Create a Mount Directory**  
```bash
sudo mkdir -p /mnt/data
```

### **Step 2: Mount the Partition**  
```bash
sudo mount /dev/sdb1 /mnt/data
```

✅ **Verification Step:**  
Check if the partition is mounted:

```bash
df -h | grep /mnt/data
```
✅ **Example Output:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sdb1       50G   1M   50G   1% /mnt/data
```
Now, the partition **is mounted and ready to use**.

---

## **4. Ensuring Persistence After Reboot**  

By default, mounted disks are **not persistent** across reboots. To fix this, add an entry to `/etc/fstab`.

### **Step 1: Get the UUID of the Partition**  
```bash
blkid /dev/sdb1
```
✅ **Example Output:**
```
/dev/sdb1: UUID="e1f3a8c5-5b34-4a62-bde7-ef75b8a6f8dc" TYPE="ext4"
```

### **Step 2: Add Entry to `/etc/fstab`**  
```bash
echo 'UUID=e1f3a8c5-5b34-4a62-bde7-ef75b8a6f8dc /mnt/data ext4 defaults,nofail 0 2' | sudo tee -a /etc/fstab
```

### **Step 3: Test the Configuration**  
```bash
sudo mount -a
```
If there are no errors, the mount will persist after reboot.

✅ **Verification Step:**  
Reboot the VM and run:
```bash
df -h | grep /mnt/data
```
The disk should remain mounted.

---

## **5. Monitoring Disk Usage**  

To ensure **efficient storage management**, regularly monitor disk usage.

### **5.1 Check Disk Space Usage (`df`)**  
```bash
df -h
```
✅ **Example Output:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1       30G   12G   18G  40% /
/dev/sdb1       50G   2G    48G   5% /mnt/data
```

### **5.2 Check Directory Size (`du`)**  
Find the **largest directories**:
```bash
du -ah /mnt/data | sort -rh | head -10
```

### **5.3 Check Inode Usage (`df -i`)**  
```bash
df -i
```
This helps to prevent **"No space left on device" errors** caused by inode exhaustion.

---

## **6. Expanding a Disk Without Data Loss**  

If you need **more space**, expand the Azure disk and resize the file system **without losing data**.

### **Step 1: Expand the Disk in Azure Portal**  
1. Go to **Azure Portal** → **Virtual Machines**.  
2. Select your **VM** → Click **Disks**.  
3. Click on the **attached data disk**.  
4. Increase the **disk size** (e.g., 50GB → 100GB).  
5. Click **Save**.

### **Step 2: Rescan the Disk**  
```bash
echo 1 | sudo tee /sys/class/block/sdb/device/rescan
```

### **Step 3: Resize the Partition**  
```bash
sudo growpart /dev/sdb 1
```

### **Step 4: Resize the File System**  

For **EXT4**:
```bash
sudo resize2fs /dev/sdb1
```

For **XFS**:
```bash
sudo xfs_growfs /mnt/data
```

✅ **Verification Step:**  
```bash
df -h
```
The **disk size should now reflect the new value**.

---

## **Key Takeaways**  

- **File systems** organize and store data efficiently.  
- **EXT4** is a general-purpose file system; **XFS** is optimized for performance.  
- **Formatting is required** before using a new disk in Azure.  
- **Mounting a disk is temporary** unless added to `/etc/fstab`.  
- **Monitoring tools** like `df`, `du`, and `df -i` help manage disk usage.  
- **Azure disks can be expanded** without data loss using `growpart` and `resize2fs`.  

---

## **What’s Next?**  

Now that we understand **file system management**, the next step is:  

- **Automating disk management with Bash scripting.**  
- **Using cron jobs to monitor and clean up disk space.**  
- **Configuring advanced storage solutions in cloud environments.**  

Let’s move forward and **learn Bash Scripting for automation!**
