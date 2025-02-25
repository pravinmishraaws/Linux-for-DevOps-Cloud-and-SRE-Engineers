### **Analyze System Logs**
✅ Extracts **important events** (errors, warnings, and failed login attempts).  
✅ Uses **basic functions, user input, and filtering** to teach Bash scripting.  
✅ **Simple to run and understand**, making it great for students!

---

### **Updated Script: Analyze System Logs**
```bash
#!/bin/bash

# ==========================
# Step 1: Define Variables
# ==========================
LOG_FILE="/var/log/syslog"  # Change to /var/log/messages for CentOS/RHEL systems
OUTPUT_FILE="/tmp/system_log_analysis.log"

# ==========================
# Step 2: Function to Check if the Log File Exists
# ==========================
check_log_file() {
    if [ ! -f "$LOG_FILE" ]; then
        echo "Error: Log file $LOG_FILE not found!"
        exit 1
    fi
}

# ==========================
# Step 3: Function to Analyze Logs for Errors and Warnings
# ==========================
analyze_logs() {
    echo "Analyzing system logs for errors and warnings..."
    
    # Filter out log entries with "error", "failed", or "warning" (case insensitive)
    grep -iE "error|failed|warning" "$LOG_FILE" > "$OUTPUT_FILE"
    
    echo "Analysis complete. Results saved in: $OUTPUT_FILE"
}

# ==========================
# Step 4: Function to Search Logs for a Specific Keyword
# ==========================
search_logs() {
    local search_term=$1
    echo "Searching system logs for keyword: $search_term"
    
    # Search the log file for the user-specified keyword
    grep -i "$search_term" "$LOG_FILE" | tee -a "$OUTPUT_FILE"
}

# ==========================
# Step 5: Take User Input for Analysis
# ==========================
echo "Welcome to the System Log Analyzer!"
echo "This script will help you find errors, failed logins, and warnings in system logs."
echo ""

# Ensure the log file exists before proceeding
check_log_file

# Analyze system logs
analyze_logs

# ==========================
# Step 6: Ask User If They Want to Search for a Keyword
# ==========================
echo "Enter a keyword to search within logs (or press Enter to skip):"
read SEARCH_TERM

if [ -n "$SEARCH_TERM" ]; then
    search_logs "$SEARCH_TERM"
fi

# ==========================
# Step 7: Store Analysis Result in an Environment Variable
# ==========================
export LAST_LOG_ANALYSIS="$OUTPUT_FILE"

echo "Script execution complete. You can access the analysis at: $OUTPUT_FILE"
```

---

### **How This Works (Step by Step)**
1. **Checks if system logs exist** (`/var/log/syslog`).
2. **Extracts important events** (Errors, Failed Logins, Warnings).
3. **Allows users to search logs** for specific terms.
4. **Saves results to a file** for further review.
5. **Stores the log file path in an environment variable** (`LAST_LOG_ANALYSIS`).

---

### **How to Run This Script**
1. **Copy and save the script** as `analyze_system_logs.sh`
2. **Give it execute permissions:**
   ```bash
   chmod +x analyze_system_logs.sh
   ```
3. **Run the script:**
   ```bash
   ./analyze_system_logs.sh
   ```
