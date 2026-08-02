# 01. Shell (Bash)

## 1. What is a Shell?

A **Shell** is a command-line interpreter that provides an interface between the **user** and the **Linux operating system**.

It accepts commands entered by the user, interprets them, passes them to the Linux kernel for execution, and displays the output back to the user.

In simple terms, the shell acts as a **translator** between humans and the Linux kernel.

---

## Why is a Shell Needed?

The Linux kernel is the core of the operating system, but it cannot directly understand commands typed by users.

For example, when you type:

```bash
ls
```

the kernel does not know what `ls` means.

The shell first interprets the command, identifies the executable program associated with `ls`, requests the kernel to execute it, and then displays the result on the screen.

Without a shell, users would not be able to interact with the operating system through commands.

---

## Communication Flow

The interaction between the user and Linux follows this sequence:

```text
+---------+
|  User   |
+---------+
     │
     ▼
+------------------+
|  Shell (Bash)    |
+------------------+
     │
     ▼
+------------------+
| Linux Kernel     |
+------------------+
     │
     ▼
+------------------+
|    Hardware      |
+------------------+
```

### Step-by-Step Process

1. The user enters a command.
2. The shell reads and interprets the command.
3. The shell requests the Linux kernel to execute the command.
4. The kernel communicates with the required hardware or system resources.
5. The result is returned to the shell.
6. The shell displays the output to the user.

---

## Real Example

Suppose you execute:

```bash
pwd
```

The sequence is:

```text
User
 │
 │  pwd
 ▼
Shell (Bash)
 │
 │ Executes /usr/bin/pwd
 ▼
Kernel
 │
 │ Retrieves current working directory
 ▼
Shell
 │
 ▼
/home/krishnatejanuti
```

The shell hides all the complexity from the user and provides a simple interface for interacting with the operating system.

---

## Key Points

- A shell is a command-line interpreter.
- It acts as an interface between the user and the Linux kernel.
- It interprets user commands and requests the kernel to execute them.
- It displays the output returned by the kernel.
- Most Linux system administration tasks are performed through the shell.
- **Bash (Bourne Again SHell)** is the default shell on most Linux distributions.

---

## 2. Why Do We Need a Shell?

A shell is required because the **Linux kernel cannot communicate directly with users**.

The kernel is responsible for managing hardware resources such as the CPU, memory, storage devices, and input/output devices. However, it does not understand commands like `ls`, `pwd`, or `mkdir` typed by a user.

The shell acts as an interpreter that understands user commands and translates them into requests that the Linux kernel can execute.

Without the shell, users would have no convenient way to interact with the operating system.

---

## Responsibilities of the Shell

The shell performs several important tasks:

- Accepts commands entered by the user.
- Interprets and validates commands.
- Locates executable programs.
- Requests the Linux kernel to execute commands.
- Displays command output or error messages.
- Supports shell scripting for automation.
- Manages environment variables.
- Handles input/output redirection and pipes.

---

## Real-World Example

Suppose you type:

```bash
mkdir Storage
```

The sequence is:

```text
User
 │
 │ mkdir Storage
 ▼
Shell (Bash)
 │
 │ Finds the mkdir executable
 │
 ▼
Linux Kernel
 │
 │ Creates a new directory
 ▼
Filesystem
 │
 ▼
Storage/
```

The shell makes this entire process simple for the user.

---

## Another Example

When you type:

```bash
ls
```

The shell:

1. Reads the command.
2. Searches for the `ls` executable using the `PATH` environment variable.
3. Requests the kernel to execute the program.
4. Receives the output.
5. Displays the directory contents on the terminal.

Without the shell, this process would have to be performed manually using low-level system calls.

---

## Why is the Shell Important for Linux Administrators?

Most enterprise Linux servers do not have a graphical user interface (GUI).

Administrators connect remotely using tools such as SSH and manage the entire server through the shell.

Common administrative tasks performed using the shell include:

- Managing files and directories
- Monitoring system performance
- Managing users and groups
- Installing software
- Configuring networking
- Managing storage devices
- Writing automation scripts
- Troubleshooting system issues

For this reason, a strong understanding of the shell is one of the most important skills for a Linux Administrator or Storage Engineer.

