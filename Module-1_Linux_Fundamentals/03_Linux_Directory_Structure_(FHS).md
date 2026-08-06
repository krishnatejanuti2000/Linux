# Linux Directory Structure (FHS)

# 1. Introduction to Filesystem Hierarchy Standard (FHS)

## What is FHS?

The **Filesystem Hierarchy Standard (FHS)** is a standard that defines how directories should be organized in Linux operating systems. It specifies the purpose of each directory, the type of files it should contain, and where applications, system files, configuration files, logs, libraries, and user data should be stored.

The primary goal of FHS is to provide a **consistent directory structure** across different Linux distributions. Whether you are working on Red Hat Enterprise Linux (RHEL), Ubuntu, Debian, SUSE, Rocky Linux, or Fedora, the overall filesystem layout remains largely the same because these distributions follow the FHS standard.

Without a common standard, every Linux distribution could organize its files differently. This would make Linux administration, software installation, scripting, troubleshooting, and system maintenance much more difficult.

---

## Why was FHS introduced?

Imagine three Linux distributions storing system files in different locations.

**Distribution A**

```text
/system/bin
```

**Distribution B**

```text
/commands
```

**Distribution C**

```text
/executables
```

If every distribution used different directory names, Linux administrators would have to learn a different filesystem layout for every operating system. Software developers would also need to modify their applications separately for each distribution.

To solve this problem, Linux adopted the **Filesystem Hierarchy Standard (FHS)**, which defines a common directory layout that distributions are expected to follow.

---

## Why is FHS Important?

Following a common filesystem hierarchy provides several advantages:

- Provides a consistent directory structure across Linux distributions.
- Makes Linux administration easier.
- Simplifies software installation and package management.
- Helps administrators quickly locate system files.
- Improves troubleshooting and debugging.
- Allows scripts and applications to work across multiple Linux distributions with minimal changes.

For example:

- Boot-related files are stored under `/boot`.
- Configuration files are stored under `/etc`.
- Device files are stored under `/dev`.
- Log files are generally stored under `/var/log`.
- User home directories are stored under `/home`.

Because these locations are standardized, administrators know where to look regardless of the Linux distribution.

---

## Why is FHS Important for a Storage Engineer?

As a Storage Engineer, understanding the Linux directory structure is essential because storage-related tasks frequently involve navigating different parts of the filesystem.

For example:

- Investigating boot failures often involves checking files under `/boot`.
- Working with disks, SSDs, SAN LUNs, and NVMe devices requires interacting with device files under `/dev`.
- Mounting filesystems involves directories such as `/mnt` or other mount points.
- Storage configuration files are commonly found under `/etc`.
- Storage-related logs are usually located under `/var/log`.

Without understanding the FHS, troubleshooting storage issues becomes significantly more difficult.

---

## Key Takeaways

- FHS stands for **Filesystem Hierarchy Standard**.
- FHS defines a standard directory layout for Linux operating systems.
- Most Linux distributions follow the FHS specification.
- FHS provides consistency, making Linux administration, software development, and troubleshooting easier.
- Understanding FHS is fundamental for Linux Administrators, System Engineers, and Storage Engineers.

---

# 2. Root Directory (/)

## What is the Root Directory?

The **root directory (`/`)** is the highest level directory in the Linux filesystem hierarchy. It serves as the starting point of the entire filesystem. Every file, directory, device, and mounted filesystem in Linux ultimately exists under this single root directory.

Unlike Windows, which uses multiple drive letters such as `C:\`, `D:\`, and `E:\`, Linux follows a **single unified directory hierarchy**. Regardless of how many disks, partitions, SSDs, USB drives, or network filesystems are connected to the system, they all become part of the same directory tree that begins at `/`.

```text
                /
      ┌─────────┼─────────┐
      │         │         │
    /boot     /etc      /home
      │         │         │
    /dev      /usr      /var
