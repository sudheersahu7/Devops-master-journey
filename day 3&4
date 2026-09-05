oday we are starting one of the most important Linux topics for a DevOps Engineer.

If you truly understand today's topic, you'll understand:

Why Kubernetes Pods are actually Linux processes
How Docker creates containers
Why zombie processes happen
Why PID 1 is important
How signals work (SIGTERM, SIGKILL)
Why Kubernetes first sends SIGTERM before SIGKILL
Why docker stop behaves differently from docker kill

This topic is the foundation for Docker, Kubernetes, systemd, and Linux troubleshooting.

Module 1 – Advanced Linux
Day 3 – Linux Processes & Process Management
Today's Goal

By the end of today, you should understand:

What is a process?
Program vs Process
Process Lifecycle
Process States
Process Control Block (PCB)
Parent & Child Processes
fork()
exec()
Process IDs (PID, PPID)
Process Scheduling
Process Priority
Context Switching
Signals
Zombie Processes
Orphan Processes
Process Monitoring
Process Troubleshooting
What is a Program?

A program is just a file stored on disk.

Example:

/usr/bin/python3

/usr/bin/ls

/usr/bin/nginx

A program is inactive.

It doesn't consume:

CPU
RAM
Network
File Descriptors

It is just code on storage.

What is a Process?

A process is a running instance of a program.

Example

Program

/usr/bin/python3

Running

python3 app.py

Now it becomes

Process

which consumes

CPU
Memory
PID
File descriptors
Stack
Heap
Interview Question

Difference between Program and Process?

Program	Process
Static	Running
Stored on Disk	Stored in RAM
No PID	Has PID
Doesn't Execute	Executes
Doesn't use CPU	Uses CPU
Process Lifecycle

A process doesn't simply start and stop.

It goes through several stages.

New

↓

Ready

↓

Running

↓

Waiting

↓

Running

↓

Terminated

Let's understand each one.

1. New

The operating system creates a process.

Resources are allocated.

2. Ready

The process is waiting for CPU.

Imagine

100 Chrome tabs

Only one CPU.

All processes wait.

3. Running

CPU executes instructions.

4. Waiting

The process waits for

Disk
Network
User input
Database
Sleep()

CPU switches to another process.

5. Terminated

Process exits.

Memory is released.

Process States in Linux

Check using

ps aux

You'll see states like

R

S

D

Z

T
R

Running

Currently using CPU.

S

Sleeping

Waiting for something.

Most Linux processes stay here.

D

Uninterruptible Sleep

Usually waiting for disk I/O.

Very important.

These processes cannot even be killed easily.

Z

Zombie

Dead process.

Parent hasn't collected exit status.

We'll learn this later today.

T

Stopped

Example

Ctrl + Z

Process IDs

Every process has

PID

Example

echo $$

Current shell PID.

Parent PID

ps -ef

Look for

PID

PPID

Example

systemd

↓

bash

↓

python

↓

child process
PID 1

Always remember.

systemd

PID

1

Every orphan process eventually becomes a child of PID 1.

Interviewers love this question.

Process Tree

View

pstree

or

pstree -p

Example

systemd

├── sshd

│      └── bash

│              └── python

└── docker
Process Control Block (PCB)

Every process has a PCB.

Think of it as the kernel's record for that process.

Contains

PID
Process State
Registers
Memory info
Open files
Priority
Scheduling info

The kernel uses the PCB during context switching.

fork()

This is one of the most important Linux concepts.

When a process wants another process

it calls

fork();

What happens?

Parent

↓

fork()

↓

Parent

Child

The child is almost an exact copy of the parent.

exec()

Usually after fork()

the child calls

exec();

Now

instead of copying

it loads a new program.

Example

bash

↓

fork()

↓

child

↓

exec(ls)

↓

ls

This is exactly what happens when you type

ls
Internal Flow of ls
You type ls

↓

bash

↓

fork()

↓

child process

↓

execve("/usr/bin/ls")

↓

Kernel loads ls

↓

ls executes

↓

Exit

↓

Parent continues

This is a common interview question.

Process Scheduling

Imagine

1000 processes

Only

8 CPU cores.

Who decides?

The

Linux Scheduler

It decides

who runs
for how long
when to switch
Context Switching

CPU cannot run every process simultaneously.

It rapidly switches.

Chrome

↓

Python

↓

Docker

↓

Nginx

↓

Java

↓

Chrome

The switch happens in milliseconds.

This is

Context Switching.

Nice Value

Linux priority

Check

top

or

ps -el

Priority

-20

↓

19

Lower value

Higher priority

Change priority

nice -n 10 python app.py

Existing process

renice
Signals