---

## Key Points

- The shell provides a user-friendly interface to the Linux operating system.
- It interprets user commands and communicates with the Linux kernel.
- It simplifies system administration by hiding low-level kernel interactions.
- Almost all enterprise Linux administration is performed through the shell.
- Learning the shell is the first step toward mastering Linux administration.

---

## 4. Shell vs Kernel

Although the **Shell** and the **Kernel** work closely together, they perform completely different roles in the Linux operating system.

A common misconception among beginners is that the shell is part of the kernel. In reality, the shell is a **user-space program**, while the kernel is the **core of the operating system**.

The shell acts as an intermediary between the user and the kernel.

---

## Understanding the Difference

Think of the Linux operating system as a company.

```text
                Linux Operating System

        +------------------------------+
        |          User                |
        +--------------+---------------+
                       |
                       v
        +------------------------------+
        |        Shell (Bash)          |
        | Accepts user commands        |
        +--------------+---------------+
                       |
                       v
        +------------------------------+
        |      Linux Kernel            |
        | Manages system resources     |
        +--------------+---------------+
                       |
                       v
        +------------------------------+
        | CPU | RAM | Disk | Network   |
        | Hardware Devices             |
        +------------------------------+
```

The user never communicates directly with the kernel.

Instead, the shell receives user commands and forwards appropriate requests to the kernel.

---

## Responsibilities of the Shell

The shell is responsible for:

- Accepting user commands.
- Interpreting command syntax.
- Searching for executable programs.
- Executing shell scripts.
- Managing environment variables.
- Handling input/output redirection.
- Managing pipes and command chaining.
- Displaying command output.

Examples:

```bash
ls

pwd

mkdir Linux

grep error logfile.txt
```

All these commands are processed by the shell before execution.

---

## Responsibilities of the Kernel

The kernel is responsible for managing the entire operating system.

Its responsibilities include:

- Process management
- Memory management
- CPU scheduling
- Device management
- Filesystem management
- Storage management
- Network management
- Security and permissions

The kernel never interacts directly with users.

It only responds to requests made through system calls from applications or the shell.

---

## Comparison

| Shell | Kernel |
|--------|--------|
| User Interface | Core of the Operating System |
| Runs in User Space | Runs in Kernel Space |
| Accepts user commands | Executes system operations |
| Can be replaced (Bash, Zsh, Ksh, etc.) | Cannot be replaced while the system is running |
| Executes programs | Manages hardware resources |

---

## Real Example

Suppose the user types:

```bash
mkdir Storage
```

Execution flow:

```text
User
 │
 │ mkdir Storage
 ▼
Shell
 │
 │ Validates command
 │
 │ Finds executable
 ▼
Kernel
 │
 │ Creates new directory
 ▼
Filesystem
 │
 ▼
Storage/
```

Notice that the shell never creates the directory itself.

It requests the kernel to perform the operation.

---

## Storage Engineer Perspective

Understanding the difference between the shell and the kernel is important because many storage operations involve both.

For example:

```bash
mount /dev/sdb1 /mnt
```

- The **Shell** interprets the command.
- The **Kernel** mounts the filesystem.
- The **Storage Device** becomes accessible to applications.

Similarly, commands such as:

- `lsblk`
- `fdisk`
- `mkfs`
- `mount`
- `umount`

are entered through the shell, but the actual storage operations are performed by the kernel.

---

## Interview Questions

### 1. What is the difference between the Shell and the Kernel?

**Answer:**

The shell is a command-line interpreter that accepts user commands, while the kernel is the core of the operating system that manages hardware resources and executes system operations.

---

### 2. Does the Shell communicate directly with hardware?

**Answer:**

No.

The shell communicates with the Linux kernel, and the kernel communicates with the hardware.

---

### 3. Can Linux work without a Shell?

**Answer:**

The Linux kernel can run without a shell, but users cannot interact with the system through commands without one.

---

## Key Points

- The shell is an interface between the user and the kernel.
- The kernel is the core of the operating system.
- The shell interprets commands.
- The kernel executes system-level operations.
- Together, they provide a complete user interaction model for Linux.

