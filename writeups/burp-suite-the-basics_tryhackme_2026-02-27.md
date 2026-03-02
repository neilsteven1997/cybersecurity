# Burp Suite: The Basics

---

In my ongoing exploration of web application testing tools, I've been digging into Burp Suite as a foundational framework for 
penetration assessments. It's essentially a Java-powered platform that handles everything from intercepting HTTP and HTTPS traffic to 
manipulating requests and responses between browsers and servers, which proves critical for spotting vulnerabilities in web and mobile 
apps, including those tied to APIs. This interception lets me route traffic to different built-in tools, making manual testing far more 
effective than automated scans alone. I appreciate how it positions itself as the go-to for hands-on security work, and I've found it 
indispensable when debugging or hunting bugs in development environments.

Burp comes in a few flavors, but I'm sticking with the Community Edition for now since it's free for non-commercial use within legal 
limits. The Professional version unlocks extras like an automated scanner for vulnerabilities, an unrestricted fuzzer for brute-forcing, 
project saving with reports, API integrations, and full extension access, plus the Collaborator for catching unique requests on a 
self-hosted or PortSwigger server. That makes it a powerhouse for pros, whereas the Enterprise edition shifts to server-based continuous
scanning, akin to Nessus for infrastructure, constantly checking apps for issues rather than manual local attacks. Given the licensing 
needs for those, my notes here center on Community's core capabilities.

Even in the limited Community setup, the toolkit impresses with essentials like the Proxy for grabbing and tweaking requests and 
responses during app interactions, Repeater for capturing, editing, and replaying requests repeatedly—handy for iterative payload 
crafting in things like SQL injection—and Intruder for blasting endpoints with requests, though it's throttled for brute-forcing or 
fuzzing. Decoder handles transforming data, encoding payloads or decoding captures efficiently right inside the suite. Comparer lets me
diff two data chunks at word or byte levels, speeding up analysis with shortcuts, and Sequencer checks randomness in tokens like session
cookies to flag weak algorithms that could lead to exploits. The Java base also supports extensions in Java, Python via Jython, or Ruby
via JRuby, loadable through the Extender module, with the BApp Store offering third-party add-ons like Logger++ to boost logging, even 
if some need Pro.

Installation's straightforward across platforms; on Kali Linux, it's pre-installed or grabable via apt repos. For Linux, macOS, or 
Windows, PortSwigger's site has dedicated installers—select Community from the dropdown and download at 
https://portswigger.net/burp/communitydownload. Run the exe on Windows or script on Linux, optionally with sudo; without it, it lands 
in the home dir without PATH addition. Wizards guide defaults, which are usually fine, but I always scan them. On TryHackMe's AttackBox,
it's already there, so no setup needed. Launching brings a project type prompt—stick with defaults in Community—then config selection, 
again defaults work, and hit start to enter the interface. First-time users get training prompts, worth reviewing later.

The dashboard splits into quadrants: top-left for tasks like the default live passive crawl that logs visited pages automatically; 
bottom-left event log tracking actions and connections; bottom-right advisory detailing vulnerabilities with refs and fixes, exportable
to reports though Community shows none; top-right issue activity for Pro's scanner results by severity and certainty. Help icons 
throughout pop context-specific info, which I've clicked often for quick clarifications. Navigation relies on top menu bars for module 
switching, with sub-tabs below for specifics like Proxy's Intercept. Detach windows via the app menu for multi-viewing, and shortcuts 
speed access—I've memorized a few for efficiency.

Settings divide into global user options affecting the whole install and project-specific ones, lost on close in Community since 
saving's Pro-only. Access via the top settings button, with a left menu for search, type filters, or categories. Tools often link 
directly to relevant sections, like Proxy's button. I've poked around these extensively to tailor behavior.

At the core, Burp Proxy captures traffic for manipulation, sending to other modules, or forwarding/dropping. It logs requests even with 
intercept off, including WebSockets for fuller app analysis, viewable in HTTP and WebSockets history sub-tabs. Proxy settings allow 
response interception via rules, or match-and-replace with regex for dynamic tweaks like user agents or cookies—I've experimented here 
to see how it alters flows.

To route browser traffic through it, I use FoxyProxy on Firefox: install the extension (pre-done on AttackBox), add a config with title
like Burp, IP 127.0.0.1, port 8080, save, and activate. With intercept on, requests hang until handled in Proxy. 
Testing on http://<MACHINE_IP>/ populates the intercept, and right-clicks offer actions like send to tools. Close unrelated tabs to 
avoid extraneous WebSocket noise.

