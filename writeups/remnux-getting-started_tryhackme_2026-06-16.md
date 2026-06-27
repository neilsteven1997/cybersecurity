# REMnux: Getting Started

---

REMnux provides a specialized Linux distribution tailored for malware analysis during security incidents. It comes preloaded with 
essential tools such as Volatility, YARA, Wireshark, oledump, and INetSim, creating an isolated environment for safely dissecting 
suspicious files, documents, and memory captures without endangering the host system. The setup eliminates the need for manual tool
installations, allowing analysts to focus on examination tasks.

Static analysis of a potentially malicious Excel document named agenttesla.xlsm began in the designated lab directory using oledump.py. 
This Python utility parses OLE2 files, also known as Structured Storage or Compound File Binary Format, which Microsoft developed for
Object Linking and Embedding to bundle various data types into single containers like spreadsheets. The initial scan revealed embedded
streams within xl/vbaProject.bin, including indicators of VBA macros in specific data streams marked by capital M. Selecting the 
relevant stream with the -s parameter exposed the macro contents, which appeared as a hex dump. Applying the --vbadecompress option 
then yielded readable VBA script. Key elements included an obfuscated string assigned to Sqtnew containing PowerShell instructions. 
Processing this string through CyberChef with successive Find/Replace operations to strip asterisks and carets produced a clear 
command that launched hidden PowerShell, bypassed execution policy, downloaded a secondary executable from a remote location to a 
temporary file renamed with .exe extension, and executed it. This pattern represents a common technique threat actors employ to evade
initial detection. Familiarity with CyberChef, covered in its dedicated room, aided the deobfuscation.

Simulating network behavior during analysis proved critical for observing malware interactions. INetSim, the Internet Services 
Simulation Suite included in REMnux, enabled this without building full virtual infrastructure. Configuration involved identifying the
local machine IP via ifconfig, editing /etc/inetsim/inetsim.conf to set dns_default_ip to that address, and starting the service with
sudo inetsim. The simulation listened on the machine IP and handled various protocols, with a minor http service failure noted but 
overall operation confirmed by the "Simulation running" message. From a separate AttackBox, connections to the simulated environment 
via browser or wget retrieved fake payloads such as second_payload.zip and second_payload.ps1, mimicking secondary stage downloads. 
Connection logs generated in /var/log/inetsim/report/ captured details including timestamps, protocols, methods, URLs, and served fake
files, providing visibility into the emulated traffic.

Memory image preprocessing formed another core investigative step. Using Volatility 3 on the wcry.mem sample from the Wcry directory,
multiple Windows-focused plugins were executed individually: windows.pstree.PsTree for process hierarchy, windows.pslist.PsList for
active processes, windows.cmdline.CmdLine for command-line arguments, windows.filescan.FileScan for file objects,
windows.dlllist.DllList for loaded modules, windows.psscan.PsScan for process scanning, and windows.malfind.Malfind for injected code
detection. To accelerate bulk processing, a for-loop iterated over these plugins in quiet mode, redirecting each output to 
correspondingly named text files prefixed with wcry. Complementary extraction via the Linux strings utility captured ASCII, 
little-endian Unicode, and big-endian Unicode strings into separate files. These preparatory steps allow subsequent analysts to search
artifacts more efficiently.

---

**Commands Extracted**

