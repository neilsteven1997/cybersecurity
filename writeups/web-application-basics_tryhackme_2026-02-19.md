# Web Application Basics

---

In my ongoing exploration of foundational web security concepts, I revisited the core structure of web applications, drawing from a 
structured overview that frames them as layered systems. A web app functions within a browser, with the front end handling user 
interactions through technologies like Hypertext Markup Language for basic content rendering, Cascading Style Sheets for visual styling, 
and JavaScript for dynamic decision-making and behavior. Beneath that lies the back end, encompassing databases for data storage and 
retrieval, along with infrastructure elements such as servers, networking gear, and optional protections like a Web Application Firewall 
to block malicious traffic. This setup reminds me of how surface-level visibility belies the critical undercurrents that sustain 
functionality, much like in real-world deployments where overlooking backend safeguards can lead to breaches.

Delving into access mechanisms, the Uniform Resource Locator stands out as the navigational backbone, comprising elements that direct 
browser requests precisely. The scheme dictates the protocol, typically HTTP for unencrypted or HTTPS for secure transfers, emphasizing 
encryption to thwart interception. Occasionally, a user component embeds authentication details, though that's risky and largely obsolete
due to exposure potential. The host or domain identifies the target site, warranting vigilance against deceptive variations in phishing 
schemes. Ports specify service entry points, defaulting to 80 or 443, while paths map to specific resources, demanding authorization 
checks to prevent unauthorized traversal. Query strings, prefixed by a question mark, carry parameters like search data, necessitating 
sanitization against injection threats, and fragments, starting with a hash, anchor to page sections with similar validation needs.

HTTP communications form the dialogue between client and server, structured as messages with a start line summarizing intent, headers as 
key-value metadata, an empty line delimiter, and a body for payload data. Requests initiate actions, while responses convey outcomes, and 
grasping this exchange aids in pinpointing vulnerabilities during audits. For requests, the start line integrates method, path, and 
version; methods include GET for retrieval without alteration—crucial to avoid leaking secrets in URLs—POST for submitting data with 
input validation against injections, PUT and DELETE for updates or removals requiring strict access controls, PATCH for partial 
modifications, HEAD for metadata checks, OPTIONS and TRACE for capability probes often disabled to curb reconnaissance, and CONNECT for 
tunneling secure links. Versions have evolved from the rudimentary HTTP/0.9 supporting only GET, through 1.0 and 1.1 adding persistence
and efficiency, to HTTP/2 with multiplexing and HTTP/3 leveraging QUIC for enhanced speed and security, though 1.1 persists in many 
environments.

Request headers supplement details, such as Host naming the target, User-Agent identifying the client, Referer tracing origins, Cookie 
storing session states, and Content-Type specifying data formats. Bodies in methods like POST vary: URL-encoded pairs separated by 
ampersands with percent-encoding for specials; multipart form data for segmented uploads including binaries; JSON as braced key-value 
objects; or XML with nested tags. On the response side, the status line combines version, a three-digit code, and a phrase; codes range 
from 100s for interim continuations, 200s for successes, 300s for redirects, 400s for client faults like 404 not found, to 500s for 
server errors. Response headers include Date for timestamps, Content-Type for format guidance, Server for software disclosure—often 
masked to reduce fingerprinting—and others like Set-Cookie with security flags, Cache-Control for storage directives, and Location for 
redirection paths, all needing scrutiny to avoid open redirects.

Security-specific headers bolster defenses: Content-Security-Policy restricts resource sources to mitigate XSS, using directives like 
default-src 'self' or script-src allowing trusted domains; Strict-Transport-Security enforces HTTPS with max-age expiry, subdomain 
inclusion, and preload options; X-Content-Type-Options 'nosniff' prevents MIME guessing; and Referrer-Policy modulates origin sharing, 
from no-referrer blocking all to strict-origin-when-cross-origin for nuanced control. Tools like the analyzer at 
https://securityheaders.io/ prove invaluable for evaluating implementations in practice. I found reflecting on these in a controlled 
request emulator reinforces how misconfigurations invite exploits, underscoring the need for rigorous header audits in my assessments.

---

