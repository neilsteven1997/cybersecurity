# Chapter 1: The Silicon Brain (Processes & Persistence)

---
1.1 

To understand a computer, you must stop thinking of it as a magic box and start seeing it as a factory with many moving parts inside. Every computer on earth, from your smartwatch to an AWS Server, runs on the Von Neumann Architecture. It has a brain (CPU) and a workspace (RAM).


1. The CPU (The Chef)

The Central Processing Unit, if I had to make an analogy, can be thought of as the Chef. It is the only part of the computer that actually does anything. It takes instructions (code) and executes them.

    Clock Speed (GHz): This is how fast the Chef chops onions. A 4GHz processor cycles 4 billion times per second.
    Cores: This is how many hands the Chef has. A single-core CPU can only do one thing at a time. A multi-core CPU can chop onions, boil water, and grill a steak (medium-rare obviously) simultaneously.

2. The RAM (The Countertop)

Random Access Memory is the Kitchen Counter. It is where the Chef puts the ingredients (Data) and recipes (Code) they are using right now.

    The Counter (RAM): It is small (16GB - 32GB), but it is instant (nanoseconds). The Chef can grab ingredients without moving their feet.


Storage (The Pantry vs. The Warehouse)

When the power goes off, the RAM is wiped clean. We need a place to store data permanently. But not all storage is created equal:

    The SSD (Solid State Drive): This is the Walk-in Pantry. It is down the hall. It is fast (microseconds), but the Chef still has to stop chopping and walk over to get ingredients. This is the standard for modern laptops.
    The HDD (Hard Disk Drive): This is the Off-site Warehouse. It is huge and cheap, but it relies on spinning physical disks. Retrieving data from here is like driving to the warehouse (milliseconds). If a system uses an HDD, the Chef spends more time driving than cooking.


Understanding the differences between RAM, SSD, and HDD

    Throughput (Bandwidth): RAM is about 5x to 10x faster than the fastest SSDs at moving massive amounts of data.
    Latency (Response Time): RAM is about 2,000x to 10,000x faster at finding a single piece of data.

This speed difference is why Volatile Memory (RAM) exists. The CPU is so fast (Billions of cycles a second) that if it had to fetch every instruction directly from the SSD, the computer would be unusable.


The Bottleneck

A computer is only as fast as its slowest part.

    High CPU & Low RAM: The Chef is fast, but has no counter space. They waste time running back and forth to the Pantry (This is called "Paging" or "Swapping"). The computer slows to a crawl.
    Low CPU & High RAM: Huge counter space, but the Chef is slow. Ingredients just sit there waiting to be chopped.


Processes vs. Services

When you double-click on an icon, you aren't just "opening an app". You are telling the Operating System (OS) to execute a set of instructions. This then creates a Process.


Think of a Process like a contractor you hired. They show up, do a specific job (like "Run Google Chrome"), use your resources (CPU/RAM), and then leave. On a side note - contractor positions suck.


    Foreground Processes: These are the apps you see. You launched them. You control them.
    Background Processes & Services: This is the noise. These are the hundreds of contractors working in the basement that you didn't hire personally—Windows (OS) hired them. Windows hires so much. Too many. Windows stop with the bloat already. Why did you force AI on us. I'm not saying Linux is superior. But anyways, these background processes and services handle printing, audio, updates, and networking.


Services often run with SYSTEM privileges (God Mode). If a hacker hijacks a Service, they own the machine. If they hijack your Calculator app, they only own you.


Processes don't appear out of thin air, they are spawned (yes I say spawned at work) by other processes.

    Parent Process: The omnipotent creator.
    Child Process: The creation. I've personally got four child processes.


Why Cybersecurity Operators Care

When a user says "My computer is slow", they usually have no idea what they're talking about, but it usually means one of two things:

    Resource Exhaustion: A contractor (process) went rogue. They are eating all the food (RAM) or screaming so loud nobody else can hear (CPU).

