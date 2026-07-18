# Devops-master-journey
# 🐧 Linux Notes – Complete Beginner to Advanced Guide

---

# Page 1 – Linux Practice Tasks

## Task 1 – Find Kernel Version

```bash
uname -r
```

## Task 2 – View Complete Kernel Information

```bash
uname -a
```

## Task 3 – Display Operating System Details

```bash
cat /etc/os-release
```

## Task 4 – Identify Your Current Shell

```bash
echo $SHELL
```

## Task 5 – Locate the `ls` Executable

```bash
which ls
```

## Task 6 – Check Whether `ls` is an ELF Executable

```bash
file /usr/bin/ls
```

## Task 7 – List All Environment Variables

```bash
env
```

## Task 8 – Display CPU Information

```bash
lscpu
```

## Task 9 – Check Memory Usage

```bash
free -m
``

## Task 10 – Inspect Mounted File Systems

```bash
mount | head
```

---

# Page 2 – Operating System Basics

## What is an Operating System?

An **Operating System (OS)** is system software that acts as a bridge between **hardware** and **software**.

It allows users and applications to communicate with computer hardware.

Without an operating system, hardware components cannot work together.

### Responsibilities of an Operating System

- CPU Management
- Memory (RAM) Management
- Disk Management
- Process Management
- Device Driver Management
- File System Management
- Networking
- Security

The operating system manages all hardware and software resources.

---

## Why Linux?

Linux is widely used because it is:

- Open Source
- Secure
- Lightweight
- Stable
- Fast
- Reliable
- Highly Scalable

Most cloud platforms and technology companies use Linux for their infrastructure.

Examples:

- Google
- Amazon
- Microsoft Azure
- Meta
- Netflix
- LinkedIn

---

# Page 3 – Linux Architecture

``
+----------------------+
|     Applications     |
+----------------------+
|   System Libraries   |
+----------------------+
|    System Calls      |
+----------------------+
|       Kernel         |
+----------------------+
|   Device Drivers     |
+----------------------+
| CPU | RAM | SSD/HDD  |
+----------------------+


Every application requests services from the Linux Kernel through **System Calls**.



## Example

``python
file = open("file.txt", "r")
data = file.read()


Flow:


Python Program
      │
      ▼
Python Library
      │
      ▼
System Call
      │
      ▼
Linux Kernel
      │
      ▼
Filesystem Driver
      │
      ▼
SSD / HDD
```

Applications never access hardware directly.

---

# Page 4 – What is the Linux Kernel?

The **Kernel** is the **heart of the Linux Operating System**.

It manages communication between software and hardware.

Without the kernel, the operating system cannot function.

---

## Responsibilities of the Kernel

- CPU Scheduling
- Memory Allocation
- Process Management
- Disk I/O
- Networking
- File System Management
- Security
- Device Driver Management
- Hardware Communication

The kernel decides which process runs, how memory is allocated, and how hardware resources are shared.

--

# User Space vs Kernel Space

## User Space

User Space contains applications such as:

- Chrome
- Firefox
- Python
- Java
- VS Code
- Terminal
- Docker
- Kubernetes CLI

Applications running in User Space **cannot access hardware directly**.

--

## Kernel Space

Kernel Space contains:

- CPU Scheduler
- Memory Manager
- Network Stack
- File System
- Device Drivers
- Security Modules

Only the kernel can directly communicate with hardware.

--

## Architecture

``
+----------------------+
|      User Space      |
| Chrome               |
| Python               |
| Java                 |
| Terminal             |
| Docker               |
+----------------------+
           │
      System Calls
           │
+----------------------+
|     Kernel Space     |
| CPU Scheduler        |
| Memory Manager       |
| Network Stack        |
| File System          |
| Device Drivers       |
+----------------------+
           │
+----------------------+
| Hardware             |
| CPU                  |
| RAM                  |
| SSD/HDD              |
| Network Card         |
+----------------------+
```

--

# Page 5 – System Calls

## What is a System Call?

A **System Call** is a request made by an application to the Linux Kernel.

Examples:

- `open()`
- `read()`
- `write()`
- `close()`
- `fork()`
- `execve()`

Every Linux command uses **hundreds or even thousands of system calls**.

--

## Example

```python
open("file.txt")
```

Flow:

Python
    │
System Library
    │
System Call
    │
Linux Kernel
    │
Filesystem Driver
    │
SSD



# Page 6 – Shell

## What is a Shell?

A **Shell** is a program that allows users to interact with Linux through the command line.

Examples:

- Bash
- Zsh
- Fish
- Sh

---

## What Happens When You Run a Command?

For example:

```bash
ls
```

The Shell performs the following steps:

1. Reads your input.
2. Finds the executable (`/usr/bin/ls`).
3. Creates a new process.
4. Sends a system call to the Linux Kernel.
5. The kernel executes the command.
6. The shell prints the output.
7. Waits for the command to finish.
8. Returns control to the terminal.

---

## Command Execution Flow

User
   │
   ▼
Shell
   │
Read Command
   │
Locate Executable
   │
Create Process
   │
System Call
   │
Linux Kernel
   │
Hardware
   │
Output
   │
Terminal

---

# Quick Revision

| Component | Purpose |
|----------|---------|
| Operating System | Manages hardware and software |
| Kernel | Core of Linux |
| Shell | User interface for Linux |
| System Call | Communication between application and kernel |
| User Space | Applications run here |
| Kernel Space | Hardware management |
| Device Driver | Controls hardware |
| File System | Stores and manages files |
| Process | Running program |
| Memory Manager | Allocates RAM |
| CPU Scheduler | Decides which process runs first |

---

# Author

**Sudheer Sahu**

**DevOps | Linux | AWS | Kubernetes**
