# **Listing Disks and Partitions in an Azure Virtual Machine**  

## **Why Do DevOps and Cloud Engineers Need to Manage Disks?**  

In the **previous lesson**, we attached a **20GB disk (`/dev/sdb`)** to an **Azure Virtual Machine**. But before we can use it, we need to verify:  
- **Is the system recognizing the disk?**  
- **Does the disk already have partitions?**  
- **What file system (if any) is present on the disk?**  

As a **DevOps Engineer**, it’s crucial to inspect storage before making changes to **avoid accidental data loss**.  

By the end of this lesson, you will be able to:  
✔ **List all attached disks and partitions in an Azure VM.**  
✔ **Check if a disk has existing partitions before formatting.**  
✔ **Retrieve information about storage devices using Linux tools.**  
✔ **Prepare a disk for partitioning and formatting if needed.**  

---

## **Step 1: List Available Disks in the Azure VM**  

After logging into your Azure VM via SSH, check the currently attached storage devices.  

### **1.1 Use `lsblk` to View Block Devices**  

```bash
lsblk
```
✅ **Expected Output:**
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  30G  0 disk 
├─sda1   8:1    0  29G  0 part /
└─sda2   8:2    0   1G  0 part [SWAP]
sdb      8:16   0  20G  0 disk
```
### **Understanding the Output:**
- `sda` → The **main OS disk** (30GB).  
- `sda1` → The **root partition (`/`)**.  
- `sda2` → The **swap partition**.  
- `sdb` → The **20GB disk** we attached in the last lab (**unpartitioned**).  

---

## **Step 2: Check Partition Details of the New Disk (`/dev/sdb`)**  

Before formatting a disk, we need to check if it has **existing partitions**.

### **2.1 Use `fdisk -l` to List Partition Information**  

```bash
sudo fdisk -l
```
✅ **Expected Output:**
```
Disk /dev/sda: 30 GiB, 32212254720 bytes, 62914560 sectors
Device     Boot Start      End  Sectors  Size Id Type
/dev/sda1  *     2048  62914559  62912512  29G 83 Linux
/dev/sda2       62914560 62926847  12288  1G 82 Linux swap

Disk /dev/sdb: 20 GiB, 21474836480 bytes, 41943040 sectors
```
### **What This Tells Us:**  
- `/dev/sda` has **two partitions (`sda1` and `sda2`)**, which are being used.  
- `/dev/sdb` **has no partitions yet** (it’s raw storage).  

✅ **Since `/dev/sdb` has no partitions, we can now prepare it for use.**  

---

## **Step 3: Display Partition Information Using Other Tools**  

Different tools provide additional insights about disks and partitions.  

### **3.1 Check Partitions with `parted`**
```bash
sudo parted -l
```
✅ **Expected Output:**
```
Model: Virtio Block Device (virtblk)
Disk /dev/sda: 30GB
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system     Name  Flags
 1      1049kB  30GB    30GB    ext4

Disk /dev/sdb: 20GB
Partition Table: unknown
```
🚨 **Notice that `/dev/sdb` has no partition table yet.**  

---

### **3.2 Check Filesystem Information Using `blkid`**  
```bash
blkid
```
✅ **Expected Output:**  
```
/dev/sda1: UUID="e1a3f8c5-5b34-4a62-bde7-ef75b8a6f8dc" TYPE="ext4"
/dev/sda2: UUID="4f3c8f65-2f44-4d84-a5a8-2dcfbc5e52c3" TYPE="swap"
```
🚨 **Since `/dev/sdb` is missing from this output, it means it has no filesystem yet.**  

---

### **3.3 Check Disk Usage with `df -h`**
```bash
df -h
```
✅ **Expected Output:**  
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1       29G   10G   19G  34% /
```
🚨 **Since `/dev/sdb` is not listed here, it is not yet mounted.**  

---

## **Step 4: Summary & Next Steps**  

✅ **We confirmed that `/dev/sdb` (20GB) is attached to the Azure VM.**  
✅ **It has no partitions, no file system, and is not mounted.**  
✅ **The disk is ready to be partitioned and formatted.**  

---

## **Key Takeaways**  

- `lsblk` **lists all block devices** attached to the system.  
- `fdisk -l` **shows partition details** for each disk.  
- `parted -l` **displays partition tables** and filesystem types.  
- `blkid` **retrieves filesystem UUIDs and types**.  
- `df -h` **displays mounted filesystems and disk usage**.  

---

## **What’s Next?**  

Now that we have identified and verified the new disk, the next step is:  

- **Partitioning and formatting the disk.**  
- **Creating an `ext4` or `xfs` filesystem.**  
- **Mounting the partition and ensuring it persists after reboot.**  

Let’s move to the next lesson: **Creating and Formatting File Systems in an Azure VM**.  
