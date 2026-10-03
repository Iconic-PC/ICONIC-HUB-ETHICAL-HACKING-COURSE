# Lab 2 Assignment — Linux Command Line Fundamentals

**Course Code:** ICH-EH101<br>
**Course Title:** Cybersecurity Fundamentals & Ethical Hacking<br>
**Provider:** Iconic Hub<br>
**Week:** 1<br>
**Lab:** 2<br>
**Assignment Type:** Practical Take-Home Assignment

> **Secure. Build. Innovate.**
> *Where Web Development Meets Cybersecurity.*

---

# Assignment Title

## Linux Command Line Practical Assessment

---

# Assignment Overview

This assignment is designed to test your ability to use the Linux command line to perform basic system navigation, file management, text processing, searching, system enumeration, and evidence organization.

You are expected to perform the practical tasks on:

* Your own Kali Linux virtual machine
* Your own Linux system
* Or another authorized Linux laboratory environment

You should **not** perform these activities against systems that you do not own or have explicit permission to test.

---

# Learning Objectives

By completing this assignment, you should demonstrate that you can:

* Navigate the Linux filesystem.
* Create and manage directories.
* Create, copy, move, and rename files.
* Read and modify text files.
* Search files and text.
* Use pipes and command combinations.
* Perform basic system enumeration.
* Understand basic Linux permissions.
* Create a structured cybersecurity workspace.
* Create and inspect a compressed archive.
* Document practical work with screenshots.

---

# PART A — Linux Navigation

## Task 1 — Identify Your Environment

Open your Linux terminal and run:

```bash
whoami
```

Then:

```bash
hostname
```

Then:

```bash
uname -a
```

### Submit

Record:

1. Your current username
2. Your hostname
3. Your operating system/kernel information

### Evidence

Take a screenshot showing the three commands and their outputs.

---

# PART B — Filesystem Navigation

## Task 2 — Explore the Filesystem

Run:

```bash
pwd
```

Then:

```bash
ls -la
```

Navigate to:

```bash
/
```

using:

```bash
cd /
```

Run:

```bash
ls -la
```

Then return to your home directory:

```bash
cd ~
```

Run:

```bash
pwd
```

### Questions

Answer the following:

1. What directory did `pwd` initially display?
2. What is the purpose of `/`?
3. What does `~` represent?
4. What is the difference between `/` and `/root`?

---

# PART C — Create a Cybersecurity Workspace

## Task 3 — Build the Directory Structure

Create the following structure:

```text
Linux-Assessment/
├── notes/
├── logs/
├── scans/
├── evidence/
│   ├── screenshots/
│   └── packets/
└── reports/
```

You should create it using the command line.

### Requirement

Do not manually create the folders using the graphical file manager.

Use Linux commands.

### Evidence

Take a screenshot showing the commands you used.

Then verify the structure with:

```bash
find ~/Linux-Assessment -type d
```

Take another screenshot of the result.

---

# PART D — File Creation and Management

## Task 4 — Create Assessment Files

Inside the `notes` directory, create:

```text
system-notes.txt
commands.txt
```

Inside the `logs` directory, create:

```text
system.log
security.log
```

Inside the `reports` directory, create:

```text
initial-report.txt
```

You may use:

```bash
touch
```

to create the files.

---

# PART E — Writing and Reading Files

## Task 5 — Create Notes

Add at least five lines of information to:

```text
system-notes.txt
```

Your notes should include information such as:

* Linux distribution
* Username
* Hostname
* Current directory
* Date of assessment

You may use:

```bash
echo
```

and output redirection.

For example:

```bash
echo "Linux Distribution: Kali Linux" > system-notes.txt
```

Additional information can be appended using:

```bash
>>
```

---

## Task 6 — Display the File

Display the contents using:

```bash
cat system-notes.txt
```

Then use:

```bash
head system-notes.txt
```

and:

```bash
tail system-notes.txt
```

### Questions

Explain:

1. What does `cat` do?
2. What does `head` do?
3. What does `tail` do?
4. What is the difference between `>` and `>>`?

---

# PART F — Copying and Moving Files

## Task 7 — Create a Backup

Create a backup of:

```text
system-notes.txt
```

The backup should be named:

```text
system-notes-backup.txt
```

Use:

```bash
cp
```

---

## Task 8 — Move a File

Move:

```text
commands.txt
```

from:

```text
notes/
```

to:

```text
reports/
```

Use:

```bash
mv
```

---

## Task 9 — Rename a File

Rename:

```text
initial-report.txt
```

to:

```text
lab2-report.txt
```

---

# PART G — Searching for Files

