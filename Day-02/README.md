# Linux Boot Process and Boot Troubleshooting

## Objective

This document summarizes the Linux boot process, BIOS vs UEFI, MBR vs GPT, initramfs, systemd, boot troubleshooting, and interview questions.

---

# Linux Boot Process

When a Linux computer starts, it follows these stages:

```text
Power Button
     │
     ▼
BIOS / UEFI Firmware
     │
     ▼
POST (Power-On Self-Test)
     │
     ▼
Find Boot Device
     │
     ▼
GRUB Bootloader
     │
     ▼
Load Linux Kernel (vmlinuz)
     │
     ▼
Load initramfs
     │
     ▼
Kernel Initializes Hardware
     │
     ▼
Mount Root Filesystem
     │
     ▼
Start systemd (PID 1)
     │
     ▼
Start System Services
     │
     ▼
Login Prompt / Desktop
```

---

# Step-by-Step Boot Process

## 1. Power On

When the power button is pressed, electricity reaches the motherboard and CPU.

The CPU immediately begins executing firmware stored on the motherboard.

---

## 2. BIOS or UEFI

The firmware initializes hardware and prepares the system for booting.

### BIOS

- Legacy firmware
- Supports MBR
- Maximum disk size: 2 TB
- Slower startup

### UEFI

- Modern firmware
- Supports GPT
- Supports Secure Boot
- Faster startup
- Supports disks larger than 2 TB

---

# BIOS vs UEFI

| BIOS | UEFI |
|------|------|
| Older firmware | Modern firmware |
| Uses MBR | Uses GPT |
| 2 TB disk limit | Supports very large disks |
| Slower | Faster |
| No Secure Boot | Secure Boot supported |
| Text interface | Graphical interface supported |

---

# 3. POST (Power-On Self-Test)

POST checks whether hardware is functioning correctly.

It tests:

- CPU
- RAM
- Keyboard
- Storage devices
- Video hardware

If a hardware problem is found, booting stops and an error or beep code may be shown.

---

# 4. Boot Device Detection

After POST, BIOS/UEFI searches for a bootable device such as:

- SSD
- HDD
- USB drive
- DVD
- Network (PXE)

The first bootable device is selected according to the configured boot order.

---

# 5. GRUB Bootloader

GRUB (GRand Unified Bootloader) is responsible for loading Linux.

GRUB can:

- Display the boot menu
- Load different kernels
- Boot recovery mode
- Pass kernel parameters
- Load the Linux kernel and initramfs

Common GRUB files:

```
/boot/grub/
/boot/grub/grub.cfg
```

---

# 6. Linux Kernel

The kernel is the core of the operating system.

Its responsibilities include:

- Memory management
- Process management
- Device drivers
- CPU scheduling
- Security
- Filesystem management

Kernel image:

```
/boot/vmlinuz
```

---

# 7. initramfs

The kernel cannot always access the real root filesystem immediately.

initramfs is a temporary root filesystem loaded into RAM.

It contains:

- Storage drivers
- Filesystem drivers
- RAID support
- LVM support
- Encryption tools
- Scripts to locate the real root filesystem

Without initramfs, the kernel may fail with:

```
Kernel panic:
Unable to mount root filesystem
```

After loading the necessary drivers, initramfs mounts the real root filesystem and hands control to it.

---

# 8. Root Filesystem

The kernel mounts the actual Linux filesystem.

Usually mounted as:

```
/
```

This contains:

- /bin
- /etc
- /home
- /var
- /usr
- /tmp

---

# 9. systemd (PID 1)

After the root filesystem is mounted, the kernel starts systemd.

systemd is always:

```
PID 1
```

Responsibilities:

- Start services
- Manage dependencies
- Mount filesystems
- Handle shutdown and reboot
- Manage logging
- Launch login services

Verify:

```bash
ps -p 1
```

Expected output:

```
systemd
```

---

# 10. User Login

systemd starts:

- SSH
- Network
- Login manager
- Desktop environment (if installed)

The system is now ready for users.

---

# MBR vs GPT

## MBR

- Maximum disk size: 2 TB
- Maximum 4 primary partitions
- Boot code stored in the first sector
- Used with BIOS

## GPT

- Supports disks larger than 2 TB
- Up to 128 partitions
- Stores multiple copies of the partition table
- More reliable with CRC checks
- Used with UEFI

---

# MBR vs GPT Comparison

| MBR | GPT |
|------|------|
| Legacy | Modern |
| 2 TB limit | Very large disks |
| 4 partitions | Up to 128 partitions |
| Less reliable | More reliable |
| BIOS | UEFI |

---

# Why initramfs is Required

The Linux kernel does not include every storage or filesystem driver.

