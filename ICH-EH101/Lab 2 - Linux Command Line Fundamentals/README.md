# Lab 2 — Linux Command Line Fundamentals

**Course Code:** ICH-EH101<br>
**Course Title:** Cybersecurity Fundamentals & Ethical Hacking<br>
**Provider:** Iconic Hub<br>
**Week:** 1<br>
**Lab:** 2

> **Secure. Build. Innovate.**
> *Where Web Development Meets Cybersecurity.*

---

# Course Focus

**Linux Command Line Fundamentals for Cybersecurity and Ethical Hacking**

---

# Main Goal

The command line is one of the most important environments in cybersecurity.

Security professionals regularly use Linux terminals to:

* Investigate systems
* Analyze files
* Search logs
* Examine network information
* Run security tools
* Automate repetitive tasks
* Manage permissions
* Collect evidence
* Perform authorized reconnaissance
* Configure laboratory environments

By the end of Lab 2, students should be able to confidently navigate a Linux system from the command line and understand the purpose of the commands they are using.

---

# Learning Objectives

By the end of this lab, students should be able to:

* Explain what Linux is.
* Explain what a Linux distribution is.
* Explain the purpose of Kali Linux.
* Understand the difference between GUI and CLI.
* Explain the Linux shell and Bash.
* Understand the Linux filesystem hierarchy.
* Navigate directories using the command line.
* Understand absolute and relative paths.
* Create and manage files and directories.
* Copy, move, rename, and delete files.
* Read and modify basic text files.
* Search for files and text.
* Use pipes and command redirection.
* Perform basic text processing.
* Understand basic Linux permissions.
* Collect basic system information.
* Create and extract compressed archives.
* Build an organized cybersecurity workspace.
* Understand safe and responsible use of Linux commands.

---

# 1. WHAT IS LINUX?

Linux is an open-source operating system kernel.

A complete Linux operating system is normally built by combining the Linux kernel with other software such as:

* System utilities
* Libraries
* Applications
* Package managers
* Configuration tools
* Desktop environments
* Command-line tools

A complete packaged Linux operating system is called a **Linux distribution**, or **distro**.

Examples include:

* Kali Linux
* Ubuntu
* Debian
* Fedora
* Arch Linux
* Linux Mint

---

# 2. WHAT IS A LINUX DISTRIBUTION?

A Linux distribution provides the components required to turn the Linux kernel into a usable operating system.

Different distributions are designed for different purposes.

### Ubuntu

Commonly used for:

* General computing
* Servers
* Development
* Cloud environments

### Debian

Known for:

* Stability
* Large software repositories
* Being the foundation for several other distributions

### Fedora

Often used for:

* Development
* Modern Linux technologies
* Enterprise-related experimentation

### Arch Linux

Known for:

* Customization
* Minimal installations
* Learning how Linux systems are assembled

### Kali Linux

Designed primarily for:

* Cybersecurity
* Penetration testing
* Security research
* Digital forensics
* Vulnerability assessment

---

# 3. WHAT IS KALI LINUX?

Kali Linux is a Debian-based Linux distribution developed for cybersecurity and security testing.

It includes many tools used by security professionals.

Examples of security activities include:

* Reconnaissance
* Network analysis
* Vulnerability assessment
* Web application testing
* Password auditing
* Wireless security testing
* Digital forensics
* Malware analysis

However, Kali Linux is still a Linux operating system.

Before learning specialized security tools, students need to understand the operating system itself.

That is why command-line fundamentals are important.

---

# 4. IMPORTANT ETHICAL RULE

Installing Kali Linux does **not** give someone permission to attack other systems.

The same rule from Lab 1 applies:

> **Authorization comes before testing.**

Only perform security testing against:

* Systems you own
* Your own virtual machines
* Intentionally vulnerable laboratory systems
* Systems for which you have explicit authorization

The commands in this lab are primarily normal Linux administration and learning commands, but the skills can later be applied to security testing.

---

# 5. GUI VS CLI

There are two common ways to interact with a Linux system.

## Graphical User Interface

A **GUI** allows users to interact with the system using:

* Windows
* Icons
* Menus
* Buttons
* Mouse
* Graphical applications

For example, opening Kali's file manager and clicking through folders is a GUI activity.

---

## Command Line Interface

