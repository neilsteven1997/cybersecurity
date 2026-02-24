# JavaScript Essentials

---

In my journal today, I'm documenting the core elements of JavaScript from a security angle, drawing directly from a beginner-oriented 
overview that ties into how attackers twist legitimate features for harm. JavaScript serves as a scripting tool for injecting 
interactivity into web pages built on HTML and CSS, handling tasks like form checks, click responses, and animations. This material 
assumes familiarity with Linux basics and web fundamentals, such as how sites operate, and aims to grasp JS foundations, its embedding 
in HTML, misuse of dialog tools, evading logic controls, dissecting compressed code, and linking to a virtual setup for hands-on work. 
The setup involves launching a machine that appears in split view after a brief wait for scripts to initialize, with sample files ready 
on the desktop if manual creation proves tricky.

Variables act as storage units for data, named for easy recall, declared via var for broader scope or let and const for tighter block 
limits, which aids in managing visibility and preventing unintended overwrites. Data types cover strings for text, numbers, booleans for
truth values, null for intentional emptiness, undefined for unassigned states, and objects for structured collections like arrays. 
Functions bundle reusable code for tasks, such as outputting a student's exam result based on a roll number input, while loops repeat 
actions under conditions, like cycling through a list to invoke that function multiple times even if the data runs short, leading to 
undefined entries beyond the array's end. The request-response loop describes a client's browser querying a server for resources, which 
replies with content; for deeper insight, Mozilla's guide at 
https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/How_the_Web_works proves reliable.

JavaScript runs interpreted in the browser, enabling quick tests without builds. A basic script might log a greeting, set a numeric 
age variable, use an if-else to classify adulthood, define a greeting function that concatenates a name, and call it. Executing this 
in Chrome's console—accessed via Ctrl+Shift+I—highlights client-side nature, where code is inspectable and manipulable. A simple 
addition script declares two numbers, computes their sum, and logs it, pasting directly into the console for instant output. This ease 
of access underscores why security pros must probe JS in audits.

Embedding JavaScript in HTML happens internally or externally. Internal placement wraps code in script tags within head or body, ideal 
for learning direct interactions, like summing numbers and updating a paragraph's innerHTML via getElementById. Saving this as an HTML 
file and viewing in a browser shows the dynamic update. External separation stores logic in a .js file, linked via script src, keeping 
markup clean for larger apps, with the browser fetching and running it seamlessly. In pentests, spotting internal versus external JS 
involves source code review: no src means inline, while src points to separate files, as seen on sites like https://tryhackme.com. For 
HTML refreshers, TryHackMe's room at https://tryhackme.com/room/howwebsiteswork covers essentials.

Dialog functions enable user engagement but invite abuse. Alert pops a message box with OK, like notifying "Hello THM." Prompt solicits 
input, returning the value or null on cancel, useful for capturing a name then alerting a personalized hello. Confirm offers OK/Cancel, 
yielding true or false. Attackers exploit these in malicious HTML attachments, looping alerts endlessly to harass, as in a file that 
triggers "Hacked" repeatedly, emphasizing the risk of untrusted scripts.

Control flow directs execution via conditions. An if-else might verify age from a prompt, displaying adult or minor status accordingly.
Client-side logins, checking username and password against fixed values, are bypassable by altering code or inputs, a flaw I've 
encountered in weak apps where devs skip server checks.

Minification strips whitespace and shortens names for faster loads, while obfuscation adds confusion through renames and filler. A 
greeting function alerting "Welcome to THM" runs fine post-obfuscation, viewable but garbled in dev tools. Tools like the one at 
https://obfuscator.io handle this safely, and deobfuscators at https://deobfuscate.io reverse it to readable form, reminding me that 
obscurity isn't true security.

---