2. The Hang: A process is waiting for an answer that isn't coming. It freezes the system while it waits.

It is your job as a operative learning the fundamentals to understand if the machine is actually underpowered, or if a rogue process is bleeding it dry. We will explore how to identify this later.


The Process Tree

Analyzing a chain of events that led to computer's being compromised either via malware or hands-to-keyboard hacking relies on the Process Tree - a hierarchical, tree-like structure that maps the parent-child relationships between processes running on a computer. We love process trees because we can use it to quickly identify something out of the ordinary. Does Microsoft Defender for Endpoint (an Endpoint Detection and Response tool) make easy to read trees like the one below? No. But, you'll see that once Azure Fundamentals is available.


It's important to understand what is a normal process tree and what is abnormal:

    Normal: You open Word (winword.exe). You type a document.
    Abnormal: You open Word (winword.exe). Word immediately spawns Command Prompt (cmd.exe).

Word shouldn't be launching a command terminal. That document contained a macro virus. If you only look at the list of processes, you see winword.exe and cmd.exe (both legit programs). If you look at the parent-child relationship, you see an attack.


---

1.2 TASK MANAGER

Beyond the Theory

Most regular users open Task Manager through their desktop search or if they're somewhat fancy (Ctrl + Shift + Esc) and stare at the "Processes" tab. Maybe they know how to find and kill a stuck process. However, most people do not know how to.


The Processes tab is the "Kiddie Pool" for those who don't know how to swim. It hides details to keep regular ID10T users from panicking. As an Operative, you live in the Details tab.


The PID (Process ID)

Names are effectively meaningless. Processes spawned by Malware can be named chrome.exe. But, Windows doesn't care about names, it cares about the PID.

    The PID is a unique number assigned to a process when it starts (e.g., 4522).
    If you see two svchost.exe files, the PID tells them apart.
    PIDs are recycled. If you reboot, the numbers change. If a process crashes and restarts, it gets a new PID. Watching a PID change is a sign of instability or persistence fighting.
    Constant Respawning: If a PID is changing every few seconds, the old process has crashed or exited and a new one is taking its place. The issue is rarely the process (PID) itself, but rather the parent process (PPID) that is failing to keep the child process alive.
    Rootkits or malware: A process with no name that constantly changes its PID may be a malware mechanism designed to be difficult to terminate.


The "Command Line" Column

By default, Windows hides how a program was launched. You need to enable the Command Line column.

    Normal: C:\Program Files\Google\Chrome\Application\chrome.exe
    Suspicious: C:\Users\Ben\AppData\Local\Temp\chrome.exe
    Malicious: powershell.exe -windowstyle hidden -enc X83...

Task: Go to your Task Manager -> Details Tab. Right-click the top header bar -> Select Columns -> Check "Command Line". This is good to keep enabled at all times as we want to see the mess of command lines.

If you only look at the Names, you could see something like:

    svchost.exe
    svchost.exe

But, if you look at the Command Line, you see actual useful information about the process:

    C:\Windows\System32\svchost.exe -k netsvcs (✅ Safe, running from System32)
    C:\Temp\svchost.exe (❌ DANGER, running from Temp)

You'll often find that malware tries to hide by running things from Temp folders. No one's going to suspect an innocent little Temp folder right? WRONG. Trust nothing.

The "Path" Loophole

Windows does not allow two files with the same name to exist in the same folder.

    You cannot have chrome.exe (Real) and chrome.exe (Fake) both sitting in C:\Program Files\Google\Chrome\.

HOWEVER, Windows has no problem at all running two files with the same name if they are in different folders.

    Real: C:\Windows\System32\svchost.exe
    Fake: C:\Users\Ben\AppData\Local\Temp\svchost.exe

In the standard "Processes" tab (the Kiddie Pool), both of these will just show up as "Service Host" or svchost.exe. To the untrained eye, they look identical.


