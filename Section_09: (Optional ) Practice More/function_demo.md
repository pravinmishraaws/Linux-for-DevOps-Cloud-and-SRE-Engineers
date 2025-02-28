**Demonstrating how to **define and call a function** in Bash.

---

### **Script: `function_demo.sh`**
```bash
#!/bin/bash

# Define a function
greet_user() {
    echo "Hello, $1! Welcome to Bash scripting."
}

# Call the function with an argument
greet_user "Alice"

# Define another function to display system information
show_system_info() {
    echo "System Information:"
    echo "-------------------"
    echo "Hostname: $(hostname)"
    echo "Uptime: $(uptime -p)"
    echo "Current User: $(whoami)"
}

# Call the function
show_system_info
```

---

### **How to Run the Script**
1. **Make the script executable**:
   ```bash
   chmod +x function_demo.sh
   ```
2. **Run the script**:
   ```bash
   ./function_demo.sh
   ```

---

### **Example Output**
```
Hello, Alice! Welcome to Bash scripting.
System Information:
-------------------
Hostname: my-server
Uptime: up 2 hours, 15 minutes
Current User: azureuser
```

---

### **Explanation**
1. **Defining a Function**:  
   - `greet_user()` function takes an **argument ($1)** and prints a greeting.
   - `show_system_info()` function retrieves system details using built-in commands.

2. **Calling a Function**:  
   - `greet_user "Alice"` passes `"Alice"` as an argument.
   - `show_system_info` runs without arguments.

