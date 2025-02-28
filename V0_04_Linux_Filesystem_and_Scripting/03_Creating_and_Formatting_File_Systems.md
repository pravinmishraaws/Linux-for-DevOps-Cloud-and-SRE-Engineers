# **Creating and Formatting File Systems in Azure Virtual Machines**  

## **Why Should DevOps and Cloud Engineers Learn File System Management?**  

In the **previous lab**, we attached a **20GB disk (`/dev/sdb`)** to our **Azure Virtual Machine** and verified that it was recognized by the system. However, the disk was **not yet usable** because:  
- It had **no partitions**  
- It was **not formatted** with a file system  
- It was **not mounted** for use  

As a **DevOps Engineer**, you need to ensure that attached disks are properly **partitioned, formatted, and mounted** before they can store data.  

By the end of this lesson, you will be able to:  
- Create and format a partition on an Azure VM disk  
- Mount and persistently attach the new disk  
- Monitor disk usage and manage storage efficiently  

---

## **1. Understanding File Systems**  

A **file system** is responsible for organizing and storing data on a disk. Without a file system, the OS cannot read or write data.  

### **Common File Systems in Linux**
| File System | Description | Best Use Case |
|-------------|------------|---------------|
| ext4 | Default Linux file system with journaling support | General workloads |
| xfs | High-performance file system, optimized for large files | Databases and logs |
| btrfs | Supports snapshots and compression | Advanced storage needs |
| vfat/exFAT | Compatible with Windows and macOS | USB drives and external storage |

---

## **2. Partitioning the Disk on Azure VM**  

In the **previous lesson**, we verified that `/dev/sdb` (20GB) is attached but has no partitions.

### **Step 1: Create a Partition Using `fdisk`**  
Launch `fdisk` for the new disk:  

```bash
sudo fdisk /dev/sdb
```
Follow these steps in the interactive prompt:  
1. Press `n` → Create a new partition  
2. Press `p` → Select Primary partition  
3. Press `Enter` → Accept the default partition number  
4. Press `Enter` → Accept the default start sector  
5. Press `Enter` → Accept the default end sector (entire disk)  
6. Press `w` → Write changes and exit  

### **Verification Step**  
Check if the partition is created:  
```bash
lsblk
```
Expected Output:  
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sdb      8:16   0  20G  0 disk  
└─sdb1   8:17   0  20G  0 part  
```
Now, `/dev/sdb1` is ready for formatting.

---

## **3. Formatting the Partition**  

Now that we have a partition, we need to format it before use.

### **Step 1: Choose the File System**  
- Use EXT4 for general workloads:  
  ```bash
  sudo mkfs.ext4 /dev/sdb1
  ```
- Use XFS for high-performance workloads:  
  ```bash
  sudo mkfs.xfs /dev/sdb1
  ```

### **Verification Step**  
Check the file system type:  
```bash
sudo blkid /dev/sdb1
```
Expected Output:  
```
/dev/sdb1: UUID="e1f3a8c5-5b34-4a62-bde7-ef75b8a6f8dc" TYPE="ext4"
```
The partition is now formatted and ready for mounting.

---

## **4. Mounting the File System**  

### **Step 1: Create a Mount Directory**  
```bash
sudo mkdir -p /mnt/data
```

### **Step 2: Mount the Partition**  
```bash
sudo mount /dev/sdb1 /mnt/data
```

### **Verification Step**  
Check if the partition is mounted:  
```bash
df -h | grep /mnt/data
```
Expected Output:  
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sdb1       20G   1M   20G   1% /mnt/data
```
The partition is now mounted and ready to use.

---

## **5. Ensuring Persistent Mounting After Reboot**  

By default, the mount will not persist after a reboot. To make it permanent, we add it to `/etc/fstab`.

### **Step 1: Get the UUID of the Partition**  
```bash
blkid /dev/sdb1
```
Expected Output:  
```
/dev/sdb1: UUID="e1f3a8c5-5b34-4a62-bde7-ef75b8a6f8dc" TYPE="ext4"
```

### **Step 2: Add the Entry to `/etc/fstab`**  
```bash
echo 'UUID=e1f3a8c5-5b34-4a62-bde7-ef75b8a6f8dc /mnt/data ext4 defaults,nofail 0 2' | sudo tee -a /etc/fstab
```

### **Step 3: Test the Configuration**  
```bash
sudo mount -a
```
If there are no errors, the mount is now persistent.

### **Final Verification**  
Reboot the system and run:  
```bash
df -h | grep /mnt/data
```
If the disk is still mounted, everything is configured correctly.

---

## **6. Monitoring Disk Usage**  

To prevent running out of space, regularly monitor disk usage.

### **Check Disk Space Usage (`df`)**  
```bash
df -h
```
### **Check Directory Size (`du`)**  
Find the largest directories:  
```bash
du -ah /mnt/data | sort -rh | head -10
```
### **Check Inode Usage (`df -i`)**  
```bash
df -i
```

---

## **7. Expanding a Disk Without Data Loss**  

If more space is needed, you can expand the Azure disk and resize the file system.

### **Step 1: Expand the Disk in Azure Portal**  
1. Go to Azure Portal → Virtual Machines  
2. Select your VM → Click Disks  
3. Click on the attached data disk  
4. Increase the disk size (e.g., 20GB → 50GB)  
5. Click Save  

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

### **Verification Step**  
```bash
df -h
```
The disk size should now reflect the new value.

---

## **Key Takeaways**  

- A partitioned and formatted file system is required for storing data.  
- EXT4 is a reliable choice for most workloads, while XFS is optimized for performance.  
- Mounting a disk manually does not persist after reboot unless configured in `/etc/fstab`.  
- Monitoring tools like `df`, `du`, and `df -i` help track disk usage.  
- Azure disks can be expanded dynamically using `growpart` and `resize2fs`.  

---

## **What’s Next?**  

Now that we understand file system management, the next step is:  

- Automating disk management using Bash scripting  
- Using cron jobs to monitor and clean up disk space  
- Configuring advanced storage solutions in cloud environments  

Let’s move forward to **Bash Scripting for Automation**.  
