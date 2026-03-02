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



