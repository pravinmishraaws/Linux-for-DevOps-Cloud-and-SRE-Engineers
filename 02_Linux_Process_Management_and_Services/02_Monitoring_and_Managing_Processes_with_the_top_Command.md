# **Monitoring and Managing Processes with the `top` Command**  

## **Why Should DevOps and Cloud Engineers Learn the `top` Command?**  

Imagine you are a **DevOps engineer** managing a cloud server running multiple applications. One day, you receive an alert—your server’s CPU usage is at 95%, and your application is responding slowly.  

What could be the problem?  

- A **background process** might be consuming too much CPU.  
- A **memory leak** might be causing your system to slow down.  
- A **stuck process** might be preventing other applications from running smoothly.  

You need a way to **quickly analyze system performance, identify the problem, and take action** without restarting the server.  

This is where the **`top` command** becomes an essential tool for **DevOps engineers, cloud engineers, and system administrators**.  

By the end of this lesson, you will be able to:  

- **Monitor system performance** in real-time using `top`.  
- **Sort and filter processes** to find the most resource-intensive ones.  
- **Kill an unresponsive process** directly from `top`.  
- **Change process priority** to control system performance.  

Let’s start by launching `top` and understanding its output.  

---

## **Launching the `top` Command**  

The `top` command provides a **real-time view of running processes**, displaying CPU and memory usage, system load, and other useful details.  

To start `top`, run:  
```bash
top
```
You will see an interface similar to this:  
```
top - 10:30:15 up 1 day,  4:30,  2 users,  load average: 0.58, 0.75, 0.80
Tasks: 210 total,   2 running,  208 sleeping,   0 stopped,   0 zombie
%Cpu(s):  2.0 us,  1.0 sy,  0.0 ni, 96.0 id,  0.5 wa,  0.2 hi,  0.3 si,  0.0 st
KiB Mem :  8000000 total,  4200000 free,  1000000 used,  2800000 buff/cache
KiB Swap:  2000000 total,  1900000 free,   100000 used.  580000 avail Mem
```
Below this, a **list of running processes** is displayed.  

**Note:** type Shift 0 (Zero) to clear filter

Now that we have launched `top`, let’s understand how to interpret its output.  

---

## **Understanding the `top` Output**  

When `top` is running, it shows two key sections:  

### **1. System Summary (First Few Lines)**  
This section provides an overview of the system’s health.  

| Field | Description |
|-------|------------|
| **Load Average** | The system’s workload over the last 1, 5, and 15 minutes. |
| **Tasks** | Shows the number of total, running, sleeping, stopped, and zombie processes. |
| **%CPU(s)** | Displays CPU usage by user processes, system processes, and idle time. |
| **Memory Usage (KiB Mem)** | Shows total, free, and used memory. |
| **Swap Usage (KiB Swap)** | Displays swap memory usage details. |

### **2. Process List (Below the System Summary)**  
Each row represents a running process with important details:  

| Column | Meaning |
|--------|---------|
| **PID** | Process ID (Unique number assigned to each process). |
| **USER** | The user who owns the process. |
| **%CPU** | CPU usage percentage. |
| **%MEM** | Memory usage percentage. |
| **TIME+** | Total CPU time the process has used. |
| **COMMAND** | The name of the process. |

### **Real-World Example:**
A **cloud engineer** working on an AWS EC2 instance might use `top` to check if an **autoscaling policy** needs adjustment. If the CPU usage remains high for an extended period, increasing the **instance count** might be necessary.  

Now that we understand how to read the `top` output, let’s learn how to sort and filter processes to **find resource-heavy applications**.  

---

## **Sorting and Filtering Processes in `top`**  

By default, `top` sorts processes by **CPU usage**, but you can change this to sort by **memory usage, process ID, or running time**.  

### **Sorting Processes**  
While `top` is running, press the following keys to sort the list:  

| Key | Action |
|-----|--------|
| `P` | Sort by CPU usage (default). |
| `M` | Sort by memory usage. |
| `N` | Sort by Process ID (PID). |
| `T` | Sort by running time. |

### **Filtering Processes**  
If you are managing a cloud environment with multiple applications running, it might be helpful to filter the processes.  

1. Press `o` while in `top`.  
2. Type the filter criteria, such as:  
   ```
   USER=azureus
   ```
   This will only show processes started by the **devops** user.  

Now that we know how to view and filter processes, let’s move on to **killing an unresponsive process**.  

---

## **Killing a Process from `top`**  