A **CLI** allows users to interact with a computer by typing commands.

For example:

```bash
pwd
```

The computer executes the command and returns the result.

---

# 6. WHY CYBERSECURITY PROFESSIONALS USE THE CLI

The command line is extremely important in cybersecurity because it provides direct and efficient access to many system functions.

Security professionals use the CLI to:

### Investigate files

```bash
ls
find
file
```

### Search logs

```bash
grep
```

### Examine systems

```bash
whoami
id
uname
hostname
```

### Manage files

```bash
cp
mv
rm
```

### Analyze information

```bash
sort
uniq
cut
wc
```

### Run security tools

Many cybersecurity tools are primarily command-line based.

### Automate tasks

Commands can later be combined into scripts.

---

# 7. THE TERMINAL

A **terminal** is an interface through which users can interact with a command-line shell.

When you open a terminal in Kali Linux, you may see something similar to:

```text
┌──(user㉿kali)-[~]
└─$
```

The exact appearance depends on the user's configuration.

The important part is that the terminal allows you to enter commands.

---

# 8. WHAT IS A SHELL?

A shell is a program that interprets commands and communicates with the operating system.

One of the most common Linux shells is:

> **Bash — Bourne Again Shell**

For example:

```bash
ls
```

Bash interprets the command and asks Linux to execute it.

Other shells exist, but Bash is extremely common and is important for cybersecurity work.

---

# 9. UNDERSTANDING THE LINUX PROMPT

You may see something similar to:

```text
┌──(student㉿kali)-[~]
└─$
```

Different sections provide information.

For example:

```text
student
```

represents the current user.

```text
kali
```

represents the hostname.

```text
~
```

represents the user's home directory.

```text
$
```

normally indicates a regular user shell.

A root shell may traditionally use:

```text
#
```

---

# 10. THE LINUX FILESYSTEM

Linux uses a hierarchical filesystem.

At the top is:

```text
/
```

This is called the **root directory**.

Everything else exists underneath it.

For example:

```text
/
├── home
├── root
├── etc
├── var
├── tmp
├── usr
├── opt
└── dev
```

---

# 11. IMPORTANT LINUX DIRECTORIES

## `/`

The root of the entire filesystem.

```text
/
```

Do not confuse this with `/root`.

---

## `/home`

Contains the home directories of normal users.

Example:

```text
/home/student
```

---

## `/root`

The home directory belonging to the root user.

```text
/root
```

---

## `/etc`

Contains many system configuration files.

Examples include configuration for:

* Users
* Services
* Networking
* System components

---

## `/var`

Contains variable data.

Examples include:

* Logs
* Caches
* Spool files
* Application data

---

## `/tmp`

Used for temporary files.

---

## `/usr`

Contains many user-space programs, libraries, documentation, and other resources.

---

## `/opt`

Often used for optional or third-party software.

---

## `/dev`

Contains device files through which Linux interacts with hardware and other system resources.

---

# 12. FINDING YOUR CURRENT LOCATION

Use:

```bash
pwd
```

`pwd` means:

> **Print Working Directory**

Example:

```text
/home/student
```

This tells you exactly where you are in the filesystem.

### Practical Exercise

Run:

```bash
pwd
```

Then run:

```bash
ls
```

Observe the relationship between your current location and the files displayed.

---

# 13. LISTING FILES

The most basic command is:

```bash
ls
```

It lists files and directories in the current directory.

### Detailed listing

```bash
ls -l
```

This displays additional information such as:

* Permissions
* Owner
* Group
* File size
* Modification time

### Show hidden files

```bash
ls -a
```

Linux filenames beginning with `.` are normally hidden from a standard `ls` listing.

### Detailed listing including hidden files

```bash
ls -la
```

This is one of the most useful commands for inspecting a directory.

---

# 14. CHANGING DIRECTORIES

Use:

```bash
cd
```

`cd` means:

> **Change Directory**

Example:

```bash
cd /home
```

This moves into `/home`.

---

## Go to Your Home Directory

```bash
cd ~
```

You can also simply use:

```bash
cd
```

---

## Move Up One Directory

```bash
cd ..
```

The `..` means the parent directory.

---

## Go to the Filesystem Root

```bash
cd /
```

---

## Return to the Previous Directory

```bash
cd -
```