initramfs temporarily provides:

- Drivers
- Modules
- LVM tools
- RAID tools
- Encryption support

Without it, the kernel cannot mount the root filesystem.

---

# Why systemd is PID 1

The Linux kernel launches the first userspace process after initialization.

That process is systemd.

PID 1:

- Starts all services
- Controls boot
- Handles shutdown
- Restarts failed services
- Manages system state

If PID 1 stops, the operating system cannot continue normally.

---

# Boot Troubleshooting

Common symptoms:

- SSH unavailable
- Kernel panic
- GRUB screen
- Boot loop
- Black screen

Investigation order:

1. Access the server console.
2. Verify GRUB loads correctly.
3. Boot an older kernel.
4. Check whether the root filesystem can be mounted.
5. Run filesystem checks (`fsck`) if needed.
6. Review boot logs with `journalctl -b`.
7. Check kernel logs with `dmesg`.
8. Rebuild initramfs if required.
9. Reinstall GRUB if necessary.

---

# Useful Commands

## View boot logs

```bash
journalctl -b
```

## View kernel logs

```bash
dmesg
```

## Check boot performance

```bash
systemd-analyze
```

## Find slow boot services

```bash
systemd-analyze blame
```

## View boot dependency chain

```bash
systemd-analyze critical-chain
```

## Check PID 1

```bash
ps -p 1
```

## Repair filesystem

```bash
fsck /dev/sdX
```

Replace `/dev/sdX` with the correct partition.

---

# Incident Example

## Problem

A Linux server does not boot after a kernel update.

### Symptoms

- SSH unavailable
- Console displays GRUB
- Kernel panic
- Server unreachable

### Investigation

1. Open the server console.
2. Confirm GRUB is working.
3. Boot an older kernel.
4. Verify the root filesystem.
5. Check for filesystem corruption.
6. Inspect logs using:

```bash
journalctl -b
dmesg
```

7. Rebuild initramfs if required.
8. Reinstall GRUB if damaged.
9. Remove or fix the problematic kernel.
10. Reboot and verify successful startup.

---

# Interview Questions and Answers

## 1. What happens immediately after pressing the power button?

The CPU starts executing BIOS or UEFI firmware, which initializes hardware, performs POST, detects a boot device, and starts the bootloader.

---

## 2. What is the difference between BIOS and UEFI?

BIOS is legacy firmware that uses MBR and has hardware limitations. UEFI is the modern replacement, supporting GPT, Secure Boot, faster booting, and larger disks.

---

## 3. What is POST?

POST (Power-On Self-Test) checks hardware components such as the CPU, RAM, storage, and keyboard before the operating system starts.

---

## 4. What is the purpose of the bootloader?

The bootloader loads the Linux kernel and initramfs into memory and transfers control to the kernel.

---

## 5. What is GRUB?

GRUB (GRand Unified Bootloader) is the default Linux bootloader. It displays the boot menu, allows kernel selection, and loads the Linux kernel and initramfs.

---

## 6. What is the difference between MBR and GPT?

MBR supports disks up to 2 TB and only four primary partitions. GPT supports much larger disks, many more partitions, and includes redundancy and integrity checks.

---

## 7. Why is initramfs needed?

initramfs provides the drivers and tools required to locate and mount the real root filesystem before the operating system can continue booting.

---

## 8. Why is systemd PID 1?

The Linux kernel starts systemd as the first userspace process. It initializes the system, starts services, manages dependencies, and controls shutdown and reboot.

---

## 9. How do you check which service is slowing boot?

Use:

```bash
systemd-analyze blame
```

You can also use:

```bash
systemd-analyze
systemd-analyze critical-chain
```

---

## 10. A Linux server is stuck at the GRUB screen after an update. How would you troubleshoot it?

- Access the server console.
- Check whether GRUB is functioning correctly.
- Boot a previous kernel.
- Verify the root filesystem.
- Run `fsck` if filesystem corruption is suspected.
- Review boot logs using `journalctl -b`.
- Check kernel logs with `dmesg`.
- Rebuild initramfs or reinstall GRUB if necessary.
- Remove or replace the faulty kernel.
- Reboot and verify normal operation.

---

# Key Takeaways

- Linux boots through firmware, bootloader, kernel, initramfs, systemd, and services.
- BIOS is the legacy firmware; UEFI is the modern standard.
- MBR is older; GPT is the modern partitioning scheme.
- initramfs provides the temporary environment needed to mount the root filesystem.
- systemd runs as PID 1 and manages the entire userspace startup process.
- Effective boot troubleshooting follows a structured process: verify firmware, bootloader, kernel, root filesystem, filesystem integrity, and logs.