```

The root directory is the foundation of the Linux filesystem hierarchy.

---

## Why Does Linux Have Only One Root Directory?

Linux follows the Unix philosophy of maintaining a **single unified filesystem**.

Instead of assigning separate drive letters to different storage devices, Linux makes every storage device accessible through a single directory hierarchy.

For example, consider a system with:

- One NVMe SSD
- One SATA HDD
- One USB Drive

Although these are physically different storage devices, Linux integrates them into the same filesystem tree by mounting them at different locations.

```text
/
├── boot
├── home
├── backup
└── media
```

From the user's perspective, everything appears under the same root directory.

---

## Root Directory vs Root User

These two terms are often confused.

### Root Directory

```text
/
```

- Highest directory in the filesystem.
- Starting point of the Linux directory hierarchy.

### Root User

```text
root
```

- Superuser account.
- Has unrestricted administrative privileges.

Although both use the word **root**, they represent completely different concepts.

---

## Mount Points

One of the most important concepts in Linux is the **mount point**.

A mount point is simply an existing directory where another filesystem is attached.

For example, before mounting:

```text
/
└── backup
```

Here, `/backup` is just an ordinary directory.

Now suppose another disk is mounted:

```bash
mount /dev/sdb1 /backup
```

After mounting:

```text
/
└── backup
    ├── documents
    ├── images
    └── videos
```

The contents displayed inside `/backup` now come from the mounted filesystem rather than the original directory.

---

## What Happens to the Original Directory?

Mounting **does not delete** the original directory or its contents.

Instead, the mounted filesystem temporarily hides the original contents.

For example:

Before mounting:

```text
/backup
├── file1.txt
└── file2.txt
```

After mounting another filesystem:

```bash
mount /dev/sdb1 /backup
```

The files `file1.txt` and `file2.txt` become temporarily hidden because the mounted filesystem now occupies the `/backup` directory.

After unmounting:

```bash
umount /backup
```

The original files become visible again.

This demonstrates that mounting overlays one filesystem on top of an existing directory.

---

## Real Example from Our Learning

We discussed that the same concept applies to `/boot`.

Before the `/boot` partition is mounted, `/boot` exists as a normal directory inside the root filesystem.

After the system mounts the boot partition:

```text
/dev/nvme0n1p2
        │
        ▼
      /boot
```

The contents visible inside `/boot` are actually coming from the mounted filesystem on `/dev/nvme0n1p2`.

This is exactly the same mounting behavior demonstrated with the `/backup` example.

---

## Storage Engineer Perspective

Understanding the root directory and mount points is essential for storage administration.

Storage Engineers frequently:

- Mount new SAN LUNs.
- Mount NVMe SSDs.
- Mount USB drives.
- Mount NFS shares.
- Mount backup volumes.
- Troubleshoot mount failures.

Every mounted storage device becomes part of the same filesystem hierarchy rooted at `/`.

Without understanding mount points, filesystem management and storage troubleshooting become extremely difficult.

---

## Interview Notes

**Q1. What is the root directory in Linux?**

**Answer:**

The root directory (`/`) is the highest level directory in the Linux filesystem hierarchy. Every file, directory, and mounted filesystem is accessible through this directory.

---

**Q2. Why does Linux have only one root directory?**

**Answer:**

Linux follows a single unified filesystem hierarchy. Instead of assigning separate drive letters to each storage device, all filesystems are mounted somewhere under the root directory.

---

**Q3. What is a mount point?**

**Answer:**

A mount point is an existing directory where another filesystem is attached, making the contents of that filesystem accessible through the Linux directory hierarchy.

---

## Key Takeaways

- `/` is the highest level directory in Linux.
- Every file and directory ultimately exists under `/`.
- Linux uses a single unified filesystem hierarchy.
- Additional storage devices are integrated using mount points.
- Mounting temporarily hides the original contents of the mount point.
- The same mounting concept applies to directories such as `/boot`, `/mnt`, `/media`, and custom mount points.

---

# 3. /bin - Essential User Command Binaries

## What is `/bin`?

The `/bin` directory contains **essential user command binaries (executables)** required for basic system operation.

These commands are fundamental utilities that users and the operating system require even during the early stages of booting or while performing system recovery.

Some common commands include:

```text
ls
cp
mv
rm
cat
pwd
mkdir
rmdir
chmod
chown
echo
```

These are executable programs that allow users to interact with the Linux operating system.

---

## What is a Binary Executable?

A binary executable is a compiled program that the CPU can execute directly.

When a user runs a command such as:

```bash
ls
```

the shell searches for the executable file corresponding to the command.

On our RHEL 9.8 system:

```bash
which ls
```

Output:

```text
/usr/bin/ls
```

This means the command `ls` is actually an executable program stored on the filesystem.

Similarly,

```bash
which cp
```

Output:

```text
/usr/bin/cp
```

and

```bash
which mv
```

Output:

```text
/usr/bin/mv
```

These are examples of binary executables.

---

## Why Was `/bin` Created?

To understand `/bin`, we need to look at the history of Linux and UNIX.

Earlier UNIX and Linux systems often had separate filesystems for:

- Root filesystem (`/`)
- `/usr`
- `/home`

During the early boot process, only the root filesystem was mounted initially.

The `/usr` filesystem was mounted later.

However, administrators still needed basic commands before `/usr` became available.

For example:

- `ls`
- `cp`
- `mv`
- `cat`
- `sh`

To solve this problem, these essential executable programs were placed inside:

```text
/bin
```

Since `/bin` was part of the root filesystem, these commands remained available even before other filesystems were mounted.

---

## Modern Linux Systems (Merged `/usr`)

Modern Linux distributions such as:

- RHEL 9
- Fedora
- Ubuntu
- Debian
- SUSE

have adopted the **Merged `/usr`** layout.

In this design, `/bin` is no longer a separate directory containing its own executables.

Instead, it is implemented as a symbolic link to `/usr/bin`.

On our RHEL 9.8 system:

```bash
ls -ld /bin
```

Output:

```text
lrwxrwxrwx. 1 root root 7 ... /bin -> usr/bin
```

This confirms that `/bin` is a symbolic link pointing to `/usr/bin`.

---

## How Does Symbolic Linking Work?

Suppose a user executes:

```bash
/bin/ls
```

Internally, Linux resolves the symbolic link:

```text
/bin
    │
    ▼
