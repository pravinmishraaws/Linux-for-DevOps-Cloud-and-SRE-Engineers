# **Customizing the Shell Prompt in Linux**  

As a Cloud engineer, customizing the **shell prompt** can enhance productivity by making the command line **more informative and visually distinct**. This is especially useful when managing multiple servers, working in different environments (dev, staging, production), or tracking time and user details.

---

## **Understanding the Default Shell Prompt**  

By default, the Linux shell prompt appears as:  

```bash
user@hostname:~$
```

This format includes:
- **`user`** → Your current username.  
- **`hostname`** → The name of the computer/server.  
- **`~`** → The current working directory (`~` represents home).  
- **`$` or `#`** → `$` for a regular user, `#` for root (superuser).  

✅ **Example:** If you log in as `cloud` on a machine called `server1`, the prompt looks like:  
```bash
cloud@server1:~$
```
---

## **Temporarily Changing the Shell Prompt (PS1)**  
The `PS1` variable controls how the prompt is displayed. You can modify it temporarily using:  

✅ **Example 1: Adding the Current Time to the Prompt**  
```bash
PS1="[\t] \u@\h:\w$ "
```
🔹 **Explanation:**  
- `\t` → Shows the current time.  
- `\u` → Displays the username.  
- `\h` → Shows the hostname.  
- `\w` → Displays the current directory.  

🖥 **Output:**  
```bash
[14:35:10] cloud@server1:~$
```
---

✅ **Example 2: Changing the Prompt Color**  
```bash
PS1="\[\e[32m\]\u@\h:\w$\[\e[0m\] "
```
🔹 **Explanation:**  
- `\[\e[32m\]` → Sets the text color to **green**.  
- `\[\e[0m\]` → Resets the color after the prompt.  

🖥 **Output:** *(Green prompt)*
```bash
cloud@server1:~$
```
---

✅ **Example 3: Show Git Branch in Prompt (Useful for Cloud!)**  
```bash
PS1='\u@\h:\w$(git branch 2>/dev/null | grep "^*" | colrm 1 2) $ '
```
🔹 **Why?**  
- This displays the **current Git branch** when inside a Git repository.  
- Helps when managing **GitOps workflows**, tracking deployments, and switching branches easily.  

🖥 **Output:** *(Inside a Git repo)*
```bash
cloud@server1:~/project (main)$
```
---

## **Making Changes Permanent**  
Changes made with `PS1` are **temporary** and reset when you log out.  

To **make them permanent**, edit the `~/.bashrc` file:  

✅ **Steps to Customize the Prompt Permanently**  
1️⃣ Open the `.bashrc` file in your home directory:  
```bash
nano ~/.bashrc
```
2️⃣ Scroll to the bottom and add your new `PS1` customization. Example:  
```bash
export PS1="[\t] \u@\h:\w$ "
```
3️⃣ Save and exit (`CTRL + X`, then `Y`, then `Enter`).  
4️⃣ Apply the changes:  
```bash
source ~/.bashrc
```

---

## **Advanced Prompt Customization**  

✅ **Example 1: Show Username, Host, and Exit Code of Last Command**  
```bash
PS1="\u@\h \[\e[31m\]\$?\[\e[0m\]:\w$ "
```
🔹 **Why?**  
- Displays the **exit code of the last command** in **red** (useful for debugging errors).  

🖥 **Output:**  
```bash
cloud@server1 0:~/scripts$
```
*(Exit code `0` means success, non-zero means failure.)*  

---

✅ **Example 2: Displaying Current Kubernetes Context (For Cloud!)**  
```bash
PS1="\u@\h:\w \$(kubectl config current-context) $ "
```
🔹 **Why?**  
- Helps track which **Kubernetes cluster** you're working on.  
- Prevents mistakes when switching between clusters (e.g., **production vs staging**).  

🖥 **Output (Inside a K8s Cluster):**  
```bash
cloud@server1:~/project minikube$
```

---

## **Summary Table: Shell Customization**
| **Customization** | **Command** | **Use Case** |
|------------------|-------------|--------------|
| **Add Time to Prompt** | `PS1="[\t] \u@\h:\w$ "` | Helpful for tracking command execution time |
| **Change Color** | `PS1="\[\e[32m\]\u@\h:\w$\[\e[0m\] "` | Makes prompt visually distinct |
| **Show Git Branch** | `PS1='\u@\h:\w$(git branch 2>/dev/null | grep "^*" | colrm 1 2) $ '` | Essential for version control workflows |
| **Show Exit Code** | `PS1="\u@\h \[\e[31m\]\$?\[\e[0m\]:\w$ "` | Debugging command failures |
| **Show Kubernetes Context** | `PS1="\u@\h:\w \$(kubectl config current-context) $ "` | Prevents deploying to the wrong cluster |

---

## **Hands-On Challenge for Students**
**Try these on your Linux VM:**  
✅ **Change the prompt color to blue:**  
```bash
PS1="\[\e[34m\]\u@\h:\w$\[\e[0m\] "
```
✅ **Display the hostname in uppercase:**  
```bash
PS1="\U@\H:\w$ "
```
✅ **Make it permanent by adding it to `.bashrc`:**  
```bash
nano ~/.bashrc
```
*(Add your customized PS1 line, save, and run `source ~/.bashrc`.)*

---

### **Why This Matters for Cloud?**
- Customizing the shell **improves efficiency** in managing servers.  
- Helps **differentiate environments** (dev, staging, production).  
- **Git branch tracking** is a must-have for teams working with CI/CD.  
- **Kubernetes context tracking** prevents costly mistakes in cloud deployments.  
