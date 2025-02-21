# **Managing Users in Linux**  

User management is an essential task for **system administrators and Cloud engineers**. It ensures proper access control, security, and efficient collaboration in a multi-user Linux environment.  

---
  
## **1. Creating a User**  

🔹 **Why Create Users?**  
- Each user should have **separate credentials** to **enhance security**.  
- Needed for **team collaboration on shared servers**.  
- Best practice: **Don’t use root for daily tasks!**  

✅ **Command to Create a New User**  
```bash
sudo useradd clouduser
```
📌 This **creates a user** but does **not set a password**.  

✅ **Set a Password for the User**  
```bash
sudo passwd clouduser
```
📌 It will prompt for a **new password** for `clouduser`.  

🔹 **Verify the User Creation**  
```bash
cat /etc/passwd | grep clouduser
```
📌 The output shows the user’s details like **username, home directory, and shell**.  

✅ **Example Use Case**  
**Scenario:** You want to **add a new Cloud engineer to the team** and give them access to the server.  
```bash
sudo useradd devuser
sudo passwd devuser
```
📌 Now, `devuser` can log in with their password.  

---

## **2. Deleting a User**  

🔹 **Why Delete Users?**  
- **Security measure** when an employee leaves.  
- Prevent **unauthorized access** to critical systems.  

✅ **Command to Delete a User**  
```bash
sudo userdel devuser
```
📌 This **removes the user but keeps their home directory and files**.  

✅ **Delete a User and Their Home Directory**  
```bash
sudo userdel -r devuser
```
📌 The `-r` flag **removes the user’s home directory and files**.  

✅ **Example Use Case**  
**Scenario:** A Cloud engineer resigns, and you need to remove their access:  
```bash
sudo userdel -r devuser
```
📌 Now, `devuser` **cannot access the system** anymore.  

---

## **3. Modifying a User**  

🔹 **Why Modify Users?**  
- To **change user privileges** or account settings.  
- To **lock or unlock** user accounts temporarily.  

✅ **Lock a User Account**  
```bash
sudo usermod -L devuser
```
📌 Prevents `devuser` from logging in.  

✅ **Unlock a User Account**  
```bash
sudo usermod -U devuser
```
📌 Allows `devuser` to log in again.  

✅ **Example Use Case**  
**Scenario:** You need to temporarily **disable an inactive user account**:  
```bash
sudo usermod -L devuser
```
📌 Now, `devuser` **cannot log in** until unlocked.  

---

## **4. Viewing User Information**  

🔹 **Why Check User Details?**  
- To confirm **which user is currently logged in**.  
- To **view user permissions**.  

✅ **Check the Current Logged-in User**  
```bash
whoami
```
📌 Shows the **currently logged-in user**.  

✅ **View User ID (UID) and Group ID (GID)**  
```bash
id devuser
```
📌 Displays **UID, GID, and group memberships**.  

✅ **List All Users on the System**  
```bash
cat /etc/passwd
```
📌 Shows all **user accounts** on the system.  

✅ **Example Use Case**  
**Scenario:** You need to **check user details** before modifying account settings:  
```bash
id devuser
```
📌 This verifies **the user’s UID, GID, and groups**.  

---

# **Key Takeaways**  

✅ **Create a user** with `useradd` and **set a password** with `passwd`.  
✅ **Delete users** securely with `userdel -r`.  
✅ **Modify users** by locking/unlocking accounts.  
✅ **Check user details** with `whoami`, `id`, and `/etc/passwd`.  

🎯 **Next Step:** Now that we know how to manage users, let's explore more Linux administration tasks!

