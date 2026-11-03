# Enumeration & Brute Force

---

Authentication enumeration is the process of examining an application's authentication 
mechanisms to identify information that could expose valid accounts or weaknesses in 
account protection. The assessment covers username validation, password requirements, 
login responses, password-reset functionality, and session-related mechanisms. 
Enumeration establishes which accounts may exist before attempting password-based attacks,
reducing unnecessary guesswork.

Username enumeration commonly relies on differences in application responses. 
Registration forms may reveal whether an email address or username is already registered.
Password-reset forms can disclose the same information through different confirmation 
messages. Login forms may distinguish between an unknown username and an incorrect 
password. These responses provide evidence that can be used to compile lists of 
potentially valid accounts.

Password policies can also reveal useful information. The PHP example in the source uses
a regular expression to require at least one uppercase letter, one digit, and one symbol.
If an application exposes these requirements through validation errors, an attacker can
use the disclosed policy to construct a more targeted password dictionary.

Several application features are particularly relevant during enumeration. Registration
pages, password-reset functions, verbose authentication errors, and information from 
previous data breaches can all expose account-related information. Reused credentials 
are especially relevant when previously compromised usernames and passwords are tested 
against other platforms.

Verbose errors can disclose more than authentication status. Depending on the 
application's implementation, error messages may expose internal file paths, directory
structures, database table and column names, usernames, and other personal information. 
Such disclosures can provide additional clues about the application's internal structure.

The source describes several ways of inducing verbose errors, including submitting 
invalid login credentials, introducing SQL syntax errors through input fields, 
manipulating file paths, modifying form parameters, and sending unexpected inputs through
application fuzzing. Burp Suite Intruder is identified as a tool for automating requests
with different payloads and observing the resulting responses.

A practical authentication enumeration example uses a login form that responds 
differently depending on whether an email address exists. An unregistered address 
produces an account-not-found message, while a registered address produces an 
incorrect-password response. This difference establishes a reliable response pattern for
distinguishing potentially valid accounts.

The source demonstrates automating this process with Python and the Requests library. 
The script reads email addresses from a supplied file, submits each address to the 
application's login endpoint using an HTTP POST request, and examines the returned JSON
response. A predefined error message identifies invalid addresses. Other responses are 
treated as potentially valid, printed to the console, and added to a separate collection.
The script accepts the input filename as a command-line argument and displays the 
resulting list.

Password-reset functionality presents a separate set of authentication risks. The source
describes three approaches: email-based resets, security-question verification, and 
SMS-delivered reset codes. Email-based mechanisms depend on the security of the 
registered mailbox and the confidentiality of the reset link or token. Security questions
can be compromised when their answers are obtainable through personal information. 
SMS-based mechanisms depend on control of the associated phone number and can be affected
by SIM-swapping attacks or interception.

The reset process can also expose weaknesses through predictable tokens, inadequate 
expiration, insufficient identity verification, account-enumeration messages, and 
transmission over unencrypted HTTP connections. Tokens that remain valid after use or
stay active for excessive periods create additional opportunities for misuse.

The predictable-token laboratory demonstrates a particularly weak implementation. Its 
PHP code generates a numeric reset token using a limited range of values and stores it 
against the submitted email address. Because the token has only 101 possible values, the
search space is small. The exercise uses Crunch to generate the candidate values and Burp
Suite Intruder to test them against the password-reset endpoint. Successful responses can
be distinguished by their larger content length compared with responses reporting an 
invalid token. The demonstration uses a deliberately small token range because of the 
limitations of Burp Suite Community Edition; the source notes that production applications
commonly use six-digit codes instead.

HTTP Basic Authentication provides another practical example of credential testing. 
Defined by RFC 7617, this authentication mechanism sends a username and password 
combination in the HTTP Authorization header after Base64 encoding. The header follows 
the Basic authorization format, with the encoded credentials representing the username 
and password separated by a colon. Base64 is an encoding mechanism rather than encryption,
so the credentials can be decoded if intercepted. The source highlights the risks of 
transmitting these credentials without HTTPS and relying on weak passwords.