/usr/bin
    │
    ▼
/usr/bin/ls
```

Although the user accesses `/bin/ls`, Linux actually executes:

```text
/usr/bin/ls
```

This redirection is performed automatically by the kernel during pathname resolution.

---

## Practical Verification

We verified this on our RHEL 9.8 system.

### Step 1

```bash
ls -li /bin/ls
```

Output:

```text
201345586 ... /bin/ls
```

### Step 2

```bash
ls -li /usr/bin/ls
```

Output:

```text
201345586 ... /usr/bin/ls
```

Notice that both files have the **same inode number**.

Although we have not yet studied inodes, this observation tells us that both paths refer to the same underlying executable.

---

## Logical Path vs Physical Path

We also verified the difference between logical and physical paths.

### Logical Path

```bash
cd /bin

pwd
```

Output:

```text
/bin
```

The shell preserves the logical path that the user entered.

---

### Physical Path

```bash
pwd -P
```

Output:

```text
/usr/bin
```

The `-P` option resolves symbolic links and displays the actual physical directory.

---

## Why Keep `/bin` If Everything Exists in `/usr/bin`?

This decision maintains **backward compatibility**.

Thousands of existing shell scripts and applications contain absolute paths such as:

```text
/bin/bash
/bin/sh
/bin/ls
/bin/cp
```

If `/bin` were removed entirely, these scripts would fail.

Keeping `/bin` as a symbolic link allows older software to continue working without modification.

---

## Storage Engineer Perspective

Although Storage Engineers rarely modify `/bin`, understanding its behavior is important when:

- Recovering systems in rescue mode.
- Troubleshooting boot issues.
- Investigating missing or corrupted system binaries.
- Understanding filesystem layouts on enterprise Linux distributions.

Knowing that `/bin` is a symbolic link on modern RHEL systems also helps when navigating the filesystem and understanding command locations.

---

## Interview Notes

### Q1. What is the purpose of `/bin`?

**Answer:**

`/bin` stores essential user command binaries (executables) required for basic system operation.

---

### Q2. Why is `/bin` a symbolic link on modern Linux systems?

**Answer:**

Modern Linux distributions use the Merged `/usr` layout. Instead of storing duplicate executables, `/bin` is implemented as a symbolic link to `/usr/bin`, preserving backward compatibility with older software.

---

### Q3. What is the difference between `pwd` and `pwd -P`?

**Answer:**

- `pwd` displays the logical current working directory.
- `pwd -P` resolves symbolic links and displays the physical directory.

---

## Key Takeaways

- `/bin` contains essential user command binaries.
- Binary executables are compiled programs executed directly by the CPU.
- Historically, `/bin` existed to make essential commands available before `/usr` was mounted.
- Modern Linux distributions use the Merged `/usr` layout.
- On RHEL 9.8, `/bin` is a symbolic link to `/usr/bin`.
- The kernel transparently resolves symbolic links during pathname resolution.
- `pwd` displays the logical path, while `pwd -P` displays the physical path.
- Backward compatibility is the primary reason `/bin` still exists on modern systems.

---

# 4. /boot - Linux Boot Files

## What is `/boot`?

The `/boot` directory contains the files required to boot the Linux operating system.

When the system powers on, the bootloader (GRUB) loads the Linux kernel and other required boot files from the `/boot` directory before handing control over to the kernel.

Without the files stored in `/boot`, Linux cannot start successfully.

---

## Why Does `/boot` Exist?

During the boot process, Linux must load the kernel before the operating system is fully running.

However, the kernel itself is stored on a storage device.

This creates an important requirement:

- The bootloader must know where the kernel is located.
- The bootloader must be able to access the kernel before the operating system starts.

To solve this problem, Linux stores all boot-critical files inside the `/boot` directory.

---

## Linux Boot Flow

The relationship between `/boot` and the boot process can be represented as:

```text
Power ON
     │
     ▼