Processes communicate using signals.

Examples

SIGTERM

SIGKILL

SIGSTOP

SIGCONT

SIGHUP

SIGINT
SIGTERM (15)

Polite request.

"Please stop."

Process can clean resources.

Kubernetes sends this first.

SIGKILL (9)

Immediate kill.

Cannot be ignored.

Kernel removes process.

SIGINT (2)

Ctrl + C

SIGSTOP

Pause process.

SIGCONT

Resume process.

kill Command
kill PID

Actually sends

SIGTERM

Force kill

kill -9 PID

Sends

SIGKILL
Zombie Process

One of the favorite interview questions.

Imagine

Parent creates child.

Child finishes.

Parent never collects exit status.

Child becomes

Zombie.

State

Z

Zombie

Uses

almost no memory

but occupies

a PID.

Orphan Process

Parent dies.

Child still running.

Kernel assigns

Parent

↓

systemd (PID 1)

Systemd adopts it.

Why Kubernetes Uses SIGTERM First?

Pod deletion

↓

SIGTERM

↓

30 second grace period

↓

Application cleans resources

↓

Close DB

↓

Finish requests

↓

Save logs

↓

Still alive?

↓

SIGKILL

Now you understand why.

Practical Labs

Complete all of these.

Lab 1

View processes

ps -ef
Lab 2

Monitor processes

top
Lab 3

Show process tree

pstree -p
Lab 4

Current shell PID

echo $$
Lab 5

Find PID 1

ps -p 1 -f
Lab 6

Start a background process

sleep 300 &

Check it

ps -ef | grep sleep
Lab 7

Send SIGTERM

kill PID

Observe that the process exits cleanly.

Lab 8

Start another sleep 300 process and send SIGKILL:

kill -9 PID

Compare the behavior with SIGTERM.

Lab 9

Observe priorities

ps -el

and

top

Look for the NI (nice) and PR (priority) columns.

Lab 10

Inspect the process hierarchy:

pstree -p

Find your shell, then identify its parent and one child process that it creates.

Homework
Draw the complete lifecycle of a Linux process.
Explain the difference between a program and a process.
Explain fork() and exec() with a diagram.
Explain the difference between SIGTERM and SIGKILL.
Explain what a zombie process is.
Explain what an orphan process is.
Update your 01-linux/README.md with today's notes.
Create an incident-report.md describing a process that would not terminate with SIGTERM and how you would investigate and resolve it.
Interview Questions

Try to answer these without looking up the answers:

What is the difference between a process and a thread?
What is a Process Control Block (PCB)?
What happens internally when you type ls?
What is the purpose of fork()?
What is the purpose of exec()?
Why is systemd always PID 1?
What is a zombie process, and how do you remove it?
What is an orphan process?
What is the difference between kill, kill -15, and kill -9?
Why does Kubernetes send SIGTERM before SIGKILL?
Extra Challenge (Recommended)

Since you're aiming for a senior-level understanding, after completing the labs, run:

strace ls

Watch the system calls made by ls (execve, openat, getdents64, write, etc.). Don't worry if every line doesn't make sense yet—we'll study strace in detail later. For now, simply observe how a simple command interacts with the Linux kernel.

Tomorrow (Day 4) we'll dive into Threads, CPU Scheduling, Memory Layout, Virtual Memory, Stack vs Heap, Context Switching, and CPU Affinity. Understanding these topics will make Docker and Kubernetes resource management much easier later in the course.

Today 5:14 PM
day 4
🚀 Day 4 — Linux Threads, CPU Scheduling & Memory

Good progress. Day 1–3 gave you the foundation:

Day 1: OS, Kernel, User Space, System Calls
Day 2: Linux Boot Process, systemd, PID 1
Day 3: Processes, fork(), exec(), signals, zombies/orphans

Today we go one level deeper.

For your 3-year DevOps Engineer goal, today's topics are extremely important because later they directly connect to:

Docker → Kubernetes → CPU limits → Memory limits → OOMKilled → JVM/Python performance → node troubleshooting.

🎯 Day 4 Objectives

Today you will understand:

Process vs Thread
Linux threads
CPU cores
CPU scheduling
Context switching
CPU affinity
Process priority
Stack vs Heap
Virtual memory
Physical memory
Page
Page fault
Swap
OOM Killer
How these concepts relate to Kubernetes

We'll do theory first, then practical labs.

1. Process vs Thread

You already learned:

A process is a running instance of a program.

But a process can contain multiple threads.

Think:

Process
│
├── Thread 1
├── Thread 2
├── Thread 3
└── Thread 4

Threads are the actual units that execute instructions.

Example

Suppose you run:

python app.py

You have:

Process
│
├── Main Thread
├── Worker Thread
├── Worker Thread
└── Worker Thread

The threads belong to the same process and share many resources.

Process Memory

A process generally has:

Process
│
├── Code
├── Data
├── Heap
├── Shared Libraries
└── Stack

Threads inside the same process share things like:

Code
Heap
Global Data
Open Files

But each thread has its own:

Stack
Registers
Program Counter

This distinction is extremely important.

Process vs Thread
Process	Thread
Independent execution environment	Execution unit within a process
Has its own virtual address space	Shares address space with threads in same process
More expensive to create	Generally cheaper to create
Process isolation	Less isolation
Communication can require IPC	Threads can share memory
2. Why Do We Need Threads?

Imagine a web server receives:

Request 1
Request 2
Request 3
Request 4

A single-threaded application might process them sequentially:

Request 1
   ↓
Request 2
   ↓
Request 3
   ↓
Request 4

A multi-threaded application can have:

Thread 1 → Request 1

Thread 2 → Request 2

Thread 3 → Request 3

Thread 4 → Request 4

This can improve concurrency, particularly when work spends time waiting for I/O.

3. CPU Cores

Suppose your server has:

lscpu

and:

CPU(s): 16
Core(s) per socket: 8
Thread(s) per core: 2

You have:

8 physical cores
16 logical CPUs

because of SMT/Hyper-Threading.

Important

Don't automatically assume:

16 logical CPUs = 16 physical cores

They're different concepts.

4. CPU Scheduling

Suppose you have:

CPU = 4 cores

Processes = 100

Obviously all 100 processes cannot execute simultaneously on four cores.

Linux uses a scheduler to decide which runnable tasks get CPU time.

Conceptually:

          Linux Scheduler
                 │
       ┌─────────┼─────────┐
       ↓         ↓         ↓
    Process A Process B Process C
       │
       ↓
      CPU

The scheduler repeatedly selects runnable tasks.

5. Context Switching

Suppose CPU is running:

Process A

Then scheduler decides:

Process B should run now.

The CPU state for A has to be preserved and B's state restored.

Conceptually:

Process A
   │
   │ save state
   ↓
Scheduler
   │
   │ restore state
   ↓
Process B

This is a context switch.

Why Context Switching Has Cost

The CPU has to maintain execution state.

Too many runnable tasks can create scheduling overhead.

Therefore:

More processes/threads does NOT automatically mean better performance.

This becomes important when you configure:

WORKERS=512

on a server.

More workers can actually make the application slower if the workload and CPU cannot support them.

6. CPU Affinity

You can control which CPUs a process is allowed to run on.

Check:

taskset -p PID

Run a process on CPU 0:

taskset -c 0 sleep 300

Now that process is restricted to CPU 0.

Why DevOps Engineers Care

CPU affinity can be useful for:

High-performance applications
Databases
Networking workloads
Latency-sensitive workloads
Some Kubernetes workloads

But don't use CPU pinning blindly.

7. Process Priority

Linux has a nice value.

Check:

ps -eo pid,ni,pri,comm

Typical nice range:

-20 → highest priority
  0 → normal
+19 → lowest priority

Example:

nice -n 10 python3 app.py

The process starts with a nice value of 10.

8. Stack vs Heap

This is another very important topic.

A process's memory contains different regions.

Simplified:

High Address
┌─────────────────┐
│      Stack      │
├─────────────────┤
│                 │
│      ...        │
│                 │
├─────────────────┤
│      Heap       │
├─────────────────┤
│   Data / BSS    │
├─────────────────┤
│      Code       │
└─────────────────┘
Low Address
Stack

Used for things such as:

Function calls
Local variables
Function arguments
Return information

Example:

def calculate():
    x = 10
    y = 20

Function-call-related state is associated with the thread's stack.

Each thread has its own stack.

Heap

Used for dynamically allocated memory.

For example, applications can allocate objects dynamically.

In Python:

data = []

The object is managed in dynamically allocated memory.

The exact memory behavior of Python is more complicated because CPython has its own memory-management mechanisms, but the key concept is:

Heap is used for dynamically allocated application data.

9. Virtual Memory

This is extremely important for DevOps.

Applications don't normally work directly with physical RAM addresses.

Instead, each process gets a virtual address space.

Think:

Process A
Virtual Memory
     │
     ▼
Physical RAM

Process B
Virtual Memory
     │
     ▼
Physical RAM

The kernel and CPU's memory-management hardware translate virtual addresses to physical addresses.

Why Virtual Memory?

It provides:

Isolation

Process A shouldn't normally access Process B's memory.

Flexibility