| Description | Code/Command |
|-------------|--------------|
| URL Encoded POST request example | POST /profile HTTP/1.1<br>Host: tryhackme.com<br>User-Agent: Mozilla/5.0<br>Content-Type: application/x-www-form-urlencoded<br>Content-Length: 33<br><br>name=Aleksandra&age=27&country=US |
| Form Data POST request example | POST /upload HTTP/1.1<br>Host: tryhackme.com<br>User-Agent: Mozilla/5.0<br>Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW<br><br>----WebKitFormBoundary7MA4YWxkTrZu0gW<br>Content-Disposition: form-data; name="username"<br><br>aleksandra<br>----WebKitFormBoundary7MA4YWxkTrZu0gW<br>Content-Disposition: form-data; name="profile_pic"; filename="aleksandra.jpg"<br>Content-Type: image/jpeg<br><br>[Binary Data Here representing the image]<br>----WebKitFormBoundary7MA4YWxkTrZu0gW-- |
| JSON POST request example | POST /api/user HTTP/1.1<br>Host: tryhackme.com<br>User-Agent: Mozilla/5.0<br>Content-Type: application/json<br>Content-Length: 62<br><br>{<br>    "name": "Aleksandra",<br>    "age": 27,<br>    "country": "US"<br>} |
| XML POST request example | POST /api/user HTTP/1.1<br>Host: tryhackme.com<br>User-Agent: Mozilla/5.0<br>Content-Type: application/xml<br>Content-Length: 124<br><br><user><br>    <name>Aleksandra</name><br>    <age>27</age><br>    <country>US</country><br></user> |

---

## Extracted Tables

| Request Header | Example | Description |
|----------------|---------|-------------|
| Host | Host: tryhackme.com | Specifies the name of the web server the request is for. |
| User-Agent | User-Agent: Mozilla/5.0 | Shares information about the web browser the request is coming from. |
| Referer | Referer: https://www.google.com/ | Indicates the URL from which the request came from. |
| Cookie | Cookie: user_type=student; room=introtowebapplication; room_status=in_progress | Information the web server previously asked the web browser to store is held in cookies. |
| Content-Type | Content-Type: application/json | Describes what type or format of data is in the request. |

---

## Key Takeaways
- Grasp the essence of web applications and their browser-based execution.
- Deconstruct URL elements to understand resource access.
- Comprehend HTTP request and response mechanics.
- Familiarize with various HTTP request methods.
- Interpret meanings of HTTP response codes.
- Explore HTTP headers and their security implications.
- Front end: HTML for structure, CSS for appearance, JavaScript for interactivity.
- Back end: Database for data management, infrastructure for support, WAF for request filtering.
- URL components: Scheme (HTTP/HTTPS), User (authentication, rare), Host/Domain (unique identifier, watch for typosquatting), Port
  (80/HTTP, 443/HTTPS), Path (resource location, secure access), Query String (? prefixed, sanitize inputs), Fragment (# anchored,
  validate data).
- HTTP message structure: Start line, Headers, Empty line, Body.
- HTTP methods and security: GET (fetch, no sensitive data in URLs), POST (submit, validate inputs), PUT (update, authorize), DELETE
  (remove, authorize), PATCH (partial update, validate), HEAD (headers only), OPTIONS (available methods), TRACE (debug, often disabled),
   CONNECT (secure tunnels).
- HTTP versions: 0.9 (GET only, 1991), 1.0 (headers/content support, 1996), 1.1 (persistent connections, 1997), 2 (multiplexing, 2015),
   3 (QUIC-based, 2022).
- Response status categories: 100-199 (informational), 200-299 (success), 300-399 (redirection), 400-499 (client error), 500-599
  (server error).
- Common codes: 100 (Continue), 200 (OK), 301 (Moved Permanently), 404 (Not Found), 500 (Internal Server Error).
- Required response headers: Date (timestamp), Content-Type (format/charset), Server (software, often obscured).
- Other headers: Set-Cookie (with HttpOnly/Secure), Cache-Control (caching rules), Location (redirects, validate).
- Security headers: CSP (source restrictions, e.g., default-src 'self'; script-src 'self' https://cdn.tryhackme.com), HSTS
  (HTTPS enforcement, max-age=63072000; includeSubDomains; preload), X-Content-Type-Options (nosniff), Referrer-Policy
  (no-referrer, same-origin, strict-origin, strict-origin-when-cross-origin).

---



