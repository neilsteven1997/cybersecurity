# IDOR

---

Insecure Direct Object Reference (IDOR) arises whenever a web application accepts an object identifier supplied by the user and 
retrieves the corresponding resource without confirming that the authenticated session is authorised to access it. Profiles, invoices,
tickets and documents are each located by some reference value, commonly a number or string. When that reference is taken at face value 
and the server performs no ownership check, the result is an access-control failure. The issue is catalogued under Broken Access 
Control, ranked first in the OWASP Top 10, and is equivalently known as Broken Object Level Authorisation (BOLA).  

The practical impact of the flaw is disproportionate to the effort required to exploit it. Changing a single value in a URL parameter 
or request body is often sufficient; neither injection nor session theft is needed. Depending on the endpoint, the same missing check
can expose personal data, permit modification of another account’s details, or enable complete account takeover.  

A minimal illustration appears when a profile page is requested with a query parameter such as 
http://online-service.thm/profile?user_id=1305. The server simply looks up record 1305 and returns its contents. Substituting a 
different identifier, for example 1000, yields another user’s details because no server-side logic ever asks whether the current 
session is entitled to view that record. Authentication may be functioning correctly, yet authorisation is absent. The same pattern 
applies equally to write operations: altering an identifier on an update or delete endpoint can change another user’s e-mail address, 
reset a password or remove resources.  

Object references are not always presented in clear text. Developers frequently apply encoding so that special characters survive 
transmission. Base64 is the most common scheme; the integer 123 becomes MTIz and a short JSON object such as {"user_id": 5} becomes 
eyJ1c2VyX2lkIjogNX0=. Exploitation follows a short sequence of decode, alter and re-encode steps that can be performed with online 
utilities at base64decode.org and base64encode.org or with the corresponding terminal commands. Because base64 is a reversible 
transformation rather than encryption, the presence of encoding confers no security once the value can be manipulated.  

Some applications hash identifiers instead. An MD5 digest of the integer 123 yields the fixed 32-character string 
202cb962ac59075b964b07152d234b70. When the original values are sequential integers the attacker can simply compute the same hash for 
every plausible input and compare the results against observed values. Hash length immediately indicates the algorithm (32 characters 
for MD5, 40 for SHA-1, 64 for SHA-256); tools such as hash-identifier or hashid assist in confirmation. Lookup services such as 
CrackStation further accelerate recovery of short, predictable inputs. Hashing therefore adds only obscurity; it does not substitute 
for an authorisation decision on the server.  

Even when identifiers are deliberately unpredictable—UUIDs or random strings—the vulnerability remains if the server still fails to 
verify ownership. Enumeration is blocked, yet any identifier obtained through another channel (shared links, response bodies, e-mail 
notifications, exported reports) can be substituted. The standard verification method uses two accounts: resources belonging to the 
first account are requested while authenticated as the second. Successful retrieval of the first account’s data demonstrates that the 
identifier format is irrelevant and that the missing check is the sole defect.  

Vulnerable references appear far beyond the browser address bar. Background AJAX calls, JavaScript files that embed API paths, cookie 
values, request headers, REST path segments and parameters that the front-end never normally transmits must all be examined. 
Intercepting every request with browser developer tools or a proxy such as Burp Suite is therefore essential; restricting attention to 
visible URLs leaves many instances undiscovered.  

In the accompanying laboratory exercise the target is reached at https://LAB_WEB_URL.p.thmlabs.com. After creating any account and 
navigating to the account page, the Network panel reveals a request to /api/v1/customer?id={user_id}. The JSON response is controlled
solely by the numeric identifier. Replaying the request with successive values such as 1 and 3 returns other users’ records, 
confirming the absence of any authorisation test.

---

| Description | Code/Command |
| --- | --- |
| Decode a base64 value on the command line | echo 'value' \| base64 -d |
| Re-encode a modified value on the command line | echo 'value' \| base64 |

---

### Key Takeaways
- Explain what an IDOR vulnerability is and how it relates to broken access control
- Identify the different forms object references can take, including plaintext, encoded, and hashed identifiers
- Recognise the locations in a web application where IDOR vectors commonly appear
- Exploit an IDOR vulnerability in a practical scenario to access another user's data
- Decode the value from the request using a tool such as base64decode.org or the terminal command echo 'value' | base64 -d
- Modify the decoded output to reference a different object
- Re-encode the modified value using base64encode.org or echo 'value' | base64
- Substitute the re-encoded string back into the request and submit it
- Create two accounts on the application
- Log into the first account and record the identifiers associated with its resources
- Log into the second account and substitute the first account’s identifiers into its requests
- Test query-string parameters, POST body data, cookie values, HTTP request headers, REST API path segments and background AJAX
  requests

---


