# **Monitoring and Controlling Processes in Linux**

## **Introduction**
Process management in Linux is crucial for **monitoring, controlling, and optimizing** running processes. Every task running on a Linux system—whether started by a user or the system itself—is a **process**.

In this lesson, we will cover:
- **Viewing active processes** with `ps` and `top`
- **Identifying and filtering processes**
- **Terminating processes using `kill`**
- **Displaying process hierarchy with `pstree`**

By the end, you will understand how to effectively manage processes for **system stability and performance optimization**.

---

## **1. Understanding Linux Processes**
### **What is a Process?**
A **process** is an instance of a running program. Linux allows multiple processes to run simultaneously, and they can be categorized as:
- **Foreground processes**: Directly interact with the user (e.g., text editors).
- **Background processes (daemons)**: Run without direct user interaction (e.g., Nginx web server).

Each process has a **Process ID (PID)**, a unique number assigned by the system.

---

## **2. Viewing Running Processes with `ps`**
The `ps` (Process Status) command provides a **snapshot** of active processes.

### **Basic Syntax:**
```bash
ps [options]
```

### **Example: Listing All Running Processes**
```bash
ps -e
```
This displays every running process along with:
- **PID** (Process ID)— A unique number assigned by the operating system to identify the process.
- **TTY** (Terminal type)— Shows the terminal associated with the process (e.g., pts/0). If it says ?, the process isn’t connected to a terminal (often system/background processes).
- **TIME** (CPU time used)— The total amount of CPU processing time the process has consumed since it started, shown as minutes:seconds (e.g., 00:01).
- **CMD** (Command that started the process)— The command or program name that started the process. For example, bash, sshd, or nginx.

### **Filtering Specific Processes**
To find details of a specific process (e.g., `nginx`):
```bash
ps aux | grep nginx
```
Here:
- `ps aux` → Displays all processes with detailed information.
- `| grep nginx` → Filters output to show only `nginx` processes.

### **Understanding the Output**
| Column | Description |
|--------|-------------|
| **USER** | Owner of the process |
| **PID** | Unique process ID |
| **%CPU** | CPU usage |
| **%MEM** | Memory usage |
| **VSZ** | Virtual memory used (KB) |
| **RSS** | Resident memory (KB) |
| **TTY** | Terminal associated with the process |
| **STAT** | Process state (`R`=Running, `S`=Sleeping, `Z`=Zombie) |
| **START** | Time the process started |
| **TIME** | Total CPU time used |
| **CMD** | Command used to start the process |

### **Viewing Only Active (Running) Processes**
```bash
ps -r
```
This filters out processes that are sleeping or inactive.

---

## **3. Monitoring Processes in Real-Time with `top`**
The `top` command provides a **real-time** view of system processes.

### **Syntax:**
```bash
top
```
This command continuously updates the process list, displaying:
- CPU usage
- Memory usage
- Load averages
- Running, sleeping, and stopped processes

### **Understanding `top` Output**
| Column | Description |
|--------|-------------|
| **PID** | Process ID |
| **USER** | Owner of the process |
| **PR** | Priority |
| **NI** | Niceness value (priority modifier) |
| **VIRT** | Virtual memory used |
| **RES** | Physical memory used |
| **SHR** | Shared memory used |
| **S** | Process state (`R`=Running, `S`=Sleeping) |
| **%CPU** | CPU usage |
| **%MEM** | Memory usage |
| **TIME+** | Total CPU time used |
| **COMMAND** | Command that started the process |

### **Navigating `top`**
- Press `Q` to exit.
- Press `M` to sort by memory usage.
- Press `P` to sort by CPU usage.

---

## **4. Terminating Processes with `kill`**
The `kill` command **stops** a process by sending a termination signal.

### **Syntax:**
```bash
kill [signal] PID
```
Where:
- `PID` is the Process ID.
- `[signal]` is optional (default is `SIGTERM`).

### **Example: Gracefully Stopping a Process**
```bash
kill 3742
```
This **requests** the process to stop.

### **Forcefully Terminating a Process (`SIGKILL`)**
If a process does not terminate with `kill`, use:
```bash
kill -9 3742
```
Here, `-9` (SIGKILL) **forces** termination.

---

## **5. Displaying Process Hierarchy with `pstree`**
The `pstree` command **visually represents** parent-child relationships between processes.

### **Syntax:**
```bash
pstree
```
This displays all processes in a tree structure.

### **Filtering for a Specific Process**
To locate a process like `nginx`:
```bash
pstree -a | grep nginx
```
This reveals whether `nginx` has child processes.

---

## **6. Summary of Process Management Commands**
| Command | Description |
|---------|-------------|
| `ps -e` | List all running processes |
| `ps aux | grep process` | Search for a specific process |
| `top` | Monitor processes in real-time |
| `kill PID` | Terminate a process gracefully |
| `kill -9 PID` | Forcefully kill a process |
| `pstree` | Display process hierarchy |

---

## **Next Steps**
Now that you understand **monitoring and controlling processes in Linux**, the next lesson will cover **Understanding networking commands in Linux**.