BIOS / UEFI
     │
     ▼
GRUB Bootloader
     │
     ▼
Reads files from /boot
     │
     ├── vmlinuz
     ├── initramfs
     └── GRUB configuration
     │
     ▼
Linux Kernel
     │
     ▼
systemd
     │
     ▼
Operating System
```

The `/boot` directory acts as the storage location for the files that allow the operating system to start.

---

# Common Files Inside `/boot`

A typical `/boot` directory contains files similar to:

```text
/boot
├── vmlinuz-...
├── initramfs-....img
├── System.map-...
├── config-...
├── grub2/
└── efi/
```

Each file serves a specific purpose during system startup.

---

# vmlinuz

Example:

```text
/boot/vmlinuz-5.14.0-687.33.1.el9_8.x86_64
```

`vmlinuz` is the compressed Linux kernel image.

During boot:

```text
GRUB
   │
   ▼
Loads vmlinuz
   │
   ▼
Linux Kernel Starts
```

Without this file, Linux cannot boot.

---

# initramfs

Example:

```text
initramfs-5.14.0-687.33.1.el9_8.x86_64.img
```

`initramfs` stands for **Initial RAM Filesystem**.

It contains the temporary userspace environment required during the early stages of booting.

Its primary responsibility is to prepare the system so that the real root filesystem can be mounted.

For example, if the root filesystem depends on:

- LVM
- RAID
- NVMe drivers
- Device Mapper
- Filesystem modules

the required components are made available through the initramfs image before switching to the actual root filesystem.

---

# System.map

Example:

```text
System.map-5.14.0-687.33.1.el9_8.x86_64
```

`System.map` contains a mapping between kernel symbols and their memory addresses.

It is mainly used for:

- Kernel debugging
- Diagnostics
- Low-level troubleshooting

Most Linux administrators rarely modify this file directly.

---

# config

Example:

```text
config-5.14.0-687.33.1.el9_8.x86_64
```

This file contains the configuration options that were used when building the Linux kernel.

It records which kernel features, drivers, filesystems, and options were enabled during compilation.

---

# grub2

The directory:

```text
/boot/grub2
```

contains GRUB-related files.

GRUB is the bootloader responsible for:

- Displaying the boot menu.
- Selecting the kernel.
- Loading the kernel into memory.
- Passing control to the Linux kernel.

---

# EFI Directory

On UEFI-based systems, another directory exists:

```text
/boot/efi
```

This is the mount point for the **EFI System Partition (ESP)**.

The UEFI firmware accesses bootloader files stored in this partition before Linux starts.

---

# Multiple Kernels

Modern Linux systems often keep multiple kernel versions.

For example, on our RHEL 9.8 system:

```text
vmlinuz-5.14.0-687.33.1...
vmlinuz-5.14.0-687.5.3...
vmlinuz-0-rescue...
```

Keeping older kernels provides a recovery mechanism.

If a newly installed kernel fails to boot, GRUB can boot an older working kernel instead.

---

# Rescue Kernel

Example:

```text
vmlinuz-0-rescue-...
```

The rescue kernel is intended for system recovery.

It allows administrators to boot the system when the normal kernels fail.

---

# Separate `/boot` Partition

One of the most important concepts we learned is that `/boot` can be a separate filesystem.

On our RHEL 9.8 system:

```bash
findmnt /boot
```

Output:

```text
TARGET SOURCE         FSTYPE
/boot  /dev/nvme0n1p2 xfs
```

This tells us:

- `/boot` is a mount point.
- The filesystem mounted there resides on `/dev/nvme0n1p2`.
- The filesystem type is XFS.

---

# Separate EFI Partition

We also verified:

```bash
findmnt /boot/efi
```

Output:

```text
TARGET    SOURCE         FSTYPE
/boot/efi /dev/nvme0n1p1 vfat
```

This confirms that:

- `/boot/efi` is a separate filesystem.
- It resides on `/dev/nvme0n1p1`.
- It uses the VFAT filesystem because the EFI System Partition follows the UEFI specification.

---

# Mount Point Relationship

Before mounting:

```text
/
└── boot
```

The `/boot` directory is simply another directory inside the root filesystem.

After mounting:

```text
/dev/nvme0n1p2
        │
        ▼
      /boot
