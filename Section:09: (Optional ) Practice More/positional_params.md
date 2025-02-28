### **Script: `positional_params.sh`**
```bash
#!/bin/bash

# Display the script name
echo "Script Name: $0"

# Display individual arguments
echo "First Argument: $1"
echo "Second Argument: $2"

# Display the total number of arguments
echo "Total Number of Arguments: $#"

# Display all arguments as a list
echo "All Arguments: $@"

# Loop through all arguments
echo "Iterating through all arguments:"
for arg in "$@"; do
    echo "- $arg"
done
```

---

### **How to Run the Script**
1. **Make the script executable**:
   ```bash
   chmod +x positional_params.sh
   ```
2. **Run the script with arguments**:
   ```bash
   ./positional_params.sh apple banana orange
   ```

---

### **Example Output**
```
Script Name: ./positional_params.sh
First Argument: apple
Second Argument: banana
Total Number of Arguments: 3
All Arguments: apple banana orange
Iterating through all arguments:
- apple
- banana
- orange
```

This script shows how to access the script name (`$0`), individual arguments (`$1`, `$2`), the total number of arguments (`$#`), and all arguments as a list (`$@`). It also **loops through all arguments** to display them one by one.