---

## 5. Types of Shells

Linux supports multiple types of shells.

Although they all perform the same basic function—providing an interface between the user and the Linux kernel—they differ in syntax, features, scripting capabilities, and user experience.

Different organizations and users choose different shells based on their requirements.

---

## Common Types of Shells

| Shell | Full Form | Description |
|--------|-----------|-------------|
| `sh` | Bourne Shell | The original UNIX shell developed by Stephen Bourne. |
| `bash` | Bourne Again Shell | The most widely used shell in Linux. An enhanced version of the Bourne Shell. |
| `ksh` | Korn Shell | Combines features of Bourne Shell and C Shell. Popular in enterprise UNIX systems. |
| `csh` | C Shell | Uses syntax similar to the C programming language. |
| `tcsh` | TENEX C Shell | An improved version of the C Shell with command-line editing and auto-completion. |
| `zsh` | Z Shell | An advanced shell with powerful customization, plugins, and auto-completion features. |

---

## 1. Bourne Shell (`sh`)

The Bourne Shell (`sh`) was one of the first command-line shells developed for UNIX.

Features:

- Simple command interpreter.
- Supports shell scripting.
- Portable across UNIX systems.
- Foundation for many modern shells.

Although `sh` is still available on most Linux systems, it is rarely used as the default interactive shell today.

---

## 2. Bourne Again Shell (`bash`)

Bash stands for **Bourne Again SHell**.

It was developed as an improved replacement for the Bourne Shell.

Features:

- Command history.
- Command-line editing.
- Tab completion.
- Variables and aliases.
- Powerful scripting support.
- Job control.
- Command substitution.

Bash is the default shell on most Linux distributions such as:

- Rocky Linux
- RHEL
- Ubuntu
- Debian
- Fedora

---

## 3. Korn Shell (`ksh`)

The Korn Shell was designed to improve upon the Bourne Shell.

Features:

- Better scripting capabilities.
- Arithmetic operations.
- Command history.
- Job control.

It is still commonly found on enterprise UNIX systems such as IBM AIX and Oracle Solaris.

---

## 4. C Shell (`csh`)

The C Shell was designed for users familiar with the C programming language.

Features:

- C-like syntax.
- Aliases.
- Command history.

It is less commonly used on modern Linux systems.

---

## 5. TENEX C Shell (`tcsh`)

`tcsh` is an enhanced version of the C Shell.

Additional features include:

- Command-line editing.
- Filename completion.
- Improved command history.

---

## 6. Z Shell (`zsh`)

The Z Shell is one of the most feature-rich shells available.

Features:

- Advanced auto-completion.
- Plugin support.
- Themes.
- Better customization.
- Improved scripting features.

It is popular among developers and power users.

---

## Which Shell is Used Most?

Today, **Bash** is the most widely used shell on Linux systems.

Reasons include:

- Installed by default on most distributions.
- Stable and mature.
- Excellent scripting capabilities.
- Extensive documentation.
- Strong community support.
- Widely used in enterprise environments.

---

## How to Check Your Current Shell

Display the current shell:

```bash
echo $SHELL
```

Example output:

```text
/bin/bash
```

Check the shell currently running:

```bash
ps -p $$
```

Example output:

```text
PID TTY          TIME CMD
2458 pts/0   00:00:00 bash
```

---

## Storage Engineer Perspective

For Linux and Storage Engineers, **Bash** is the standard shell.

Almost all enterprise automation scripts, storage validation scripts, maintenance scripts, and system administration tasks are written using Bash.

While it is useful to know that other shells exist, mastering **Bash** should be the primary goal.

---

## Interview Questions

### 1. What are the different types of shells available in Linux?

**Answer:**

Common Linux shells include:

- Bourne Shell (`sh`)
- Bourne Again Shell (`bash`)
- Korn Shell (`ksh`)
- C Shell (`csh`)
- TENEX C Shell (`tcsh`)
- Z Shell (`zsh`)

---

### 2. Which shell is the default on most Linux distributions?

**Answer:**

**Bash (Bourne Again SHell)** is the default shell on most Linux distributions.

---

### 3. How do you check your current shell?

**Answer:**