```

The contents displayed inside `/boot` come from the mounted filesystem rather than the original directory.

This is exactly the same mounting concept demonstrated earlier using the `/backup` example.

---

# Storage Engineer Perspective

Understanding `/boot` is essential for storage engineers because boot failures often involve:

- Missing kernel images.
- Corrupted initramfs.
- Incorrect bootloader configuration.
- Missing storage drivers.
- LVM or RAID initialization failures.
- Incorrect boot partitions.

Troubleshooting boot-related storage issues frequently requires investigating the `/boot` directory.

---

# Interview Notes

## Q1. What is the purpose of `/boot`?

**Answer:**

`/boot` stores the files required to boot the Linux operating system, including the Linux kernel, initramfs, GRUB bootloader files, kernel configuration files, and System.map.

---

## Q2. Why do enterprise systems sometimes use a separate `/boot` partition?

**Answer:**

Keeping `/boot` separate simplifies the boot process, isolates boot-critical files, improves recovery, and ensures the bootloader can access the kernel before the root filesystem is fully available.

---

## Q3. What is the purpose of initramfs?

**Answer:**

`initramfs` provides the temporary userspace environment required during early boot. It loads the necessary drivers and modules needed to locate and mount the real root filesystem.

---

# Key Takeaways

- `/boot` stores boot-critical files.
- `vmlinuz` is the Linux kernel image.
- `initramfs` prepares the system before the real root filesystem is mounted.
- `System.map` stores kernel symbol mappings.
- `config` stores kernel build configuration.
- `grub2` contains bootloader-related files.
- `/boot/efi` is the EFI System Partition on UEFI systems.
- Enterprise systems often maintain multiple kernel versions for recovery.
- `/boot` is frequently implemented as a separate filesystem mounted on the `/boot` directory.

---

# 5. /dev - Device Files

## What is `/dev`?

The `/dev` directory contains **device files** that provide an interface between user-space applications and hardware devices.

Unlike ordinary files, device files do **not** store user data. Instead, they represent hardware devices such as:

- Hard Disk Drives (HDD)
- Solid State Drives (SSD)
- NVMe Drives
- USB Devices
- Keyboards
- Mouse
- Terminals
- Printers
- Consoles

Linux follows the UNIX philosophy:

> **Everything is a file.**

This means that hardware devices are exposed through special files located under the `/dev` directory.

Applications communicate with hardware by reading from or writing to these device files.

---

# Why Does Linux Use Device Files?

Imagine an application wants to read data from an NVMe SSD.

Instead of communicating directly with the hardware, Linux provides a device file:

```text
/dev/nvme0n1
```

The application simply opens this file.

Internally, Linux redirects the request to the appropriate kernel driver, which communicates with the physical hardware.

This abstraction allows applications to use the same file operations (`open`, `read`, `write`, `close`) regardless of the underlying hardware.

---

# Device File Communication Flow

```text
Application
      │
      ▼
