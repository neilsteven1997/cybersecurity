# Logs Fundamentals

--- 

Logs serve as the digital records of activities on systems and applications, capturing both routine operations and malicious actions 
to support reconstruction of events even when attackers attempt to minimize their footprint. Just as investigators combine physical 
evidence such as damaged entry points, environmental traces, and external recordings to identify a perpetrator, security teams rely on
these records to determine attack execution and attribution. 

Their practical value appears across security events monitoring where real-time review detects anomalies, incident investigation and 
forensics where they supply the full sequence of events for root cause analysis, troubleshooting by recording errors in systems or 
applications, performance monitoring through application insights, and auditing and compliance by creating verifiable trails of actions.

Categorization of logs by content type prevents analysts from drowning in unrelated entries during an investigation. System logs 
document operating system behaviors such as startups and shutdowns, driver loading, errors, and hardware changes for troubleshooting. 
Security logs track authentication, authorization, policy modifications, account alterations, and abnormal events to support incident 
detection and investigation. Application logs record user interactions, program changes, updates, and errors tied to specific software.
Audit logs detail data access, system modifications, user behaviors, and policy enforcement for compliance and monitoring needs. 
Network logs capture inbound and outbound traffic details useful for diagnostics and attack tracing. Access logs describe interactions
with resources including web servers, databases, and applications. 

Further log types exist depending on the applications and services deployed. Analysis involves scanning for abnormal patterns or 
specific activities, with manual methods applied to Windows event logs and web server access logs in the covered exercises. 

Windows operating systems store activities in dedicated files covering application errors and warnings, system operations such as 
driver and service issues plus startup and shutdown data, and security events including logins, account changes, and policy updates. 
Event Viewer supplies the graphical interface for review, launched by searching the Start menu, with logs grouped under Windows Logs 
and tools for filtering and inspection. Individual entries list a description of the activity, the log name, the logged timestamp, and 
an Event ID that uniquely identifies the type of occurrence. Targeted searches become straightforward with identifiers such as 4624 for
successful logins, 4625 for failed logins, 4634 for successful logoffs, 4720 for account creation, 4724 for password reset attempts, 
4722 for enabling accounts, 4725 for disabling accounts, and 4726 for account deletion. The Filter Current Log function isolates entries
by Event ID, for instance displaying every instance of 4624. 

In the exercise replicating a Friday breach of a file server with data exfiltration, the compromised system’s logs are examined on the
provided virtual machine started in split-screen view using username Administrator, password ********, and IP <MACHINE_IP> to trace 
attacker actions preceding file server access. 

Web server access logs, commonly located at /var/log/apache2/access.log on Apache installations, record every request with the client
IP address, timestamp, request method and URL, server status code, and User-Agent string describing the operating system and browser.
Manual review on Linux systems uses command-line utilities to inspect or manipulate these text files. 

---

| Description | Code/Command |
|-------------|--------------|
| Display the contents of a log file | cat access.log |
| Combine multiple log files into one | cat access1.log access2.log > combined_access.log |
| Search for a specific string such as an IP address in a log file | grep "192.168.1.1" access.log |
| View a log file page by page with interactive search | less access.log |

---

**Extracted Tables**

**Use Cases of Logs**

| Use Case | Description |
|----------|-------------|
| Security Events Monitoring | Logs help us detect anomalous behavior when real-time monitoring is used. |
| Incident Investigation and Forensics | Logs are the traces of every kind of activity. It offers detailed information on what happened during the incident. The security team utilizes the logs to perform root cause analysis of incidents. |
| Troubleshooting | As the logs also record the errors in systems or applications, they can be used to diagnose issues and helpful in fixing them. |
| Performance Monitoring | Logs can also provide valuable insights into the performance of applications. |
| Auditing and Compliance | Logs play a major role in Auditing and Compliance, making it easier with its capability to establish a trail of different kinds of activities. |

---

**Types Of Logs**

| Log Type | Usage | Example |
|----------|-------|---------|
| System Logs | The system logs can be helpful in troubleshooting running issues in the . These logs provide information on various operating system activities. | - System Startup and shutdown events<br>- Driver Loading events<br>- System Error events<br>- Hardware events |
| Security Logs | The security logs help detect and investigate incidents. These logs provide information on the security-related activities in the system. | -Authentication events<br>- Authorization events<br>- Security Policy changes events<br>- User Account changes events - Abnormal Activity events |
| Application Logs | The application logs contain specific events related to the application. Any interactive or non-interactive activity happening inside the application will be logged here. | - User Interaction events<br>- Application Changes events<br>- Application Update events<br>- Application Error events |
| Audit Logs | The Audit logs provide detailed information on the system changes and user events. These logs are helpful for compliance requirements and can play a vital role in security monitoring as well. | - Data Access events<br>- System Change events<br>- User Activity events<br>- Policy Enforcement events |
| Network Logs | Network logs provide information on the network’s outgoing and incoming traffic. They play crucial roles in troubleshooting network issues and can also be handy during incident investigations. | - Incoming Network Traffic events<br>- Outgoing Network Traffic events<br>- Network Connection Logs - Network Logs |
| Access Logs | The Access logs provide detailed information about the access to different resources. These resources can be of different types, providing us with information on their access. | - Webserver Access Logs<br>- Database Access Logs - Application Access Logs<br>- Access Logs |

---

**Important Windows Events**

| Event ID | Description |
|----------|-------------|
| 4624 | A user account successfully logged in |
| 4625 | A user account failed to login |
| 4634 | A user account successfully logged off |
| 4720 | A user account was created |
| 4724 | An attempt was made to reset an account’s password |
| 4722 | A user account was enabled |
| 4725 | A user account was disabled |
| 4726 | A user account was deleted |

---

### Key Takeaways
- The different types of logs
- How to analyze logs
- Analyzing Windows Event logs
- Analyzing Web Access logs

---




