# Insecure Direct Object Reference (IDOR): An Access Control Failure

---
Insecure Direct Object Reference (IDOR) is an access control vulnerability where a web application exposes an internal object 
identifier, such as a database key, file name, or sequential user ID, and fails to implement sufficient server-side authorization 
checks on the user-supplied reference. This failure allows an attacker to manipulate the identifier—for example, changing a 
packageID=1001 to packageID=1002—to access, view, or modify data belonging to another user without explicit permission. The root 
cause of IDOR is neglecting the Authorization step after Authentication has occurred.

## Technical Mechanism and Privilege Escalation
To understand the exploit mechanism, one must differentiate between authentication and authorization. Authentication is the 
process of verifying who a user is (via credentials and subsequent session tokens). Authorization is the process of 
erifying what that user is permitted to do or access. IDOR occurs when a request includes session information (proving who 
the user is) but the server does not check the necessary permissions (i.e., verifying the authenticated user is the owner of 
the requested object). This vulnerability is typically a form of Horizontal Privilege Escalation, where the attacker gains 
access to resources belonging to a peer user at the same privilege level. Vertical Privilege Escalation, conversely, involves 
gaining access to higher-privileged features, such as those reserved for an administrator.

## Exploitation Vectors and Hidden References
IDOR is often found in direct, sequential identifiers (e.g., auto-incrementing database IDs) but can be hidden through encoding 
or weak hashing. An application may attempt to obscure the ID using Base64 encoding (e.g., encoding 2 to Mg==) or non-cryptographic 
hashes like MD5 or SHA1. However, these methods provide only minimal protection, as Base64 is trivially decoded, and hashes can be 
identified and replicated if the input value (like a sequential ID) is known. A more subtle vector involves the use of 
UUID Version 1 (UUIDv1). UUIDv1 is generated based on the system's MAC address and a precise timestamp, making it predictable 
if the generation window is narrow. This predictability allows an attacker to generate a large number of possible valid IDs 
within a timeframe, which can then be used in a brute-force attack to enumerate resources.

## Mitigation and Secure Design
Effective IDOR mitigation requires a shift away from client-controlled identifiers toward robust, server-side authorization 
enforcement. Hiding IDs via encoding or weak hashing is a fragile measure and does not constitute a security fix. The primary 
defense is to implement strict access control checks for every request that accesses a sensitive object. The server must verify 
that the authenticated user possesses the required permissions or ownership of the requested resource. Furthermore, developers 
should avoid using predictable, direct object references whenever possible, favoring Indirect Object Reference Maps or 
cryptographically strong, non-sequential identifiers like UUID Version 4 (UUIDv4) for publicly exposed parameters. Continuous 
testing for IDOR by attempting to access other users' data is non-negotiable for maintaining application security.

---
## IDOR Exploitation Steps via Inspect Element
The following steps are extracted directly from the provided text and outline how to use the browser's Developer Tools to perform 
horizontal privilege escalation by changing the stored `user_id`.

1. Initial Access and Setup
- Authenticate to the web application as a normal user (e.g., using the provided credentials).
- Open Developer Tools by right-clicking the page and selecting Inspect.
2. Locate the Reference
- Navigate to the Storage tab within the Developer Tools window.
- Expand the Local Storage dropdown on the left side and click on the specific URL (origin) listed for the application.
- Identify the relevant data entry containing the object reference. In the example, this is the auth_user data key, which holds a
  JSON value like {"user_id":10}.
3. Tamper the Identifier
- Modify the Value: Double-click the Value field of the auth_user entry.
- Change the ID: Update the object reference (user_id) to a different, guessable sequential number (e.g., change the value from 10
  to 11).
- Save/Confirm: Press the Enter key to save the modified value in Local Storage.
4. Observe and Exploit
- Refresh the Page: Refresh the entire browser page (F5 or Ctrl+r).
- Observe Response: The application uses the newly modified user_id from Local Storage for authorization checks. Since the server
- lacks proper authorization validation, the application now retrieves and displays data belonging to User ID 11, resulting in
- unauthorized access and confirming the IDOR vulnerability.

## Alternative Reference Discovery (Network Tab)
While the main exploit focused on Local Storage, the text also mentions using the Network Tab to discover which IDs are being used 
in API calls:

- Open the Network tab in Developer Tools.
- Refresh the Page to capture all HTTP requests.
- Examine the requests (like view_accountinfo) to find the user_id or other object references being passed as parameters.
This helps identify identifiers that are not stored locally but are passed in the request body or URL query strings, which would
then require a tool like Burp Suite or the browser's URL bar to tamper with.

>[!Note]
>## You did it! Wareville is one step safer.
>The townsfolk are counting on you to keep Christmas secure.
>Head back to Wareville to continue your mission!




