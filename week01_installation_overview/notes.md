# 🐧 Linux Journey — Spring 2025

Welcome to my **Linux Learning Repository** for the Spring 2025 semester.  
This repository documents everything I learn week-by-week — from **installation** to **system administration**, **Bash scripting**, and **process management**.

I’m using **WSL (Windows Subsystem for Linux)** with **Ubuntu** as my primary environment throughout this course.

---

## 🗓 Course Roadmap (University Outline)

| Week | Topic |
|------|--------|
| 1 | Installation and Overview |
| 2 | Commands (basic and file related) |
| 3 | More Commands |
| 4 | Users, Groups and Privileges |
| 5 | Introduction to Bash and Scripting |
| 6 | Structures and Programs |
| 7 | Administration tasks in Unix |
| 8 | Mid-term Examination |
| 9 | Permissions and Shared Directories |
| 10 | Processes and SJF algorithm |
| 11 | Processes and FCFS algorithm |
| 12 | Process Management and Scheduling |
| 13 | Packages in UNIX & CentOS 6 systems |
| 14 | Networking in CentOS 6 systems |
| 15 | Project Task I |

---

# 🧠 Week 01 — Installation & Overview

## 🎯 Objective
Understand what Linux is, how it differs from Windows, and set up a working Linux environment for the semester.

---

## 💻 What is Linux?

- **Linux** is an **open-source operating system kernel**.  
- A **Linux distribution (distro)** like Ubuntu, Fedora, or CentOS is a complete OS built on top of that kernel.  
- These distros package the Linux kernel + system utilities + software managers + optional GUI.

| Concept | Description |
|----------|--------------|
| **Kernel** | The core of the operating system — controls hardware, CPU, memory, I/O, etc. |
| **Distribution** | A “flavor” or version of Linux built using the Linux kernel (e.g., Ubuntu, CentOS). |
| **Shell / Terminal** | Interface for communicating with the system through commands. |

---

## 🪟 Windows vs Linux (Quick Comparison)

| Feature | Windows | Linux |
|----------|----------|--------|
| **Source** | Closed (Microsoft) | Open-source |
| **File System** | NTFS | ext4, XFS, etc. |
| **Command Line** | CMD / PowerShell | Bash / Zsh |
| **Software Management** | Installers (.exe) | Package Managers (apt, yum, dnf) |
| **Cost** | Paid (licensed) | Free |

---

## 🧰 What is Ubuntu?

Ubuntu is one of the most popular and beginner-friendly **Linux distributions**, built on Debian.  
It’s perfect for developers, sysadmins, and students.

It includes:
- Bash shell  
- `apt` package manager  
- Pre-installed core tools  
- Optional GUI support  

---

## 🧩 What is WSL (Windows Subsystem for Linux)?

**WSL** allows you to run a **full Linux environment inside Windows** — no need for dual boot or a virtual machine.

---

### 🔧 Steps to Install WSL + Ubuntu

1. Open **PowerShell (Admin mode)**  
2. Run:
   ```bash
   wsl --install
   ```
   This installs the latest Ubuntu automatically.
3. Restart your PC  
4. Search **Ubuntu** in the Start menu and open it  
5. Set up your **username** and **password** (Linux user)  
6. Verify installation:
   ```bash
   lsb_release -a
   uname -r
   ```
7. Update your system:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

---

## 🧭 Folder Navigation in Linux

| Command | Description |
|----------|-------------|
| `pwd` | Show current directory |
| `ls` | List files/folders |
| `cd <path>` | Change directory |
| `mkdir <dir>` | Create folder |
| `rmdir <dir>` | Remove folder |
| `touch <file>` | Create file |
| `rm <file>` | Delete file |

---

## 🧩 Where is Linux Actually Running?

When you use WSL, your setup looks like this:

```
Windows OS
    ↓
WSL (Subsystem Layer)
    ↓
Ubuntu (Linux Distro)
    ↓
Bash Shell
```

You are inside a **real Linux environment**, but hosted by Windows.

### 📂 Accessing Files Between Systems

- Access **Linux files from Windows**:
  ```
  \\wsl$\Ubuntu\home\<username>
  ```
- Access **Windows files from Linux**:
  ```bash
  /mnt/c/
  ```

---

## ⚙️ Optional: Using a GUI (Visual Linux)

If you ever want a GUI:
- Install **Windows Terminal** or the **Ubuntu App** from Microsoft Store for a smoother interface.  
- You can install a lightweight GUI like **XFCE** later — optional.

---

## 🧠 Why I Chose WSL

✅ No need to partition or dual boot  
✅ Fast startup, low resource usage  
✅ Seamless access between Windows and Linux files  
✅ Works perfectly for commands, users, groups, scripting, and admin tasks  
✅ Compatible with **VS Code** (edit Linux files directly)

---

## 🚫 Limitations of WSL

| Limitation | Explanation |
|-------------|--------------|
| **No Full Kernel Control** | You can’t modify or rebuild the kernel directly (rarely needed). |
| **Networking Differences** | Some advanced network setups may behave slightly differently than full Linux. |
| **GUI Tools** | Some graphical Linux applications won’t run unless display support is configured. |

---

## 🧩 Summary

You’ve now:
- Installed **WSL + Ubuntu**
- Understood what **Linux**, **kernel**, and **distributions** mean
- Learned how to **navigate directories**
- Linked **Windows ↔ Linux** file access
- Understood **why WSL is ideal** for learning Linux

