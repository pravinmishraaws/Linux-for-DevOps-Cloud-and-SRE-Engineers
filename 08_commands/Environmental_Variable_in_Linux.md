# **Walking Tutorial: Defining an Environmental Variable in Linux**

## **Introduction**
Imagine you are deploying a script across **multiple environments** — development, staging, and production. Each environment might have **different database URLs, API keys, or file paths**.  

**Hardcoding these values inside scripts is risky and inflexible.** If you need to change a database URL, you would have to **edit and redeploy** the script every time.  

This is where **environment variables** come in. They act as **dynamic placeholders** for your system or scripts, helping you manage configurations **without hardcoding sensitive or environment-specific information**.

---

## **Step 1: Understanding Environment Variables**

Environment variables are **key-value pairs** stored in the **shell session or operating system**. These variables provide **configuration information** to processes and applications.

### **Common Environment Variables**
| Variable | Description |
|----------|-------------|
| **PATH** | Defines directories where executable programs are searched. |
| **HOME** | Indicates the current user’s home directory. |
| **USER** | Stores the username of the current user. |
| **SHELL** | Specifies the default shell for the user. |

**Check Your Current Environment Variables**  
Run:
```bash
printenv
```
or  
```bash
env
```
This will display **all active environment variables** in your session.

---

## **Step 2: Creating a Temporary Environment Variable**
To **define** a variable in your current session, use:
```bash
export MY_VAR="Hello, World!"
```
Verify it using:
```bash
echo $MY_VAR
```
**Expected Output:**
```
Hello, World!
```
However, this variable **will only exist for the current session**. If you close the terminal, it will disappear.

---

## **Step 3: Making Environment Variables Permanent**
If you need a variable to **persist across sessions**, you must add it to a **configuration file**.

### **3.1 User-Specific Environment Variables**
To make an environment variable permanent for **only your user**, add it to `~/.bashrc` or `~/.bash_profile`:
```bash
echo 'export MY_VAR="Hello, Permanent World!"' >> ~/.bashrc
```
**Apply the changes immediately**:
```bash
source ~/.bashrc
```
Now, `MY_VAR` will persist even after logging out.

### **3.2 System-Wide Environment Variables**
If you want **all users** to have access to the variable, define it in `/etc/environment`:
```bash
sudo nano /etc/environment
```
Add the variable at the end of the file:
```
MY_VAR="Hello, System-Wide World!"
```
Save and exit (`CTRL + X`, then `Y`, then `Enter`).  

Apply changes with:
```bash
source /etc/environment
```

---

## **Step 4: Viewing and Using Environment Variables**
### **4.1 View a Specific Environment Variable**
To check the value of an environment variable:
```bash
echo $MY_VAR
```
### **4.2 View All Environment Variables**
```bash
printenv
```
or
```bash
set
```

---

## **Step 5: Unsetting and Deleting Environment Variables**
### **5.1 Unset a Variable Temporarily**
To remove a variable from the current session:
```bash
unset MY_VAR
```
After this, running:
```bash
echo $MY_VAR
```
Will return an **empty line**, meaning the variable is removed.

### **5.2 Delete a Permanent Variable**
If the variable was added to `~/.bashrc`, `~/.bash_profile`, or `/etc/environment`, manually **remove the line** from the respective file using:
```bash
nano ~/.bashrc
```
After deletion, apply changes:
```bash
source ~/.bashrc
```

---

## **Step 6: Real-World Use Case**
Let's apply environment variables to a **real-world scenario**. Assume we are setting up an **Nginx server** and want to configure a **database connection** dynamically.

1. **Define the Database Connection String**
```bash
export DB_CONNECTION="mysql://user:password@db-host:3306/my_database"
```
2. **Use it in a Bash Script**
```bash
#!/bin/bash
echo "Connecting to database at: $DB_CONNECTION"
```
3. **Run the Script**
```bash
chmod +x db_connect.sh
./db_connect.sh
```
**Expected Output:**
```
Connecting to database at: mysql://user:password@db-host:3306/my_database
```
Now, if we **change the database connection**, we only update the **environment variable** instead of modifying the script.

---

## **Key Takeaways**
✔ **Environment variables help manage configurations dynamically.**  
✔ **Temporary variables exist only in the current session.**  
✔ **Permanent variables must be added to `~/.bashrc`, `~/.bash_profile`, or `/etc/environment`.**  
✔ **Unset variables when they are no longer needed.**  
✔ **Using environment variables in scripts improves flexibility and security.**  