Hiding in Plain Sight

There are few main ways malware tries to hide:

    Filename (The "Lazy" Way): Using a name that looks like the real thing but has a slight typo to avoid conflict.
    Example: cvchost.exe (instead of svchost.exe) or chrom.exe.
    Detection: You catch these by reading the name carefully. It may be difficult if not outright impossible to detect character lookalikes. And honestly who's going to memorize all the names of these background services running. That's what Google is for.
    For example here's Unicode character lookalikes. They may look the same on this site, but try copy pasting them into a Google search bar😉:
    a versus а
    c versus с
    d versus ԁ
    The bad guys know you won't try and kill a process named System32.exe because you're scared you'll break the computer. And rightly so, since Windows will bluescreen at any minor inconvenience. So, they may name their virus Systm32.exe (missing the 'e'). This is typically done so the command line path doesn't look suspicious and allows them to run the virus from legitimate folders.

    Path (The "Smart" Way): Using the exact legitimate name but running it from a temporary or hidden folder.
    Example: A crypto-miner named notepad.exe running out of your Downloads folder.
    Detection: You catch these by looking at the Command Line or Path column.
    Resource Hiding (The "Sneaky" Way): Smart malware (like C2 agents) sleeps 99% of the time, using 0% CPU so they don't show up on the radar. Dumb malware like Cryptojackers, which are a type of malware that mine cryptocurrency, can get greedy and max out the CPU/GPU. Making it easier to catch and remove.


Performance Tab: The Computer's EKG

This tab is where you can look for anomalies (spikes) in resource consumption.

    Cryptojackers which are a type of malware that mine cryptocurrency, will cause sustained spikes in CPU or GPU usage even when the computer is idle.

    High "Up time" (found under the CPU section) can indicate a machine hasn't been rebooted to apply critical security patches, leaving it vulnerable to exploits. I can not tell you how many times I've heard someone say they're restarted when they did not and uptime shows 7+ days.


App History Tab: The Paper Trail

By default, Windows 11 now has an option enabled called "Show history for all processes" under the Settings (gear icon) in the bottom left. With this enabled, you see every executable that has run on the system, including Chrome, Discord, and system tools. If unchecked (disabled), the list of "apps" under App history only show Microsoft Store apps (like Calculator, Mail, or News).


Finding the "Short-Lived" Malware

This tab is one of the few places that records data for a process after it has already closed. This is critical for catching malware that "bursts" (runs for a few seconds to steal data and then deletes itself).


The "Uninstalled Processes" Entry

You might notice a category named "Uninstalled processes". If this number is unusually high (multiple gigabytes of network usage), it means a program was run, performed a massive task, and was then deleted from the disk. This can be a sign of what we like to call a "living off the land" attack where a hacker downloads a tool, uses it, and wipes the evidence.

Network Usage vs. CPU Time

We typically focus on CPU spikes for malware detection. But in App History, you can sort by Network.

    If an executable named svchost.exe or notepad.exe (which should have zero network usage) shows 5GB of "Network" data over the last 30 days, you may have just found a data exfiltration point.


As a Mad Hat Operative, never assume the App History is just for "apps". It is your 30-day Black Box Flight Recorder. If you don't know what that is, a black box flight recorder is a nearly indestructible, bright orange device installed on aircraft to record flight data and cockpit audio for accident investigation. The more you know. Lucky for us, if a virus ran yesterday and is gone today we have LOGS to see how much data was sent and how hard it worked the CPU.


Startup Apps

Persistence is the goal of every hacker. They want their malware to survive a reboot.

    Check the "Startup impact" and the "Publisher." Malicious scripts often disguise themselves with names like Update.exe or SystemCheck.vbs.
    An "Enabled" startup item with a blank publisher or a suspicious file path (right-click and select "Open file location") can be useful for identifying signs of persistence.

Users Tab

