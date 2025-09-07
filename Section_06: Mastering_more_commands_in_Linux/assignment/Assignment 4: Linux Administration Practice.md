# Assignment 4: Linux Administration Practice

## Objective
Your **[React app](https://github.com/pravinmishraaws/my-react-app)** is already deployed on an **Ubuntu VM with Nginx**.  
Now, you will perform a set of **post-deployment administrative tasks** to strengthen your Linux troubleshooting skills.  

This assignment focuses on:
- Networking commands  
- Process monitoring  
- System information  

---

## Step 1: Server Access
Make sure you are logged in to your **Ubuntu EC2 instance** where your React app is running (from the last assignment).  

---

## Step 2: Run Each Task
- Use the **exact commands** provided (or valid alternatives).  
- **Save the command output** as a screenshot.  
- Write a **1–2 sentence explanation** for each command describing what it does and why it’s useful.  

---

## Task 1 – Networking Commands

### Check server network interface configuration
```bash
ifconfig
# (If ifconfig is not installed, install it first)
sudo apt install net-tools
````

### Test connectivity to a website

```bash
ping -c 4 thecloudadvisory.com
```

### Check open ports and listening services

```bash
sudo netstat -tulnp
# If netstat is not installed:
ss -tuln
```

### Perform DNS lookup

```bash
dig pravinmishra.in
# OR
host pravinmishra.in
```

### Download a file using wget

```bash
wget -O /tmp/Untitled-design-40.png https://pravinmishra.in/wp-content/uploads/2023/08/Untitled-design-40.png
```

---

## Task 2 – Process Monitoring & Control

### List all running processes

```bash
ps -e
```

### Search for the nginx process

```bash
ps aux | grep nginx
```

### View process hierarchy

```bash
pstree
```

### Terminate a process (choose a harmless one)

```bash
kill <PID>
# Force kill (use with caution):
kill -9 <PID>
```

---

## Task 3 – System Information

### Display full system information

```bash
uname -a
```

### Check system uptime

```bash
uptime
```

### Display currently logged-in users

```bash
who
```

### Check memory usage

```bash
free -h
```

### Check disk usage

```bash
df -h
```

### Check directory size usage in /var

```bash
sudo du -sh /var/*
```

---

## How to Submit Your Assignment

### For Live Cohort Students

Please follow the assignment submission guidelines explained during the live session.

DevOps Micro Internship [Assignment Master Sheets](https://docs.google.com/spreadsheets/d/1HnlenHEjytvLJMy84bBF-5B1RABaY_BjbfwCj-qnvHM/edit?gid=0#gid=0)

---

## Questions for This Assignment

1. Run the commands exactly as described.
2. Save the **output as screenshots**.

   * Make sure your **name/date (from the proof in App.js)** is visible.
3. Write a **1–2 sentence explanation** for each command:

   * What it does
   * Why it’s useful in a real-world scenario
4. Combine all **screenshots + explanations** into a **single Google Document**.

---

## General Rules

* Create **one Google Doc** for this assignment.
* Add all solutions exactly as requested.
* Get the **View Link** to the document:

  1. Click **Share** (top-right corner).
  2. Under *General access*, change from **Restricted** → **Anyone with the link**.
  3. Set access to **Viewer**.
  4. Click **Copy Link** and paste it in the **Google Form**.