Processes can have large virtual address spaces.

Memory management

The OS can manage physical memory efficiently.

10. Pages

Memory is divided into fixed-size blocks called pages.

A common page size is:

4 KB

although systems can use other sizes.

Physical memory is similarly managed in page frames.

Page Fault

Suppose a process accesses a virtual page that isn't currently mapped to usable physical memory.

The CPU triggers a page fault.

The kernel handles it.

Depending on the situation, the kernel might:

establish a mapping,
load data from storage,
perform other memory-management work,

or determine that the access is invalid and terminate the process.

Important

Not every page fault means something is wrong.

There are different types of page faults.

Some are normal parts of virtual memory operation.

11. Swap

Suppose RAM becomes heavily utilized.

Linux can use swap space.

RAM
 ↓
Pressure
 ↓
Swap

Swap can be:

A partition
A file

But swap is much slower than RAM.

Therefore:

Swap is not a replacement for RAM.

12. OOM Killer

Now one of the most important concepts for Kubernetes.

Suppose:

RAM = 8 GB

Applications want = 12 GB

Eventually the system may reach a severe memory shortage.

Linux has an Out-Of-Memory (OOM) mechanism that can kill processes to recover memory.

You may see:

Out of memory
Killed process ...

Check kernel messages:

dmesg | grep -i oom

or:

journalctl -k | grep -i oom
Kubernetes Connection

This is where today's lesson becomes extremely useful.

Suppose:

resources:
  limits:
    memory: "512Mi"

and your container exceeds its memory limit.

Kubernetes/container runtime/cgroup mechanisms can cause the process to be terminated, commonly resulting in:

OOMKilled

Then:

kubectl describe pod POD_NAME

may show:

Reason: OOMKilled

So when you see:

OOMKilled

don't just say:

Increase memory.

Think:

Application
     ↓
Memory allocation
     ↓
cgroup memory constraint
     ↓
Memory pressure / limit exceeded
     ↓
Process killed
     ↓
Container exits
     ↓
Kubernetes restarts it

We'll study cgroups deeply when we reach Docker/Kubernetes internals.

13. Check Memory

Run:

free -h

You'll see something like:

               total   used   free
Mem:            ...
Swap:           ...

Don't assume:

used = bad

Linux intentionally uses available memory for caching.

We'll go much deeper into this later.

14. Check Process Memory

Run:

ps aux --sort=-%mem | head

This shows processes consuming significant memory.

CPU:

ps aux --sort=-%cpu | head
15. /proc — Very Important

Linux exposes kernel/process information through:

/proc

For example:

cat /proc/cpuinfo

Memory:

cat /proc/meminfo

Process:

cat /proc/PID/status

Command line:

cat /proc/PID/cmdline

Open files:

ls -l /proc/PID/fd

This is extremely useful for production troubleshooting.

🔥 Practical Lab — Day 4

Now let's do actual engineering practice.

Lab 1 — CPU Information

Run:

lscpu

Record:

CPU(s)
Core(s) per socket
Thread(s) per core
Socket(s)

Then explain what each means.

Lab 2 — Process Threads

Run:

ps -eLf | head -20

Important columns include:

PID
PPID
LWP
NLWP

NLWP represents the number of threads in the process.

Find a process with multiple threads:

ps -eLf | sort -k4 -nr | head
Lab 3 — Thread View in top

Run:

top

Then press:

H

This toggles thread display.

Observe the difference.

Lab 4 — CPU Affinity

Start:

sleep 300 &

Find PID:

pgrep sleep

Check affinity:

taskset -p PID

Then start:

taskset -c 0 sleep 300 &

Check its affinity.

Lab 5 — Nice

Run:

nice -n 10 sleep 300 &

Find it:

ps -o pid,ni,pri,comm -C sleep

Understand:

NI = nice value
PRI = scheduler priority
Lab 6 — Memory

Run:

free -h

Then:

cat /proc/meminfo | head -20

Compare what you see.

Lab 7 — Find Memory-Heavy Processes
ps aux --sort=-%mem | head -10

Identify:

PID
User
CPU %
Memory %
Command
Lab 8 — Find CPU-Heavy Processes
ps aux --sort=-%cpu | head -10
Lab 9 — Inspect a Process

Start:

sleep 300 &

Find:

PID=$(pgrep -n sleep)

Then:

cat /proc/$PID/status

Look specifically at:

Name
State
Pid
PPid
Threads
VmSize
VmRSS
Lab 10 — File Descriptors

For the same process:

ls -l /proc/$PID/fd

This shows the file descriptors opened by the process.

You should recognize:

0 → stdin
1 → stdout
2 → stderr
🔥 Senior Challenge