The Target tab's site map builds a tree of visited pages via proxy, useful for enumeration and spotting odd endpoints like APIs; in 
Community, it's manual browsing, but Pro adds auto-crawling. Issue definitions list all scanner-checked vulnerabilities with 
descriptions and refs, great for manual findings or reports. Scope settings include/exclude domains or IPs to focus testing, avoiding 
clutter.

Burp's built-in Chromium browser simplifies things, pre-configured for proxy—launch from Proxy tab. On Linux root like AttackBox, it 
might error on sandbox; I opt for the settings toggle to run without, mindful of security risks though low in training. For scoping, 
right-click targets in site map to add to scope, confirming to log only in-scope; then in Proxy settings, enable intercept only for 
in-scope URLs to ignore the rest.

For HTTPS, browsers flag untrusted PortSwigger CA; fix by downloading the cert from http://burp/cert as cacert.der, then in Firefox 
prefs search certificates, import, and trust for sites. This lets seamless TLS proxying. (Full: https://portswigger.net/burp/documentation/desktop/external-browser-config/certificate/ca-cert-firefox)

In a practical run-through on http://<MACHINE_IP>/ticket/'s support form, I tested for reflected cross-site scripting by injecting a 
script alert payload into the email field, but client-side filters block special chars. Bypassing via proxy: enter valid data, submit 
to intercept, swap email to the payload, URL-encode it, forward—and it triggers the alert, proving the vuln. This highlights how proxy 
manipulation sidesteps front-end checks.

Wrapping this entry, Burp's setup and proxy form a strong base for web pentesting; I've found experimenting key to mastery, and I'll 
build on this in deeper tools like Repeater.

---

| Description | Code/Command |
|-------------|--------------|
| Reflected XSS payload for bypassing client-side filter in support form email field | <script>alert("Succ3ssful XSS")</script> |

---

## Extracted Tables

| Shortcut | Tab |
|----------|-----|
| Ctrl + Shift + D | Dashboard |
| Ctrl + Shift + T | Target tab |
| Ctrl + Shift + P | Proxy tab |
| Ctrl + Shift + I | Intruder tab |
| Ctrl + Shift + R | Repeater tab |

---

### Key Takeaways
- Thorough introduction to Burp Suite.
- Comprehensive overview of tools within the framework.
- Detailed guidance on installing Burp Suite.
- Navigating and configuring Burp Suite.
- Introduction to Burp Proxy as the core.
- Burp Suite Professional features: automated vulnerability scanner; fuzzer/brute-forcer without rate limits; saving projects and
  generating reports; built-in API for integrations; unrestricted extension access; Burp Suite Collaborator for unique request
  catching.
- Burp Suite Community features: Proxy for interception and modification; Repeater for capturing, modifying, resending requests;
  Intruder for spraying endpoints (rate-limited); Decoder for data transformation; Comparer for data comparison; Sequencer for
  randomness assessment.
- Installation on Kali: pre-installed or via apt.
- For other OS: use installers from download page.
- Run installer with defaults.
- Dashboard quadrants: Tasks (e.g., live passive crawl); Event log (actions and connections); Issue Activity (Pro-only vulnerabilities
  by severity); Advisory (detailed vuln info, refs, remediations).
- Navigation: top menu for modules; sub-tabs below; detach via Window menu.
- Settings types: Global (user) for baseline; Project for session (lost in Community).
- Proxy key points: Intercept requests for actions like forward, drop, edit; disable intercept to pass through; logs requests even off;
  captures WebSockets; view in history sub-tabs.
- Notable Proxy settings: Response interception via rules; Match and Replace with regex for modifications.
- FoxyProxy setup: Install extension; Access options; Add proxy with title, 127.0.0.1:8080; Save; Activate; Enable intercept in Burp;
  Test by accessing site. [FoxyProxy Basic](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-basic/)
- Target sub-tabs: Site map for tree structure; Issue definitions for vuln list with refs; Scope for include/exclude.
- Burp Browser: Launch from Proxy; On Linux root, toggle no-sandbox in settings.
- Scoping: Add to scope from site map; Confirm log only in-scope; In Proxy settings, intercept only in-scope.
- HTTPS setup: Download cert from http://burp/cert; In Firefox prefs, view certificates; Import der; Trust for websites.
- (Full: https://portswigger.net/burp/documentation/desktop/external-browser-config/certificate/ca-cert-firefox)
- Example attack steps: Enter payload in email but filter blocks; Enter valid data, submit; Intercept in proxy; Replace email with
  payload; URL-encode selection (Ctrl+U); Forward to trigger alert.

---






