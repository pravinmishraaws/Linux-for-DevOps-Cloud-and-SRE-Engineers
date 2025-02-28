### **Lesson 4: Input, Output, and Debugging Bash Scripts**  

---

### **Real-World Problem Statement**  

Imagine you are a **DevOps/Cloud Engineer** managing an **Azure Virtual Machine** running critical applications. Your team needs a **flexible monitoring script** that:  
- Allows users to **dynamically specify which services to check**.  
- **Logs detailed output** for later troubleshooting.  
- **Handles errors gracefully** instead of failing silently.  

Without proper **input handling and debugging techniques**, scripts can be **rigid and difficult to troubleshoot** when something goes wrong.

By the end of this lesson, you will be able to:  
✔ **Read user input from the command line.**  
✔ **Redirect input and output efficiently.**  
✔ **Debug Bash scripts using built-in tools.**  
✔ **Improve error handling in scripts.**  

This lesson **builds on Lesson 3** by making our script **interactive, more robust, and easier to debug**.

---

## **1. Accepting User Input in Bash**  

Instead of hardcoding service names in the script, let’s allow the user to **pass them dynamically**.

### **1.1 Using `read` to Accept Input**  

Modify the **`check_services` function** to prompt the user for service names:

```bash
check_services() {
    echo "Enter the services you want to check (separate by space):"
    read -r SERVICES

    for SERVICE in $SERVICES; do
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
```

### **1.2 Using Command-Line Arguments Instead of `read`**  

To allow users to specify services **as arguments when running the script**, modify the function:

```bash
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
```

✅ **Usage Example:**  
```bash
./system_info.sh nginx apache2 mysql
```
This allows **dynamic service monitoring** instead of hardcoding service names.

---

## **2. Redirecting Output to Files for Logging**  

Bash scripts generate **output** that can be logged for later analysis.

### **2.1 Redirect Standard Output (`>` and `>>`)**  

| **Operator** | **Usage** |
|-------------|----------|
| `>` | Overwrites a file with new output. |
| `>>` | Appends new output to an existing file. |

Modify `log_message` to store logs persistently:

```bash
log_message() {
    echo "$(date) - $1" >> /var/log/system_health.log
}
```

✅ **Example:**  
```bash
log_message "System check completed."
```
This **appends** logs instead of overwriting them.

---

### **2.2 Redirect Standard Error (`2>`, `2>>`)**  

By default, errors appear on the terminal. To store them in a log file:

```bash
./system_info.sh 2>> /var/log/system_health_errors.log
```

✅ **Now, errors are logged separately for easy debugging.**

---

## **3. Debugging Bash Scripts**  

Bash scripts can fail for various reasons. **Debugging techniques help identify and fix issues quickly.**

### **3.1 Using `set -x` for Debugging**  

To enable **debug mode**, modify the script:

```bash
#!/bin/bash
set -x  # Enable debugging
```

✅ **Now, every command gets printed before execution.**  

To run a script **temporarily in debug mode**, use:  
```bash
bash -x system_info.sh
```

---

### **3.2 Using `set -e` to Stop on Errors**  

By default, Bash **ignores errors** and continues execution. To prevent this:

```bash
#!/bin/bash
set -e  # Exit script if any command fails
```

✅ **Example:**  
If `systemctl restart` fails, the script **stops immediately** instead of continuing.

---

### **3.3 Using `trap` for Error Handling**  

`trap` helps **clean up resources** and **handle errors gracefully**.

Modify the script to **log and exit on errors**:

```bash
error_handler() {
    echo "An error occurred. Check logs for details."
    exit 1
}

trap error_handler ERR
```

✅ **Now, any error triggers the `error_handler` function and stops execution.**  

---

## **4. Final Improved Script**  

Now, let’s integrate **everything we learned** into a fully functional, interactive script.

```bash
#!/bin/bash
set -e  # Stop script on errors
LOGFILE="/var/log/system_health.log"
ERROR_LOG="/var/log/system_health_errors.log"

log_message() {
    echo "$(date) - $1" | tee -a $LOGFILE
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

monitor_system() {
    while true; do
        log_message "Checking system health..."
        check_services "$@"
        sleep 10
    done
}

trap 'log_message "An error occurred. Check logs for details."; exit 1' ERR

monitor_system "$@"
```

---

## **5. Running and Testing the Script**  

### **Make the Script Executable**
```bash
chmod +x system_info.sh
```

### **Run the Script with Services**
```bash
./system_info.sh nginx apache2 mysql
```

✅ **View Logs**
```bash
cat /var/log/system_health.log
cat /var/log/system_health_errors.log
```

✅ **Run the Script in Debug Mode**
```bash
bash -x system_info.sh nginx apache2 mysql
```

---

## **Key Takeaways**  

- **User input allows dynamic script execution** (`read`, command-line arguments).  
- **Redirecting output (`>`, `>>`) helps store logs persistently.**  
- **Debugging with `set -x`, `set -e`, and `trap` improves reliability.**  
- **Final script now accepts services as input, logs results, and handles errors properly.**  

---

## **What’s Next?**  

Now that you can **handle input, manage output, and debug scripts**, the next step is:  

- **Handling files and directories in Bash.**  
- **Automating backups and log rotation.**  
- **Using Bash to manage system files effectively.**  

Let’s move forward and explore **Working with Environment Variables in Lesson 5**.