| Description | Code/Command |
|-------------|--------------|
| Initial oledump scan of Excel document | oledump.py agenttesla.xlsm |
| Select and view specific macro stream | oledump.py agenttesla.xlsm -s 4 |
| Decompress VBA macro for readability | oledump.py agenttesla.xlsm -s 4 --vbadecompress |
| Start INetSim simulation | sudo inetsim |
| Edit INetSim config | sudo nano /etc/inetsim/inetsim.conf |
| Verify dns_default_ip setting | cat /etc/inetsim/inetsim.conf \| grep dns_default_ip |
| Download fake payload via simulated HTTPS | sudo wget https://<REDACTED_IP>/second_payload.zip --no-check-certificate |
| Download alternative fake payload | sudo wget https://<REDACTED_IP>/second_payload.ps1 --no-check-certificate |
| View INetSim report | sudo cat /var/log/inetsim/report/report.<ID>.txt |
| Run single Volatility plugin (example) | vol3 -f wcry.mem windows.pstree.PsTree |
| Bulk preprocess multiple Volatility plugins to files | for plugin in windows.malfind.Malfind windows.psscan.PsScan windows.pstree.PsTree windows.pslist.PsList windows.cmdline.CmdLine windows.filescan.FileScan windows.dlllist.DllList; do vol3 -q -f wcry.mem $plugin > wcry.$plugin.txt; done |
| Extract ASCII strings from memory image | strings wcry.mem > wcry.strings.ascii.txt |
| Extract little-endian Unicode strings | strings -e l wcry.mem > wcry.strings.unicode_little_endian.txt |
| Extract big-endian Unicode strings | strings -e b wcry.mem > wcry.strings.unicode_big_endian.txt |

---

### Key Takeaways
- Explore the preinstalled tools in REMnux for efficient malware analysis without custom setup.
- Use oledump.py to inspect OLE2 structures and extract VBA macros from documents, applying -s to target streams and --vbadecompress
  for readable output.
- Deobfuscate embedded scripts via CyberChef Find/Replace operations to reveal PowerShell behaviors such as hidden execution, policy
  bypass, web downloads, and process starts.
- Configure and run INetSim by updating dns_default_ip in its config file to the local machine address, then start the service to
  simulate network responses for observing malware callbacks.
- Switch between REMnux and AttackBox VMs as needed to perform realistic download tests against the simulated environment.
- Generate and review INetSim reports in /var/log/inetsim/report/ to log captured connections, protocols, and served files.
- Preprocess memory images with Volatility 3 plugins focused on processes, command lines, files, DLLs, and injected code, using loops
  to output results to text files for streamlined review.
- Supplement with strings extractions in ASCII and Unicode variants to pull readable artifacts from memory dumps.
- REMnux centers on analysis of malicious programs, documents, memory captures, and related artifacts, serving as a ready lab
  environment.

---

### Gallery 

<p align="center">
  <table>
    <tr>
      <td align="center"><img src="images/day-14-aoc-2025-defaced-website.png" alt="DoorDash website defaced with Hopperoo message after container escape" 
  width="450"/>
      <td align="center"><img src="images/day-14-aoc-2025-restored-website.png" alt="Restored website" width="450"/></td>
    </tr>
    <tr>
      <td align="center"><strong>Figure 1a:</strong> Final defacement after container escape</td>
      <td align="center"><strong>Figure 1b:</strong> Restored website after running restoration script</td>
    </tr>
    <tr>
      <td align="center"><img src="images/day-14-aoc-2025-deployer-bash-flag.png" alt="Using deployer bash to find the flag" 
  width="450"/>
      <td align="center"><img src="images/day-14-aoc-2025-secret-code.png" alt="Finding secret code by incrementing the number on website link" width="450"/></td>
    </tr>
     <tr>
      <td align="center"><strong>Figure 2a:</strong> Using deployer bash to find the flag</td>
      <td align="center"><strong>Figure 2b:</strong> Incrementing the number on link to find secret code</td>
    </tr>
  </table>
</p>




<p align="center">
  <table>
    <tr>
      <td align="center"><img src="images/day-14-aoc-2025-defaced-website.png" alt="DoorDash website defaced with Hopperoo message after container escape" 
  width="450"/>
      <td align="center"><img src="images/day-14-aoc-2025-restored-website.png" alt="Restored website" width="450"/></td>
    </tr>
    <tr>
      <td align="center"><strong>Figure 3a:</strong> Final defacement after container escape</td>
      <td align="center"><strong>Figure 3b:</strong> Restored website after running restoration script</td>
    </tr>
    <tr>
      <td align="center"><img src="images/day-14-aoc-2025-deployer-bash-flag.png" alt="Using deployer bash to find the flag" 
  width="450"/>
    </tr>
     <tr>
      <td align="center"><strong>Figure 4a:</strong> Using deployer bash to find the flag</td>
    </tr>
  </table>
</p>




---