```bash
echo $SHELL
```

---

## Key Points

- Linux supports multiple command-line shells.
- Each shell has its own features and scripting capabilities.
- Bash is the most commonly used shell in Linux.
- Most Linux administration and enterprise automation tasks are performed using Bash.

---

## 6. What is Bash?

**Bash** stands for **Bourne Again SHell**.

It is a command-line interpreter developed as an enhanced replacement for the original **Bourne Shell (`sh`)**.

Bash is the default shell on most Linux distributions because it combines ease of use, powerful scripting capabilities, and extensive administrative features.

For Linux Administrators, DevOps Engineers, and Storage Engineers, Bash is the primary tool used to interact with Linux systems.

---

## Why is Bash So Popular?

Bash became the default shell because it provides several features that make system administration easier.

Some of its advantages include:

- Easy to learn and use.
- Powerful scripting capabilities.
- Command history.
- Tab auto-completion.
- Command aliases.
- Environment variable support.
- Job control.
- Input and output redirection.
- Pipes for combining commands.

These features make Bash suitable for both beginners and experienced administrators.

---

## Features of Bash

### 1. Command History

Bash remembers previously executed commands.

You can navigate through command history using:

- **↑ (Up Arrow)** – Previous command
- **↓ (Down Arrow)** – Next command

Or display the complete history:

```bash
history
```

---

### 2. Auto Completion

Press the **Tab** key while typing a command or filename.

Example:

```bash
cd Doc<Tab>
```

Bash automatically completes the directory name if it is unique.

This saves time and reduces typing errors.

---

### 3. Command Editing

Bash allows editing commands before execution.

Useful keys:

| Key | Action |
|------|--------|
| ← → | Move cursor |
| Ctrl + A | Move to beginning of line |
| Ctrl + E | Move to end of line |
| Ctrl + U | Delete from cursor to beginning |
| Ctrl + K | Delete from cursor to end |
| Ctrl + L | Clear the terminal screen |

These shortcuts improve productivity during command-line work.

---

### 4. Aliases

An alias creates a shortcut for a command.

Example:

```bash
alias ll='ls -lh'
```

Now instead of typing:

```bash
ls -lh
```

you can simply type:

```bash
ll
```

Display all aliases:

```bash
alias
```

---

### 5. Environment Variables

Bash stores important system information in environment variables.

Examples:

```bash
echo $HOME

echo $PATH

echo $USER

echo $SHELL
```

We'll study environment variables in detail in a later chapter.

---

### 6. Shell Scripting

Bash allows multiple commands to be written into a script file.

Example:

```bash
#!/bin/bash

echo "Hello Linux"
date
pwd
```

Instead of typing commands repeatedly, administrators can automate tasks using Bash scripts.

Shell scripting will be covered in Module 13.

---

## Where is Bash Located?

Find the Bash executable:

```bash
which bash
```

Example output:

```text
/usr/bin/bash
```

Display the Bash version:

```bash
bash --version
```

Example:

```text
GNU bash, version 5.x
```

---

## Real-World Usage

A Linux administrator may use Bash to:

- Monitor system health.
- Create users.
- Manage storage devices.
- Check log files.
- Install software.
- Automate backups.
- Restart services.
- Troubleshoot servers.

Almost every administrative task begins with the Bash shell.

---

## Storage Engineer Perspective

Storage Engineers rely heavily on Bash for day-to-day operations.

Typical tasks include:

- Discovering new disks.
- Checking mounted filesystems.
- Monitoring storage usage.
- Collecting diagnostic information.
- Running validation scripts.
- Automating repetitive storage tasks.

Most enterprise storage automation is written using Bash.

Learning Bash well will make later topics such as LVM, filesystem management, and troubleshooting much easier.

---

## Hands-on Lab

Run the following commands:

```bash
echo $SHELL

which bash

bash --version

history

alias

echo $HOME

echo $PATH
```

Observe:

- Which shell are you using?
- Where is the Bash executable located?
- Which version of Bash is installed?
- What information is stored in your environment variables?

---

## Interview Questions

### 1. What does Bash stand for?

**Answer:**

**Bash** stands for **Bourne Again SHell**.

---