## Task 10 — Use `find`

Use `find` to locate all `.txt` files inside your assessment directory.

Example:

```bash
find ~/Linux-Assessment -name "*.txt"
```

Then find all regular files:

```bash
find ~/Linux-Assessment -type f
```

Then find all directories:

```bash
find ~/Linux-Assessment -type d
```

### Questions

Explain the difference between:

```bash
find ~/Linux-Assessment -name "*.txt"
```

and:

```bash
find ~/Linux-Assessment -type f
```

---

# PART H — Log Analysis

## Task 11 — Create a Sample Security Log

Create a file:

```text
security.log
```

Add the following entries:

```text
INFO User login successful
ERROR Authentication failed
INFO User accessed dashboard
WARNING Multiple login attempts
ERROR Authentication failed
INFO User logged out
ERROR Authentication failed
INFO User login successful
```

You may create the file using:

```bash
cat
```

with a here-document, or by using `echo`.

---

# PART I — Search the Log

## Task 12 — Find Errors

Use:

```bash
grep "ERROR" security.log
```

### Requirement

Identify all lines containing:

```text
ERROR
```

---

## Task 13 — Count Errors

Use:

```bash
grep -c "ERROR" security.log
```

Record the number of errors.

---

## Task 14 — Find Warnings

Use:

```bash
grep "WARNING" security.log
```

Record the warning found.

---

# PART J — Pipes and Command Combination

## Task 15 — Use a Pipe

Use:

```bash
cat security.log | grep "ERROR"
```

Explain what the pipe:

```text
|
```

does.

---

## Task 16 — Count Matching Entries

Use:

```bash
grep "ERROR" security.log | wc -l
```

Explain how the two commands work together.

Your explanation should describe:

```text
grep
↓
pipe
↓
wc
```

---

# PART K — Text Processing

## Task 17 — Use `sort`

Create a file named:

```text
users.txt
```

Add at least the following usernames:

```text
admin
student
analyst
admin
developer
student
researcher
```

Sort the file:

```bash
sort users.txt
```

---

## Task 18 — Remove Duplicates

Use:

```bash
sort users.txt | uniq
```

Explain why `sort` is commonly used before `uniq`.

---

## Task 19 — Use `cut`

Create a file called:

```text
accounts.txt
```

Add entries using this format:

```text
admin:administrator
student:student-account
analyst:security-analyst
developer:web-developer
```

Use:

```bash
cut -d ':' -f 1 accounts.txt
```

### Question

What information does the command extract?

---

# PART L — Linux Permissions

## Task 20 — Examine Permissions

Navigate to your assessment directory:

```bash
cd ~/Linux-Assessment
```

Run:

```bash
ls -la
```

Choose one of your files and examine its permission string.

For example:

```text
-rw-r--r--
```

### Explain

Identify:

* Owner permissions
* Group permissions
* Other permissions

Explain what:

```text
r
w
x
```

represent.

---

# PART M — System Information

## Task 21 — Collect System Information

