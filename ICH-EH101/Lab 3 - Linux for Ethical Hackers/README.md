
Absolutely. Since you’ll be teaching this yourself, I’m going to make the README teacher-friendly, so you can read the explanation before demonstrating each command. I’ll also explain the Bash concepts you mentioned — especially chmod +x, ./filename, .sh, the shebang, and why we use them.

Use this as:

ICH-EH101/Lab 3 - Bash Scripting & Security Automation/README.md

Lab 3 — Bash Scripting & Security Automation
Course Code: ICH-EH101<br>
Course Title: Cybersecurity Fundamentals & Ethical Hacking<br>
Provider: Iconic Hub<br>
Week: 1<br>
Lab: 3

1. Lab Overview
In the previous lab, we learned how to work with Linux from the command line.

We learned commands such as:

pwd
ls
cd
mkdir
touch
cp
mv
rm
cat
grep
find
chmod
Today, we are going to take the next step.

Instead of manually typing many commands one after another, we are going to learn how to automate commands using Bash scripts.

For example, imagine that you want to collect information about a Linux computer.

You could manually type:

whoami
hostname
uname -a
ip addr
ip route
ss -tuln
That works.

But imagine doing this on 20 systems.

It becomes repetitive.

Instead, we can put the commands inside a script:

#!/bin/bash

whoami
hostname
uname -a
ip addr
ip route
ss -tuln
Then we can execute the entire collection of commands with:

./system_info.sh
This is the power of scripting.

A Bash script is simply a file containing commands that Bash can execute for us.

2. Learning Objectives
By the end of this lab, students should be able to:

Explain what Bash is.

Explain what a Bash script is.

Create a Bash script.

Understand the .sh file extension.

Understand the Bash shebang.

Make a script executable using chmod.

Execute a script using ./filename.

Use echo.

Create and use variables.

Accept user input with read.

Use command substitution.

Use if, elif, and else.

Use comparison operators.

Use case.

Use for and while loops.

Create functions.

Pass arguments to functions.

Automate simple Linux security tasks.

Build a basic Linux enumeration script.

3. What Is Bash?
Before writing scripts, we need to understand what Bash actually is.

3.1 What does Bash mean?
Bash stands for:

Bourne Again SHell

It is a command-line shell commonly used on Linux systems.

A shell allows us to communicate with the operating system by typing commands.

For example:

pwd
Bash receives the command and asks Linux to execute it.

Another example:

ls
Bash executes the ls program and displays the result.

So we can think of Bash as an interpreter between us and the operating system.

You
 ↓
Bash
 ↓
Linux Operating System
 ↓
Command / Program
 ↓
Result
4. What Is Bash Scripting?
A Bash script is a text file containing a series of commands.

Instead of manually typing:

mkdir reports
cd reports
touch report.txt
echo "Security Assessment" > report.txt
cat report.txt
we can put those commands into a script.

For example:

#!/bin/bash

mkdir reports
cd reports
touch report.txt
echo "Security Assessment" > report.txt
cat report.txt
Now Bash can execute the commands for us.

This is called automation.

5. Why Is Bash Important in Cybersecurity?
Cybersecurity professionals perform many repetitive tasks.

For example, during system enumeration, we may want to collect:

Current user

User ID

Hostname

Operating system

Kernel version

IP address

Routing information

Listening ports

Running processes

Instead of manually typing every command, we can automate the process.

This is one reason Bash is valuable to:

Penetration testers

SOC analysts

System administrators

Security engineers

Incident responders

Digital forensics investigators

Linux administrators

6. What Is a .sh File?
You will often see files such as:

script.sh
backup.sh
scan.sh
enumeration.sh
The .sh usually means:

This file is intended to be a shell script.

For example:

hello.sh
The .sh extension helps humans recognize that the file is a shell script.

However, there is something important to understand:

Linux does not depend entirely on the .sh extension to know what a script is.

The contents of the file and especially the shebang help determine how it should be interpreted.

So this can technically be a Bash script:

hello
But using:

hello.sh
is a useful convention because it tells other people:

"This is a shell script."

7. The Shebang
Most of our scripts will start with:

#!/bin/bash
This is called the shebang.

It tells the operating system which interpreter should be used to execute the script.

Break it down:

#!        → special instruction
/bin/bash → location of the Bash interpreter
Therefore:

#!/bin/bash
basically means:

"Run this script using Bash."

This is why we normally put it at the top of our Bash scripts.

8. Creating Our First Bash Script
Let's create a working directory.

Run:

mkdir -p ~/Lab3
cd ~/Lab3
Check where you are:

pwd
You should see something similar to:

/home/username/Lab3
Your username may be different.

9. Our First Script
Create a file:

nano first_script.sh
Enter:

#!/bin/bash

echo "Hello, World!"
echo "I am learning Bash scripting!"
echo "I am studying ethical hacking."
Save the file:

Ctrl + X

Then:

Y

Then:

Enter

10. Looking at the Script
Check that the file exists:

ls
You should see:

first_script.sh
We can also inspect it:

cat first_script.sh
You should see:

#!/bin/bash

echo "Hello, World!"
echo "I am learning Bash scripting!"
echo "I am studying ethical hacking."
11. What Does echo Do?
echo simply displays text in the terminal.

For example:

echo "Hello"
Output:

Hello
Another example:

echo "Cybersecurity is interesting"
Output:

Cybersecurity is interesting
We use echo heavily in scripts because it allows us to display information to the user.

12. Understanding chmod
This is one of the most important concepts for today's class.

When we create:

first_script.sh
Linux may not automatically consider it executable.

Check the permissions:

ls -l first_script.sh
You may see something like:

-rw-r--r-- 1 user user 123 Oct 4 19:00 first_script.sh
Look at the beginning:

-rw-r--r--
There is no x.

Remember from the previous Linux class:

r = read
w = write
x = execute
Therefore, the file currently does not have execute permission.

13. What Does chmod Mean?
chmod means:

change mode

It changes the permissions of a file or directory.

For example:

chmod +x first_script.sh
means:

Add execute permission to this file.

Now check:

ls -l first_script.sh
You may see:

-rwxr-xr-x
Notice the x.

The file is now executable.

14. Understanding chmod +x
Break this command down:

chmod +x first_script.sh
chmod
Change file permissions.

+
Add a permission.

x
Execute permission.

first_script.sh
The file whose permission we want to change.

Therefore:

chmod +x first_script.sh
means:

"Add execute permission to first_script.sh."

15. What Does Execute Permission Mean?
Think about a normal document.

You can read it.

You can edit it.

But that doesn't mean the operating system should treat it like a program that can be executed.

An executable file has permission to be run as a program/script.

For our Bash script:

chmod +x first_script.sh
we are telling Linux:

"This file is allowed to be executed."

16. Running the Script With ./
Now that the script is executable, run:

./first_script.sh
You should get:

Hello, World!
I am learning Bash scripting!
I am studying ethical hacking.
Now let's understand the strange-looking part:

./
17. What Does ./ Mean?
The . means:

the current directory

And / separates directories/files.

Therefore:

./first_script.sh
means:

"Execute first_script.sh from the current directory."

For example, if:

pwd
returns:

/home/obinna/Lab3
then:

./first_script.sh
means:

/home/obinna/Lab3/first_script.sh
18. Why Can't We Just Type the Filename?
A common beginner question is:

Why don't we simply type first_script.sh?

Try:

first_script.sh
You may receive:

command not found
Why?

Because Bash normally searches specific directories listed in your $PATH for commands.

Your current directory is not automatically searched as a command location.

When we write:

./first_script.sh
we explicitly tell Bash:

"The file is right here in the current directory. Run this one."

This is also a security feature.

Imagine if Linux automatically executed any file in your current directory whenever you typed its name. That could create security problems.

19. Another Way to Run a Bash Script
We can also run the script by explicitly giving it to Bash:

bash first_script.sh
Notice that we don't need:

chmod +x
for this method.

Why?

Because we are not asking Linux to execute the file directly.

We are asking the Bash program to read the file and execute its commands.

Compare:

./first_script.sh
with:

bash first_script.sh
Method 1
./first_script.sh
Requires the script to have execute permission.

Method 2
bash first_script.sh
Bash reads and executes the file directly.

Both can work.

For our course, we will normally demonstrate:

chmod +x script.sh
./script.sh
because it teaches students how executable permissions work.

20. First Exercise
Modify your script so that it displays:

=================================
       ICONIC HUB
   ETHICAL HACKING COURSE
=================================

