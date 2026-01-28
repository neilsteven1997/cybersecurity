# Offensive Security Intro

--- 

Offensive security centers on adopting an attacker's mindset to identify and exploit weaknesses in systems before malicious actors do. 
The approach involves breaking into computers, leveraging software vulnerabilities, and discovering unintended access paths in 
applications to simulate unauthorized entry, ultimately strengthening defensive postures.

In the TryHackMe Offensive Security Intro room, the exercise walks through a controlled first website compromise within a legal, 
isolated lab. The platform supplies virtual machines that reset to a clean state, allowing unrestricted experimentation without 
permanent consequences.

The lab deploys a simulated banking web application named FakeBank running on a virtual machine that launches automatically or via a 
manual start button. The interface splits the screen, placing instructional content on one side and the target environment on the 
other, with a browser already open to the site. The split view can be toggled later if needed.

The objective requires compromising the FakeBank application to illicitly add funds, using a legitimate low-privilege user account 
provided as the starting point. A straightforward tactic targets undiscovered endpoints that expose privileged functionality through 
non-obvious URLs, often left accessible due to predictable naming patterns.

Directory enumeration serves as the initial technique. The tool dirb executes brute-force discovery against the target by testing 
entries from a wordlist of common filenames and paths. From the machine's terminal—accessed via the desktop icon—the command dirb 
http://fakebank.thm runs using the default wordlist at /usr/share/dirb/wordlists/common.txt (a duplicate resides on the desktop for 
inspection). Execution scans approximately 4610 generated words against the base URL, reporting discovered resources with HTTP 
status codes and sizes.

The scan reveals two accessible paths: http://fakebank.thm/bank-deposit (200 OK, size 4663) and http://fakebank.thm/images 
(301 redirect, size 179). Accessing the /bank-deposit endpoint in the browser exposes a deposit form not intended for regular users.

Using this form, funds can be credited directly to the provided account number 8881. Depositing $2000 or more updates the balance 
visibly on the main account page after confirming the transaction and returning via the receipt's navigation button.

---

**Code/Command Table**

| Description                          | Code/Command                                      |
|--------------------------------------|---------------------------------------------------|
| Run dirb against the FakeBank site using default common wordlist | dirb http://fakebank.thm                          |

---

**Key Takeaways**
- Offensive security requires emulating attacker thinking to expose vulnerabilities proactively.
- Virtual lab environments like TryHackMe's allow safe, reversible experimentation on simulated targets.
- Directory brute-forcing with tools such as dirb uncovers hidden endpoints by testing predictable path names against a wordlist.
- Hidden administrative or privileged pages (e.g., /bank-deposit) can grant unintended functionality like unauthorized fund addition
  when left unauthenticated or insufficiently protected.
- Starting from a regular user account, discovering and exploiting such endpoints demonstrates a basic privilege escalation vector in
  web applications.

---
