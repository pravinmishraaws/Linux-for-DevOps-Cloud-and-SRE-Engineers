### **Bash Script Demonstrating Output Redirection, Input Redirection, and Piping**

This script will:
1. **Create and write** to a file using `>`.
2. **Append** content using `>>`.
3. **Redirect input** from a file using `<`.
4. **Use a pipe (`|`)** to filter command output.

---

### **Script: `redirection_demo.sh`**
```bash
#!/bin/bash

# Step 1: Standard Output Redirection (>)
echo "Creating and writing to output.txt..."
ls > output.txt  # Save the output of 'ls' command to output.txt
echo "File 'output.txt' created with directory listing."

# Step 2: Append Output (>>)
echo "Appending text to output.txt..."
echo "This is additional content" >> output.txt
echo "Text has been appended to output.txt."

# Step 3: Input Redirection (<)
echo -e "banana\napple\ncherry" > input.txt  # Create an input file
echo "Sorting contents of input.txt using input redirection..."
sort < input.txt  # Sort the contents of input.txt
echo "Sorted output displayed above."

# Step 4: Using Pipes (|)
echo "Filtering 'file' from directory listing using pipes..."
ls | grep "file"  # List files and filter those containing "file"

echo "Script execution complete."
```

---

### **How to Run the Script**
1. **Make the script executable**:
   ```bash
   chmod +x redirection_demo.sh
   ```
2. **Run the script**:
   ```bash
   ./redirection_demo.sh
   ```

---

### **Example Output**
```
Creating and writing to output.txt...
File 'output.txt' created with directory listing.
Appending text to output.txt...
Text has been appended to output.txt.
Sorting contents of input.txt using input redirection...
apple
banana
cherry
Sorted output displayed above.
Filtering 'file' from directory listing using pipes...
my_file.txt
Script execution complete.
```

---

### **Explanation**
1. **`>` Standard Output Redirection**  
   - `ls > output.txt` **writes** the output of `ls` to `output.txt` (overwrites the file).
  
2. **`>>` Append Output**  
   - `echo "Additional text" >> output.txt` **adds** text at the end of `output.txt` without overwriting.

3. **`<` Input Redirection**  
   - `sort < input.txt` reads input **from `input.txt` instead of the keyboard**, sorts it, and prints the result.

4. **`|` Pipe Output**  
   - `ls | grep "file"` lists files, then filters results that contain `"file"`.