### 2. Why is Bash the default shell on most Linux systems?

**Answer:**

Because it provides powerful scripting capabilities, command history, auto-completion, aliases, job control, and strong community support.

---

### 3. How do you check the current Bash version?

**Answer:**

```bash
bash --version
```

---

### 4. How do you find the location of the Bash executable?

**Answer:**

```bash
which bash
```

---

## Summary

- Bash is the default shell on most Linux distributions.
- It is an enhanced version of the Bourne Shell.
- Bash provides features such as command history, auto-completion, aliases, scripting, and environment variables.
- It is the primary interface used by Linux administrators and Storage Engineers for system management and automation.

---

## 7. Interactive vs Non-Interactive Shell

Bash can operate in two different modes:

- **Interactive Shell**
- **Non-Interactive Shell**

The difference depends on **how the shell is started** and **whether it expects input from a user**.

Understanding these two modes is important because they determine how Bash behaves, which configuration files are read, and how commands are executed.

---

## Interactive Shell

An **Interactive Shell** is a shell session where the user directly interacts with the operating system by entering commands.

The shell waits for user input, executes commands, displays the output, and then waits for the next command.

Example:

```bash
krishnatejanuti@hostname:~$
```

This prompt indicates that you are working in an interactive shell.

Examples of interactive sessions:

- Opening the Terminal application.
- Connecting to a Linux server using SSH.
- Switching users using:

```bash
su -
```

or

```bash
sudo -i
```

Characteristics:

- Displays a command prompt.
- Waits for user input.
- Supports command history.
- Supports tab completion.
- Supports command-line editing.
- Executes one command at a time.

---

## Non-Interactive Shell

A **Non-Interactive Shell** executes commands automatically without waiting for user input.

Instead of displaying a prompt, it reads commands from a script or another program.

Example:

```bash
#!/bin/bash

echo "Backup Started"

date

echo "Backup Completed"
```

When executed:

```bash
./backup.sh
```

Bash runs every command in the script automatically and exits after completion.

Examples of non-interactive shells:

- Running a Bash script.
- Cron jobs.
- Automation scripts.
- CI/CD pipelines.
- Startup scripts.

Characteristics:

- Does not display a prompt.
- Does not wait for user input.
- Executes commands sequentially.
- Terminates after execution.

---

## Interactive vs Non-Interactive Shell

| Interactive Shell | Non-Interactive Shell |
|-------------------|-----------------------|
| User enters commands manually | Commands are read from a script or program |
| Displays a command prompt | No command prompt |
| Waits for user input | Executes automatically |
| Supports command history | No command history during execution |
| Used for daily administration | Used for automation and scripting |

---

## Real-World Example

### Interactive Shell

```bash
$ ls

$ pwd

$ cd Documents

$ mkdir Linux
```

The shell waits after every command for the next user input.

---

### Non-Interactive Shell

File:

```bash
#!/bin/bash

mkdir Backup

date

ls
```

Execution:

```bash
bash backup.sh
```

The shell executes all commands automatically and exits when finished.

---

## Storage Engineer Perspective

Storage Engineers use both types of shells regularly.

### Interactive Shell

Used for:

- Checking storage devices.
- Mounting filesystems.
- Monitoring disk usage.
- Troubleshooting storage issues.
- Running administrative commands.

### Non-Interactive Shell

Used for:

- Daily backup scripts.
- Health check automation.
- Storage validation scripts.
- Log collection.
- Performance monitoring.
- Scheduled maintenance using cron.

Most enterprise storage automation is performed using **non-interactive Bash scripts**, while troubleshooting is usually performed in an **interactive shell**.

---

## Hands-on Lab

### Verify your current shell

```bash
echo $SHELL
```

### Open an interactive Bash shell

```bash
bash
```

Exit:

```bash
exit
```

### Create a simple Bash script

```bash
nano hello.sh
```

Add:

```bash
#!/bin/bash

echo "Hello Linux"
pwd
date
```

Make it executable:

```bash
chmod +x hello.sh
```

Run it:

```bash
./hello.sh
```

Observe that the commands execute automatically without waiting for user input.

---

## Interview Questions

### 1. What is an interactive shell?