The practical exercise captures a Basic Authentication request through Burp Suite and 
sends it to Intruder. The encoded authorization value is decoded to identify the 
credential structure. A password wordlist from SecLists, specifically 500-worst-passwords.txt, 
is then configured as the payload source. Intruder's payload-processing rules combine the username
with each candidate password and encode the resulting string using Base64. The padding character 
is removed through the payload-encoding configuration. The attack results are examined for a 
successful HTTP 200 response, after which the corresponding authorization value can be decoded to
identify the working credentials.

Open-source intelligence (OSINT) provides another way to identify potentially exposed application 
resources. The Internet Archive's Wayback Machine 
([https://archive.org/web/](https://archive.org/web/?utm_source=chatgpt.com)) allows historical 
website versions to be examined. Older pages may reveal references to files and directories that 
are no longer immediately visible in the current website.

The source introduces waybackurls, a Go-based tool hosted at 
[https://github.com/tomnomnom/waybackurls](https://github.com/tomnomnom/waybackurls?utm_source=chatgpt.com), for retrieving URLs 
recorded by the Wayback Machine. Its installation involves cloning the GitHub repository, 
entering the project directory, optionally installing the Go compiler through the system package
manager, and building the executable. The resulting program can retrieve archived URLs associated 
with a specified domain.

Google Dorks provide another OSINT technique. Carefully constructed search queries can identify 
publicly indexed administrative pages, log files, and backup directories. The examples in the 
source demonstrate searching for administrative paths, password-related log files, and indexed 
backup directories using domain restrictions and search operators.

The exercises collectively demonstrate how authentication responses, password-reset 
implementations, HTTP authentication headers, archived URLs, and search-engine indexing can 
expose information useful during security assessments. I found the connection between enumeration
and brute-force testing particularly useful: identifying differences in application behavior 
provides the information needed to make subsequent testing more targeted.

---

| Description                                                    | Code/Command                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PHP password complexity validation                             | `\$password = \$_POST['pass'];` `\$pattern = '/^(?=.*[A-Z])(?=.*\d)(?=.*[\W_]).+$/';` `if (preg_match(\$pattern, \$password)) { echo "Password is valid."; } else { echo "Password is invalid. It must contain at least one uppercase letter, one number, and one symbol."; }`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Python imports                                                 | `import requests` `import sys`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Python email enumeration script                                | `URL = 'http://<TARGET_HOST>/<REDACTED_ENDPOINT>'` `HEADERS = {'Host': '<TARGET_HOST>', 'User-Agent': 'Mozilla/5.0', 'Accept': 'application/json, text/javascript, */*; q=0.01', 'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8', 'X-Requested-With': 'XMLHttpRequest', 'Origin': 'http://<TARGET_HOST>', 'Connection': 'close'}` `def check_email(email): return requests.post(URL, headers=HEADERS, data={'username': email, 'password': '********', 'function': 'login'}).json()` `def enumerate_emails(email_file):` `    valid_emails = []` `    invalid_error = 'Email does not exist'` `    with open(email_file) as file:` `        emails = file.readlines()` `    for email in emails:` `        email = email.strip()` `        if email:` `            response = check_email(email)` `            if response['status'] == 'error' and invalid_error in response['message']:` `                print(f'[INVALID] {email}')` `            else:` `                print(f'[VALID] {email}')` `                valid_emails.append(email)` `    return valid_emails` `if __name__ == '__main__':` `    if len(sys.argv) != 2:` `        print('Usage: python3 script.py <email_list_file>')` `        sys.exit(1)` `    valid_emails = enumerate_emails(sys.argv[1])` `    print('\\nValid emails found:')` `    for email in valid_emails:` `        print(email)` |
| Alternative request-handling example from the script breakdown | `data = {'username': email, 'password': '********', 'action': 'login'}` `response = requests.post(url, headers=headers, data=data)` `if invalid_error in response.text: print(f'{email} is invalid.')` `else: print(f'{email} is valid.')`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Python execution                                               | `python3 script.py <email_list_file>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| PHP predictable reset token                                    | `$token = mt_rand(100, 200);` `$query = $conn->prepare("UPDATE users SET reset_token = ? WHERE email = ?");` `$query->bind_param("ss", $token, $email);` `$query->execute();`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Crunch numeric token generation                                | `crunch 3 3 -o otp.txt -t %%% -s 100 -e 200`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| HTTP Basic Authentication header                               | `Authorization: Basic <BASE64_ENCODED_CREDENTIALS>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Git repository installation                                    | `git clone https://github.com/tomnomnom/waybackurls` `cd waybackurls`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Optional Go installation                                       | `sudo apt install golang-go -y`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Compile waybackurls                                            | `go build`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Inspect project files                                          | `ls -lah`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Retrieve archived URLs                                         | `./waybackurls <TARGET_DOMAIN>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Google Dork: administrative pages                              | `site:<TARGET_DOMAIN> inurl:admin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Google Dork: password-related logs                             | `filetype:log "password" site:<TARGET_DOMAIN>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Google Dork: backup directories                                | `intitle:"index of" "backup" site:<TARGET_DOMAIN>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |

---

### Key Takeaways

\- Authentication enumeration

- Examine registration, login, and password-reset responses.
- Identify differences between nonexistent accounts and incorrect passwords.
- Record potentially valid usernames and email addresses.
- Examine password-policy validation messages for disclosed requirements.

\- Verbose error analysis

- Inspect errors generated by invalid login attempts.
- Test how SQL syntax errors affect application responses.
- Examine file-path manipulation and form validation behavior.
- Use Burp Suite Intruder to automate input variation and observe informative responses.

\- Email enumeration procedure

- Prepare an email address wordlist.
- Submit each address through the login endpoint using Python Requests.
- Inspect the JSON response.
- Classify responses containing the account-not-found message as invalid.
- Record the remaining addresses as potentially valid.

\- Password-reset assessment

- Examine email, security-question, and SMS-based recovery mechanisms.
- Assess token predictability and expiration.
- Check whether tokens remain usable after successful redemption.
- Examine identity-verification weaknesses and account-existence disclosures.
- Check whether reset information is transmitted over HTTPS.

\- Predictable-token laboratory

- Generate candidate numeric tokens using Crunch.
- Configure Burp Suite Intruder to use the generated wordlist.
- Identify the reset-token parameter.
- Compare response content lengths to distinguish invalid-token responses from potentially successful ones.
- The demonstration uses a three-digit range of 100–200, producing 101 candidates.

\- HTTP Basic Authentication

- Capture the authentication request using Burp Suite.
- Inspect and decode the Base64 authorization value.
- Configure Intruder with the SecLists `500-worst-passwords.txt` wordlist.
- Combine the username and candidate password.
- Base64-encode the resulting credentials.
- Remove the `=` padding character through the payload-encoding configuration.
- Examine successful responses and decode the corresponding authorization value.

\- OSINT

- Use the Wayback Machine to inspect historical website versions.
- Install and compile waybackurls using Go.
- Retrieve archived URLs for the authorized target domain.
- Apply Google Dorks to identify indexed administrative pages, log files, and backup directories.

\- Ethical responsibility

- Conduct enumeration and brute-force testing only with explicit authorization from the system owner.
- Consider rate limiting and account-lockout mechanisms during authorized assessments.

Reference verification: RFC 7617 confirms the Base64 credential format and the need for TLS protection. OWASP also documents the risk of account enumeration through inconsistent authentication responses.&#x20; [image](https://www.google.com/s2/favicons?domain=https://www.rfc-editor.org\&sz=32) RFC Editor +1

---





