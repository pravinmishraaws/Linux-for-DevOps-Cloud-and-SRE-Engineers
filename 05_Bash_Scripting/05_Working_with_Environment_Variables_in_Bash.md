### **Lesson 5: Working with Environment Variables in Bash**  

---

### **Real-World Problem Statement**  

Imagine you are a **DevOps/Cloud Engineer** automating infrastructure deployment in **Azure**. Your team manages multiple servers, and your scripts need to:  

- Dynamically reference **server names, database credentials, and API keys**.  
- Adapt to different **environments (development, staging, production)**.  
- Keep sensitive data **secure and configurable**.  

Hardcoding values in scripts is **risky and inefficient**. This is where **environment variables** come in.  

By the end of this lesson, you will be able to:  
✔ **Define and use environment variables in Bash scripts.**  
✔ **Modify system environment variables dynamically.**  
✔ **Securely pass sensitive values without exposing them in scripts.**  
✔ **Use environment variables for automation in an Azure VM.**  

This lesson **builds on Lesson 4** by making our script **dynamic and configurable**.

---

## **1. What Are Environment Variables?**  

Environment variables store **key-value pairs** that affect **how programs and scripts run**.  

### **Why Use Environment Variables?**  

| **Use Case** | **Example** |
|-------------|------------|
| **Referencing system paths** | `HOME`, `PATH` |
| **Storing configuration values** | `DB_HOST`, `APP_ENV` |
| **Hiding sensitive data** | `AWS_SECRET_KEY`, `API_KEY` |
| **Passing values to scripts** | `export SERVER_NAME=web01` |

✅ **Example:**  

```bash
echo "My home directory is $HOME"
```
**Output:**  
```
My home directory is /home/azureuser
```

---

## **2. Viewing Environment Variables**  

### **2.1 List All Environment Variables**  

To see all available environment variables:  
```bash
env
```

Or, use:  
```bash
printenv
```

### **2.2 View a Specific Variable**  

To check the value of an individual variable:  
```bash
echo $HOME
```

✅ **Example Output:**  
```
/home/azureuser
```

---

## **3. Creating and Using Environment Variables**  

### **3.1 Defining a Temporary Variable**  

To create a variable for the **current session only**:  
```bash
MY_NAME="DevOps Engineer"
echo "Hello, $MY_NAME!"
```

✅ **Output:**  
```
Hello, DevOps Engineer!
```

However, this variable **disappears after logout**.

---

### **3.2 Exporting a Variable for Child Processes**  

To make a variable available to **subshells and child processes**, use `export`:  
```bash
export APP_ENV="production"
```

Now, any script or application started in this session can access `APP_ENV`.

✅ **Check if Export Worked:**  
```bash
printenv | grep APP_ENV
```
✅ **Output:**  
```
APP_ENV=production
```

---

### **3.3 Persisting Environment Variables Across Reboots**  

To make variables **permanent**, add them to:  
- **For user-specific variables:** `~/.bashrc`  
- **For system-wide variables:** `/etc/environment`  

✅ **Example: Add to `~/.bashrc` for Persistence**  
```bash
echo 'export DB_HOST="db.example.com"' >> ~/.bashrc
source ~/.bashrc
```

✅ **Check the Variable:**  
```bash
echo $DB_HOST
```
✅ **Output:**  
```
db.example.com
```

---

## **4. Using Environment Variables in Scripts**  

Let’s modify our **system monitoring script** to use environment variables dynamically.

### **4.1 Updating the Script to Use Variables**  

Modify **`system_info.sh`** to reference environment variables:

```bash
#!/bin/bash
set -e  
LOGFILE="${LOG_PATH:-/var/log/system_health.log}"  # Use LOG_PATH if set, otherwise default

log_message() {
    echo "$(date) - $1" | tee -a "$LOGFILE"
}

check_services() {
    if [ $# -eq 0 ]; then
        echo "Usage: $0 service1 service2 ..."
        exit 1
    fi

    for SERVICE in "$@"; do
        if systemctl is-active --quiet "$SERVICE"; then
            log_message "$SERVICE is running."
        else
            log_message "WARNING: $SERVICE is NOT running!"
            log_message "Attempting to restart $SERVICE..."
            sudo systemctl restart "$SERVICE"
            if systemctl is-active --quiet "$SERVICE"; then
                log_message "$SERVICE successfully restarted."
            else
                log_message "ERROR: Failed to restart $SERVICE!"
            fi
        fi
    done
}

trap 'log_message "An error occurred. Check logs for details."; exit 1' ERR

check_services "$@"
```

### **4.2 Running the Script with a Custom Log Path**  

To **override the default log file location**, use:  
```bash
export LOG_PATH="/tmp/system_health.log"
./system_info.sh nginx apache2
```

✅ **Now, logs are stored in `/tmp/system_health.log` instead of `/var/log/system_health.log`.**  

---

## **5. Passing Environment Variables to a Script**  

Instead of exporting variables globally, we can **pass them inline**:  

```bash
LOG_PATH="/tmp/mylog.log" ./system_info.sh nginx apache2
```

✅ **This keeps the variable limited to the script execution only.**  

---

## **6. Securing Sensitive Environment Variables**  

### **6.1 Avoid Hardcoding Secrets in Scripts**  

**❌ Bad Practice:**  
```bash
DB_PASSWORD="mypassword"
```

**✅ Best Practice:** Store in an environment file (`/etc/environment`) or `.env` file.

### **6.2 Using `.env` Files**  

Create a `.env` file:  
```bash
echo 'DB_HOST="db.example.com"' > .env
echo 'DB_USER="admin"' >> .env
echo 'DB_PASSWORD="securepass"' >> .env
```

To load the variables dynamically:  
```bash
source .env
```

✅ **Now, variables are accessible without exposing them in scripts.**

---

## **7. Best Practices for Using Environment Variables**  

✔ **Use environment variables for dynamic configurations instead of hardcoding.**  
✔ **Store sensitive credentials securely using `.env` files or secret management tools.**  
✔ **Use `export` to make variables available for all processes.**  
✔ **Persist important variables in `~/.bashrc` or `/etc/environment` for long-term use.**  
✔ **Use `printenv` and `env` to debug and list current environment variables.**  

---

## **8. Final Script with Environment Variables**  

```bash
#!/bin/bash
set -e  

LOGFILE="${LOG_PATH:-/var/log/system_health.log}"  # Use LOG_PATH if set, otherwise default

log_message() {
    echo "$(date) - $1" | tee -a "$LOGFILE"
}

check_services() {
    if [ $# -eq 0 ]; then
        echo "Usage: $0 service1 service2 ..."
        exit 1
    fi

    for SERVICE in "$@"; do
        if systemctl is-active --quiet "$SERVICE"; then
            log_message "$SERVICE is running."
        else
            log_message "WARNING: $SERVICE is NOT running!"
            log_message "Attempting to restart $SERVICE..."
            sudo systemctl restart "$SERVICE"
            if systemctl is-active --quiet "$SERVICE"; then
                log_message "$SERVICE successfully restarted."
            else
                log_message "ERROR: Failed to restart $SERVICE!"
            fi
        fi
    done
}

trap 'log_message "An error occurred. Check logs for details."; exit 1' ERR

check_services "$@"
```

✅ **Now, the script dynamically logs data to the path set in `LOG_PATH`.**

---

## **Key Takeaways**  

- **Environment variables allow dynamic, reusable, and configurable scripts.**  
- **Use `export` to define session-wide environment variables.**  
- **Store secrets securely in `.env` files, not hardcoded in scripts.**  
- **Pass variables dynamically when executing a script (`VAR=value ./script.sh`).**  
- **Modify our script to support configurable log paths.**  

---

## **What’s Next?**  

Now that we understand **environment variables**, the next step is:  

- **Handling files and directories in Bash.**  
- **Automating backups and log rotation.**  
- **Using Bash to manage system files effectively.**  

Let’s move forward and explore **Practical Use Cases in Lesson 6**.
