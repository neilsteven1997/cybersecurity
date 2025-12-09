Leveraging Object-Oriented Automation in PowerShell

PowerShell is a command-line shell and scripting environment designed by Microsoft for system administration, 
configuration management, and automation. Unlike traditional shells like Command Prompt (CMD) or Bash, which 
process data as plain **text streams**, PowerShell is built on the **.NET framework** and handles data as 
structured **objects**. This object-oriented approach is the single most important distinction and forms the 
basis for its superior capabilities.

Core Concepts Learned
The structure of PowerShell is centered around **Cmdlets** (pronounced "command-lets"). These are small, 
single-function commands that follow a predictable **Verb-Noun** naming convention (e.g., `Get-Process`, 
`Set-ExecutionPolicy`). This standardization improves script readability and makes the language 
self-discoverable. Crucially, Cmdlets output objects—not text—allowing the use of the **Pipeline** (`|`) 
to pass the structured output of one command as input to the next. For instance, piping the output of 
`Get-Process` to `Where-Object` allows for advanced filtering and manipulation that is vastly more efficient 
than text parsing in CMD.

Relevance to Cybersecurity
PowerShell has dual utility in security operations. For **Blue Teams** (defenders), its deep integration with 
Windows systems allows for comprehensive **log analysis** of Windows Event Logs, automated **incident response** 
actions (like isolating a compromised endpoint via firewall rules), and auditing security configurations at scale. 
For **Red Teams** and threat actors, PowerShell is a powerful **post-exploitation** tool, frequently leveraged for 
**file-less attacks** (living off the land) where malicious code is executed directly in memory, bypassing many 
legacy endpoint security solutions and leaving minimal artifacts on disk. Modern versions of PowerShell include 
security features like **AMSI** (Antimalware Scan Interface) integration and detailed **script block logging** to 
help mitigate this threat.


Type command: PowerShell

Get-Content = retrieves and displays the content of a file in the console. (Verb: Get, Noun: Content).

Set-Location = changes the current working directory (similar to cd in Linux/CMD). (Verb: Set, Noun: Location).

Get-Command = lists all available cmdlets, functions, aliases, and scripts that can be executed in the current 
PowerShell session.

Get-Command -CommandType "Function" = filters the list of available commands to display only those with the type "Function".

Get-Help cmdlet_name = provides detailed information about a specific cmdlet, including usage, parameters, and examples 
(the go-to tool for learning PowerShell commands).

Get-Help cmdlet_name -examples = displays a list of common usage examples for the specified cmdlet.

Get-Alias = lists all available aliases (shortcuts or alternative names) for cmdlets in the current PowerShell session.

dir = an alias for Get-ChildItem (used to list files and folders, similar to the Windows CMD command).

cd = an alias for Set-Location (used to change directories, similar to the Windows CMD command).


Find-Module module_name
Online
Searches for modules in online repositories (like the PowerShell Gallery).

Find-Module -Name "pattern*"
Online
Searches online for modules with a similar name by using a wildcard (*) after a partial name pattern.

Install-Module module_name
Online
Downloads and installs the specified module from the online repository, making its cmdlets available.

Get-ChildItem -Path "C:\folder" = lists the files and directories at the path specified by the -Path parameter.

Get-ChildItem = lists the files and directories in the current working directory (if no path is specified).

New-Item -Path "path" -ItemType "type" = creates a new item (file or directory) at the specified path, where type is 
either "File" or "Directory".

Copy-Item source destination
Copies files and directories (equivalent to copy in Windows CLI).

Move-Item source destination
Moves files and directories (equivalent to move in Windows CLI).

Remove-Item path
Removes both files and directories (equivalent to del and rmdir in Windows CLI).

Where-Object -Property -eq "value"
Filters objects where a property is equal to (-eq) a specified value (e.g., -
Extension -eq ".txt").

Where-Object -Property -ne "value"
Filters objects where a property is not equal to (-ne) a specified value.

Where-Object -Property -gt value
Filters objects where a property is greater than (-gt) a specified value (strict comparison).

Where-Object -Property -ge value
Filters objects where a property is greater than or equal to (-ge) a specified value (non-strict).

Where-Object -Property -lt value
Filters objects where a property is less than (-lt) a specified value (strict comparison).

Where-Object -Property -le value
Filters objects where a property is less than or equal to (-le) a specified value (non-strict).

Select-Object properties
Selects specific properties from objects or limits the number of objects returned (used to refine output).

Select-String "pattern" file.txt
Searches for a text pattern within files (similar to grep in Linux or findstr in Windows CLI).

Get-ComputerInfo
Retrieves comprehensive system information (OS, hardware, BIOS, etc.), providing a full system configuration snapshot 
(more detail than systeminfo).

Get-LocalUser
Lists all local user accounts on the system, displaying the username, account status, and description for each.

Get-NetIPConfiguration
Retrieves detailed network interface information, including IP addresses, DNS servers, and gateway configurations 
(similar to the traditional ipconfig command).

Get-NetIPAddress
Retrieves detailed information for all IP addresses configured on the system, including those that are not currently active.

Get-Process
Retrieves a detailed view of all currently running processes on the system, including CPU and memory usage (powerful 
for monitoring and troubleshooting).

Get-Service
Retrieves the status of services on the machine (running, stopped, paused); useful for troubleshooting and anomaly detection.

Get-NetTCPConnection
Displays current TCP network connections, showing local and remote endpoints (useful for incident response and malware 
analysis to find backdoors).

Get-FileHash filename
Generates a cryptographic hash of a file (useful in incident response and malware analysis to verify file integrity or 
identify known malware).

Invoke-Command -ComputerName "ServerName" -FilePath "C:\script.ps1"
Executes a custom script (C:\script.ps1) on a remote computer specified by -ComputerName (essential for remote management 
and automation).

Invoke-Command -ComputerName "ServerName" -ScriptBlock { Get-Service }
Executes an inline command or sequence of commands (Get-Service in this case) on a remote computer without needing a 
separate script file.

Service | TCP Port | Command Sequence | Purpose 

Echo Server
7
telnet <target_VM_IP> 7
Establishes connection to the echo service.

Daytime Server
13
telnet <target_VM_IP> 13
Connects and retrieves the time/date before automatically closing.

Web Server (HTTP)
80
1. telnet <target_VM_IP> 80 
2. GET / HTTP/1.1 
3. Host: telnet.thm 
4. Press Enter (Blank Line)
Manually sends an HTTP request to retrieve the main page.
