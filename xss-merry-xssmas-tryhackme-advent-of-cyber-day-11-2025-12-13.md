# Learn about types of XSS vulnerabilities and how to prevent them.

---
## Leave the Cookies, Take the Payload

Cross-Site Scripting (XSS) represents a critical web application vulnerability where an attacker can inject malicious client-side 
code, typically JavaScript, into content intended for viewing by other users. This vulnerability arises when an application fails 
to properly validate, sanitize, or escape user input, allowing the input to be interpreted by the browser as executable code rather 
than benign text. Successful XSS attacks can facilitate unauthorized actions, including the theft of session credentials, website 
defacement, or user impersonation. XSS vulnerabilities are generally categorized by the mechanism of execution, primarily focusing 
on **Reflected** and **Stored** variants. 

**Reflected XSS** occurs when the malicious code is immediately and non-persistently returned in the server's HTTP response. An 
attacker typically crafts a payload within a URL parameter—such as a search query—and then tricks a victim into clicking the 
crafted link. If the application's search function, for example, directly reflects the user-supplied term in the results without 
encoding, the victim's browser executes the injected script upon loading the page. The impact of this vector allows the attacker 
to execute any action the victim could perform, view information accessible to the victim, or modify data in their session, often 
leveraged in phishing schemes.

**Stored XSS** represents a more severe, persistent threat. In this scenario, the malicious script is successfully submitted to 
the application (e.g., via a comment or message form) and then permanently saved on the backend server or database. Any subsequent 
user, including administrative staff, who loads the affected page automatically retrieves and executes the malicious script. 
This is a "set-and-forget" attack, giving the attacker continuous access to all users viewing that content. The malicious script 
can perform actions like stealing session cookies, presenting fake login popups, or defacing the website content.

Protection against XSS requires a defense-in-depth strategy, focusing on secure design and implementation practices across the 
input and output boundaries. A primary mitigation is to avoid dangerous rendering methods, instead using properties like 
`textContent` over `innerHTML` to ensure all user-supplied data is treated as text. Furthermore, session cookies must be secured 
with attributes like `HttpOnly`, `Secure`, and `SameSite` to limit their accessibility to client-side scripts, reducing the impact 
of a successful XSS exploit. Crucially, all user-supplied input must be thoroughly sanitized and output-encoded. This process 
removes or escapes any characters or elements that could be interpreted as executable code (e.g., `<script>` tags or event handlers) 
while preserving necessary elements like safe formatting. Confirming susceptibility to XSS requires testing with basic payloads 
within input fields, such as search bars or message forms, to observe if the browser successfully executes the injected code.

| Description | Test Payload for XSS confirmation | Code/Command |
| :--- | :--- | :--- |
| Reflected XSS Payload Example | `<script>alert('Reflected Meow Meow')</script>` | `<script>alert('Reflected Meow Meow')</script>` |
| Stored XSS Payload Example | `<script>alert('Stored Meow Meow')</script>` | `<script>alert('Stored Meow Meow')</script>` |
| XSS Payload via URL (Reflected Example) | `https://trygiftme.thm/search?term=<script>alert( atob("VEhNe0V2aWxfQnVubnl9") )</script>` | `<script>alert( atob("VEhNe0V2aWxfQnVubnl9") )</script>` |
| Stored XSS Script used in POST request | `comment=<script>alert(atob("VEhNe0V2aWxfU3RvcmVkX0VnZ30="))</script> + "This gift set my carpet on fire but my kid loved it!"` | `<script>alert(atob("VEhNe0V2aWxfU3RvcmVkX0VnZ30="))</script>` |
| Recommended secure DOM property | `textContent` | `textContent` |

---

### Key Takeaways

* XSS exploits browser trust in application input, allowing injection of malicious JavaScript to steal data or impersonate users.
* **Reflected XSS** is non-persistent, executed via a crafted link, with the payload immediately returned in the response.
* **Stored XSS** is persistent, saved on the server, and executed every time an affected page is loaded by any user.
* Mitigation requires a multi-layered approach focusing on input sanitation and output encoding.
* Secure development practice includes disabling dangerous rendering methods like `innerHTML` in favor of safer options
* like `textContent`.
* Cookies should be secured with `HttpOnly`, `Secure`, and `SameSite` attributes to limit post-exploitation impact.



