# Study Notes: Project Completion and Results

## Finalizing the Project
We have successfully learned to navigate the **Linux file system**, manage **files and directories**, and configure a **web server**. Now, we will complete our project by transferring our **website files** to the correct location and verifying the final deployment.

---

## Merging and Displaying File Contents with `cat`
The `cat` command is used to **view, merge, and create files**.

### Viewing Multiple Files
To display the contents of multiple files:
```bash
cat index.html powered-by.png
```
This command shows the contents of both files in sequence.

### Adding Line Numbers
To display line numbers in a file:
```bash
cat -n index.html
```
Each line in the file will now be numbered.

### Merging Files and Saving to a New File
To combine multiple files into one:
```bash
cat index.html powered-by.png > newfile.txt
```
This redirects the contents of `index.html` and `powered-by.png` into `newfile.txt`.

---

## Creating New Files with `touch`
The `touch` command is used to **create an empty file** or **update timestamps**.

### Creating a Single File
```bash
touch newfile.txt
```
This creates an empty file named `newfile.txt`.

### Creating Multiple Files
```bash
touch file1.txt file2.txt file3.txt
```
This command creates three empty files at once.

---

## Removing Files with `rm`
The `rm` (Remove) command is used to **delete files and directories**.

### Deleting a File
```bash
rm index.html
```
Deletes `index.html`.

### Deleting a File with Confirmation
```bash
rm -i index.html
```
Prompts before deletion.

### Deleting a Directory and Its Contents
```bash
rm -r mydirectory
```
Removes a directory and all its files.

---

## Transferring Files to the EC2 Instance with `scp`
The `scp` (Secure Copy) command is used to transfer files **securely** between a local machine and a remote server.

### Copying a File from Local Computer to EC2
```bash
scp -i mykey.pem mywebsite.zip ec2-user@<EC2-IP>:/home/ec2-user/
```
- `-i mykey.pem` → Specifies the SSH key.
- `mywebsite.zip` → The file to transfer.
- `ec2-user@<EC2-IP>` → The EC2 user and public IP.
- `/home/ec2-user/` → The destination directory.

### Copying a File from EC2 to Local Machine
```bash
scp -i mykey.pem ec2-user@<EC2-IP>:/home/ec2-user/file.txt .
```
Copies `file.txt` from EC2 to the local machine.

---

## Extracting Files with `unzip`
Once the template files are transferred to the EC2 instance, we need to extract them.
```bash
unzip mywebsite.zip
```
Lists the extracted files.

---

## Moving Files with `mv`
The `mv` (Move) command transfers files and directories to a new location.

### Moving Website Files to Nginx Directory
```bash
sudo mv mywebsite/* /usr/share/nginx/html/
```
Moves all files from `mywebsite` to the **Nginx web server directory**.

---

## Final Verification
After moving the files, we check the contents of the Nginx directory:
```bash
ls /usr/share/nginx/html/
```
If the files are present, our website is ready.

### Accessing the Website
1. Go to **AWS EC2 Console**.
2. Select your **EC2 instance**.
3. Copy the **Public IP Address**.
4. Open a browser and enter:
   ```
   http://<Public-IP>
   ```
5. If the website loads, the deployment is **successful**.

---

## Summary
- Used `cat` to **view, merge, and save** file contents.
- Created files using `touch`.
- Deleted files using `rm`.
- Transferred files securely with `scp`.
- Extracted files with `unzip`.
- Moved files to the correct location using `mv`.
- Verified the website deployment by accessing the **Public IP**.

### 🎉 Congratulations! You have successfully hosted a static website on an **Amazon Linux EC2 Instance using Nginx**.