| Description | Code/Command |
|-------------|--------------|
| Function to print student result based on roll number | <script> function PrintResult(rollNum) { alert("Username with roll number " + rollNum + " has passed the exam"); // any other logic to display the result } // prepare an array of roll numbers (data) const rollNumbers = [101, 102, 103]; // Note: We only have 3 roll numbers, so after i = 2, rollNumbers[i] becomes undefined for (let i = 0; i < 100; i++) { PrintResult(rollNumbers[i]); } </script> |
| Alternative function and loop for printing results | // Function to print student result function PrintResult(rollNum) { alert("Username with roll number " + rollNum + " has passed the exam"); // any other logic to the display result } // prepare an array of roll numbers (data) const rollNumbers = [101, 102, 103]; // Note: We only have 3 roll numbers, so after i = 2, rollNumbers[i] becomes undefined for (let i = 0; i < 100; i++) { PrintResult(rollNumbers[i]); // this will be called 100 times } |
| Basic console logging and control flow demo | // Hello, World! program console.log("Hello, World!"); // Variable and Data Type let age = 25; // Number type // Control Flow Statement if (age >= 18) { console.log("You are an adult."); } else { console.log("You are a minor."); } // Function function greet(name) { console.log("Hello, " + name + "!"); } // Calling the function greet("Bob"); |
| Simple addition in console | let x = 5; let y = 10; let result = x + y; console.log("The result is: " + result); |
| Internal JS in HTML for addition display | <!DOCTYPE html> <html lang="en"> <head> <title>Internal JS</title> </head> <body> <h1>Addition of Two Numbers</h1> <p id="result"></p> <script> let x = 5; let y = 10; let result = x + y; document.getElementById("result").innerHTML = "The result is: " + result; </script> </body> </html> |
| External JS script for addition | let x = 5; let y = 10; let result = x + y; document.getElementById("result").innerHTML = "The result is: " + result; |
| External HTML linking to JS | <!DOCTYPE html> <html lang="en"> <head> <meta charset="UTF-8"> <meta name="viewport" content="width=device-width, initial-scale=1.0"> <title>External JS</title> </head> <body> <h1>Addition of Two Numbers</h1> <p id="result"></p> <!-- Link to the external JS file --> <script src="script.js"></script> </body> </html> |
| Alert function example | alert("Hello THM"); |
| Prompt and alert for greeting | name = prompt("What is your name?"); alert("Hello " + name); |
| Confirm function example | confirm("Do you want to proceed?"); |
| Malicious looped alert in HTML | <!DOCTYPE html> <html lang="en"> <head> <title>Hacked</title> </head> <body> <script> for (let i = 0; i < 3; i++) { alert("Hacked"); } </script> </body> </html> |
| Age verification with prompt and if-else | <!DOCTYPE html> <html lang="en"> <head> <title>Age Verification</title> </head> <body> <h1>Age Verification</h1> <p id="message"></p> <script> age = prompt("What is your age") if (age >= 18) { document.getElementById("message").innerHTML = "You are an adult."; } else { document.getElementById("message").innerHTML = "You are a minor."; } </script> </body> </html> |
| Original hello function | function hi() { alert("Welcome to THM"); } hi(); |
| Obfuscated hello function | (function(_0x114713,_0x2246f2){var _0x51a830=_0x33bf,_0x4ce60b=_0x114713();while(!![]){try {var _0x51ecd3=-parseInt(_0x51a830(0x88))/(-0x1bd3+-0x9a+0x2*0xe37)*(parseInt(_0x51a830(0x94)) /(-0x15c1+-0x2*-0x3b3+0xe5d))+parseInt(_0x51a830(0x8d))/(0x961*0x1+0x2*0x4cb+0x4bd*-0x4)* (-parseInt(_0x51a830(0x97))/(-0x22b3+0x16e9+0x1*0xbce))+parseInt(_0x51a830(0x89)) /(-0x631+0x20cd+0x8dd*-0x3)*(-parseInt(_0x51a830(0x95))/(-0x8fc+0x161+0x7a1))+ -parseInt(_0x51a830(0x93))/(-0x1c38+0x193+0x1aac)*(parseInt(_0x51a830(0x8e)) /(-0x1*-0x17a6+-0x167e+-0x3*0x60))+-parseInt(_0x51a830(0x91))/(-0x2*-0x1362+-0x4a8*0x5+-0xf73)* (parseInt(_0x51a830(0x8b))/(-0xb31*0x2+0x493*0x5+0x1*-0x73))+parseInt(_0x51a830(0x8f)) /(-0x257a+-0x1752+0x3cd7)+parseInt(_0x51a830(0x90))/(-0x2244+-0x15f9+0x3849);if(_0x51ecd3 ===_0x2246f2)break;else _0x4ce60b['push'](_0x4ce60b['shift']());}catch(_0x38d15c) {_0x4ce60b['push'](_0x4ce60b['shift']());}}}(_0x11ed,-0x17d11*-0x1+0x2*0x2e27+0x100f*0x17)); function hi(){var _0x48257e=_0x33bf,_0xab1127={'xMVHQ':function(_0x4eefa0,_0x4e5f74) {return _0x4eefa0(_0x4e5f74);},'FvtWc':_0x48257e(0x96)+_0x48257e(0x92)};_0xab1127[_0x48257e(0x8c) ](alert,_0xab1127[_0x48257e(0x8a)]);}function _0x33bf(_0xb07259,_0x5949fe){var _0x3a386b =_0x11ed();return _0x33bf=function(_0x4348ee,_0x1bbf73){_0x4348ee=_0x4348ee-(0x11f7+- 0x1*0x680+-0x3a5*0x3);var _0x423ccd=_0x3a386b[_0x4348ee];return _0x423ccd;},_0x33bf (_0xb07259,_0x5949fe);}function _0x11ed(){var _0x4c8fa8=['7407EbJESQ','\x20THM', '2700698TTmqXC','10ILFtfZ','190500QONgph','Welcome\x20to', '4492QOmepo','21623eEAyaP','65XMlsxw','FvtWc','2410qfnGAy','xMVHQ','321PfYXZg', '8XBaIAe','1946483GviJfa','15167592PYYhTN'];_0x11ed=function(){return _0x4c8fa8;};return _0x11ed();}hi(); |
| Bad practice hardcoded key | // Bad Practice const privateAPIKey = '<redacted>'; |

---

### Key Takeaways

- Prerequisites include completing Linux Fundamentals and understanding web application basics along with how websites function.
- Objectives focus on mastering JavaScript essentials, embedding it in HTML, exploiting dialog features, circumventing logic controls,
  and analyzing compacted scripts.
- To connect and begin: Initiate the virtual machine, wait for split-screen visibility, allow 1-2 minutes for boot and script execution;
   use desktop exercise folder scripts if needed.
- For internal JS integration: Create an HTML file on desktop, open in editor, insert code with script tags in body, save, and view in
   browser to see dynamic output.
- For external JS: Craft a .js file with logic, create linking HTML with script src, save both on desktop, open HTML in browser for
   identical results.
- To verify JS type: Open target HTML in browser, right-click for source view; check script tags—src absence means internal, presence
  indicates external.
- Alert abuse demo: In console, enter alert command; for loop exploitation, build HTML with for loop triggering multiple alerts, open
  to experience disruption.
- Prompt usage: In console, prompt for input, use returned value in alert.
- Confirm test: In console, enter confirm query, note boolean return.
- For control flow bypass example: Build age HTML with prompt and if-else, open in browser, input values to see conditional output; for
  login, open provided file, enter credentials to observe client-side check.
- Obfuscation process: Create hello HTML linking to js, add original function to js, view and inspect; paste js into
  https://obfuscator.io, copy output back to js, reload to see garbled but functional code.
- Deobfuscation: Paste obfuscated code into https://deobfuscate.io to retrieve readable version.
- Best practices: Supplement client-side validation with server-side checks since users can tamper with JS; only include libraries
  from verified sources to avoid malicious mimics; steer clear of embedding secrets like keys in code, as source is viewable; apply
  minification and obfuscation in production for load efficiency and added analysis hurdles, though reversible with effort.

---