On a typical Windows 11 computer, you should generally will only see your own username.

    If you see an unfamiliar user account listed as "Active" or "Disconnected," someone may have remotely logged in (via RDP or a malicious RAT). Yikes.
    Windows 11 natively restricts Remote Desktop (RDP) to a single user session at a time. HOWEVER, multiple RDP sessions are common on Windows Servers.
    Multiple users on a computer when there should only be one is a cause for concern. Obviously.
    You can expand the user to see exactly what processes users are running. If they are running cmd.exe or powershell.exe, which regular users typically do not, you are potentially dealing with an Adversary (yay cybersecurity buzzwords). This may be an active breach. You may want to delete your browser history... 

Services Tab: The Hidden Workers

Services are programs that run in the background without a user interface. They are the favorite hiding spot for advanced persistent threats (APTs). I generally will open Services tool directly if I ever need to check what's running or stopped, but it's available in the Task Manager as well.

    Hackers often create services that mimic legitimate ones (e.g., W32Time vs. W32Times).
    A service in a "Stopped" state that should be "Running" (like your antivirus service) suggests the malware has disabled your defenses. Right-click any suspicious service and select "Search online" to verify its legitimacy.

Understanding the finer details of background services will come in time after we explore how these are abused by hackers.


---

1.3 RESOURCE MONITOR

Seeing the Invisible

Task Manager buries the truth. It tells you who is working, but not what they are working on. For that, we use Resource Monitor (resmon.exe).


The "File in Use" Problem

Sometimes you try to delete a folder and Windows freaks out: "The action cannot be completed because the file is open in another program". Windows won't tell you which program, but Resource Monitor will.


Handles

A "Handle" is a digital grip. When a program opens a file, it grabs a Handle. It tells the Operating System "I am holding this file. Do not let anyone else touch it".

    Why Hackers use Handles: Malware often keeps a Handle on its own executable file to prevent anti-virus software from deleting it.

The CPU Tab

In Resource Monitor, the CPU tab allows you to search "Associated Handles". You can type the name of a file (e.g., passwords.txt), and it will show you exactly which process is secretly reading it.


"The Hand in the Cookie Jar"

For example, if we take a well known malware type Spyware, Resource Monitor can help spot unusual file interactions:

    Task Manager shows you a program named svchost.exe. That looks normal.
    You then search for "Associated Handles" and see that svchost.exe has a handle on chrome_cookies.sqlite or passwords.txt.
    A legitimate Windows service has no business holding a handle on your browser cookies. You have just identified process injection (injecting code into legitimate processes to hide from detection) without relying on an antivirus scan.


MISSION OBJECTIVE [UNLOCK_THE_MALWARE]:


Step 1: Create the "Malicious" Lock

Run this command in PowerShell on a Windows desktop. I promise it does nothing nefarious. It creates a file and locks it explicitly so that nothing else can touch it (simulating malware protecting its own executable):

$bytes = [System.IO.File]::Open("C:\Users\Public\malware.txt", "OpenOrCreate", "Read", "None")

Step 2: The Failure

Go to C:\Users\Public\ and try to delete malware.txt.

    Result: Windows throws the "File in Use" error.


Step 3: Investigate the "Malware"

Now, let's try and find the "malware" we just created:

    Open Resource Monitor.
    Go to the CPU tab.
    Expand Associated Handles.
    Search for: malware.txt.

Step 4: Kill the "Malware"

Resource Monitor will reveal that powershell.exe is the culprit.

    Right-click powershell.exe -> End Process.

    Now, you can delete the file. Nice.


Handles can be a mechanism in which malware uses to survive, and Resource Monitor is the tool used to break that grip.


Task Manager versus Resource Monitor

