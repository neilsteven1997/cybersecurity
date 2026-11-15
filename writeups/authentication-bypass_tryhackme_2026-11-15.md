# Authentication Bypass

---

Authentication testing in the Acme IT Support environment focused on username enumeration, brute-force testing, authentication logic 
flaws, password-reset manipulation, cookie tampering, hashing, and encoding. The exercises demonstrated how weaknesses in application
behavior and insufficient validation can expose user accounts or allow unauthorized privilege changes.

Username enumeration was performed using ffuf (Fuzz Faster U Fool) and a predefined SecLists username wordlist. The Acme IT Support 
registration page provided an observable response when an existing username was submitted: an account-already-exists error. This 
response could be used to distinguish registered usernames from invalid ones. The discovered usernames were collected in a file named 
`valid_usernames.txt` for subsequent testing. The ffuf utility and its installation resources are available at
[https://github.com/ffuf/ffuf](https://github.com/ffuf/ffuf?utm_source=chatgpt.com).

The next exercise involved brute-force testing against the customer login page. The previously collected usernames were combined with 
a password wordlist containing commonly used credentials. ffuf accepted multiple wordlists through the `-w` argument, with separate 
identifiers assigned to each list. The exercise used `W1` for usernames and `W2` for passwords. The HTTP response filter was configured
to exclude status code 200, allowing alternative responses to identify a potentially successful authentication attempt. The username
file needed to contain clean entries; directly piping raw ffuf output into the file could introduce formatting problems.

Authentication logic flaws were examined through an example involving URL path validation. The application checked whether a requested
path began with a particular administrative route before performing an authorization check. Because the comparison used strict string
matching, including letter casing, changing the capitalization of the requested path could bypass the intended condition. An 
unauthenticated request using a differently capitalized administrative path could therefore reach content without undergoing the 
expected privilege verification.

A practical example involved the password-reset functionality of Acme IT Support. The process initially requested the account email 
address, followed by the associated username. The application retrieved the account using the email supplied through the query string, 
while the username was submitted through a POST request. However, the password-reset delivery mechanism relied on PHP's `$_REQUEST` 
variable, which combines query-string and POST data. When identical parameter names are supplied through both sources, POST data takes
precedence under the behavior described in the exercise. This created an opportunity to manipulate the destination of the 
password-reset email by supplying an additional email parameter in the POST body.

The testing process demonstrated that the original account could remain the subject of the password-reset request while the delivery 
address was changed. The exercise then used the Acme IT Support customer registration system, which assigned each account a unique
email address following a predefined customer-domain format. Repeating the manipulated request with that assigned address caused the 
resulting support ticket to appear in the attacker's account. The ticket contained a login link associated with the targeted account, 
allowing access to its support tickets and the exercise's flag.

Cookie tampering demonstrated another authentication weakness. Cookies issued by a web server may contain readable values that 
influence application behavior. The examples included separate `logged_in` and `admin` values, representing authentication status and 
administrative privileges. Testing began with a request containing no authentication cookies, which returned a not-logged-in response. 
Supplying a logged-in value changed the response to indicate an authenticated regular user. Changing the administrative value as well
resulted in an administrative response. This demonstrated the consequences of applications trusting client-controlled cookie values 
to determine authorization.

Hashing and encoding were also examined as methods of representing information within cookies. Hashes provide an irreversible 
representation of input data, and the same input produces a consistent output when processed using the same hashing method. The 
examples covered MD5 (Message Digest Algorithm 5), SHA-256 (Secure Hash Algorithm 256-bit), SHA-512 (Secure Hash Algorithm 512-bit),
and SHA-1 (Secure Hash Algorithm 1). The exercise referenced
[https://crackstation.net/](https://crackstation.net/?utm_source=chatgpt.com) as a service maintaining databases that can help 
identify original values associated with known hashes.

Encoding differs from hashing because encoded information can be reversed through decoding. Its purpose in the described context is to 
represent binary information as readable text suitable for transmission through systems supporting ASCII (American Standard Code for 
Information Interchange). Base32 and Base64 were introduced as common encoding formats. Base32 uses letters A–Z and digits 2–7, while
Base64 uses uppercase and lowercase letters, digits, plus and slash characters, with equals signs available for padding.

The final example involved a session cookie containing Base64-encoded JSON data. Decoding the value revealed an account identifier and
an administrative property. Modifying the property and encoding the data again demonstrated how an application that trusts unsigned, 
client-controlled session information could expose administrative access.

### Code and Commands

| Description                                          | Code/Command                                                                                                                                                                                                                                                                  |
| ---------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Username enumeration using ffuf                      | `ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://<TARGET_HOST>/customers/signup -mr "username already exists"`                       |
| Brute-force authentication testing                   | `ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://<TARGET_HOST>/customers/login -fc 200` |
| Administrative path validation example               | `if( url.substr(0,6) === '/admin') {`<br>`    # Code to check user is an admin`<br>`} else {`<br>`    # View Page`<br>`}`                                                                                                                                                     |
| Password-reset request: original parameters          | `curl 'http://<TARGET_HOST>/customers/reset?email=<TARGET_EMAIL>' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=<TARGET_USERNAME>'`                                                                                                                       |
| Password-reset request: manipulated delivery address | `curl 'http://<TARGET_HOST>/customers/reset?email=<TARGET_EMAIL>' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=<TARGET_USERNAME>&email=<ATTACKER_EMAIL>'`                                                                                                |
| Password-reset request: customer-domain delivery     | `curl 'http://<TARGET_HOST>/customers/reset?email=<TARGET_EMAIL>' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=<TARGET_USERNAME>&email={username}@customer.<TARGET_DOMAIN>'`                                                                             |
| Example plain-text authentication cookies            | `Set-Cookie: logged_in=true; Max-Age=3600; Path=/`<br>`Set-Cookie: admin=false; Max-Age=3600; Path=/`                                                                                                                                                                         |
| Initial cookie test                                  | `curl http://<TARGET_HOST>/cookie-test`                                                                                                                                                                                                                                       |
| Authenticated regular-user cookie test               | `curl -H "Cookie: logged_in=true; admin=false" http://<TARGET_HOST>/cookie-test`                                                                                                                                                                                              |
| Administrative cookie test                           | `curl -H "Cookie: logged_in=true; admin=true" http://<TARGET_HOST>/cookie-test`                                                                                                                                                                                               |
| Example Base64 session cookie                        | `Set-Cookie: session=<REDACTED>; Max-Age=3600; Path=/`                                                                                                                                                                                                                        |
| Decoded session data structure                       | `{"id":1,"admin":false}`                                                                                                                                                                                                                                                      |

### Extracted Tables

Hashing examples

| Original String | Hash Method | Output                                                                                                                             |
| --------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| 1               | MD5         | `c4ca4238a0b923820dcc509a6f75849b`                                                                                                 |
| 1               | SHA-256     | `6b86b273ff34fce19d6b804eff5a3f5747ada4eaa22f1d49c01e52ddb7875b4b`                                                                 |
| 1               | SHA-512     | `4dff4ea340f0a823f15d3f4f01ab62eae0e5da579ccb851f8db9dfe84c58b2b37b89903a740e1ee172da793a6e79d560e5f7f9bd058a12a280433ed6fa46510a` |
| 1               | SHA-1       | `356a192b7913b04c54574d18c28d46e6395428ab`                                                                                         |

### Key Takeaways

- Username Enumeration: Use ffuf with a predefined username wordlist to identify registered accounts through observable
  registration-page responses.
- Save discovered usernames in `valid_usernames.txt` and ensure the file contains clean entries.
- Brute-Force Testing: Combine username and password wordlists using separate ffuf identifiers, such as `W1` and `W2`.
- Use HTTP response filtering to distinguish potentially successful authentication attempts from ordinary responses.
- Logic Flaws: Examine application conditions for inconsistencies that allow authentication checks to be bypassed. The example
   demonstrated how case-sensitive path comparisons can create an authorization weakness.
- Password Reset Manipulation: Compare query-string parameters and POST fields when investigating how an application retrieves account
   information and determines password-reset delivery destinations.
- Understand how PHP's `$_REQUEST` combines query-string and POST parameters and how conflicting parameter values can influence
  application behavior.
- Cookie Tampering: Inspect readable cookie values and determine whether authentication status or administrative privileges depend on
   client-controlled information.
- Compare application responses when cookies are absent, indicate a regular authenticated user, or contain administrative privileges.
- Hashing: Recognize MD5, SHA-256, SHA-512, and SHA-1 outputs. Identical inputs produce consistent outputs under the same hashing
  method.
- Encoding: Distinguish irreversible hashing from reversible encoding. Understand the character sets used by Base32 and Base64.
- Session Analysis: Examine encoded session-cookie contents, identify structured JSON data, and understand the consequences of
  trusting modified client-side session information.


---

