# **Advanced Process Monitoring with `htop`**  

## **Why Should DevOps and Cloud Engineers Learn `htop`?**  

As a **DevOps or Cloud Engineer**, monitoring system performance is a daily task. Whether you are managing **AWS EC2 instances, Kubernetes nodes, or on-premises Linux servers**, keeping an eye on CPU, memory, and process usage is essential.  

You may already be familiar with `top`, which provides a **real-time view** of system performance. But let’s be honest—`top` is not the most user-friendly tool.  

This is where **`htop`** comes in.  

`htop` is an **interactive and more visual alternative to `top`**, making it easier to:  

- **Monitor system performance in real-time**.  
- **Sort and filter processes** with simple keyboard shortcuts.  
- **Kill or renice processes with a single keypress**.  
- **Use a color-coded interface** to quickly analyze CPU, memory, and load distribution.  

By the end of this lesson, you will be able to:  

- Install and launch `htop`.  
- Navigate the `htop` interface.  
- Filter, sort, and manage processes interactively.  
- Kill or change process priorities easily.  

Let’s start by installing `htop` on your system.  

---

## **Installing `htop`**  

Unlike `top`, which is built into Linux, `htop` needs to be installed separately.  

### **Install `htop` on Debian-based Systems (Ubuntu, Debian, Kali, etc.)**  
```bash
sudo apt update
sudo apt install htop
```

### **Install `htop` on Red Hat-based Systems (RHEL, CentOS, Fedora, Amazon Linux)**  
```bash
sudo yum install htop
```

Once installed, you can verify the installation by running:  
```bash
htop --version
```
If `htop` is installed correctly, this command will display the version of `htop` installed on your system.  

Now that we have installed `htop`, let’s launch it and explore its features.  

---

## **Launching and Navigating `htop`**  

To start `htop`, simply run:  
```bash
htop
```
This opens a **colorful, interactive interface** that looks similar to `top`, but with more details and an easier-to-read layout.  

The `htop` interface is divided into three sections:  

### **1. System Summary (Top Section)**  
Displays system-wide statistics, including:  

| Field | Description |
|-------|------------|
| **CPU Usage** | Shows CPU utilization for each core. |
| **Memory Usage** | Displays total, used, and free RAM. |
| **Swap Usage** | Indicates swap space usage. |
| **Load Average** | Shows system workload over the last 1, 5, and 15 minutes. |
| **Uptime** | Displays how long the system has been running. |

### **2. Process List (Middle Section)**  
Displays all running processes in a structured, scrollable view.  

| Column | Meaning |
|--------|---------|
| **PID** | Process ID (Unique number for each process). |
| **USER** | The user who started the process. |
| **PRI/NICE** | The priority of the process (lower means higher priority). |
| **%CPU** | Percentage of CPU used by the process. |
| **%MEM** | Percentage of memory used by the process. |
| **TIME+** | Total CPU time consumed by the process. |
| **COMMAND** | The name of the process. |

### **3. Function Keys (Bottom Section)**  
Provides a list of shortcut keys to perform various actions.  

Now that we understand the layout of `htop`, let’s see how we can **sort and filter processes** efficiently.  

---

## **Sorting and Filtering Processes in `htop`**  

### **Sorting Processes**  
Unlike `top`, `htop` allows **easy sorting** with function keys.  

1. Press **F6 (Sort by)**.  
2. Use the arrow keys to select a sorting method (e.g., **CPU%, MEM%, PID, TIME+**).  
3. Press **Enter** to apply sorting.  

Alternatively, you can press:  

| Key | Action |
|-----|--------|
| `P` | Sort by CPU usage. |
| `M` | Sort by memory usage. |
| `T` | Sort by running time. |

### **Filtering Processes by Name**  
If you want to focus on a specific process, press:  
```bash
F3 (Search)
```
Then type part of the process name, such as:  
```
nginx
```
This will highlight all processes related to **nginx**.  

Now that we can view and filter processes, let’s move on to **managing them interactively**.  

---

## **Managing Processes with `htop`**  

One of the biggest advantages of `htop` over `top` is that you can manage processes **without typing additional commands**.  

### **Killing a Process in `htop`**  

If a process is consuming too many resources, you can terminate it easily:  

1. Use the arrow keys to **select the process**.  
2. Press **F9 (Kill)**.  
3. Choose the **signal** you want to send:
   - `15` (SIGTERM) – Politely ask the process to terminate.  
   - `9` (SIGKILL) – Forcefully terminate the process.  
4. Press **Enter** to confirm.  

### **Changing Process Priority (Renice a Process)**  

If a background task is using too much CPU, you can **lower its priority** to free up resources for more critical applications.  

1. Select the process using the arrow keys.  
2. Press **F7** to **increase priority** (higher CPU access).  
3. Press **F8** to **decrease priority** (lower CPU access).  

Example Scenario:  
- A **database query is consuming too much CPU** on a cloud server.  
- You reduce its priority so the web server continues to function smoothly.  

Now that we know how to manage processes, let’s learn how to **exit `htop` and troubleshoot common issues**.  

---

## **Exiting `htop`**  

To **quit `htop`**, simply press:  
```bash
q
```
This will return you to the terminal.  

---

## **Troubleshooting Common Issues**  

| Issue | Cause | Solution |
|-------|-------|----------|
| `htop: command not found` | `htop` is not installed | Install it using `sudo apt install htop` (Debian) or `sudo yum install htop` (RedHat). |
| Cannot kill a process | The process is system-critical or permission denied | Use `sudo htop` to gain root privileges. |
| Cannot change process priority | The user doesn’t have permission | Use `sudo renice -n -5 -p PID`. |

---

## **Hands-On Challenges**  

Try the following exercises to practice `htop`:  

1. Open `htop` and sort processes by memory usage.  
2. Find the **Process ID (PID)** of the process using the most CPU.  
3. Kill a process from `htop`.  
4. Change the priority of a process using `htop`.  
5. Filter `htop` to show only processes owned by your user.  

---

## **Key Takeaways**  

- **`htop` is a more user-friendly alternative to `top`** for real-time process monitoring.  
- It provides **an interactive interface** with color-coded CPU and memory usage.  
- Processes can be **sorted and filtered easily** using function keys.  
- **Killing processes and changing their priority** is much simpler in `htop` than in `top`.  
- This tool is **especially useful for DevOps and Cloud Engineers** managing production servers.  

---

## **What’s Next?**  

Now that you have learned how to monitor system processes with `htop`, the next step is:  

- **pidof and pgrep commands**.  
