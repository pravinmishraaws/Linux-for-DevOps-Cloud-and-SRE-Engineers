# **Managing Disks and Partitions in an Azure Virtual Machine**

## **Why Do DevOps and Cloud Engineers Need to Manage Disks in Azure?**

Imagine you are managing an **Azure VM** hosting a **database** for a high-traffic e-commerce platform. As the data grows, you realize the existing disk is running **out of space**.  

Or, you are setting up a **new Azure VM** and need to attach additional disks to separate logs, applications, and databases for better performance and reliability.  

This is where **disk management in Azure** becomes crucial.

By the end of this guide, you will be able to:  
✔ **List all attached disks and partitions in an Azure VM.**  
✔ **Attach a new disk to an Azure VM and configure it.**  
✔ **Partition, format, and mount the new disk.**  
✔ **Ensure the disk is mounted persistently across reboots.**

---

## **Step 1: List Disks and Partitions in Azure VM**  

After provisioning an **Azure VM**, the first step is to check the **existing storage configuration**.  

### **1.1 List Available Disks in the VM**
Use `lsblk` to view all attached block devices.  

```bash
lsblk
```
✅ **Example Output:**
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  30G  0 disk 
├─sda1   8:1    0  29G  0 part /
└─sda2   8:2    0   1G  0 part [SWAP]
```
**Explanation:**
- `sda` → The **main OS disk** (30GB).
- `sda1` → The **root partition (`/`)**.
- `sda2` → The **swap partition**.

---

### **1.2 View Partition Details Using `fdisk`**
To check detailed partition information, use:

```bash
sudo fdisk -l
```
✅ **Example Output:**
```
Disk /dev/sda: 30 GiB, 32212254720 bytes
Device     Boot Start      End  Sectors  Size Id Type
/dev/sda1  *     2048  62914559  62912512  29G 83 Linux
/dev/sda2       62914560  62926847     12288  1G 82 Linux swap
```
**Explanation:**
- `/dev/sda1` → The **root file system** (`/`).
- `/dev/sda2` → The **swap partition**.

---

### **1.3 Display Mounted File Systems Using `df`**
To see **mounted partitions and disk usage**, run:

```bash
df -h
```
✅ **Example Output:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1       29G   10G   19G  34% /
```
This confirms that **only the primary disk is mounted**.

---

## **Step 2: Attach a New Disk in Azure Portal**

To **add additional storage** to your **Azure VM**, follow these steps:

1. **Go to Azure Portal** → Navigate to **Virtual Machines**.
2. **Select Your VM** → Click on **Disks**.
3. **Click ‘Add Data Disk’** → Choose **Create a new disk**.
4. **Set Disk Size (e.g., 50GB)** → Select **Premium SSD** or **Standard HDD**.
5. **Click Save** → The new disk will be attached as **/dev/sdb**.

✅ **Verification Step:**  
After adding the disk, log into the **Azure VM** via SSH and run:

```bash
lsblk
```
✅ **Example Output:**
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  30G  0 disk 
├─sda1   8:1    0  29G  0 part /
└─sda2   8:2    0   1G  0 part [SWAP]
sdb      8:16   0  50G  0 disk  # New attached disk
```
This confirms that **`/dev/sdb` (50GB)** is successfully attached but **not yet partitioned or formatted**.

---

## **Step 3: Partition and Format the New Disk**

Now, let’s partition and format the new disk `/dev/sdb`.

### **3.1 Create a Partition Using `fdisk`**
Launch `fdisk` for the new disk:

```bash
sudo fdisk /dev/sdb
```
**Follow These Steps in the Interactive Menu:**
1. Press `n` → Create a new partition.
2. Press `p` → Select Primary partition.
3. Press `Enter` → Use the default partition number.
4. Press `Enter` → Use the default start sector.
5. Press `Enter` → Use the default end sector (entire disk).
6. Press `w` → Write changes to disk.

✅ **Verification Step:**  
Check the partition table:

```bash
sudo fdisk -l /dev/sdb
```
✅ **Example Output:**
```
Disk /dev/sdb: 50 GiB, 53687091200 bytes
Device     Boot Start      End  Sectors  Size Id Type
/dev/sdb1        2048 104857599 104855552  50G 83 Linux
```
Now the partition **`/dev/sdb1` (50GB)** is ready.

---

### **3.2 Format the Partition as EXT4**
To use the new partition, format it:

```bash
sudo mkfs.ext4 /dev/sdb1
```
✅ **Example Output:**
```
Creating filesystem with 13107200 4k blocks and 3276800 inodes
Filesystem UUID: e1f3a8c5-5b34-4a62-bde7-ef75b8a6f8dc
```
Now, the partition is **formatted and ready to be mounted**.

---

## **Step 4: Mount the New Partition**  

### **4.1 Create a Mount Directory**
```bash
sudo mkdir -p /mnt/data
```

### **4.2 Mount the New Partition**
```bash
sudo mount /dev/sdb1 /mnt/data
```
✅ **Verification Step:**  
Run `df -h` to confirm:

```bash
df -h | grep /mnt/data
```
✅ **Example Output:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sdb1       50G   1M   50G   1% /mnt/data
```
The new partition is **successfully mounted**.

---

## **Step 5: Ensure Persistent Mounting After Reboot**  

If the VM is rebooted, the partition will **not mount automatically** unless we add it to `/etc/fstab`.

### **5.1 Get the UUID of the Partition**
```bash
blkid /dev/sdb1
```
✅ **Example Output:**
```
/dev/sdb1: UUID="e1f3a8c5-5b34-4a62-bde7-ef75b8a6f8dc" TYPE="ext4"
```
### **5.2 Add Entry to `/etc/fstab`**
```bash
echo 'UUID=e1f3a8c5-5b34-4a62-bde7-ef75b8a6f8dc /mnt/data ext4 defaults,nofail 0 2' | sudo tee -a /etc/fstab
```

✅ **Verification Step:**  
Run:
```bash
sudo mount -a
```
If there are no errors, the partition will **automatically mount after reboot**.

---

## **Key Takeaways**

- `lsblk` lists all block devices.  
- `fdisk -l` shows detailed partition info.  
- `parted` works with GPT partitions.  
- `blkid` retrieves the UUID of a partition.  
- `df -h` displays mounted partitions.  
- `mkfs.ext4` formats the partition.  
- `mount` attaches a partition to a directory.  
- `/etc/fstab` ensures persistence after reboot.  

---

## **What’s Next?**

Now that you have attached, formatted, and mounted a new disk in **Azure VM**, the next step is:

- **Creating file systems (ext4, xfs, etc.).**  
- **Monitoring disk usage with `du` and `df` commands.**  
- **Expanding disk size without losing data.**  

Let’s move forward to **Creating and Formatting File Systems in Azure Linux VMs**.