Run:

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
du -sh ~/Linux-Assessment
```

### Create a report

Place your observations in:

```text
reports/system-enumeration.txt
```

The report should contain:

```text
Username:
Hostname:
Kernel/System:
Disk Usage:
Assessment Directory Size:
```

---

# PART N — Archive Your Evidence

## Task 22 — Create an Archive

Create a compressed archive of your assessment directory:

```bash
tar -czf linux-assessment.tar.gz ~/Linux-Assessment
```

Verify that it exists:

```bash
ls -lh linux-assessment.tar.gz
```

---

## Task 23 — Inspect the Archive

Use:

```bash
tar -tzf linux-assessment.tar.gz
```

Verify that the archive contains your:

* Notes
* Logs
* Reports
* Evidence directories

---

# PART O — Final Practical Challenge

## Task 24 — Complete the Investigation Workspace

Starting from an empty directory, create:

```text
Cybersecurity-Lab/
├── evidence/
│   ├── screenshots/
│   └── logs/
├── notes/
├── reports/
└── tools/
```

Then:

### 1.

Create:

```text
notes/environment.txt
```

### 2.

Record your:

* Username
* Hostname
* Linux version/kernel

### 3.

Create:

```text
evidence/logs/authentication.log
```

### 4.

Add at least:

* 3 successful login events
* 3 failed login events
* 2 warning events

### 5.

Use `grep` to identify failed logins.

### 6.

Use `grep -c` to count them.

### 7.

Use a pipe with `wc -l`.

### 8.

Use `find` to locate all `.log` files.

### 9.

Display the directory structure.

### 10.

Create a compressed archive of the entire project.

---

# PART P — Written Questions

Answer the following questions in your own words.

### Question 1

What is the difference between Linux and Kali Linux?

### Question 2

What is the purpose of the Linux command line?

### Question 3

What is Bash?

### Question 4

What is the difference between an absolute path and a relative path?

### Question 5

What does `pwd` do?

### Question 6

What is the purpose of `grep`?

### Question 7

What is the purpose of `find`?

### Question 8

Explain how a pipe works.

### Question 9

What is the difference between:

```text
>
```

and:

```text
>>
```

### Question 10

Explain the meaning of:

```text
755
```

in Linux permissions.

### Question 11

Why should a cybersecurity professional understand Linux?

### Question 12

Why is it dangerous to execute commands without understanding what they do?

---

# Submission Requirements

Submit the following:

## 1. Compressed Project

Submit:

```text
linux-assessment.tar.gz
```

or your final practical challenge archive.

---

## 2. Screenshots

Include screenshots demonstrating important tasks.

Recommended screenshots:

### Screenshot 1 — System Information

Show:

```bash
whoami
hostname
uname -a
```

### Screenshot 2 — Workspace

Show:

```bash
find ~/Linux-Assessment -type d
```

### Screenshot 3 — File Operations

Show evidence of:

* File creation
* Copying
* Moving
* Renaming

### Screenshot 4 — Log Analysis

Show:

```bash
grep "ERROR" security.log
```

and:

```bash
grep -c "ERROR" security.log
```

### Screenshot 5 — Pipes

Show:

```bash
grep "ERROR" security.log | wc -l
```

### Screenshot 6 — Permissions

Show:

```bash
ls -la
```

### Screenshot 7 — Archive

Show:

```bash
tar -tzf linux-assessment.tar.gz
```

---

# Evidence Guidelines

Screenshots should:

* Clearly show the terminal.
* Clearly show the command.
* Clearly show the result.
* Be readable.
* Avoid unnecessary personal information.
* Be taken from your own authorized laboratory environment.

Do not submit random screenshots that do not demonstrate the required task.

---

# Academic Integrity

Students are expected to perform the practical work themselves.

You may use:

* Linux documentation
* `man` pages
* Official documentation
* Course notes
* Your own previous work

However, you should understand and be able to explain the commands you submit.

For example, if you submit:

```bash
grep "ERROR" security.log | wc -l
```

you should be able to explain:

* What `grep` does.
* What the pipe does.
* What `wc -l` does.
* What the final number represents.

---

# Ethical and Security Requirements

All practical work must be performed in an authorized environment.

Acceptable environments include:

* Your own Kali Linux VM
* Your own Linux machine
* An intentionally vulnerable cybersecurity lab
* A system for which you have explicit authorization

Do not use these exercises to access, modify, scan, or interfere with systems belonging to other people or organizations without permission.

---

# Submission Checklist

Before submitting, confirm:

* [ ] I completed the Linux navigation tasks.
* [ ] I created the required directory structure.
* [ ] I created and managed files using the CLI.
* [ ] I used `grep`.
* [ ] I used `find`.
* [ ] I used pipes.
* [ ] I used `wc`.
* [ ] I used `sort`.
* [ ] I used `uniq`.
* [ ] I used `cut`.
* [ ] I examined Linux permissions.
* [ ] I collected system information.
* [ ] I created a compressed archive.
* [ ] I captured the required screenshots.
* [ ] I organized my evidence.
* [ ] I answered the written questions.
* [ ] I understand the commands I submitted.
* [ ] All testing was performed in an authorized environment.

---

# Assessment Focus

The assignment will primarily assess:

| Area               | What is being assessed                       |
| ------------------ | -------------------------------------------- |
| Linux navigation   | Ability to move around the filesystem        |
| File management    | Creating, copying, moving and renaming files |
| Text processing    | Searching and processing information         |
| Command chaining   | Understanding pipes and command combinations |
| System enumeration | Collecting basic system information          |
| Permissions        | Understanding basic Linux access controls    |
| Organization       | Maintaining a structured workspace           |
| Evidence           | Providing clear screenshots and results      |
| Understanding      | Ability to explain the commands used         |
| Ethics             | Working only within authorized environments  |

---

# Final Objective

The goal of this assignment is not simply to memorize Linux commands.

The goal is to develop the ability to:

> **Navigate → Observe → Search → Analyze → Document**

These skills form the foundation for the security-focused Linux work that will be introduced in later labs.

---

**Iconic Hub**

**Secure. Build. Innovate.**

*Where Web Development Meets Cybersecurity.*
