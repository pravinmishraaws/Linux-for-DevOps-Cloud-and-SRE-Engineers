# Study Notes: Working with File Navigation Commands

## Understanding Linux File Navigation
To properly manage files and directories, we need to understand the **Linux file system hierarchy** and how to navigate through it efficiently using **file navigation commands**.

Before we can move our **website template** to the correct directory inside the **Nginx web server**, we need to determine where the **HTML files** are stored in the Linux file system. To do this, let’s explore the **Linux file system hierarchy**.

---

## Navigating the Linux File System
### Changing Directories with `cd`
The **`cd` (Change Directory)** command allows us to move between different directories in the file system.

#### Syntax:
```bash
cd <directory-name>
```
- If used without arguments (`cd`), it navigates to the **home directory**.
- If given a specific directory name, it moves to that directory.

#### Example:
```bash
cd /var/log/nginx
```
This command navigates to the **Nginx log directory**.

### Checking the Current Directory with `pwd`
The **`pwd` (Print Working Directory)** command displays the absolute path of the current directory.

#### Example:
```bash
pwd
```
**Output:**
```bash
/var/log/nginx
```
This confirms that we are inside the **Nginx log directory**.

---

## Moving to the Root Directory
To navigate to the **root directory**, use:
```bash
cd /
```
This command takes us to the **top-level directory** of the Linux file system.

### Identifying User & Current Location
To check which user is logged in:
```bash
whoami
```
To check the current directory:
```bash
pwd
```
If we are in `/root`, it means we are in the home directory of the root user.
To navigate to the **absolute top-level directory**, use:
```bash
cd /
```
Now, running `pwd` should display:
```bash
/
```
---

## Listing Files and Directories with `ls`
The **`ls`** command is used to list files and directories inside the current directory.

#### Basic Usage:
```bash
ls
```
This lists the contents of the current directory.

#### Using `ls` with Options:
| Command | Description |
|---------|-------------|
| `ls -l` | Displays detailed information (permissions, owner, size, modification date). |
| `ls -a` | Shows **hidden files** (files starting with `.`). |
| `ls -lh` | Displays human-readable file sizes (KB, MB, GB). |
| `ls -R` | Lists files in **all subdirectories** recursively. |

To view hidden files along with detailed information:
```bash
ls -la
```
To see the manual for `ls`, use:
```bash
man ls
```
This provides a list of all available options for `ls`.

---

## Moving Between Directories
### Moving One Level Up
To move **one level up** in the directory structure:
```bash
cd ..
```
This takes us to the **parent directory** of the current location.

### Moving Multiple Levels Up
To move **two levels up**, use:
```bash
cd ../..
```
This moves up two directory levels at once.

### Navigating to a Specific Directory
To go to a specific directory, use:
```bash
cd /var/www/html
```
This takes us directly to the **HTML folder** inside the **web server directory**.

---

## Special Directory References
| Command | Meaning |
|---------|---------|
| `.`     | Represents the **current directory**. |
| `..`    | Represents the **parent directory**. |
| `/`     | Represents the **root directory**. |

To remain in the current directory:
```bash
cd .
```
Although not commonly used, this is helpful when running scripts.

---

## Summary
- The **`cd` command** helps us navigate between directories.
- The **`pwd` command** shows our current location.
- The **`ls` command** lists directory contents and supports various options.
- We explored **moving up and down directory levels** efficiently.
- Special references like `.` (current directory) and `..` (parent directory) help with relative navigation.

Now that we understand **file navigation**, we are ready to move our **website template** into the Nginx web server directory and set up our blog website.