If a process is consuming too many resources, you might need to **terminate it**.  

To **kill a process in `top`**:  

1. Press `k`.  
2. Enter the **Process ID (PID)** of the process you want to kill.  
3. Press `Enter`.  

### **Example Scenario: Identifying and Terminating a High-CPU Process**  

#### **Background**  
As a **DevOps Engineer**, you are managing a Kubernetes cluster. One of your containerized applications is consuming excessive CPU, affecting the performance of other services. Instead of restarting the entire cluster, you need to **identify the faulty process and terminate it** efficiently.

To help students understand this scenario practically, let's simulate a high-CPU process and then manage it using `top`.


### **Step 1: Start a Sample High-CPU Process**  
To simulate a process that consumes CPU, run the following command:
```bash
dd if=/dev/zero of=/dev/null &
```
#### **What This Does**  
- `dd` is a command that copies data.  
- `if=/dev/zero` reads an endless stream of zeros.  
- `of=/dev/null` discards the data, keeping the process running indefinitely.  
- The `&` runs it in the background, so it doesn’t block the terminal.

Verify that the process is running:
```bash
ps aux | grep dd
```

### **Step 2: Identify the High-CPU Process Using `top`**  
Run `top` to check active processes:
```bash
top
```
Inside `top`:
- Look for a process named `dd` in the **COMMAND** column.
- Note its **PID** (Process ID) under the **PID** column.

### **Step 3: Kill the Process Using `kill`**  
If you already know the PID, you can terminate the process from the terminal:

```bash
kill <PID>
```
For example:
```bash
kill 3456
```
If the process doesn’t stop, force termination using:
```bash
kill -9 3456
```

Alternatively, you can **kill the process directly inside `top`**:
1. Press **k** while `top` is running.
2. Enter the **PID** of the process.
3. Press **Enter** to terminate it.

### **Step 4: Verify That the Process Has Stopped**  
After terminating the process, check if it is still running:
```bash
ps aux | grep dd
```
If no output appears, the process has been successfully terminated.


Now, let’s learn how to **change the priority of processes**.  

---

## **Changing Process Priority with `top`**  

Not all processes need equal CPU time. Some critical tasks (e.g., a **database query**) may need **higher priority**, while background tasks (e.g., **log processing**) should have **lower priority**.  

Linux assigns a **priority level** to every process, called the **nice value**.  

- A **lower nice value** means **higher priority** (-20 is the highest priority).  
- A **higher nice value** means **lower priority** (19 is the lowest priority).  

### **Changing Priority in `top`**  

1. Press `r` while `top` is running.  
2. Enter the **Process ID (PID)** of the process you want to change.  
3. Enter a new priority value (-20 to 19).  

Example:  
- Press `r`.  
- Enter `3456` (PID of the process).  
- Enter `5` to lower its priority.  

Now, let’s see how to exit `top`.  

---

## **Exiting `top`**  

To close `top`, simply press:  
```bash
q
```
This will return you to the terminal.  

---

## **Troubleshooting Common Issues**  

| Issue | Cause | Solution |
|-------|-------|----------|
| `top` command not found | `top` is missing on the system | Install it using `sudo apt install procps` (Ubuntu) or `sudo yum install procps-ng` (RHEL). |
| Cannot kill a process | The process is system-critical or permission denied | Use `sudo kill -9 PID`. |
| Cannot change process priority | The user doesn’t have permission | Use `sudo renice -n -5 -p PID`. |

---

## **Hands-On Challenges**  

Try the following exercises to practice `top`:  

1. Open `top` and sort processes by memory usage.  
2. Find the **Process ID (PID)** of the process using the most CPU.  
3. Kill a process from `top`.  
4. Change the priority of a process using `top`.  
5. Filter `top` to show only processes owned by your user.  

---

## **Key Takeaways**  

- The `top` command provides **real-time monitoring** of system performance.  
- Processes can be **sorted** by CPU, memory, process ID, or runtime.  
- You can **kill processes directly from `top`** by pressing `k` and entering a PID.  
- Process priority can be changed with `r` to **increase or decrease CPU allocation**.  

This is an essential skill for **DevOps and Cloud Engineers** working in **high-availability environments**.  

---

## **What’s Next?**  

Now that you know how to monitor system processes, the next step is:  

- Using **`htop` for a more interactive process viewer**.  
- **Automating process monitoring** to detect performance issues.  
- **Using system logs** to investigate why a process failed.  

Let’s move forward and explore more advanced monitoring tools.