**Answer:**

An interactive shell is a shell session where the user enters commands manually, receives output, and continues interacting with the system.

---

### 2. What is a non-interactive shell?

**Answer:**

A non-interactive shell executes commands automatically from a script or another program without waiting for user input.

---

### 3. Give examples of non-interactive shells.

**Answer:**

- Bash scripts
- Cron jobs
- Startup scripts
- CI/CD automation pipelines

---

### 4. Which type of shell is commonly used for Linux administration?

**Answer:**

An interactive shell is commonly used for day-to-day Linux administration, while non-interactive shells are used for automation.

---

## Summary

- Bash operates in two modes: Interactive and Non-Interactive.
- Interactive shells wait for user input and are used for daily administration.
- Non-interactive shells execute scripts automatically and are used for automation.
- Both modes are essential for Linux administrators and Storage Engineers.

---

## 8. Login Shell vs Non-Login Shell

A Bash shell can also be classified based on **how it is started**:

- **Login Shell**
- **Non-Login Shell**

This classification determines **which startup (initialization) files Bash reads** when it starts.

Understanding this concept is important because it explains why environment variables, aliases, and shell settings sometimes behave differently.

---

## What is a Login Shell?

A **Login Shell** is the first shell that starts after a user successfully logs into a Linux system.

Examples:

- Logging in through the Linux console.
- Logging in using SSH.
- Switching to another user using:

```bash
su -
```

or

```bash
sudo -i
```

When a login shell starts, Bash reads initialization files to set up the user's environment.

Common initialization files include:

```text
/etc/profile
~/.bash_profile
~/.bash_login
~/.profile
```

> **Note:** We'll study these files in detail later in the **Shell Initialization Files** section.

---

## What is a Non-Login Shell?

A **Non-Login Shell** is started after you are already logged in.

It does not perform a full login process.

Common examples:

- Opening a new Terminal window in GNOME or KDE.
- Starting another Bash session by typing:

```bash
bash
```

Since the user is already authenticated, Bash skips the login initialization files.

Instead, it typically reads:

```text
~/.bashrc
```

---

## Login Shell vs Non-Login Shell

| Login Shell | Non-Login Shell |
|--------------|-----------------|
| Started after user authentication | Started after login |
| Reads login initialization files | Reads `.bashrc` |
| Used during SSH login | Used when opening a new terminal |
| Performs full environment setup | Uses the existing environment |

---

## Real-World Example

### Login Shell

```text
Power On
      │
      ▼
User Login
      │
      ▼
Login Shell
```

Bash reads:

```text
/etc/profile

~/.bash_profile
```

---

### Non-Login Shell

After logging in:

```bash
bash
```

or opening another Terminal window.

Bash reads:

```text
~/.bashrc
```

The login process is **not** repeated.

---

## How to Check if Your Shell is a Login Shell

Run:

```bash
shopt login_shell
```

Example output:

```text
login_shell     on
```

or

```text
login_shell     off
```

- `on` → Login Shell
- `off` → Non-Login Shell

---

## Storage Engineer Perspective

As a Storage Engineer, you'll often connect to remote Linux servers using SSH.

The first shell you receive is usually a **login shell**, where important environment variables and user settings are initialized.

When you open additional Bash sessions or terminals, they are generally **non-login shells** that reuse the existing environment.

Understanding this behavior helps when troubleshooting issues related to:

- Missing environment variables
- Aliases not working
- PATH differences
- Shell configuration problems

---

## Hands-on Lab

### Check your current shell type

```bash
shopt login_shell
```

### Start another Bash session

```bash
bash
```

Check again:

```bash
shopt login_shell
```

Exit:

```bash
exit
```

Observe how the shell type changes.

---

## Interview Questions

### 1. What is a Login Shell?

**Answer:**

A login shell is the first shell started after a user successfully logs into the system. It performs full environment initialization by reading login startup files.

---

### 2. What is a Non-Login Shell?

**Answer:**

A non-login shell starts after the user is already logged in. It typically reads `.bashrc` instead of the login initialization files.

---

### 3. Which command checks whether the current shell is a login shell?

**Answer:**

```bash
shopt login_shell
```