Device File (/dev/nvme0n1)
      │
      ▼
Linux Kernel
      │
      ▼
Device Driver
      │
      ▼
Physical Hardware
```

The device file acts as an **interface** between software and hardware.

---

# Everything is a File

The phrase **"Everything is a file"** does not mean that a hard disk is literally a regular file.

Instead, it means Linux provides a **file-like interface** for interacting with hardware devices.

For example:

```bash
dd if=/dev/nvme0n1 of=backup.img bs=1M
```

Here:

- `dd` opens `/dev/nvme0n1`.
- The kernel forwards the request to the NVMe driver.
- The driver reads data from the actual SSD.
- The data is written into `backup.img`.

The device file itself does not contain the SSD data. It is simply the access point used by the kernel.

---

# Types of Device Files

Linux represents devices using two primary types of device files:

- Character Devices
- Block Devices

These can be identified using the first character in the file permissions.

Example:

```text
crw-------
```

The leading `c` indicates a **Character Device**.

Example:

```text
brw-rw----
```

The leading `b` indicates a **Block Device**.

---

# Character Devices

Character devices transfer data one character (or byte) at a time.

Examples include:

- Keyboard
- Mouse
- Serial Port
- Console
- Terminal

These devices process a continuous stream of characters.

Example:

```text
Keyboard

H
↓

e
↓

l
↓

l
↓

o
```

Each character is processed sequentially.

---

# Block Devices

Block devices transfer data in fixed-size blocks rather than individual characters.

Examples include:

- HDD
- SSD
- NVMe
- USB Storage
- SAN LUNs

Storage devices are optimized to read and write blocks of data, making them significantly more efficient for large data transfers.

For example, when writing a 1 GB file, the operating system performs block-based I/O rather than transferring one byte at a time.

This is why storage devices are represented as **block devices**.

---

# Practical Verification

On our RHEL 9.8 system:

```bash
ls -l /dev | head -20
```

Output included entries such as:

```text
crw-------  acpi_thermal_rel
brw-rw----  dm-0
brw-rw----  dm-1
brw-rw----  dm-2
```

Here:

- `c` = Character Device
- `b` = Block Device

---

# Major Number and Minor Number

Every device file is associated with two numbers:

```text
Major Number, Minor Number
```

Example:

```text
brw-rw---- 253,0 dm-0
```

Here:

- Major Number = 253
- Minor Number = 0

---

## Major Number

The Major Number identifies the **kernel driver** responsible for managing the device.

Example:

```text
Major Number
      │
      ▼
NVMe Driver
SCSI Driver
Device Mapper
```

When an application accesses a device file, the kernel first examines the Major Number to determine which driver should handle the request.

---

## Minor Number

The Minor Number identifies the **specific device** managed by that driver.

For example:

```text
Major 259
```

identifies the NVMe driver.

The Minor Number distinguishes individual devices handled by that driver.

Example:

```text
259,0 → Whole NVMe Disk

259,1 → Partition 1

259,2 → Partition 2

259,3 → Partition 3
```

The driver remains the same, while the Minor Number identifies the specific disk or partition.

---

# Practical Verification

Our RHEL system:

```bash
ls -l /dev/nvme*
```

Output:

```text
crw------- 240,0 /dev/nvme0
brw-rw---- 259,0 /dev/nvme0n1
brw-rw---- 259,1 /dev/nvme0n1p1
brw-rw---- 259,2 /dev/nvme0n1p2
brw-rw---- 259,3 /dev/nvme0n1p3
```

Observation:

- Major Number (259) remains the same because all devices are managed by the same NVMe driver.
- Minor Numbers identify the individual disk and its partitions.

Similarly:

```bash
ls -l /dev/sd*
```

Output:

```text
brw-rw---- 8,0 /dev/sda
brw-rw---- 8,1 /dev/sda1
```

Again:

- Major Number identifies the storage driver.
- Minor Number identifies the individual device.

---

# Who Creates Device Files?

Device files are not created manually.

The process is:

```text
New Hardware
      │
      ▼
