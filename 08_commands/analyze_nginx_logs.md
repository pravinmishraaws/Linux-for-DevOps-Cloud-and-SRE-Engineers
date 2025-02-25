### **How This Script Works:**
1. **Defines variables** for log file paths and output locations.
2. **Checks if the log file exists** before doing anything.
3. **Analyzes Nginx logs** to extract failed login attempts.
4. **Asks the user** if they want to search for a keyword.
5. **Stores the results** in a file and sets an **environment variable** for further automation.

### **Key Learnings for Students:**
✔ **Functions** (`check_log_file`, `analyze_logs`, `search_logs`)  
✔ **Conditional statements** (`if [ ! -f "$LOG_FILE" ]; then`)  
✔ **User input handling** (`read TIME_RANGE`, `read SEARCH_TERM`)  
✔ **Using grep and awk** for log analysis  
✔ **Environment variables** (`export LAST_LOG_ANALYSIS`)  

---

```bash
#!/bin/bash  # This tells the system to use the Bash shell to run this script.

# Enable Debug Mode (Uncomment for debugging)
# set -x  # If enabled, this will show each command before executing it.

# ==========================
# Step 1: Define Variables
# ==========================

LOG_FILE="/var/log/nginx/access.log"  # Path to the Nginx access log file.
FAILED_LOGIN_PATTERN="401"            # HTTP status code for unauthorized access.
OUTPUT_FILE="/tmp/nginx_failed_logins.log"  # File to store failed login analysis results.

# =======================================================
# Step 2: Function to Check if the Log File Exists
# =======================================================

check_log_file() {
    # If the log file does NOT exist, print an error message and stop the script.
    if [ ! -f "$LOG_FILE" ]; then
        echo "Error: Log file $LOG_FILE not found!"  # Display error message.
        exit 1  # Exit the script with status 1 (indicates an error).
    fi
}

# =======================================================
# Step 3: Function to Analyze Logs for Failed Logins
# =======================================================

analyze_logs() {
    local time_range=$1  # The time range (in minutes) provided by the user.
    
    echo "Analyzing failed login attempts in the last $time_range minutes..."
    
    # Use awk to process the log file and extract relevant information.
    awk -v time_range="$time_range" '
    BEGIN { 
        print "Timestamp        | IP Address    | Request URL" 
        print "-------------------------------------------------" 
    } 
    $9 == "401" { print $4, "|", $1, "|", $7 }' "$LOG_FILE" > "$OUTPUT_FILE"
    
    echo "Analysis complete. Results saved in: $OUTPUT_FILE"
}

# =======================================================
# Step 4: Function to Search Logs for a Specific Keyword
# =======================================================

search_logs() {
    local search_term=$1  # The keyword provided by the user.
    
    echo "Searching logs for keyword: $search_term"
    
    # Use grep to find lines containing the search term and append them to the output file.
    grep "$search_term" "$LOG_FILE" | tee -a "$OUTPUT_FILE"
}

# =======================================================
# Step 5: Take User Input for Time Range
# =======================================================

echo "Enter the time range (minutes) to analyze logs:"  # Ask the user for input.
read TIME_RANGE  # Store user input in the TIME_RANGE variable.

# =======================================================
# Step 6: Check File Permissions Before Proceeding
# =======================================================

if [ ! -r "$LOG_FILE" ]; then  # If the log file is not readable:
    echo "Error: No read permission for $LOG_FILE"  # Print error message.
    exit 1  # Exit script with error status.
fi

# =======================================================
# Step 7: Run Log Analysis
# =======================================================

check_log_file  # Call function to check if the log file exists.
analyze_logs "$TIME_RANGE"  # Call function to analyze logs.

# =======================================================
# Step 8: Ask User If They Want to Search for a Keyword
# =======================================================

echo "Enter a keyword to search within logs (or press Enter to skip):"
read SEARCH_TERM  # Read user input.

# If the user entered a keyword (not empty), run the search function.
if [ -n "$SEARCH_TERM" ]; then
    search_logs "$SEARCH_TERM"
fi

# =======================================================
# Step 9: Save Analysis Path as an Environment Variable
# =======================================================

export LAST_LOG_ANALYSIS="$OUTPUT_FILE"  # Store the log file path in an environment variable.

echo "Script execution complete. You can access the analysis at: $OUTPUT_FILE"
```

---

### **How to Run This Script**
1. Copy and save the script as `analyze_nginx_logs.sh`
2. Give it execute permissions:
   ```bash
   chmod +x analyze_nginx_logs.sh
   ```
3. Run the script:
   ```bash
   ./analyze_nginx_logs.sh
   ```
