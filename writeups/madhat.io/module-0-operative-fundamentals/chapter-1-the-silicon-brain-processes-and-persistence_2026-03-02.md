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

