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
sudo useradd john_cloud
sudo passwd john_cloud
```
📌 Now, `john_cloud` can log in with their password.  

---

## **2. Deleting a User**  

🔹 **Why Delete Users?**  
- **Security measure** when an employee leaves.  
- Prevent **unauthorized access** to critical systems.  

✅ **Command to Delete a User**  
```bash
sudo userdel clouduser
```
📌 This **removes the user but keeps their home directory and files**.  

✅ **Delete a User and Their Home Directory**  
```bash
sudo userdel -r clouduser
```
📌 The `-r` flag **removes the user’s home directory and files**.  

✅ **Example Use Case**  
**Scenario:** A Cloud engineer resigns, and you need to remove their access:  
```bash
sudo userdel -r john_cloud
```
📌 Now, `john_cloud` **cannot access the system** anymore.  

---

## **3. Modifying a User**  

🔹 **Why Modify Users?**  
- To **change user privileges**, group memberships, or account settings.  
- To **lock or unlock** user accounts temporarily.  

✅ **Add a User to a Group**  
```bash
sudo usermod -aG docker clouduser
```
📌 Adds `clouduser` to the `docker` group.  

✅ **Lock a User Account**  
```bash
sudo usermod -L clouduser
```
📌 Prevents `clouduser` from logging in.  

✅ **Unlock a User Account**  
```bash
sudo usermod -U clouduser
```
📌 Allows `clouduser` to log in again.  

✅ **Example Use Case**  
**Scenario:** A Cloud engineer needs to **run Docker containers**. Add them to the `docker` group:  
```bash
sudo usermod -aG docker john_cloud
```
📌 Now, `john_cloud` can run Docker commands **without sudo**.  

---

## **4. Viewing User Information**  

🔹 **Why Check User Details?**  
- To confirm **which user is currently logged in**.  
- To **view user permissions and groups**.  

✅ **Check the Current Logged-in User**  
```bash
whoami
```
📌 Shows the **currently logged-in user**.  

✅ **View User ID (UID) and Group ID (GID)**  
```bash
id clouduser
```
📌 Displays **UID, GID, and group memberships**.  

✅ **List All Users on the System**  
```bash
cat /etc/passwd
```
📌 Shows all **user accounts** on the system.  

✅ **Example Use Case**  
**Scenario:** You need to **check if a user is part of a group** before granting permissions:  
```bash
id john_cloud
```
📌 This verifies **if `john_cloud` belongs to the `docker` group**.  

---

# **Key Takeaways**  

✅ **Create a user** with `useradd` and **set a password** with `passwd`.  
✅ **Delete users** securely with `userdel -r`.  
✅ **Modify users** by adding them to groups or locking/unlocking accounts.  
✅ **Check user details** with `whoami`, `id`, and `/etc/passwd`.  

🎯 **Next Step:** Now that we know how to manage users, let's move to **Groups in Linux!**