Now investigate your own shell.

Run:

echo $$

Suppose it returns:

5000

Then:

cat /proc/5000/status

and:

ls -l /proc/5000/fd

Then explain:

Why does your shell have file descriptors 0, 1 and 2?

Production Scenario

Imagine you receive an alert:

Server CPU: 100%

Load Average: 35

Application response time: 10 seconds

Don't immediately restart the server.

Your first investigation might be:

uptime

top

ps aux --sort=-%cpu | head

ps -eLf

lscpu

vmstat 1

Then determine:

Is CPU actually saturated?

Which process is consuming CPU?

Is it one thread or many?

How many CPUs exist?

Is the system spending time in user space or kernel space?

Is there excessive I/O wait?

Is there excessive context switching?

That is how a senior engineer approaches an incident.

🧠 Day 4 Interview Questions

Don't search for answers yet.

Write your answers yourself.

Basic
What is a thread?
Process vs thread?
Why are threads cheaper than processes?
What resources do threads share?
What resources are private to each thread?
Intermediate
What is CPU scheduling?
What is context switching?
Why does context switching have overhead?
What is CPU affinity?
What is a nice value?
What is the difference between physical and logical CPUs?
What is virtual memory?
What is a page?
What is a page fault?
What is swap?
Advanced
What happens when a process requests memory?
What happens when RAM is exhausted?
What is the Linux OOM Killer?
What is the difference between OOM at the Linux host level and a container being OOMKilled?
Why can increasing the number of application workers sometimes make performance worse?
Why does every thread have its own stack?
Why do threads share heap memory?
How would you investigate a server with 100% CPU?
How would you investigate a server running out of memory?
Why can a process show high virtual memory (VSZ) but relatively low physical memory (RSS)?
📚 Your Day 4 Deliverables

Before moving to Day 5, do these:

GitHub

Update:

01-Linux/
   README.md

Add:

Processes
Threads
CPU Scheduling
Context Switching
Virtual Memory
Stack vs Heap
OOM
/proc
Practical

Complete at least:

Labs 1–10

Interview

Answer:

25 questions

in your own words.

Senior Challenge

Investigate your shell using:

/proc/PID/status
/proc/PID/fd
One important rule for our journey

From this point onward, don't try to memorize everything in one day.

Your job today is to understand this chain:

Process
   ↓
Threads
   ↓
CPU Scheduling
   ↓
Context Switching
   ↓
Memory
   ↓
Virtual Memory
   ↓
Pages
   ↓
Memory Pressure
   ↓
OOM

Once you understand this chain, Docker CPU/memory limits and Kubernetes OOMKilled will become much easier later.

Complete the labs + 25 answers and send them to me. I'll review them like an interviewer, correct the weak areas, and then we'll start Day 5: Linux Filesystem, Inodes, File Descriptors, Mounts, /proc, /sys, /dev, Disk I/O and df vs du.

day 5
Day 5 — Linux Filesystem, Inodes, File Descriptors & Disk I/O

Aaj ka topic bahut important hai. DevOps interview + real production troubleshooting dono mein baar-baar aata hai.

Aaj hum sirf commands yaad nahi karenge. Tumhe samajhna hai:

Linux mein file actually hoti kya hai, filesystem kaise kaam karta hai, inode kya hai, process file ko kaise access karta hai, aur disk full hone par exactly kya hota hai.

🎯 Day 5 Goals

Aaj ke end tak tum confidently explain kar paoge:

Linux filesystem hierarchy
/, /etc, /var, /home, /tmp, /proc, /sys, /dev
File vs directory
Inode
Filename vs inode
Hard link vs symbolic link
File Descriptor
stdin/stdout/stderr
/proc/PID/fd
Mount points
Filesystem vs disk
df vs du
Disk usage troubleshoot karna
Deleted-but-open files
lsof
Disk I/O basics
Production mein "No space left on device" troubleshoot karna
1. Linux Filesystem

Linux mein Windows jaisa:

C:
D:
E:

concept nahi hota.

Linux ek single hierarchy use karta hai:

                    /
                    |
     ┌──────────────┼──────────────┐
     ↓              ↓              ↓
   /etc            /var           /home
     ↓              ↓
 configuration    logs/data

     ┌──────────────┼──────────────┐
     ↓              ↓              ↓
   /proc           /sys           /dev
 kernel/process   hardware       devices

Root:

/

sabse top-level directory hai.

Example:

/home/sudheer/file.txt

iska path / se start ho raha hai.

2. Important Linux Directories

Ye directories interview mein bahut important hain.