Student:
Date:
Current User:
You can use:

echo "================================="
echo "       ICONIC HUB"
echo "   ETHICAL HACKING COURSE"
echo "================================="

echo "Student: Obinna"
echo "Date: $(date)"
echo "Current User: $(whoami)"
Notice something new:

$(date)
and:

$(whoami)
We will explain this next.

21. Command Substitution
Command substitution allows us to run a command and use its output inside another command.

The syntax is:

$(command)
For example:

echo "Current user: $(whoami)"
Bash runs:

whoami
and inserts the result into the sentence.

If the username is:

obinna
the output becomes:

Current user: obinna
Another example:

echo "Today is $(date)"
This is very useful in security scripts.

22. Variables
A variable is a container used to store information.

Think of it like a labeled box.

name
 ↓
"Obinna"
In Bash:

name="Obinna"
We can then use it:

echo "$name"
Output:

Obinna
23. Important Bash Variable Rule
There must be no spaces around =.

Correct:

name="Obinna"
Incorrect:

name = "Obinna"
Bash interprets the second version differently and will produce an error.

24. Variables Example
Create:

nano variables.sh
Enter:

#!/bin/bash

name="Obinna"
course="Ethical Hacking"
school="Iconic Hub"

echo "Name: $name"
echo "Course: $course"
echo "School: $school"
Save.

Then:

chmod +x variables.sh
./variables.sh
25. User Input With read
Sometimes we don't want to hard-code information.

We want the user to provide it.

We use:

read
Example:

#!/bin/bash

echo "What is your name?"
read name

echo "Hello, $name"
Run:

./input.sh
The script waits for you to type something.

For example:

What is your name?
Obinna
Hello, Obinna
26. Better Input With read -p
Instead of:

echo "Enter your name:"
read name
we can write:

read -p "Enter your name: " name
Example:

#!/bin/bash

read -p "Enter your name: " name
echo "Welcome, $name"
27. Security Example — Asking for a Target
In authorized security testing, a script might ask for a target.

For example:

#!/bin/bash

read -p "Enter authorized lab target: " target

echo "Target selected: $target"
This does not attack anything.

It simply collects input.

Always remember:

Only test systems you own or have explicit permission to assess.

28. Conditional Statements
Sometimes a script needs to make a decision.

For example:

"If the current user is root, display a warning. Otherwise, continue."

This is where if comes in.

Basic structure:

if [ condition ]; then
    command
else
    command
fi
Think of it like:

IF something is true
    do this
ELSE
    do that
29. Example: Root Detection
Create:

nano privilege_check.sh
Enter:

#!/bin/bash

if [ "$EUID" -eq 0 ]; then
    echo "You are running as root."
else
    echo "You are running as a normal user."
fi
Run:

chmod +x privilege_check.sh
./privilege_check.sh
30. Understanding the Condition
Look at:

[ "$EUID" -eq 0 ]
$EUID contains the effective user ID.

Root normally has:

UID = 0
Therefore:

"$EUID" -eq 0
asks:

"Is the effective user ID equal to zero?"

If yes:

You are running as root.
Otherwise:

You are running as a normal user.
31. Common Numeric Comparisons
Operator	Meaning
-eq	Equal
-ne	Not equal
-gt	Greater than
-lt	Less than
-ge	Greater than or equal
-le	Less than or equal
Example:

if [ "$age" -ge 18 ]; then
    echo "Adult"
fi
32. elif
Sometimes we need more than two possibilities.

Example:

#!/bin/bash

read -p "Enter your score: " score

if [ "$score" -ge 70 ]; then
    echo "Excellent"
elif [ "$score" -ge 50 ]; then
    echo "Pass"
else
    echo "Fail"
fi
The structure is:

if
 ↓
condition 1

elif
 ↓
condition 2

else
 ↓
everything else
33. Loops
A loop allows us to repeat something.

Imagine having to run:

echo "Checking 1"
echo "Checking 2"
echo "Checking 3"
echo "Checking 4"
echo "Checking 5"
That is inefficient.

A loop can do it:

for i in 1 2 3 4 5
do
    echo "Checking $i"
done
Output:

Checking 1
Checking 2
Checking 3
Checking 4
Checking 5
34. Why Loops Matter in Cybersecurity
Loops become extremely useful when we need to process many items.

For example:

for user in alice bob charlie
do
    echo "Checking user: $user"
done
Or:

for file in *.log
do
    echo "Analyzing $file"
done
This is the beginning of automation.

35. While Loops
A while loop continues as long as a condition is true.

Example:

#!/bin/bash

count=1

while [ "$count" -le 5 ]
do
    echo "Count: $count"
    count=$((count + 1))
done
Output:

Count: 1
Count: 2
Count: 3
Count: 4
Count: 5
Notice:

count=$((count + 1))
This increases the number by one.

36. Functions
As scripts become larger, putting everything into one long block becomes difficult to understand.

Functions allow us to organize our code.

Think of a function as a small reusable task.

Example:

greet_user() {
    echo "Welcome to Iconic Hub"
}
We can call it:

greet_user
Full example:

#!/bin/bash

greet_user() {
    echo "Welcome to Iconic Hub"
}

greet_user
greet_user
Output:

Welcome to Iconic Hub
Welcome to Iconic Hub
37. Functions With Arguments
Functions can receive information.

Example:

greet_user() {
    echo "Hello, $1"
}
Then:

greet_user "Obinna"
Output:

Hello, Obinna
Here:

$1
means:

The first argument passed to the function.

For example:

greet_user "Obinna"
The function receives:

$1 = Obinna
38. Multiple Arguments
Example:

introduce() {
    echo "Name: $1"
    echo "Course: $2"
}

introduce "Obinna" "Ethical Hacking"
Output:

Name: Obinna
Course: Ethical Hacking
39. The case Statement
case is useful when we have multiple choices.

Example:

#!/bin/bash

echo "Choose an option:"
echo "1. System information"
echo "2. Network information"
echo "3. User information"

read choice

case "$choice" in
    1)
        echo "Showing system information..."
        ;;
    2)
        echo "Showing network information..."
        ;;
    3)
        echo "Showing user information..."
        ;;
    *)
        echo "Invalid option"
        ;;
esac
This is especially useful for building security tools with menus.

40. Practical Project — Linux Security Information Tool
Now we move from learning Bash syntax to using Bash for cybersecurity.

Our goal is to create a script that collects basic information about our own Linux machine.

The script will collect:

Current user

User ID

Hostname

Operating system

Kernel

IP address

Routing information

Listening ports

Running processes

This is enumeration.

Remember:

Enumeration means systematically collecting information about a system.

We are performing this only against our own authorized lab machine.

41. Create the Security Script
Create:

nano linux_enum.sh
Enter:

#!/bin/bash

echo "======================================"
echo "       ICONIC HUB LINUX ENUM"
echo "======================================"

echo ""
echo "[+] Current User"
whoami

echo ""
echo "[+] User Information"
id

echo ""
echo "[+] Hostname"
hostname

echo ""
echo "[+] Operating System"
grep PRETTY_NAME /etc/os-release

echo ""
echo "[+] Kernel"
uname -r

echo ""
echo "[+] IP Address"
hostname -I

echo ""
echo "[+] Routing Information"
ip route

echo ""
echo "[+] Listening Ports"
ss -tuln

echo ""
echo "[+] Top Processes"
ps aux --sort=-%cpu | head -6

echo ""
echo "======================================"
echo "       ENUMERATION COMPLETE"
echo "======================================"
Save it.

42. Make the Script Executable
Run:

chmod +x linux_enum.sh
Then:

ls -l linux_enum.sh
Look for the x.

For example:

-rwxr-xr-x
Now execute it:

./linux_enum.sh
43. Understanding What Our Tool Does
Let's break down the important commands.

Current user
whoami
Answers:

Who am I currently logged in as?

User information
id
Shows:

UID

GID

Groups

Hostname
hostname
Shows the system's hostname.

Operating system
grep PRETTY_NAME /etc/os-release
Looks inside /etc/os-release and extracts the human-readable operating system name.

Kernel
uname -r
Shows the Linux kernel version.

IP address
hostname -I
Displays IP addresses associated with the host.

Routing
ip route
Shows routing information.

Listening ports
ss -tuln
Shows listening TCP and UDP sockets.

Processes
ps aux --sort=-%cpu | head -6
Shows processes sorted by CPU usage.

44. Saving Script Output
We can save the output to a file.

Instead of:

./linux_enum.sh
use:

./linux_enum.sh > enumeration_report.txt
The > means:

Send the output into this file.

Now:

cat enumeration_report.txt
will show the saved results.

45. > Versus >>
This is important.

>
command > file.txt
Creates the file or overwrites it.

>>
command >> file.txt
Adds output to the end of the file.

Example:

echo "First line" > test.txt
echo "Second line" >> test.txt
The file will contain:

First line
Second line
46. Practical Challenge
Students must now build their own script.

Task
Create:

linux_security_report.sh
The script must display:

========================================
       LINUX SECURITY REPORT
========================================

Current User:
User ID:
Hostname:
Operating System:
Kernel:
IP Address:

Routing Information:

Listening Ports:

Top Processes:

========================================
          REPORT COMPLETE
========================================
Students should use Bash commands to collect the information.

47. Challenge Requirements
The script must contain:

Requirement 1 — Shebang
#!/bin/bash
Requirement 2 — At least 3 variables
For example:

user=$(whoami)
host=$(hostname)
kernel=$(uname -r)
Requirement 3 — At least one if statement
For example, check whether the script is being run as root.

Requirement 4 — At least one function
Example:

system_info() {
    ...
}
Requirement 5 — At least one loop
Students should use a loop for a simple repeated task.

Requirement 6 — Save output
The final report should be saved to:

linux_security_report.txt
48. Evidence / Screenshots
Students should capture screenshots showing:

Screenshot 1
Creating the script:

nano linux_security_report.sh
Screenshot 2
Permissions:

ls -l linux_security_report.sh
The screenshot should show the x permission.

Screenshot 3
Running the script:

./linux_security_report.sh
Screenshot 4
Generated report:

cat linux_security_report.txt
49. Important Beginner Mistakes
Mistake 1 — Forgetting chmod
If they run:

./script.sh
and get:

Permission denied
check:

ls -l script.sh
If there is no x, run:

chmod +x script.sh
Mistake 2 — Forgetting ./
If they type:

script.sh
and receive:

command not found
try:

./script.sh
assuming the script is in the current directory.

Mistake 3 — Incorrect variable syntax
Correct:

name="Obinna"
Incorrect:

name = "Obinna"
Mistake 4 — Forgetting $
When assigning:

name="Obinna"
When using:

echo "$name"
The $ tells Bash:

"Use the value stored in this variable."

Mistake 5 — Forgetting fi
An if statement must end with:

fi
Example:

if [ "$EUID" -eq 0 ]; then
    echo "Root"
else
    echo "Normal user"
fi
Mistake 6 — Forgetting done
Loops must end with:

done
Example:

for i in 1 2 3
do
    echo "$i"
done
50. A Simple Mental Model
Students should remember:

COMMAND
   ↓
Multiple commands
   ↓
SCRIPT
   ↓
Variables
   ↓
Conditions
   ↓
Loops
   ↓
Functions
   ↓
AUTOMATION
This is the progression we want.

51. Why This Matters in Ethical Hacking
Imagine you are performing an authorized assessment.

You have to collect information from a Linux machine.

Without scripting:

Run command
Record result
Run command
Record result
Run command
Record result
...
With scripting:

Run script
     ↓
Collect information
     ↓
Organize results
     ↓
Save report
This saves time and reduces repetitive work.

Professional security tools often use the same basic programming concepts:

Variables

Conditions

Loops

Functions

Input

Output

Error handling

Automation

52. Ethical Boundary
Bash can be used to automate legitimate security work.

It can also be abused.

In this course, scripts must only be used against:

Your own computer

Your own virtual machines

Intentionally vulnerable lab machines

Systems where you have explicit authorization

Never use a script to scan or attack random public systems.

Automation does not change the rules of authorization.

If something is unauthorized manually, automating it does not make it authorized.

53. Classroom Discussion
Ask students:

Question 1
Why is scripting better than manually typing the same commands repeatedly?

Question 2
What does:

chmod +x script.sh
do?

Question 3
What does:

./script.sh
mean?

Question 4
What is the purpose of:

#!/bin/bash
Question 5
What is the difference between:

./script.sh
and:

bash script.sh
Question 6
How could Bash scripting help a penetration tester?

Question 7
How could the same Bash scripting skills help a defender?

Question 8
Why should automated security scripts still be used within an authorized scope?

54. Lab 3 Knowledge Check
Students should be able to answer:

What is Bash?

What is Bash scripting?

What does .sh usually indicate?

What is a shebang?

What does #!/bin/bash mean?

What does chmod do?

What does chmod +x script.sh do?

What does ./ mean?

Why might script.sh fail while ./script.sh works?

What does bash script.sh do?

What is a variable?

Why does Bash use $ when retrieving a variable's value?

What does read do?

What does $(command) mean?

What is an if statement?

What is a loop?

What is a function?

What does $1 mean inside a function?

Why is automation useful in cybersecurity?

Why must security automation be authorized?

55. Lab 3 Practical Submission
Students should submit:

linux_security_report.sh
linux_security_report.txt
And screenshots showing:

1. Script creation
2. Executable permission
3. Script execution
4. Generated report
56. Lab 3 Completion Checklist
Students should be able to check:

I understand what Bash is.

I understand what a Bash script is.

I understand the .sh extension.

I understand the Bash shebang.

I can create a Bash script.

I can use echo.

I can create variables.

I can accept user input.

I understand command substitution.

I can use if statements.

I can use loops.

I can create functions.

I understand chmod +x.

I understand ./filename.

I can run a Bash script.

I can save script output to a file.

I can create a basic security automation script.

I understand the importance of authorization.

57. Key Takeaway
Bash scripting is not about memorizing hundreds of commands.

The important idea is learning how to combine commands into a logical process.

For example:

Collect information
       ↓
Store information
       ↓
Make a decision
       ↓
Repeat a task
       ↓
Organize the code
       ↓
Automate the process
That is the foundation of scripting.

And in cybersecurity:

A good security professional does not only know how to run tools. They know how to automate, analyze, and understand what those tools are doing.

58. Next Lab
Lab 4 — Networking Fundamentals
In the next lab, we will move from the Linux system itself to the network.

We will learn:

What a network is

IP addresses

MAC addresses

IPv4

IPv6

TCP

UDP

Ports

Protocols

DNS

HTTP/HTTPS

Routers

Switches

Gateways

Network segmentation

Basic network troubleshooting

We will then use this knowledge to prepare for Nmap and reconnaissance in later labs.

Iconic Hub
Secure. Build. Innovate.
Where Web Development Meets Cybersecurity.

The part you should understand before teaching
The four things you specifically asked about are worth getting comfortable with yourself:

chmod +x script.sh
= give the script permission to execute

./script.sh
= run the script located in my current directory

bash script.sh
= tell the Bash interpreter to read and execute this script

And:

script.sh
= usually won't work by itself when the script is just sitting in the current directory, because the current directory isn't normally searched as a command location.

Also, .sh isn't magic. It's just the conventional filename extension for shell scripts. The important part is that Bash can interpret the commands and, when using ./script.sh, the file has execute permission.

For tomorrow, don't try to memorize the whole README. Teach the concepts, demonstrate the commands, and let the students type them themselves. The biggest thing you need to be able to explain confidently is the flow:

create → write → save → chmod +x → ./script.sh → observe output → modify → run again.

That will make the rest of the Bash lesson much easier to teach.


Yes. Since the folder is currently named:

Lab 3 - Linux for Ethical Hackers
and you want:

Lab 3 - Bash Scripting & Security Automation
do this from your Git Bash terminal, from the course repository root.

1. Rename the folder
mv "ICH-EH101/Lab 3 - Linux for Ethical Hackers" "ICH-EH101/Lab 3 - Bash Scripting & Security Automation"
2. Check that it worked
ls ICH-EH101
You should see:

Lab 1 - Introduction to Cybersecurity & Ethical Hacking
Lab 2 - Linux Command Line Fundamentals
Lab 3 - Bash Scripting & Security Automation
Lab 4 - Networking Fundamentals
...
3. Check Git
git status
Git should recognize the folder/file changes.

4. Add everything
git add -A
5. Commit
git commit -m "Rename Lab 3 to Bash Scripting and Security Automation"
6. Push
git push origin main
One more important thing
Your Lab 3 README should have:

# Lab 3 — Bash Scripting & Security Automation

**Course Code:** ICH-EH101<br>
**Course Title:** Cybersecurity Fundamentals & Ethical Hacking<br>
**Provider:** Iconic Hub<br>
**Week:** 1<br>
**Lab:** 3
And if your main README.md currently links to:

Lab 3 - Linux for Ethical Hackers
we should update that link too, otherwise the GitHub course navigation can break.

If you want, send me your current main README Lab 3 link, and I'll give you the exact replacement.


hp@CRIMINAL MINGW64 ~
$ git clone Iconic-PC/ICONIC-HUB-ETHICAL-HACKING-COURSE.git%C2%83
Cloning into 'ICONIC-HUB-ETHICAL-HACKING-COURSE.git'...
remote: Repository not found.
fatal: repository 'Iconic-PC/ICONIC-HUB-ETHICAL-HACKING-COURSE.git%C2%83' not found

hp@CRIMINAL MINGW64 ~
$ git clone ggit@github.com:Iconic-PC/ICONIC-HUB-ETHICAL-HACKING-COURSE.git
fatal: destination path 'ICONIC-HUB-ETHICAL-HACKING-COURSE' already exists and is not an empty directory.

hp@CRIMINAL MINGW64 ~
$

You're actually fine. The first command failed because extra characters were pasted at the end of the URL, and the second command shows the repository folder already exists.

You already have the repository on your computer.

1. Enter the existing repository
Run:

cd ~/ICONIC-HUB-ETHICAL-HACKING-COURSE
Then check:

git status
You should see something like:

On branch main
Your branch is up to date with 'origin/main'.
2. Confirm the remote
Run:

git remote -v
You should see:

origin  git@github.com:Iconic-PC/ICONIC-HUB-ETHICAL-HACKING-COURSE.git (fetch)
origin  git@github.com:Iconic-PC/ICONIC-HUB-ETHICAL-HACKING-COURSE.git (push)
3. Check your Lab 3 folder
Run:

ls "ICH-EH101"
You should now see:

Lab 1 - Introduction to Cybersecurity & Ethical Hacking
Lab 2 - Linux Command Line Fundamentals
Lab 3 - Bash Scripting & Security Automation
Lab 4 - Networking Fundamentals
Lab 5 - Nmap
Lab 6 - Reconnaissance & Enumeration
4. If Lab 3 still has the old name
If you see:

Lab 3 - Linux for Ethical Hackers
then rename it with:

mv "ICH-EH101/Lab 3 - Linux for Ethical Hackers" "ICH-EH101/Lab 3 - Bash Scripting & Security Automation"
Then:

git status
Important
Don't run git clone again. Your folder already exists:

~/ICONIC-HUB-ETHICAL-HACKING-COURSE
We're going to work inside that existing repository.

Run these two commands now:

cd ~/ICONIC-HUB-ETHICAL-HACKING-COURSE
git status
Send me the output, and we'll continue from there.


hp@CRIMINAL MINGW64 ~/ICONIC-HUB-ETHICAL-HACKING-COURSE (main)
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    ICH-EH101/Lab 3 - Linux for Ethical Hackers/Assignment/README.md
        deleted:    ICH-EH101/Lab 3 - Linux for Ethical Hackers/README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        ICH-EH101/Lab 3 - Bash Scripting & Security Automation/

no changes added to commit (use "git add" and/or "git commit -a")

hp@CRIMINAL MINGW64 ~/ICONIC-HUB-ETHICAL-HACKING-COURSE (main)
$

Perfect. 👍 This is exactly what we expected after renaming the folder.

Git is showing the old Lab 3 files as deleted and the new Lab 3 folder as untracked. We just need to stage everything, commit it, and push.

Step 1 — Check the new Lab 3 folder
Run:

ls "ICH-EH101/Lab 3 - Bash Scripting & Security Automation"
You should see:

Assignment
README.md
If that's correct, continue.

Step 2 — Stage the rename
Run:

git add -A
Then:

git status
Git will probably recognize it as a rename, something like:

renamed:
ICH-EH101/Lab 3 - Linux for Ethical Hackers/README.md
→ ICH-EH101/Lab 3 - Bash Scripting & Security Automation/README.md
Step 3 — Commit
Run:

git commit -m "Rename Lab 3 to Bash Scripting and Security Automation"
Step 4 — Push to GitHub
Run:

git push origin main
Step 5 — Verify
Finally:

git status
You want:

On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
One important thing: after this, we should check the main README.md because its Lab 3 link may still point to:

Lab 3 - Linux for Ethical Hackers
If so, we'll fix that link next.