Task Manager is designed for a general list to get a sense of what is running and from what file path, but it keeps their specific activities hidden. With the malware.txt file you are faced with a locked file (malware.txt) that refuses to be deleted. Task Manager is blind here. It lists running processes like powershell.exe, but it cannot confirm which one (if there are multiple) has actually gripped the file. This is why Resource Monitor can be needed. It allows you to bypass the guessing game by searching 'Associated Handles' for the specific file name. It bridges the gap between the locked artifact and the running process, providing the definitive proof you need to kill the correct process without crashing the wrong service. Especially when dealing with masquerading processes.


The Memory Tab: Monitoring Data

The Memory tab in Resource Monitor is where you can potentially identify malware being sneaky with things like process injections or not being sneaky at all and causing obvious issues that cause the computer to run slow or crash.

    Hard Faults/sec: This isn't a disk error; it’s when a program tries to access data that isn't in the physical RAM and has to pull it from the "Page File" on your hard drive. High hard faults can indicate a Resource Exhaustion attack such as Denial of Service (DoS) or a program that is poorly written (leaking memory).
    Commit: Shows the amount of virtual memory reserved by the operating system for a process. A surprisingly high commit size may indicate an injection.
    The "Modified" Category: In the colored bar at the bottom, look for a "Modified" section (typically small on most computers). This represents data that must be written to disk before it can be used for something else. Large amounts of modified memory can be a sign of heavy encryption activity (like Ransomware) prepping data in the background. Ransomware being a form of malware that effectively locks someone's access to the computer, displaying a ransom message that requires payment to HOPEFULLY obtain a decryption key to unlock access to the computer.




The Disk Tab: What Data is Getting Touched

The Disk tab shows you every single file being read from or written to in real-time.

    Disk Activities: This section lists every file path currently being touched. If you see a process like notepad.exe or calc.exe having high Write (B/sec) activity in your C:\Users\Documents folder, you aren't looking at a calculator you’re looking at Ransomware encrypting your files.
    Something to look out for here is processes touching System32 or Drivers folders that aren't signed by Microsoft. For example, if a random executable is reading your NTDS.dit (the Active Directory database) or your browser's Login Data (path being C:\Users\<Username>\AppData\Local\Google\Chrome\User Data\Default\Login Data), that is one of those "Hand in the Cookie Jar" moments. You may have reason to panic.


The Network Tab: Detecting C2 Beacons

Most Malware has a form of Command and Control (C2) in place where it tries to beacon out from a compromised computer to a computer the hacker uses to control all the computers they've compromised. The Network tab shows you exactly where your data is going.

    Network Connections: This shows the Remote Address (IP) every process is talking to. An Operative looks for any C2 (Command & Control) Beacons. These are tiny, consistent "blips" of data sent every few minutes. It's also possible if the beacon is sophisticated it's inconsistent making it harder to detect.
    If you check "Listening Ports" and spot an unknown process with a port status of "Listening" it has turned your computer into a server. This is a classic sign of a Backdoor access to the computer or a RAT (Remote Access Trojan) waiting for instructions from the hacker.


MISSION OBJECTIVE [TRACE_THE_BEACON]:


Step 1: Create the "Malicious" Connection

Simulate a "calling home" beacon by running the below command in PowerShell. It will establish a connection to Google's DNS server and then end after 60 seconds (rerun the command if you run out of time before reaching the last step):

$client = New-Object System.Net.Sockets.TcpClient("8.8.8.8", 53); Write-Host "Connection Established. Monitoring for 60 seconds..."; Start-Sleep -Seconds 60; $client.Close(); Write-Host "Connection Closed."


Step 2: Now Investigate the "Beacon"

    Open Resource Monitor and go to the CPU tab.
    Locate the powershell.exe process in the list of processes. Select the toggle next to powershell.exe to filter by that process in Resource Monitor.
    Now go to the Network tab.
    Under TCP Connections you should now see powershell.exe reaching out to remote port 8.8.8.8. Our Google DNS beacon.


Obviously in a real attack, that IP wouldn't be Google (8.8.8.8). It would be a server in a different country, probably Russia. You have just identified a beacon which is a commonly seen Exfiltration Point.



