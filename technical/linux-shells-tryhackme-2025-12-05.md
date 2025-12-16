# Linux Shells: The Command Interpreter and Automation Core

A Linux shell is the command-line interface (CLI) program that serves as the essential interpreter between the user and the 
operating system's kernel. Its primary function is to read human-readable commands typed into a terminal and translate them 
into instructions the kernel can execute. This mechanism provides a fundamental, highly efficient way to manage system resources, 
execute programs, and manipulate files.

---
## Fundamental Shell Concepts
I learned that the power of the shell stems from its high action-to-keystroke ratio and its core features for automation. 
These include piping (|), which connects the output of one command directly to the input of the next, and redirection 
(>, >>, <), which manages input and output to and from files. These features enable the construction of powerful shell 
scripts, which are text files containing a sequence of commands that automate complex, repetitive administrative tasks, 
saving significant time and ensuring consistency.

---
## Key Shell Variants
While there are many variants, the dominant shells are defined by their features and use cases:
- Bash (Bourne-Again SHell): This is the default shell on most Linux distributions. It is widely compatible and highly suitable 
for writing portable scripts due to its POSIX compliance. Bash is stable, fast, and reliable for foundational system 
administration.
- Zsh (Z Shell): A modern, feature-rich shell that has become the default on recent macOS versions and distributions 
like Kali Linux. While it remains largely compatible with Bash syntax, its strength lies in advanced interactivity. 
Features like context-aware auto-completion, spelling correction, and extensive plugin support (via frameworks like Oh My Zsh) 
make it ideal for security professionals and power users focused on maximizing terminal workflow speed.

This room served as an effective review of fundamental Linux Shell concepts, providing a valuable warm-up in shell scripting.

- `echo $SHELL` = Multiple shells are installed in different Linux distributions. Use the command to see which shell you are using.

- `cat /etc/shells` = list down the available shells in your Linux OS

- `chsh -s /usr/bin/"shell name like zsh"` = this will make the shell as default

- `nano first_script.sh` = command to create a shell 
The file must be named with an extension .sh, the default extension for bash scripts.

put inside the shell:
`# Defining the Interpreter 
#!/bin/bash
echo "Hey, what’s your name?"
read name
echo "Welcome, $name"`

`after exit,
run this command to allow script execution:
chmod +x first_script.sh
run: ./first_script.sh`

---
## Loops
A loop simply represents a repetitive action within a script or program. When I have a list of identical operations that need 
to be performed on a list of items—like applying the same command to multiple files or sending the same notification to 
multiple users—I use a loop.

