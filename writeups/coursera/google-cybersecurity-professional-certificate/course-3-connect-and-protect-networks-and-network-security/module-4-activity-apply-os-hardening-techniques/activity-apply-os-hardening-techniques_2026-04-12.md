# Activity: Apply OS hardening techniques
**Google Cybersecurity Professional Certificate – Course Activity**  
Neil Steven | April 12, 2026 | Portfolio: [https://github.com/neilsteven1997/cybersecurity](https://github.com/neilsteven1997/cybersecurity/tree/main/writeups/coursera/google-cybersecurity-professional-certificate/course-3-connect-and-protect-networks-and-network-security/module-4-activity-apply-os-hardening-techniques)

## Executive Summary
>[!Note]
>_This scenario is based on a fictional company:_

Scenario

You are a cybersecurity analyst for yummyrecipesforme.com, a website that sells recipes and cookbooks. A former employee has decided to lure users to a fake website with malware. 

The former employee/ hacker executed a brute force attack to gain access to the web host. They repeatedly entered several known default passwords for the administrative account until they correctly guessed the right one. After they obtained the login credentials, they were able to access the admin panel and change the website’s source code. They embedded a javascript function in the source code that prompted visitors to download and run a file upon visiting the website. After embedding the malware, the hacker changed the password to the administrative account. When customers download the file, they are redirected to a fake version of the website that contains the malware. 

Several hours after the attack, multiple customers emailed yummyrecipesforme’s helpdesk. They complained that the company’s website had prompted them to download a file to access free recipes. The customers claimed that, after running the file, the address of the website changed and their personal computers began running more slowly. 

In response to this incident, the website owner tries to log in to the admin panel but is unable to, so they reach out to the website hosting provider. You and other cybersecurity analysts are tasked with investigating this security event.

To address the incident, you create a sandbox environment to observe the suspicious website behavior. You run the network protocol analyzer tcpdump, then type in the URL for the website, yummyrecipesforme.com. As soon as the website loads, you are prompted to download an executable file to update your browser. You accept the download and allow the file to run. You then observe that your browser redirects you to a different URL, greatrecipesforme.com, which contains the malware.  

The logs show the following process:

1. The browser initiates a DNS request: It requests the IP address of the yummyrecipesforme.com URL from the DNS server.

2. The DNS replies with the correct IP address. 

3. The browser initiates an HTTP request: It requests the yummyrecipesforme.com webpage using the IP address sent by the DNS server.

4. The browser initiates the download of the malware.

5. The browser initiates a DNS request for greatrecipesforme.com.

6. The DNS server responds with the IP address for greatrecipesforme.com.

7. The browser initiates an HTTP request to the IP address for greatrecipesforme.com.

A senior analyst confirms that the website was compromised. The analyst checks the source code for the website. They notice that javascript code had been added to prompt website visitors to download an executable file. Analysis of the downloaded file found a script that redirects the visitors’ browsers from yummyrecipesforme.com to greatrecipesforme.com. 

The cybersecurity team reports that the web server was impacted by a brute force attack. The disgruntled hacker was able to guess the password easily because the admin password was still set to the default password. Additionally, there were no controls in place to prevent a brute force attack. 

Your job is to document the incident in detail, including identifying the network protocols used to establish the connection between the user and the website.  You should also recommend a security action to take to prevent brute force attacks in the future.

**Goal:** _Writing an incident report for this security event. Using the tcpdump log file, determine which network protocol is identified in the packet captures during the investigation._

---

## Cybersecurity Incident Report: Apply OS hardening techniques

### Part 1: Identified network protocols involved in the incident
* DNS – to resolve yummyrecipesforme.com to its IP address
      - to resolve greatrecipesforme.com to its IP address after redirection
* HTTP - to request and load the compromised webpage from yummyrecipesforme.com
	    - o request and load the fake/malware site greatrecipesforme.com after redirection
* TCP – underlying transport layer for both DNS (in some cases) and all HTTP requests (reliable delivery)


### Part 2: Documented Incident

Brute-force attack on the web host admin panel using default password + no rate limiting / brute-force protection. Hacker gained access, injected malicious JavaScript into the site’s source code. JS prompt tricks visitors to download and run an executable file. File execution redirected browser from legitimate domain (yummyrecipesforme.com) → fake malware domain (greatrecipesforme.com). Slowed user PCs, potential further malware infection, loss of trust, helpdesk overload.

Evidence: tcpdump logs showed DNS → HTTP sequence + redirection; injected JS; confirmed redirect script.

14:18:36.786589 IP your.machine.36086 > yummyrecipesforme.com.http: Flags
[P.], seq 1:74, ack 1, win 512, options [nop,nop,TS val 3302576859 ecr
3302576859], length 73: HTTP: GET / HTTP/1.1
14:18:36.786595 IP yummyrecipesforme.com.http > your.machine.36086: Flags
[.], ack 74, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859],
length 0
...<a lot of traffic on the port 80>...

14:20:32.192571 IP your.machine.52444 > dns.google.domain: 21899+ A?
greatrecipesforme.com. (24)
14:20:32.204388 IP dns.google.domain > your.machine.52444: 21899 1/0/0 A
192.0.2.17 (40)

### Part 3: Recommended remediation for brute force attacks
* Implement account lockout 
* Implement strong password policy 
* Disallowing previous passwords from being reused
* Implement multifactor authentication (MFA)  
* Enable a web application firewall (WAF) to rate limiting on login attempts. 
* Enforce the immediate changing of all default passwords and password complexity. 

---

## Self-Assessment Score
Completed the Activity 1/1
Self-assessment: Passed (100% / 1 point)

## Artifacts
- Coursera activity: [https://www.coursera.org](https://www.coursera.org/learn/networks-and-network-security/assignment-submission/6XQKU/activity-apply-os-hardening-techniques/attempt)
- GitHub writeup: [https://github.com/neilsteven1997/cybersecurity](https://github.com/neilsteven1997/cybersecurity/blob/main/writeups/coursera/google-cybersecurity-professional-certificate/course-3-connect-and-protect-networks-and-network-security/module-4-security-hardening_2026-04-02.md)

---
Last updated: April 26, 2026