---

1.4 THE STACK AND THE HEAP

The Geography of RAM

When a program asks for RAM (The Countertop), it doesn't just get a blank void. The memory is divided into two distinct zones with very different rules. Understanding this geography is the prerequisite for understanding memory corruption vulnerabilities (like Buffer Overflows) and deep in the weeds Exploit Development. It's also important to understand RAM well enough to understand malware like Fileless malware that operate directly in a computer's volatile memory to be super sneaky.


The Stack (Strict Order)

Picture a stack of dinner plates, all breakable ceramic ones. You're only allowed to add a plate to the top (Push) and you can only remove a plate from the top (Pop). This is LIFO (Last In, First Out). The Stack is obsessively organized. It exists primarily to manage function calls (in code) and keep the CPU on track.


    The "Stack Frame": Every time a function is called (e.g., Login Function), a new "plate" is pushed onto the stack. This plate contains the function’s LOCAL variables (variables that exist only in the function being ran compared to GLOBAL variables which are generally accessible by all functions). But what is more important to contain is the Return Address.


    The Return Address: Think of this as a bookmark. It tells the CPU, "When you are done with this function, go back to exactly line 42 in the previous function".

    Automatic Management: When the function finishes, the plate is "popped" off, and the memory is immediately reclaimed. The CPU reads the Return Address and jumps back to where it left off.


The Heap (Controlled Chaos)

The Heap is a like dirty pile of laundry. A big pool of memory used for dynamic data like the image you just uploaded or the text you are typing. Raw unadulterated data. The Heap is the massive pool of unstructured memory used for dynamic data that changes size or lasts a long time.

    Pointers: Since the Heap is unorganized, the computer needs a map to find things. It uses Pointers. These are tiny distinct variables that live on the Stack, but point to a specific address in the Heap. When I had to learn Pointers in my C coding class, I hated every second of it. So understanding the main point of them is fine for now.


    Manual Management: Unlike the Stack, the CPU doesn't automatically clean this up. Low-level programmers must explicitly ask for a chunk of memory (malloc) and explicitly release it (free). We're not going to get into the weeds of low-level versus high-level coding languages, just know that abusing the heap or stack targets languages that do need to free things in memory.

    Fragmentation: Over time as chunks are grabbed and released the Heap gets Swiss-cheesed with gaps, making it harder to find large contiguous blocks. Kind of like trying to find a matching pair of socks in that pile of laundry. Pro tip: buy the same pair of socks. Always.


The Libraries (Shared Knowledge)

When you write a program, you don't write every single instruction from scratch. Because that would be tedious and downright awful. If you want your program to "Connect to Wi-Fi" or "Open a Window" you don't necessarily have to write the code for that. Instead, you can borrow it by calling upon the shared code in the Operating System.

    Dynamic Link Libraries (DLLs): On Windows, these are files like kernel32.dll or user32.dll. They contain standard code for common tasks. The downside to being able to reference shared libraries like this is that it if you tamper with the code in those shared libraries, unexpected things can happen when a program calls upon it.

To save precious RAM, Windows loads these libraries once and lets every running program look at them. Instead of every chef in the kitchen bringing their own cook book, there is one copy chained to the desk that everyone shares.


Programs (on the Stack) will often "jump" into a Library to do a complex task, and then "return" to the Stack when finished.


Why Should Cybersecurity Operatives Care

To a normal user, these are the unknown finer details of how software runs on a computer. To a hacker or cybersecurity professional, these are the little details where you look to break or secure things.


---

1.5 BUFFER OVERFLOWS FOR DUMMIES

Smashing the Stack (Buffer Overflows)

