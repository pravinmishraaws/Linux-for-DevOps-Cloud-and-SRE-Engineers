# **Working with Compression and Archive Files in Linux**

## **Introduction**
In Linux, **compression** and **archiving** are essential for reducing file sizes and organizing multiple files into a single package. This improves storage efficiency, speeds up file transfers, and simplifies backups.

In this lesson, we will cover:
- **Unzipping files** with `unzip`
- **Compressing files** with `zip`
- **Archiving files** with `tar`
- **Using GZip for compression**
- **Comparing file sizes before and after compression**
- **Extracting compressed archives**

By the end, you will understand how to efficiently manage compressed and archived files.

---

## **1. Understanding Compression and Archiving**
Before diving into commands, let’s clarify **compression** and **archiving**:

| **Process**     | **Description** |
|---------------|----------------|
| **Compression** | Reduces file size to save space and speed up transfers. |
| **Archiving**   | Combines multiple files into one, making them easier to manage. |

### **Benefits in a Static Website Project**
- **Faster website loading:** Compressed files reduce bandwidth usage.
- **Efficient backups:** Archiving helps store multiple website files in one package.
- **Easier transfers:** Sending a single archive is more convenient than multiple files.

---

## **2. Extracting Files Using `unzip`**
The `unzip` command extracts files from a `.zip` archive.

### **Syntax:**
```bash
unzip filename.zip
```

### **Example: Extracting a Website Template**
```bash
unzip website_template.zip
```
This extracts all files into the current directory.

### **Common Options:**
| Option | Description |
|--------|-------------|
| `-l` | Lists the contents of the zip file. |
| `-q` | Suppresses output messages. |
| `-d <directory>` | Extracts files to a specific directory. |

### **Example: Extracting to a Specific Directory**
```bash
unzip website_template.zip -d /home/user/website/
```
Now, the contents are extracted to `/home/user/website/`.

### **Verifying Extraction**
To confirm extraction:
```bash
ls -l /home/user/website/
```

---

## **3. Creating a Zip Archive Using `zip`**
The `zip` command compresses files into a `.zip` archive.

### **Syntax:**
```bash
zip archive_name.zip file1 file2 file3
```

### **Example: Compressing Files**
```bash
zip website_backup.zip index.html styles.css script.js
```
This creates a compressed archive named `website_backup.zip`.

### **Compressing a Folder (`-r` for Recursive)**
To zip an entire folder:
```bash
zip -r website_backup.zip website_folder/
```
This **recursively** adds all files and subdirectories.

### **Checking File Size Before and After Compression**
To compare:
```bash
ls -lh website_backup.zip website_folder/
```

---

## **4. Using `tar` to Archive Files**
The `tar` command **bundles multiple files** into a **single archive** but does **not** compress them.

### **Syntax:**
```bash
tar -cf archive.tar file1 file2
```
This creates `archive.tar` containing `file1` and `file2`.

### **Example: Archiving a Website Folder**
```bash
tar -cf website.tar website_folder/
```

### **Listing the Contents of a Tar Archive**
To verify:
```bash
tar -tf website.tar
```

---

## **5. Compressing Files Using `tar` and `gzip`**
Since `tar` only archives files, we use **gzip** to compress them.

### **Syntax:**
```bash
tar -czf archive.tar.gz file1 file2
```

### **Example: Creating a Compressed Tar Archive**
```bash
tar -czf website.tar.gz website_folder/
```
Here:
- `-c` → Creates an archive.
- `-z` → Compresses it using gzip.
- `-f` → Specifies the filename.

### **Checking File Size After Compression**
To compare:
```bash
ls -lh website_folder/ website.tar.gz
```
Compressed files are smaller than uncompressed ones.

---

## **6. Extracting Tar and GZip Archives**
To **extract a `.tar` file**:
```bash
tar -xf website.tar
```
To **extract a `.tar.gz` file**:
```bash
tar -xzf website.tar.gz
```

### **Extracting to a Specific Directory**
```bash
tar -xzf website.tar.gz -C /home/user/restore_directory/
```

---

## **7. Summary of Commands**
| Task | Command |
|------|---------|
| Extract a zip file | `unzip filename.zip` |
| Extract a zip file to a directory | `unzip filename.zip -d /path/to/dir/` |
| Create a zip archive | `zip archive.zip file1 file2` |
| Zip a folder | `zip -r archive.zip folder/` |
| Create a tar archive | `tar -cf archive.tar file1 file2` |
| List contents of a tar archive | `tar -tf archive.tar` |
| Compress tar archive with gzip | `tar -czf archive.tar.gz folder/` |
| Extract a tar archive | `tar -xf archive.tar` |
| Extract a compressed tar archive | `tar -xzf archive.tar.gz` |

---

## **Next Steps**
We have now covered **compression and archiving** in Linux. The next topic will explore **process management commands in Linux**.