Directory	Purpose
/	Root filesystem
/bin	Essential commands/binaries
/sbin	System/admin binaries
/etc	Configuration
/home	User home directories
/root	root user's home
/var	Variable data, logs, caches
/tmp	Temporary files
/usr	User-space programs/libraries/data
/opt	Optional/add-on software
/dev	Device files
/proc	Process/kernel virtual filesystem
/sys	Kernel/device information
/boot	Boot-related files
/mnt	Temporary mount point
/media	Removable media mounts

Modern distributions may merge /bin, /sbin, etc. into /usr, often via symlinks. Isliye exact physical layout distro par depend kar sakta hai.

3. /etc

/etc mein configuration files hoti hain.

Examples:

/etc/ssh/sshd_config
/etc/hosts
/etc/fstab
/etc/passwd
/etc/hostname

Example:

cat /etc/hostname

Server ka hostname milega.

4. /var

/var ka matlab roughly variable data.

Yahan aisi information hoti hai jo runtime mein change hoti rehti hai.

Important:

/var/log
/var/lib
/var/cache

Example:

ls /var/log

Production mein common problem:

/var/log

mein logs continuously grow karte rahe aur disk full ho gayi.

5. /tmp

Temporary files:

/tmp

Example:

touch /tmp/testfile
ls -l /tmp/testfile

Lekin /tmp ko permanent storage samajhna galat hai.

6. /proc

Ye real disk directory nahi hai.

Ye ek virtual filesystem hai jo kernel/process information expose karta hai.

Example:

ls /proc

Tumhe numbers dikhenge:

1
100
245
...

Ye mostly process PIDs hain.

Example:

cat /proc/1/status

PID 1 ki information.

7. /sys

/sys bhi virtual filesystem hai.

Ye kernel aur hardware/device information expose karta hai.

Example:

ls /sys

Aur:

ls /sys/class
8. /dev

/dev mein device files milti hain.

Examples:

/dev/null
/dev/zero
/dev/random
/dev/sda

Example:

echo hello > /dev/null

Data /dev/null mein jaake discard ho jata hai.

9. File Actually Kya Hai?

Linux mein file ko simply:

"filename + data"

samajhna incomplete hai.

Filesystem internally metadata maintain karta hai.

Important concept:

Filename
   ↓
Directory entry
   ↓
Inode
   ↓
File metadata + data block references
10. Inode

Inode = filesystem metadata structure associated with a file.

Inode generally stores information such as:

File type
Permissions
Owner
Group
Size
Timestamps
Link count
Pointers/references to data blocks

Important:

Inode normally filename store nahi karta.

Filename directory entry mein inode number se associated hota hai.

Example:

ls -li

Output:

123456 -rw-r--r-- 1 root root 100 test.txt

First number:

123456

inode number hai.

11. Filename vs Inode

Suppose:

test.txt

directory mein entry:

test.txt → inode 12345

Aur inode:

inode 12345
     ↓
metadata
     ↓
data blocks

Isliye filename aur actual filesystem object same concept nahi hain.

12. Hard Link

Hard link same inode ko reference karta hai.

Example:

touch file1
ln file1 file2

Check:

ls -li file1 file2

Tum notice karoge:

same inode number

Concept:

file1 ─────┐
           ↓
        inode 12345
           ↓
        data blocks
           ↑
file2 ─────┘

Dono names same underlying inode ko point karte hain.

13. Symbolic Link

Symbolic link ek path/reference store karta hai.

Create:

ln -s file1 file3

Check:

ls -li file1 file3

Ab inode generally different hoga.

Concept:

file3
  ↓
"file1"
  ↓
inode of file1
  ↓
data

Check:

readlink file3
14. Hard Link vs Soft Link
Feature	Hard Link	Symbolic Link
Same inode	Yes	No
Points to	Inode	Path
Can cross filesystem	Generally no	Yes
Can point to directory	Generally restricted	Yes
Breaks if target filename deleted	No	Yes

Interview question:

If original file is deleted, what happens?

Hard link:

file1 deleted
file2 still works

Because inode still has a directory entry.

Symbolic link:

target deleted
symlink becomes dangling/broken
15. File Descriptor

Now extremely important concept.

When a process opens a file/socket/pipe, Linux gives the process a number called:

File Descriptor (FD)

Example:

Process
   |
   ├── FD 0 → stdin
   ├── FD 1 → stdout
   ├── FD 2 → stderr
   └── FD 3 → opened file
16. Standard File Descriptors

Every normal process starts with:

0 → stdin
1 → stdout
2 → stderr

Meaning:

FD 0

Input:

stdin
FD 1

Normal output:

stdout
FD 2

Error output:

stderr
17. Example

Run:

cat

Terminal input:

hello

cat reads from:

FD 0

and writes to:

FD 1
18. Output Redirection
ls > output.txt

Conceptually:

ls
 |
 | FD 1
 ↓
output.txt

Now stdout terminal ke instead file mein ja raha hai.

19. Error Redirection
command 2> error.txt

FD 2:

stderr → error.txt

Both:

command > output.txt 2> error.txt
20. /proc/PID/fd

Ye Day 4 ka continuation hai.

Suppose:

sleep 300 &

PID:

pgrep sleep

Then:

ls -l /proc/PID/fd

You may see:

0 -> /dev/pts/0
1 -> /dev/pts/0
2 -> /dev/pts/0

This tells you where that process's standard streams are connected.

21. Mount Point

Linux mein storage filesystem ko directory ke andar attach kiya ja sakta hai.

Example:

/
├── etc
├── home
├── var
└── data

Suppose separate disk:

/dev/xvdf1

mount:

/dev/xvdf1
      ↓
     /data

Ab:

ls /data

us filesystem ka content dikha sakta hai.

22. mount

Currently mounted filesystems:

mount

Better:

findmnt
23. /etc/fstab

Persistent mounts ke configuration ke liye:

/etc/fstab

Example concept:

UUID=xxxx  /data  ext4  defaults  0  2

Important production point:

/etc/fstab mein incorrect entry server boot problems cause kar sakti hai.

24. df

df filesystem-level disk space batata hai.

df -h

Example:

Filesystem      Size  Used Avail Use%
/dev/root        50G   42G    8G  84%

Meaning:

Filesystem mein kitni space used/free hai.

25. du

du directories/files ka disk usage calculate karta hai.

du -sh /var/log

Example:

12G /var/log

Meaning:

/var/log ke files approximately 12 GB consume kar rahe hain.

26. df vs du

🔥 Very important interview question

df

Filesystem ko dekhta hai:

"Filesystem mein kitni space remaining hai?"
du

Files/directories ko dekhta hai:

"Files actually kitni disk space consume kar rahe hain?"
27. Production Scenario

Suppose application suddenly fail:

No space left on device

Tum run karte ho:

df -h

Output:

/dev/root  100%

First suspicion:

Disk full.

Ab:

du -sh /* 2>/dev/null

Suppose:

/var → 80G
/home → 5G
/opt → 2G

Then:

du -sh /var/*

Suppose:

/var/log → 75G

Then:

du -sh /var/log/*

Tum identify kar sakte ho kaunsa log grow kar raha hai.

28. Better Disk Investigation

Use:

df -h

then:

du -xh /var | sort -h | tail

or:

du -sh /var/* 2>/dev/null | sort -h
29. Important Problem: df Full But du Doesn't Match

Ye senior-level troubleshooting question hai.

Suppose:

df -h

says:

100%

But:

du -sh /

shows only:

60G

Where did remaining space go?

One major possibility:

Deleted file is still open by a process.

30. Deleted-but-Open File

Suppose application log:

app.log

is 20 GB.

Application has it open.

Someone runs:

rm app.log

Filename directory se remove ho gaya.

But application still has the file open.

So:

filename → gone
process → still holding FD
inode → still alive
disk blocks → still allocated

That's why df can still show the space as used.

31. Find Deleted Open Files

Use:

lsof +L1

Or:

lsof | grep deleted

You might see:

java   1234 root  5w  REG ... 20G ... /var/log/app.log (deleted)

This is a classic production issue.

32. How to Recover the Space?

First identify the process:

PID = 1234

Then understand why it still has the file open.

Usually the proper fix is to:

rotate logs correctly
restart/reload application if appropriate
close the file descriptor

Don't blindly delete random files.

33. Disk I/O

Disk operations are much slower than CPU/register operations.

Application:

Application
     ↓
System Call
     ↓
Kernel
     ↓
Filesystem
     ↓
Block Layer
     ↓
Storage Device

For example:

read()
write()

ultimately interact with storage through kernel I/O layers.

34. Check Disk Devices
lsblk

Example:

NAME    SIZE TYPE MOUNTPOINTS
nvme0n1 100G disk
└─nvme0n1p1 100G part /
nvme1n1 200G disk
└─nvme1n1p1 200G part /data

Very useful in AWS EC2 troubleshooting.

35. Filesystem Type

Check:

df -Th

Example:

Filesystem     Type  Size Used Avail Use%
/dev/root      ext4   50G  40G  10G  80%

Common Linux filesystems:

ext4
xfs
36. Inode Exhaustion

🔥 Another senior-level production issue.

Disk space available ho sakti hai:

Avail: 50G

but all inodes consumed.

Check:

df -i

Example:

Filesystem      Inodes  IUsed  IFree IUse%
/dev/root       3.2M    3.2M      0  100%

Then you may get:

No space left on device

even though:

df -h

shows free GBs.

Why?

Because filesystem mein too many files create ho gaye.

Example:

millions of tiny files
37. df -h vs df -i
Command	Checks
df -h	Disk blocks/space
df -i	Inodes

Production troubleshooting mein dono check karne ki habit banao.

38. Linux Disk Troubleshooting Flow

Agar application bole:

No space left on device

Senior engineer ka approach:

             No space left
                    ↓
                 df -h
                    ↓
          Filesystem actually full?
             /             \
           Yes              No
            ↓                ↓
          du              df -i
            ↓                ↓
     Find large files    inode exhaustion?
            ↓
       Check deleted
       open files
            ↓
       lsof +L1
🧪 Day 5 Practical Labs

Ab actual hands-on.

Lab 1 — Filesystem Structure

Run:

ls /

Then:

ls -ld /etc /var /home /tmp /proc /sys /dev

Explain what each directory is used for.

Lab 2 — Inode

Create:

mkdir -p ~/day5
cd ~/day5

touch file1
ls -li file1

Record inode number.

Then:

ln file1 hardlink
ls -li file1 hardlink

Observe inode numbers.

Lab 3 — Symbolic Link
ln -s file1 symlink
ls -li file1 symlink

Then:

readlink symlink

Explain the difference.

Lab 4 — Delete Original
rm file1

Now:

ls -li hardlink
cat hardlink

Then:

cat symlink

Observe the difference.

This experiment is very important.

Lab 5 — File Descriptors

Run:

sleep 300 &

Then:

PID=$(pgrep -n sleep)

Now:

ls -l /proc/$PID/fd

Identify:

0
1
2

and explain where each points.

Lab 6 — Redirection

Run:

echo "hello" > output.txt

Then:

cat output.txt

Now:

ls /does-not-exist 2> error.txt
cat error.txt

Explain:

> 
2>
Lab 7 — df

Run:

df -h

Then:

df -Th

Record:

root filesystem
filesystem type
total size
used
available
mount point
Lab 8 — du

Run:

du -sh ~

Then:

du -sh ~/* 2>/dev/null | sort -h

Identify your largest directory.

Lab 9 — Inode Usage

Run:

df -i

Record:

IUse%

Explain what it means.

Lab 10 — Block Devices

Run:

lsblk

Then:

findmnt

Understand:

disk
 ↓
partition
 ↓
filesystem
 ↓
mount point
🔥 Senior Challenge

Imagine production server reports:

Application error:
No space left on device

You run:

df -h

and get:

/dev/root   100G   100G   0G   100% /

But:

du -sh /*

only accounts for:

70G
Question:

Where can the remaining ~30 GB be?

Give me at least 3 possible reasons.

One of them should be:

deleted-but-open files

And tell me which command you'd use to investigate it.

🧠 Day 5 Interview Questions

Answer these without looking at the lesson if possible.

Basic
What is Linux filesystem hierarchy?
What is /etc used for?
What is /var used for?
What is /proc?
What is /sys?
What is /dev?
What is inode?
Does inode store filename?
What is a hard link?
What is a symbolic link?
File Descriptor
What is a file descriptor?
What are FD 0, 1 and 2?
What happens when you execute ls > output.txt?
Difference between > and 2>?
What is /proc/PID/fd?
Storage
What does df -h show?
What does du -sh show?
Difference between df and du?
What does lsblk show?
What does df -Th show?
What does df -i show?
Senior Level
Why can df show 100% while du shows much less?
What is a deleted-but-open file?
How do you find deleted-but-open files?
How can inode exhaustion happen?
How would you troubleshoot No space left on device?
What happens when a process opens a file?
Why can a hard link survive deletion of the original filename?
What happens to a symbolic link when its target is deleted?
Explain this flow:
Application
↓
System Call
↓
Kernel
↓
Filesystem
↓
Block Device
📁 GitHub Work

Tumhare Devops-master-journey repo mein:

01-Linux/

ke andar Day 5 add karo:

01-Linux/
├── README.md
├── day-1-architecture.md
├── day-2-boot-process.md
├── day-3-processes.md
├── day-4-threads-memory.md
├── day-5-filesystem.md
└── incident-reports/
    └── disk-full.md

day-5-filesystem.md mein apni language mein concepts likhna.

Aur:

incident-reports/disk-full.md

mein scenario document karo:

Problem:
No space left on device

Symptoms:
...

Investigation:
...

Root Cause:
...

Commands Used:
...

Solution:
...

Prevention:
...