The Stack's greatest strength of packing data next to control instructions is unfortunately its Achilles heel (weakness).

    Imagine you have a "plate" (Stack Frame) reserved for a password. The low-level programmer allocated space for 8 characters for that plate.
    An attacker comes along and inputs 100 characters.
    The input fills the password space and spills over the edge, overwriting the data below it (on the stack). Oh s#$%.
    If the attacker is precise, they can overwrite the Return Address. Instead of the CPU returning to the main program, the attacker points the Return Address to their own malicious code. Yikes. The program is now hijacked and all hell breaks loose.


Heap Exploitation (Use-After-Free)

Because the Heap requires manual cleanup, programmers can often make mistakes. Shocking I know.

    A program "frees" (deletes) a chunk of memory but keeps the Pointer (the map) that points to it. This is a "dangling pointer".

    An attacker tricks the program into putting malicious data into that exact "freed" spot.

    The program later tries to use the old "dangling pointer" thinking it's accessing safe data. But instead, it executes the attacker's payload. And boom goes the dynamite.


Memory Forensics

Understanding where data lives allows you to investigate it and hackers to steal it. If you're interested in Digital Forensics then you will have to intimately understand where data lives.

    Secrets in RAM: Encryption keys and unencrypted passwords often live in the Heap while an application is running. If you can dump the RAM (Heap analysis), you can often find credentials that are encrypted on the hard drive, but you just pulled them UNencrypted from RAM because...Mimikatz, a tool that can pull credentials from the Local Security Authority Subsystem Service (LSASS) process in RAM. One of my superiors at work heavily emphasized understanding LSASS. So...as they say, if you don't learn LSASS, then you'll look like an...

Modern Defenses

Now I, just like most everyone in a cybersecurity program learns the OG methods of abusing RAM. But modern Operating Systems (Windows 11, Linux, macOS) have introduced massive walls to stop the attacks described above. "Vanilla" Buffer Overflows rarely work on modern systems without advanced bypass techniques.


ASLR (Address Space Layout Randomization)

No, this has nothing to do with ASMR. Unless you like the sounds your computer makes. Every time you reboot or restart the program, Windows randomly shuffles the locations of the Stack, Heap, and Libraries. This stops attackers from using known hard-code memory addresses.

    Example: The password is always at address 0x123


Imagine trying to rob a specific house, but every time you drive down the street, the houses have physically swapped places. Security through obscurity? Not great, but it certainly helps slow an attacker down.


DEP (Data Execution Prevention) / NX (No-Execute)

Windows marks the Stack as "Data Only". The CPU is forbidden from executing anything written there. This prevents attackers from injecting their own malicious code (Shellcode) directly onto the Stack. Ok, not bad, getting better with the defense in depth here.


The chef (CPU) is trained to distinguish between a Menu item (instructions) and the Food (data). Even if you write "Rob the Bank" on a dinner plate, the chef refuses to leave the kitchen and go rob a bank. Because that would be silly.


Stack Canaries (/GS)

The compiler places a secret, random value (a "Canary") on the stack just before the Return Address. Before the function returns, the CPU checks if the Canary is still alive. If it has been fried into a lava chicken (overwritten), the program crashes immediately to prevent any malicious or unexpected behavior.


Shadow Stacks (Intel CET) - The Cutting Edge

The CPU maintains a second, hardware-protected "Shadow Stack" that only it can see. When a function is called, the Return Address is copied to both the normal stack and the Shadow Stack. If they don't match upon returning, the CPU kills the program. You can imagine this effectively kills many traditional attacks.


Heap Randomization & Safe Linking

Modern Heap managers (like Windows Segment Heap) heavily randomize where data lands and encrypt the pointers that link memory chunks together. If an attacker tries to overwrite a pointer, the decryption fails, and the program crashes safely. I realize we haven't covered encryption yet, but think of it like jumbling the code in a convoluted way that only the "randomizer" manager knows how to un-jumble.


How It Is Still Abused

You may be thinking, if the defenses are this good, why do we still learn this? Because attackers have moved from "kicking down the door" to "stealing the keys". And just like in math class where they teach you how to do things the "old" way before showing the "new" way, it's good to know it all.


