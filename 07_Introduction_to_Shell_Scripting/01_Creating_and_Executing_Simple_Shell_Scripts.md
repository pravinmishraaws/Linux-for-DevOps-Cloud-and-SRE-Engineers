# **Creating and Executing Simple Shell Scripts in Linux**

## **Introduction to Shell Scripting**
Shell scripting in Linux allows users to automate repetitive tasks by writing a sequence of commands in a text file. The **Shell** acts as an interface between the user and the operating system, interpreting these commands and executing them step by step.

### **What is a Shell?**
The **Shell** is a command-line interpreter that takes user input, processes it, and communicates with the **Kernel** to execute commands. There are different types of shells in Linux:

1. **Bash (Bourne Again Shell)** – The most commonly used shell in Linux distributions.
2. **Z Shell (Zsh)** – Provides additional features like better auto-completion and syntax highlighting.
3. **C Shell (CSH) / Korn Shell (KSH)** – Used in some environments but less common than Bash or Zsh.

---

## **Understanding Shell Scripts**
A **shell script** is a text file containing a series of shell commands. Instead of typing commands one by one, users can execute a script file that runs all the commands in sequence.

### **Why Use Shell Scripting?**
- Automates repetitive tasks.
- Saves time and reduces human error.
- Allows running multiple commands in sequence.
- Useful for system administration and DevOps tasks.

---

## **Creating a Simple Shell Script**
### **Step 1: Writing a Shell Script**
Shell scripts typically begin with a special line called a **shebang** (`#!`), which specifies the interpreter to be used.

### **Syntax**
```bash
#!/bin/bash
# This is a comment
echo "Hello, World! Welcome to Shell Scripting"
```

### **Explanation**
| Code | Description |
|------|------------|
| `#!/bin/bash` | Shebang line – specifies Bash as the interpreter. |
| `#` | Denotes a comment (ignored by the shell). |
| `echo "Hello, World!"` | Prints a message to the terminal. |

---

## **Step 2: Creating and Editing a Shell Script File**
1. Open a terminal and navigate to a directory where you want to create the script.
2. Use a text editor (e.g., `vim`, `nano`) to create a script file.

```bash
vim myscript.sh
```

3. Enter **insert mode** in `vim` by pressing **`i`** and type the script:

```bash
#!/bin/bash
echo "This is my first shell script!"
```

4. Save and exit:
   - Press **`Esc`**, then type `:wq` and press **`Enter`**.

---

## **Step 3: Making the Script Executable**
By default, the script is just a text file. To execute it, **you need to grant execute permission**.

```bash
chmod +x myscript.sh
```

To check if the script has execute permission:

```bash
ls -l myscript.sh
```

**Expected Output:**
```bash
-rwxr-xr-x 1 user user 42 Feb 28 12:00 myscript.sh
```
- `rwx` (for owner) means **read, write, execute** permission is granted.

---

## **Step 4: Running the Shell Script**
There are two ways to execute a shell script:

1. Using `bash`:
   ```bash
   bash myscript.sh
   ```

2. Running it directly:
   ```bash
   ./myscript.sh
   ```

**Expected Output:**
```bash
This is my first shell script!
```

---

## **Creating a More Practical Shell Script**
Let's create a shell script that **automates the creation of folders and files**.

### **Step 1: Writing the Script**
1. Open a new file:
   ```bash
   vim create_folders.sh
   ```
2. Write the following script:
   ```bash
   #!/bin/bash
   echo "Creating directories and files..."

   # Create directories
   mkdir -p folder1 folder2

   # Create files inside each directory
   touch folder1/file1.txt folder2/file2.txt

   # Write some text into the files
   echo "This is file1 inside folder1" > folder1/file1.txt
   echo "This is file2 inside folder2" > folder2/file2.txt

   echo "Folders and files created successfully!"
   ```

3. Save and exit (`Esc` → `:wq` → `Enter`).

---

### **Step 2: Assigning Execution Permission**
```bash
chmod +x create_folders.sh
```

---

### **Step 3: Running the Script**
```bash
./create_folders.sh
```

---

### **Step 4: Verifying the Output**
1. Check the created directories:
   ```bash
   ls -l
   ```
   **Expected Output:**
   ```bash
   drwxr-xr-x 2 user user 4096 Feb 28 12:10 folder1
   drwxr-xr-x 2 user user 4096 Feb 28 12:10 folder2
   ```

2. Check the created files:
   ```bash
   ls -l folder1 folder2
   ```
   **Expected Output:**
   ```bash
   -rw-r--r-- 1 user user 30 Feb 28 12:10 file1.txt
   -rw-r--r-- 1 user user 30 Feb 28 12:10 file2.txt
   ```

3. View the content of the files:
   ```bash
   cat folder1/file1.txt
   ```
   **Expected Output:**
   ```bash
   This is file1 inside folder1
   ```

---

## **Executing Multiple Commands in a Script**
A script can run **multiple Linux commands** sequentially:

### **Example: System Information Script**
```bash
#!/bin/bash
echo "System Information:"
echo "-------------------"
echo "Hostname: $(hostname)"
echo "Current User: $(whoami)"
echo "Uptime: $(uptime -p)"
echo "Disk Usage:"
df -h
echo "Memory Usage:"
free -h
```

**Save and run:**
```bash
chmod +x sysinfo.sh
./sysinfo.sh
```

---

## **Best Practices for Shell Scripting**
1. **Use Meaningful File Names** – Helps in organization (`backup_script.sh` instead of `script1.sh`).
2. **Include Comments** – Use `#` to explain sections of the script.
3. **Use Variables** – Store and reuse values:
   ```bash
   name="John"
   echo "Hello, $name!"
   ```
4. **Use Conditional Statements** – Execute commands based on conditions:
   ```bash
   if [ -f "file.txt" ]; then
       echo "File exists!"
   else
       echo "File not found!"
   fi
   ```
5. **Use Loops** – Automate repetitive tasks:
   ```bash
   for file in *.txt; do
       echo "Processing $file"
   done
   ```
6. **Handle Errors Gracefully** – Use exit codes:
   ```bash
   command || echo "Command failed!"
   ```

---

## **Summary**
| Command | Purpose |
|---------|---------|
| `#!/bin/bash` | Shebang – Defines script interpreter |
| `chmod +x script.sh` | Make script executable |
| `./script.sh` | Execute script |
| `echo "message"` | Print output |
| `mkdir folder` | Create a directory |
| `touch file.txt` | Create a file |
| `df -h` | Check disk space |
| `free -h` | Check memory usage |

### **Key Takeaways**
- Shell scripts **automate** tasks.
- The **shebang (`#!/bin/bash`)** defines the interpreter.
- Use `chmod +x` to make scripts executable.
- Scripts can **run multiple commands** sequentially.
- **Best practices** include using comments, variables, and error handling.

---

## **Next Steps**
Now that you understand the **basics of shell scripting**, in the next lesson, we will cover **variables, user input, and control statements** in shell scripting to make scripts more interactive and powerful.