---

## Key Points

- Login Shells start after user authentication.
- Non-Login Shells start after the user is already logged in.
- Login shells and non-login shells read different initialization files.
- Understanding the difference helps troubleshoot shell configuration and environment variable issues.
- This concept prepares you for learning **`.bashrc`**, **`.bash_profile`**, and other shell initialization files.

---

## 9. Bash Prompt

Every time you open a terminal, Bash displays a **prompt** indicating that it is ready to accept commands.

The prompt provides useful information about the current session, such as the logged-in user, the hostname, the current working directory, and the user's privilege level.

A typical Bash prompt looks like this:

```bash
krishnatejanuti@krishnateja-ws01:~$
```

---

## Understanding the Bash Prompt

Let's break it down:

```bash
krishnatejanuti@krishnateja-ws01:~$
```

| Part | Description |
|------|-------------|
| `krishnatejanuti` | Username of the currently logged-in user |
| `@` | Separator between username and hostname |
| `krishnateja-ws01` | Hostname (computer/server name) |
| `:` | Separator |
| `~` | Current working directory (Home Directory) |
| `$` | Indicates a normal user |

---

## Username

The first part of the prompt shows the currently logged-in user.

Example:

```bash
krishnatejanuti
```

To display the username:

```bash
whoami
```

Example output:

```text
krishnatejanuti
```

---

## Hostname

The hostname identifies the Linux system.

Example:

```bash
krishnateja-ws01
```

Display the hostname:

```bash
hostname
```

Example output:

```text
krishnateja-ws01
```

This is especially useful when managing multiple Linux servers.

---

## Current Working Directory

The prompt displays the current directory.

Example:

```bash
~
```

The symbol `~` represents the current user's home directory.

Example:

```bash
~
```

is equivalent to:

```bash
/home/krishnatejanuti
```

Display the current directory:

```bash
pwd
```

Example output:

```text
/home/krishnatejanuti
```

If you move to another directory:

```bash
cd /etc
```

The prompt changes to:

```bash
krishnatejanuti@krishnateja-ws01:/etc$
```

---

## User Privilege Symbol

The last character indicates the user's privilege level.

### Normal User

```bash
$
```

Example:

```bash
krishnatejanuti@krishnateja-ws01:~$
```

---

### Root User

```bash
#
```

Example:

```bash
root@krishnateja-ws01:~#
```

The `#` symbol indicates that the current shell has root (administrator) privileges.

Always be careful when working as the root user because commands can affect the entire system.

---

## How the Prompt Changes

Example:

Home directory:

```bash
krishnatejanuti@krishnateja-ws01:~$
```

Move to `/etc`:

```bash
cd /etc
```

Prompt:

```bash
krishnatejanuti@krishnateja-ws01:/etc$
```

Switch to the root user:

```bash
sudo -i
```

Prompt:

```bash
root@krishnateja-ws01:~#
```

The prompt automatically reflects the current user and location.

---

## Storage Engineer Perspective

When working in enterprise environments, you may manage dozens or even hundreds of Linux servers.

The Bash prompt helps you quickly identify:

- Which server you are connected to.
- Which user you are logged in as.
- Your current working directory.
- Whether you have root privileges.

Always verify the prompt before running administrative or storage-related commands, especially when connected to production systems.

---

## Hands-on Lab

Run the following commands:

```bash
whoami

hostname

pwd

cd /etc

pwd

cd

pwd

sudo -i

exit
```

Observe how the prompt changes as you switch directories and users.

---

## Interview Questions

### 1. What information does the Bash prompt display?

**Answer:**

The Bash prompt typically displays:

- Username
- Hostname
- Current working directory
- User privilege level

---

### 2. What does the `~` symbol represent?

**Answer:**

The `~` symbol represents the current user's home directory.

---

### 3. What is the difference between `$` and `#` in the Bash prompt?

**Answer:**

- `$` indicates a normal user.
- `#` indicates the root (administrator) user.

---

## Key Points

- The Bash prompt indicates that the shell is ready to accept commands.
- It provides useful information about the current user, host, directory, and privilege level.
- The prompt changes automatically as you navigate directories or switch users.
- Understanding the prompt helps prevent mistakes, especially when administering multiple Linux systems.