ROP (Return-Oriented Programming)

Since DEP stops us from writing new code on the Stack, attackers use code that is already there. Where is it? In the Libraries.

    You can't write a note because you have no pen. But you have a stack of magazines (The Libraries). You cut out letters and words from the magazines and glue them together to form your message.
    An attacker can try to overwrite the Return Address to jump to tiny snippets of code inside kernel32.dll or ntdll.dll (called Gadgets), chaining them together to bypass defenses.


DLL Hijacking & Injection

Libraries are the most common vector for persistence (maintaining access to a computer and commonly referred to as "living off the land") and privilege escalation (getting admin/root privileges). A program may need a very specific library (e.g., UpdateHelper.dll) to run. Here's how it can play out:

    An attacker places a malicious file named UpdateHelper.dll in a folder where the program looks first.
    When the program starts it picks up the fake dll instead of the real one thereby executing the attacker's code with the program's privileges.
    The attacker forces a running program to load a library it never asked for, effectively forcing the program to quietly compromise itself with malicious code. Neat.


Logic Corruption & Data-Only Attacks

Sometimes you don't need to execute code, you just need to change a decision. If a variable on the Stack says IsUserAdmin = 0 (False) and you can overflow a buffer just enough to change that 0 to a 1 (True) then you haven't executed malicious code. You've simply tricked the program's logic. ASLR and DEP generally do not stop this.


You'll come to find out that not every defense on it's own is 100% foolproof. Hence, why the term "defense in depth" is thrown around in cybersec.


Type Confusion (The "Ghost" in the Machine)

Type Confusion is some dark arts, big brain s#$%. It’s exactly why Chrome, Safari, and Firefox are constantly pushing emergency zero-day patches. And yet, browser updates can go unignored forever. Even with Chrome doing most of the work, it's too much to ask people to click the "Finish update" button...


The CPU is completely blind and incredibly...stupid. The trade off is it's fast. It doesn’t know what a "User Account" is. It doesn't know what a "PNG Image" is. All it sees is a chunk of raw 1s and 0s in the Heap, and a Blueprint, which is a structure or class definition in C++ which is a low-level programming language that web browsers are commonly built with that tells CPUs how to read those bytes.


Say we have two blueprints in our code:

    The User Blueprint: Expects 8 bytes for a Name, and 4 bytes for an IsAdmin permission (0 for No, 1 for Yes).
    The Image Blueprint: Expects 8 bytes for a Filename, and 4 bytes for PixelColor.

If the program creates a User, it uses the User Blueprint. Simple right?


Queue Advanced Hacker

Browsers are built on C++. C++ allows programmers to do something called "Type Casting". It’s a way for the programmer to tell the compiler "Trust me, bro. I know this memory chunk looks like an Image, but I need you to treat it like a User".


Modern web browsers also run JavaScript and JavaScript is a chaotic, dynamically-typed mess where a variable can be a word one second and a number the next. The browser has to guess and optimize these types constantly to run fast.


If a hacker finds a bug in the code that makes the engine guess wrong? Game over.


    An attacker uploads a carefully crafted Image. They set the Filename to something normal, but set the PixelColor data to the exact hex value of 1. Then, they trigger the logic bug. You trick the browser into pointing at your Image's memory chunk, but tell it to read it using the User Blueprint.

The blind CPU walks over to your Image data, it reads the first 8 bytes and thinks, "Ah yes, this is the User's Name. Most good I have a blueprint for this". Then, it reads the next 4 bytes (your PixelColor value of 1) and thinks, "Very nice, this is the IsAdmin permission. Look at that, it's set to 1! This dude is a VIP".


The hacker just gained Admin privileges with no buffer overflow, no malicious shellcode attempted on the Stack, and without frying the Canary.


All of the techniques mentioned could be entire lessons on their own, but it's good to have a quick understanding of where we stand today with defenses and attack methods.


---