Kernel Detects Device
      │
      ▼
Kernel Driver
      │
      ▼
Kernel Generates uevent
      │
      ▼
systemd-udevd
      │
      ▼
Creates Device File
      │
      ▼
/dev/...
```

The Linux kernel detects the hardware.

It then generates a **uevent**.

The **systemd-udevd** service receives this event and creates the corresponding device file under `/dev`.

---

# Verifying udev

We verified that the service was running.

```bash
systemctl status systemd-udevd
```

Output:

```text
Active: active (running)
```

We also confirmed the installed udev version.

```bash
udevadm info --version
```

Output:

```text
252
```

---

# Persistent Device Naming

Device names such as:

```text
/dev/sda
/dev/sdb
```

are assigned dynamically.

Their names may change after:

- Hardware changes
- Reboot
- Storage reconfiguration

For this reason, enterprise Linux systems use persistent identifiers.

Examples:

```text
/dev/disk/by-id
/dev/disk/by-uuid
/dev/disk/by-path
```

These are symbolic links created by `udev`.

---

## by-uuid

Each filesystem receives a unique identifier.

Example:

```text
/dev/disk/by-uuid/
```

These UUIDs remain constant even if the device name changes.

For this reason, `/etc/fstab` typically uses UUIDs rather than `/dev/sdX`.

---

## by-id

This identifies the storage hardware itself.

Example:

```text
nvme-BC711_NVMe_SK_hynix_256GB...
```

This identifier is based on the device's hardware identity, such as its model and serial number.

---

## by-path

This identifies the physical connection path of the storage device.

It is particularly useful for hardware troubleshooting.

Example:

```text
pci-...-nvme-1
```

---

# Storage Engineer Perspective

Storage Engineers frequently work with:

- SAN LUNs
- NVMe Drives
- Multipath Devices
- LVM
- RAID

Using persistent identifiers such as UUIDs or `/dev/disk/by-id` ensures that the correct storage device is always referenced, even if Linux assigns a different `/dev/sdX` name after a reboot.

---

# Interview Notes

### Q1. What is the purpose of `/dev`?

**Answer:**

`/dev` contains device files that provide an interface between user-space applications and hardware devices.

---

### Q2. What is the difference between a Character Device and a Block Device?

**Answer:**

Character devices transfer data one character (or byte) at a time, whereas block devices transfer data in fixed-size blocks and are typically used for storage devices.

---

### Q3. What is the purpose of Major and Minor Numbers?

**Answer:**

The Major Number identifies the kernel driver responsible for handling the device, while the Minor Number identifies the specific device managed by that driver.

---

### Q4. Who creates the files under `/dev`?

**Answer:**

The Linux kernel detects hardware and generates a device event (uevent). The `systemd-udevd` service processes the event and creates the corresponding device files under `/dev`.

---

### Q5. Why are UUIDs preferred over `/dev/sdX` in `/etc/fstab`?

**Answer:**

Device names such as `/dev/sda` are assigned dynamically and may change after reboots or hardware changes. UUIDs remain constant, ensuring the correct filesystem is always mounted.

---

# Key Takeaways

- `/dev` contains device files, not ordinary user data.
- Device files provide an interface between applications and hardware.
- Linux follows the "Everything is a file" philosophy.
- Character devices transfer data one byte at a time.
- Block devices transfer data in fixed-size blocks.
- Major Numbers identify kernel drivers.
- Minor Numbers identify specific devices managed by those drivers.
- `systemd-udevd` creates device files based on kernel-generated uevents.
- Enterprise systems use persistent device names such as UUIDs and `/dev/disk/by-id` instead of relying on `/dev/sdX`.