---

# 15. ABSOLUTE PATHS

An absolute path describes a location starting from the filesystem root.

Example:

```text
/home/student/Documents
```

Because it begins with `/`, Linux knows exactly where to start.

You can use:

```bash
cd /home/student/Documents
```

---

# 16. RELATIVE PATHS

A relative path describes a location based on your current directory.

Suppose you are currently here:

```text
/home/student
```

and there is a directory called:

```text
Documents
```

You can enter it using:

```bash
cd Documents
```

You do not need to provide the entire path.

---

# 17. SPECIAL PATH SYMBOLS

Linux provides several useful shortcuts.

| Symbol | Meaning                       |
| ------ | ----------------------------- |
| `/`    | Filesystem root               |
| `~`    | Current user's home directory |
| `.`    | Current directory             |
| `..`   | Parent directory              |

### Example

Suppose you are here:

```text
/home/student/Lab2
```

Running:

```bash
cd ..
```

takes you to:

```text
/home/student
```

Running:

```bash
cd ~
```

takes you to:

```text
/home/student
```

assuming that is your home directory.

---

# 18. CREATING DIRECTORIES

Use:

```bash
mkdir lab
```

This creates:

```text
lab/
```

### Create multiple directories

```bash
mkdir recon scans reports
```

### Create nested directories

```bash
mkdir -p project/evidence/screenshots
```

The `-p` option allows the required parent directories to be created.

---

# 19. CREATING A CYBERSECURITY WORKSPACE

Good organization is important during security assessments.

Create a dedicated Lab 2 workspace:

```bash
mkdir -p ~/Lab2/{recon/{hosts,services},scans,exploits,logs,evidence/{screenshots,packets},reports}
```

This creates:

```text
Lab2/
├── recon/
│   ├── hosts/
│   └── services/
├── scans/
├── exploits/
├── logs/
├── evidence/
│   ├── screenshots/
│   └── packets/
└── reports/
```

This structure will help students keep evidence and notes organized during future labs.

---

# 20. VERIFYING THE WORKSPACE

Enter the directory:

```bash
cd ~/Lab2
```

Check your location:

```bash
pwd
```

List the contents:

```bash
ls -la
```

Display the entire directory structure:

```bash
find ~/Lab2 -type d
```

---

# 21. CREATING FILES

Use:

```bash
touch notes.txt
```

Create several files:

```bash
touch scan.txt report.txt evidence.txt
```

Verify:

```bash
ls -la
```

---

# 22. COPYING FILES

Use:

```bash
cp notes.txt notes-backup.txt
```

This creates a copy.

You can also copy a file into another directory:

```bash
cp notes.txt ~/Lab2/reports/
```

Verify:

```bash
ls -la ~/Lab2/reports/
```

---

# 23. MOVING FILES

Use:

```bash
mv notes.txt ~/Lab2/reports/
```

This moves the file.

Verify:

```bash
ls -la ~/Lab2/reports/
```

---

# 24. RENAMING FILES

The `mv` command can also rename files.

Example:

```bash
mv report.txt final-report.txt
```

The file:

```text
report.txt
```

becomes:

```text
final-report.txt
```

---

# 25. REMOVING FILES

Use:

```bash
rm final-report.txt
```

Be careful with `rm`.

Unlike a graphical file manager, the command line may not provide a recycle-bin style recovery mechanism.

Always verify the target before deleting files.

---

# 26. REMOVING DIRECTORIES

Remove an empty directory:

```bash
rmdir oldfolder
```

Remove a directory and its contents:

```bash
rm -r oldfolder
```

Use recursive deletion carefully.

Never use destructive commands against directories unless you understand exactly what will be removed.

---

# 27. WRITING TEXT TO FILES

Use:

```bash
echo "Linux Command Line Fundamentals" > notes.txt
```

The `>` operator redirects output into a file.

If the file already contains information, `>` replaces its existing contents.

---

# 28. APPENDING TEXT

Use:

```bash
echo "Cybersecurity Fundamentals" >> notes.txt
```

The `>>` operator adds content to the end of the file.

Example:

```bash
echo "Linux Command Line Fundamentals" > notes.txt
echo "Cybersecurity Fundamentals" >> notes.txt
echo "Iconic Hub" >> notes.txt
```

---