---

## 10. Basic Bash Commands

Bash provides several built-in commands that help users obtain information about the current shell and manage shell sessions.

These commands are commonly used by Linux administrators and are useful when working with the Bash environment.

---

## Check the Current Shell

Display the default login shell for the current user.

```bash
echo $SHELL
```

Example output:

```text
/bin/bash
```

---

## Display the Bash Version

Check the installed version of Bash.

```bash
bash --version
```

Example output:

```text
GNU bash, version 5.x
```

---

## Find the Bash Executable

Locate the Bash executable on the system.

```bash
which bash
```

Example output:

```text
/usr/bin/bash
```

---

## View Command History

Display previously executed commands.

```bash
history
```

Example output:

```text
1  pwd
2  ls
3  cd Documents
4  mkdir Linux
5  history
```

---

## View Configured Aliases

Display all aliases available in the current shell.

```bash
alias
```

Example output:

```text
alias ll='ls -l'
alias la='ls -A'
```

---

## Display Environment Variables

View commonly used environment variables.

```bash
echo $HOME

echo $USER

echo $PATH

echo $SHELL
```

---

## Start a New Bash Session

Launch another Bash shell.

```bash
bash
```

Exit the shell:

```bash
exit
```

---

## Check if the Current Shell is a Login Shell

```bash
shopt login_shell
```

Example output:

```text
login_shell    on
```

or

```text
login_shell    off
```

---

## Common Keyboard Shortcuts

| Shortcut | Description |
|-----------|-------------|
| `Ctrl + C` | Stop the running command |
| `Ctrl + D` | Exit the current shell |
| `Ctrl + L` | Clear the terminal screen |
| `Ctrl + A` | Move to the beginning of the line |
| `Ctrl + E` | Move to the end of the line |
| `Tab` | Auto-complete commands and file names |
| `↑` | Previous command |
| `↓` | Next command |

---

## Storage Engineer Perspective

These commands are frequently used while:

- Logging into Linux servers.
- Verifying the active shell.
- Checking Bash versions.
- Reviewing command history.
- Troubleshooting environment variable issues.
- Working on enterprise Linux systems.

Although simple, these commands become part of daily Linux administration.

---

## Hands-on Lab

Run the following commands:

```bash
echo $SHELL

bash --version

which bash

history

alias

echo $HOME

echo $PATH

shopt login_shell

bash

exit
```

Observe:

- Which shell are you using?
- Which version of Bash is installed?
- Where is Bash located?
- Is your current shell a login shell?
- What aliases are configured?

---

## Interview Questions

### 1. Which command displays the current shell?

```bash
echo $SHELL
```

---

### 2. Which command shows the installed Bash version?

```bash
bash --version
```

---

### 3. Which command locates the Bash executable?

```bash
which bash
```

---

### 4. Which command displays previously executed commands?

```bash
history
```

---

## Key Points

- Bash provides several built-in commands for managing the shell environment.
- Commands such as `echo $SHELL`, `history`, and `alias` are frequently used by Linux administrators.
- Keyboard shortcuts improve productivity while working in the terminal.
- Mastering these basic commands provides a strong foundation for learning Bash scripting and Linux administration.

---

## 11. Storage Engineer Perspective

For a Storage Engineer, the Bash shell is one of the most important tools used every day.

Although enterprise storage systems provide graphical interfaces, almost all storage validation, troubleshooting, automation, and administration tasks are performed from the Linux command line using Bash.

Common activities performed through Bash include:

- Discovering newly attached storage devices.
- Viewing block devices using `lsblk`.
- Creating and mounting filesystems.
- Checking disk usage and filesystem information.
- Monitoring storage performance.
- Viewing system and storage logs.
- Running storage validation scripts.
- Automating repetitive administrative tasks.
- Troubleshooting storage-related issues.

As your Linux journey continues, almost every storage concept—such as LVM, RAID, multipathing, filesystems, and storage protocols—will involve working in the Bash shell.

For this reason, becoming comfortable with Bash is an essential step toward becoming a proficient Linux Administrator or Storage Engineer.

---