Instead of writing out the command individually for every item, I define the command once. I then feed the loop my target 
list (e.g., a list of friends' emails or a list of server IP addresses). The loop iterates through the list, automatically 
executing the command on each item until the list is exhausted. This principle allows me to automate tasks and makes my scripts 
efficient and scalable. This is fundamental for shell scripting.

Sample:
`# Defining the Interpreter 
#!/bin/bash
for i in {1..10};
do
echo $i
done`

---
## Loop Mechanics
This code block sets up a loop to repeat a command ten times.

The line starting with for defines the loop's process:
- The variable i is the counter; it will hold the current value during each run.
- The numbers {1..10} provide the list of values (1 through 10) the loop will cycle through.
- The code inside the loop—everything between do (start) and done (end)—is executed during each cycle.

In every iteration, the loop assigns the next number from the list to the variable i. The command echo $i simply prints that 
variable's current value. The loop stops once it has gone through all the numbers from 1 to 10.

Conditional statements are fundamental for controlling a script's execution flow. Their purpose is simple: to execute a 
specific block of code only when a defined condition is true. If that condition is not met, the script can either skip the 
code entirely or execute an alternative set of instructions.

For instance, if I need a script to reveal a secret resource, I must restrict access. I would implement a conditional 
statement that first asks for the user's identity (e.g., their name). The script then checks if the entered name matches 
the pre-defined high-authority user's name. Only if that condition is satisfied (true) will the script display the secret; 
otherwise, it denies access or runs a different piece of code. This mechanism is essential for implementing logic and 
security checks in any program or script.

Sample:
`# Defining the Interpreter 
#!/bin/bash
echo "Please enter your name first:"
read name
if [ "$name" = "Stewart" ]; then
        echo "Welcome Stewart! Here is the secret: THM_Script"
else
        echo "Sorry! You are not authorized to access the secret."
fi`

The conditional block begins with if. My script will use if to compare a specified variable's value against a static string 
(in this case, "Stewart"). If the values match, the condition is considered true, and the block of code immediately following
then will execute (e.g., displaying the secret). If the condition is false, the script skips that code block. The statement 
must be closed out using fi, which signals the end of the conditional check.

In shell scripting, the hash mark (#) denotes a comment.It tells the shell to ignore everything that follows it on that line.

Sample Script

Script
`# Defining the Interpreter 
#!/bin/bash` 

`# Defining the variables
username=""
companyname=""
pin=""`

`# Defining the loop
for i in {1..3}; do`
`# Defining the conditional statements
        if [ "$i" -eq 1 ]; then
                echo "Enter your Username:"
                read username
        elif [ "$i" -eq 2 ]; then
                echo "Enter your Company name:"
                read companyname
        else
                echo "Enter your PIN:"
                read pin
        fi
done`

# Checking if the user entered the correct details
`if [ "$username" = "John" ] && [ "$companyname" = "Tryhackme" ] && [ "$pin" = "7385" ]; then
        echo "Authentication Successful. You can now access your locker, John."
else
        echo "Authentication Denied!!"
fi`



Commands List

- `gobuster -u http://fakebank.thm -w wordlist.txt dir`

- command replacing netstat in Linux systems = `ss`
(ss means socket statistics)

- the netstat parameter in MS Windows that displays the executable associated with each active connection and listening 
port = `-b`


- Linux
https://youtu.be/kPylihJRG70?si=7nuYCgH7HtI1w0Bp

- Print Working Directory - `pwd`
  
- `whoami` - can be used to find the username we are logged in as.

- `cd` 

- `find -name passwords.txt`
- `find -name *.txt`
- `grep` = to search entire contents of the file 
sample: grep "THM*" access.log

- `&`
This operator allows you to run commands in the background of your terminal.

- `&&`
This operator allows you to combine multiple commands together in one line of your terminal.

- `>`
This operator is a redirector - meaning that we can take the output from a command (such as using cat to output a file) and
>direct it elsewhere.

- `>>`
This operator does the same function of the > operator but appends the output rather than replacing (meaning nothing is
>>overwritten).

- SSH Login to another computer
sample: `ssh tryhackme@10.49.180.37`

- `ls` = this command lists all visible files in the directory 
- `ls -a` = this command lists all visible and hidden files in the directory (or ls --all)

- `ls -la`
Lists the contents of the directory, including hidden files (-a) and additional details like file permissions and owner (-l).

- `-h` = we used to display the output in a "human-readable" way

- `cat "filename"` = check contents of a file

- `su "username"` = switch users


- `wget` = to download the file using the 10.49.189.199 address and the name of the file. Remember, because the python3 server 
is running port 8000, you will need to specify this within your wget command. 
sample: `wget http://10.49.189.199:8000/myfile`


- `ps`= we can use the friendly ps command to provide a list of the running processes as our user's session and some additional 
information such as its status code, the session that is running it, how much usage time of the CPU it is using, and the 
name of the actual program or command that is being executed

- `ps aux` = we can use the friendly ps command to provide a list of the running processes as our user's session and some 
additional information such as its status code, the session that is running it, how much usage time of the CPU it is using, 
and the name of the actual program or command that is being executed

- `ps aux | less` 

- `top` = another great command to gain insight into your system 

- `kill` = you can send signals that terminate processes; there are a variety of types of signals that correlate to exactly 
how "cleanly" the process is dealt with by the kernel. To kill a command, we can use the appropriately named kill command 
and the associated PID that we wish to kill. i.e., to kill PID 1337, we'd use kill 1337.

- Below are some of the signals that we can send to a process when it is killed:
- `SIGTERM` - Kill the process, but allow it to do some cleanup tasks beforehand
- `SIGKILL` - Kill the process - doesn't do any cleanup after the fact
- `SIGSTOP` - Stop/suspend a process


- Enter the use of `systemctl` -- this command allows us to interact with the systemd process/daemon. Continuing on with our 
example, systemctl is an easy to use command that takes the following formatting: systemctl [option] [service]
For example, to tell apache to start up, we'll use systemctl start apache2. Seems simple enough, right? Same with if we 
wanted to stop apache, we'd just replace the [option] with stop (instead of start like we provided)
We can do four options with systemctl:
- `Start`
- `Stop`
- `Enable`
- `Disable`

- `ctrl z` = pause 
This script will keep on repeating "This will keep on looping until I stop!" until I stop or suspend the process. By 
using Ctrl + Z (as indicated by T^Z). Now our terminal is no longer filled up with messages -- until we foreground it.

- `fg` = is being used to bring the background process back into use on the terminal

- A crontab is simply a special file with formatting that is recognised by the cron process to execute each line step-by-step. 
Crontabs require 6 specific values:
Value | Description | MIN

- What minute to execute at
HOUR
What hour to execute at

- DOM
What day of the month to execute at
MON

- What month of the year to execute at
DOW
What day of the week to execute at

- CMD
The actual command that will be executed.

- Let's use the example of backing up files. You may wish to backup "cmnatic"'s  "Documents" every 12 hours. We would 
use the following formatting: 
`0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/`

- `crontab -e` = Crontabs can be edited by using crontab -e, where you can select an editor (such as Nano) to edit your crontab.
sample: When will the crontab on the deployed instance (10.49.135.223) run?
ans. @reboot , when the system reboots, crontab will run 

- The `apt` command is a part of the package management software also named apt. Apt contains a whole suite of tools that 
allows us to manage the packages and sources of our software, and to install or remove software at the same time.

- `add-apt-repository` = Additional repositories can be added by using the add-apt-repository command or by listing another 
provider!
Whilst you can install software through the use of package installers such as dpkg, the benefits of apt means that whenever 
we update our system -- the repository that contains the pieces of software that we add also gets checked for updates. 

- 2.4. Once successfully updated, we can now proceed to install the software that we have trusted and added to apt using apt 
`install sublime-text`
Removing packages is as easy as reversing. This process is done by using the `add-apt-repository --remove ppa:PPA_Name/ppa` 
command or by manually deleting the file that we previously added to. Once removed, we can just use apt 
remove [software-name-here] i.e. apt remove sublime-text

- `grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' /var/log/apache2/*`
= extract all available ip address in the /var/log/apache2/ directory

- `uptime`
Displays how long the system has been running since the last reboot.

- `ip addr`
Checks and displays the system's IP addresses and network interface configuration.

- `ps aux`
Lists all currently running processes on the system, including those owned by other users.

- `cat /etc/shadow`
Views the contents of the /etc/shadow file, which stores usernames and hashed passwords (requires root privileges).

- `echo $SHELL` = Multiple shells are installed in different Linux distributions. Use the command to see which shell 
you are using.

- `cat /etc/shells` = list down the available shells in your Linux OS

- `chsh -s /usr/bin/"shell name like zsh"` = this will make the shell as default

- `nano first_script.sh` = command to create a shell 
put inside the shell:
`# Defining the Interpreter 
#!/bin/bash
echo "Hey, what’s your name?"
read name
echo "Welcome, $name"`

`after exit,
run this command to allow script execution:
chmod +x first_script.sh
run: ./first_script.sh`

- `#`= script comment, makes the code non-executable, and allows you to add comments


Service | TCP Port | Command Sequence | Purpose 

- Echo Server
7
`telnet <target_VM_IP> 7`
Establishes connection to the echo service.

- Daytime Server
13
`telnet <target_VM_IP> 13`
Connects and retrieves the time/date before automatically closing.

- Web Server (HTTP)
80
1. `telnet <target_VM_IP> 80` 
2. `GET / HTTP/1.1` 
3. `Host: telnet.thm` 
4. Press Enter (Blank Line)
Manually sends an HTTP request to retrieve the main page.