# 29. READING FILE CONTENT

Use:

```bash
cat notes.txt
```

`cat` displays the contents of a file.

---

## `head`

Displays the beginning of a file:

```bash
head notes.txt
```

You can specify the number of lines:

```bash
head -n 5 notes.txt
```

---

## `tail`

Displays the end of a file:

```bash
tail notes.txt
```

Specify the number of lines:

```bash
tail -n 5 notes.txt
```

---

## `less`

Useful for larger files:

```bash
less notes.txt
```

Inside `less`:

```text
Arrow keys → Move
Space → Next page
q → Quit
```

---

# 30. SEARCHING TEXT WITH GREP

`grep` searches text for a specified pattern.

Example:

```bash
grep "ERROR" logfile.log
```

This displays lines containing:

```text
ERROR
```

### Case-insensitive search

```bash
grep -i "error" logfile.log
```

### Count matches

```bash
grep -c "ERROR" logfile.log
```

### Search recursively

```bash
grep -r "password" ~/Lab2
```

Be careful with recursive searches because they may return sensitive information if performed against inappropriate directories.

---

# 31. SEARCHING FOR FILES WITH FIND

Search for text files:

```bash
find ~/Lab2 -name "*.txt"
```

Search for files beginning with `scan`:

```bash
find ~/Lab2 -name "scan*"
```

Find all regular files:

```bash
find ~/Lab2 -type f
```

Find directories:

```bash
find ~/Lab2 -type d
```

---

# 32. UNDERSTANDING PIPES

The pipe operator is:

```text
|
```

It sends the output of one command to another command.

Example:

```bash
ls -la | grep ".txt"
```

The process is:

```text
ls -la
   ↓
Output
   ↓
grep ".txt"
   ↓
Only matching lines
```

Pipes are extremely useful in cybersecurity because they allow commands to be chained together.

---

# 33. COUNTING RESULTS WITH WC

The `wc` command can count:

* Lines
* Words
* Characters

Example:

```bash
wc -l notes.txt
```

Count error messages:

```bash
grep "ERROR" logfile.log | wc -l
```

This answers:

> How many lines contain `ERROR`?

---

# 34. SORTING RESULTS

Use:

```bash
sort users.txt
```

This sorts lines alphabetically.

This can be useful when processing lists such as:

* Usernames
* Domains
* Hostnames
* Log entries

---

# 35. REMOVING DUPLICATES WITH UNIQ

Use:

```bash
sort users.txt | uniq
```

The process is:

```text
users.txt
   ↓
sort
   ↓
uniq
   ↓
Unique sorted entries
```

`uniq` works best when duplicate entries are adjacent, which is why `sort` is commonly used first.

---

# 36. USING TR

The `tr` command can translate or replace characters.

Example:

```bash
echo "hello" | tr 'a-z' 'A-Z'
```

Output:

```text
HELLO
```

Another example:

```bash
echo "CYBERSECURITY" | tr 'A-Z' 'a-z'
```

Output:

```text
cybersecurity
```

---

# 37. USING CUT

`cut` extracts sections from structured text.

Example:

```bash
echo "admin:password123" | cut -d ':' -f 1
```

Output:

```text
admin
```

Here:

```text
-d ':'
```

specifies the delimiter.

And:

```text
-f 1
```

selects the first field.

---

# 38. COMMAND COMBINATION

The real power of the Linux command line comes from combining simple commands.

Example:

```bash
grep "ERROR" logfile.log | wc -l
```

This:

1. Searches for errors.
2. Sends the results to `wc`.
3. Counts the matching lines.

Another example:

```bash
ls -la | grep ".txt"
```

Another:

```bash
sort users.txt | uniq
```

The objective is not to memorize hundreds of commands.

The objective is to understand how simple commands can be combined to solve problems.

---

# 39. BASIC LINUX PERMISSIONS

Linux controls access to files and directories using permissions.

A permission string may look like:

```text
-rwxr-xr--
```

The permissions are divided into:

```text
Owner | Group | Others
```

The three basic permissions are:

```text
r = read
w = write
x = execute
```

---

# 40. UNDERSTANDING READ, WRITE AND EXECUTE

## Read

Allows the contents of a file to be read.

## Write

Allows the file to be modified.

## Execute

Allows an executable file or script to be run.

For directories, permissions have related meanings:

* Read → view directory contents
* Write → create/delete entries
* Execute → access/traverse the directory

---

# 41. NUMERIC PERMISSIONS

Linux commonly represents permissions using numbers.

```text
r = 4
w = 2
x = 1
```

Therefore:

```text
rwx = 4 + 2 + 1 = 7
```

```text
rw- = 4 + 2 = 6
```

```text
r-x = 4 + 1 = 5
```

---

# 42. COMMON PERMISSION EXAMPLE — 755

```text
755
```

means:

```text
Owner  → rwx
Group  → r-x
Others → r-x
```

The owner has:

```text
4 + 2 + 1 = 7
```

The group has:

```text
4 + 0 + 1 = 5
```

Others have:

```text
4 + 0 + 1 = 5
```

---

# 43. COMMON PERMISSION EXAMPLE — 644

```text
644
```

means:

```text
Owner  → rw-
Group  → r--
Others → r--
```

This is commonly seen on regular files.

---

# 44. BASIC SYSTEM INFORMATION

Cybersecurity professionals need to understand the environment they are working in.

### Current username

```bash
whoami
```

### User and group information

```bash
id
```

### Kernel/system information

```bash
uname -a
```

### Hostname

```bash
hostname
```

### Disk usage

```bash
df -h
```

### Directory size

```bash
du -sh ~/Lab2
```

---

# 45. WHY SYSTEM ENUMERATION MATTERS

Later in the course, students will perform authorized enumeration during security assessments.

Before using advanced tools, you should understand basic information about the system.

For example:

```bash
whoami
```

answers:

> Who am I?

```bash
hostname
```

answers:

> What is this machine called?

```bash
uname -a
```

provides information about:

> The operating system kernel and system architecture.

Understanding these basic commands makes more advanced enumeration easier to understand.

---

# 46. ARCHIVES

Cybersecurity professionals frequently work with:

* Evidence
* Logs
* Reports
* Configuration files
* Tool output
* Investigation data

These may need to be compressed and archived.

A common Linux tool is:

```bash
tar
```

---

# 47. CREATING A COMPRESSED ARCHIVE

Example:

```bash
tar -czf lab2.tar.gz ~/Lab2
```

Common options:

```text
-c = create
-z = gzip compression
-f = specify archive file
```

---

# 48. VIEWING AN ARCHIVE

Use:

```bash
tar -tzf lab2.tar.gz
```

Common options:

```text
-t = list contents
-z = gzip
-f = archive file
```

---

# 49. EXTRACTING AN ARCHIVE

Use:

```bash
tar -xzf lab2.tar.gz
```

Where:

```text
-x = extract
-z = gzip
-f = archive file
```

---

# 50. PRACTICAL LAB 2 — BUILD YOUR WORKSPACE

Now we combine what we have learned.

## Step 1 — Create the workspace

```bash
mkdir -p ~/Lab2/{recon/{hosts,services},scans,exploits,logs,evidence/{screenshots,packets},reports}
```

---

## Step 2 — Enter the workspace

```bash
cd ~/Lab2
```

---

## Step 3 — Confirm your location

```bash
pwd
```

Expected location should be similar to:

```text
/home/yourusername/Lab2
```

---

## Step 4 — List the workspace

```bash
ls -la
```

---

## Step 5 — Display the directory structure

```bash
find ~/Lab2 -type d
```

---

## Step 6 — Create a notes file

```bash
touch notes.txt
```

---

## Step 7 — Add information

```bash
echo "Linux Command Line Fundamentals" > notes.txt
echo "Cybersecurity Lab 2" >> notes.txt
echo "Iconic Hub" >> notes.txt
```

---

## Step 8 — Read the file

```bash
cat notes.txt
```

---

## Step 9 — Create a backup

```bash
cp notes.txt notes-backup.txt
```

---

## Step 10 — Verify

```bash
ls -la
```

---

# 51. PRACTICAL LAB 2 — LOG ANALYSIS

Create a sample log file:

```bash
cat > logfile.log <<'EOF'
INFO User login successful
ERROR Authentication failed
INFO User accessed dashboard
ERROR Authentication failed
WARNING Multiple login attempts
INFO User logged out
ERROR Authentication failed
EOF
```

Display the file:

```bash
cat logfile.log
```

Search for errors:

```bash
grep "ERROR" logfile.log
```

Count the errors:

```bash
grep -c "ERROR" logfile.log
```

Count all lines:

```bash
wc -l logfile.log
```

Use a pipe:

```bash
cat logfile.log | grep "ERROR"
```

Count errors using a pipe:

```bash
grep "ERROR" logfile.log | wc -l
```

---

# 52. PRACTICAL LAB 2 — FILE SEARCHING

Create test files:

```bash
touch scan.txt report.txt evidence.txt image.jpg
```

Find text files:

```bash
find ~/Lab2 -name "*.txt"
```

Find all files:

```bash
find ~/Lab2 -type f
```

Find all directories:

```bash
find ~/Lab2 -type d
```

---

# 53. PRACTICAL LAB 2 — SYSTEM INFORMATION

Run each command:

```bash
whoami
```

```bash
id
```

```bash
hostname
```

```bash
uname -a
```

```bash
df -h
```

```bash
du -sh ~/Lab2
```

Record what each command tells you.

---

# 54. PRACTICAL LAB 2 — ARCHIVING

Create an archive of your workspace:

```bash
tar -czf lab2.tar.gz ~/Lab2
```

Check that the archive exists:

```bash
ls -lh lab2.tar.gz
```

View its contents:

```bash
tar -tzf lab2.tar.gz
```

The archive can now be used as a basic example of packaging a laboratory workspace.

---

# 55. PRACTICAL CHALLENGE

Complete the following without copying another student's solution.

## Task 1

Create:

```text
cyber-lab/
```

Inside it create:

```text
cyber-lab/
├── notes/
├── scans/
├── evidence/
├── logs/
└── reports/
```

---

## Task 2

Create a file inside `notes`.

Example:

```text
notes.txt
```

---

## Task 3

Add at least three lines of information to the file.

---

## Task 4

Display the file contents using:

```bash
cat
```

---

## Task 5

Create a backup copy.

---

## Task 6

Rename the backup file.

---

## Task 7

Use `find` to locate the file.

---

## Task 8

Use `grep` to search for a word inside the file.

---

## Task 9

Use a pipe to combine two commands.

---

## Task 10

Collect your:

* Username
* Hostname
* Kernel information

---

## Task 11

Create a compressed archive of the entire `cyber-lab` directory.

---

# 56. SCREENSHOT AND EVIDENCE REQUIREMENTS

Students should document important practical activities.

## Screenshot 1 — Workspace

Show:

```bash
pwd
ls -la
```

The screenshot should demonstrate your current location and workspace contents.

---

## Screenshot 2 — Directory Structure

Show:

```bash
find ~/Lab2 -type d
```

This demonstrates that the required directories were created.

---

## Screenshot 3 — File Operations

Show:

```bash
ls -la
```

after creating, copying, and renaming files.

---

## Screenshot 4 — Text Searching

Show:

```bash
grep "ERROR" logfile.log
```

and:

```bash
grep -c "ERROR" logfile.log
```

---

## Screenshot 5 — System Information

Show:

```bash
whoami
hostname
uname -a
```

---

## Screenshot 6 — Archive

Show:

```bash
tar -tzf lab2.tar.gz
```

This demonstrates that the archive was successfully created and contains your workspace.

---

# 57. COMMON BEGINNER MISTAKES

## Mistake 1 — Not knowing where you are

Always check:

```bash
pwd
```

before performing file operations when you are unsure.

---

## Mistake 2 — Deleting the wrong file

Before using:

```bash
rm
```

check the filename carefully.

Use:

```bash
ls -la
```

first when necessary.

---

## Mistake 3 — Confusing `/` and `~`

Remember:

```text
/ = filesystem root
~ = your home directory
```

They are not the same.

---

## Mistake 4 — Confusing `/root` and `/`

Remember:

```text
/      → filesystem root
/root  → root user's home directory
```

---

## Mistake 5 — Using commands without understanding them

Do not copy commands blindly.

Before executing a command, understand:

* What it does
* What files it affects
* What output it should produce
* Whether it changes or deletes anything

---

# 58. COMMAND REFERENCE

| Command    | Purpose                                 |
| ---------- | --------------------------------------- |
| `pwd`      | Show current directory                  |
| `ls`       | List files                              |
| `ls -la`   | Detailed listing including hidden files |
| `cd`       | Change directory                        |
| `mkdir`    | Create directory                        |
| `touch`    | Create empty file                       |
| `cp`       | Copy file                               |
| `mv`       | Move or rename                          |
| `rm`       | Remove file                             |
| `rmdir`    | Remove empty directory                  |
| `cat`      | Display file contents                   |
| `head`     | Show beginning of file                  |
| `tail`     | Show end of file                        |
| `less`     | Read file interactively                 |
| `grep`     | Search text                             |
| `find`     | Search for files/directories            |
| `wc`       | Count lines/words/characters            |
| `sort`     | Sort text                               |
| `uniq`     | Remove adjacent duplicates              |
| `tr`       | Translate/replace characters            |
| `cut`      | Extract fields                          |
| `whoami`   | Show current user                       |
| `id`       | Show user/group information             |
| `hostname` | Show system hostname                    |
| `uname`    | Show system/kernel information          |
| `df`       | Show filesystem disk usage              |
| `du`       | Show directory/file size                |
| `tar`      | Create/extract archives                 |

---

# 59. KEY CONCEPTS TO REMEMBER

### Linux

An operating system family built around the Linux kernel.

### Distribution

A complete Linux operating system built around the Linux kernel.

### Kali Linux

A Debian-based distribution designed for cybersecurity and security testing.

### Terminal

An interface for interacting with the command line.

### Shell

A program that interprets commands.

### Bash

A widely used Linux shell.

### Filesystem

The structure Linux uses to organize files and directories.

### Absolute Path

A complete path starting from `/`.

### Relative Path

A path interpreted from the current directory.

### Pipe

Sends the output of one command to another.

### Permission

Controls what users can do with files and directories.

---

# 60. LAB 2 KNOWLEDGE CHECK

Before moving to the next lab, students should be able to answer:

### Question 1

What is Linux?

### Question 2

What is Kali Linux?

### Question 3

What is the difference between a GUI and CLI?

### Question 4

What does `pwd` do?

### Question 5

What does `ls -la` show?

### Question 6

What is the difference between:

```text
/
```

and:

```text
/root
```

### Question 7

What does `~` represent?

### Question 8

What is the difference between an absolute path and a relative path?

### Question 9

What does `grep` do?

### Question 10

What does `find` do?

### Question 11

What does the pipe symbol `|` do?

### Question 12

What do `r`, `w`, and `x` represent?

### Question 13

What does `whoami` show?

### Question 14

What does `uname -a` provide?

### Question 15

Why should you understand a command before executing it?

---

# 61. LAB 2 COMPLETION CHECKLIST

Before moving to Lab 3, students should be able to:

* [ ] Explain Linux
* [ ] Explain Linux distributions
* [ ] Explain Kali Linux
* [ ] Explain GUI and CLI
* [ ] Explain the shell
* [ ] Explain Bash
* [ ] Navigate the Linux filesystem
* [ ] Use `pwd`
* [ ] Use `ls`
* [ ] Use `cd`
* [ ] Understand absolute and relative paths
* [ ] Create directories with `mkdir`
* [ ] Create files with `touch`
* [ ] Copy files with `cp`
* [ ] Move and rename files with `mv`
* [ ] Safely remove files and directories
* [ ] Use `echo`
* [ ] Use `cat`
* [ ] Use `head`
* [ ] Use `tail`
* [ ] Use `less`
* [ ] Search text with `grep`
* [ ] Search files with `find`
* [ ] Use pipes
* [ ] Use `wc`
* [ ] Use `sort`
* [ ] Use `uniq`
* [ ] Use `tr`
* [ ] Use `cut`
* [ ] Understand basic Linux permissions
* [ ] Understand `r`, `w`, and `x`
* [ ] Gather basic system information
* [ ] Create and inspect archives
* [ ] Organize a cybersecurity workspace
* [ ] Document practical work with screenshots
* [ ] Apply ethical and authorized-use principles

---

The objective is to move from:

> **"I know Linux commands."**

to:

> **"I understand how Linux works and how a security professional can analyze it."**

---

# Iconic Hub

**Secure. Build. Innovate.**

**Where Web Development Meets Cybersecurity.**
