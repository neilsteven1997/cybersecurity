## Training Efficacy Measurement 
- the outcomes of a phishing simulation are leveraged to establish a quantifiable measure 
of the workforce's overall security behavior. This metric directly validates the actual return on investment (ROI) and 
effectiveness of the existing cybersecurity awareness training programs.

---
## Success Indicator 
- training is deemed successful—or has yielded results—when the click-through rate (CTR) on the 
simulated phishing emails is significantly low. A low CTR is the most reliable empirical evidence that employees are 
successfully recognizing and avoiding social engineering threats.

---
## Strategic Value 
- this activity provides a critical, non-technical stress test for the organization's "human firewall." 
Since employees often represent the most exploitable vulnerability in any defense strategy, testing their vigilance through 
controlled threat emulation is a necessary and proactive defense mechanism.

---
## Social engineering 
- is essentially "human hacking," referring to the act of manipulating a person into making a mistake 
that compromises security. This could involve an employee sharing a password, opening a malicious file, or approving an 
unauthorized payment. The term "social" is used because the primary target of this attack is the human being, not the 
computer system. Consequently, the attacker relies on psychological tricks to get the user to willingly cooperate. 
Key psychological factors that play a major role in the success of these attacks are urgency, curiosity, and the 
exploitation of perceived authority.

---
## Phishing - is a specific subset of social engineering where the communication medium is primarily messages.
While email was once the most common vector, the widespread use of smartphones and ubiquitous internet access has 
expanded phishing into new areas, including short text messages (smishing), voice calls (vishing), QR codes (quishing),
and social media direct messages. The attacker's central goal is to induce the target user to click, open, or reply 
to a message to steal information, money, or system access. Unfortunately, phishing attacks are constantly evolving 
and becoming significantly harder to detect. Even careful people can be compromised if they do not exercise adequate diligence.

---
## Anti-Phishing Defense Checklists 
A. S.T.O.P. (The Suspicion Checklist)
Ask these questions about any message before acting:
- Suspicious? (Is the sender/timing unusual?)
- Telling me to click something? (Links are high risk.)
- Offering me an amazing deal? (A psychological trigger.)
- Pushing me to do something now? (A clear sign of pressure/urgency.)

---
## B. S.T.O.P. (The Action Plan for Verification)
Follow these steps for proactive defense:
- Slow down. (Counter the attacker's use of urgency.)
- Type the address yourself. (Never use the provided link; navigate independently.)
- Open nothing unexpected. (Verify context/sender first.)
- Prove the sender. (Check the full source address/number, not just the display name.)

---
## Practical Relevance
- Penetration Testing: Authorized phishing simulations are used by Red Teams to quantify the success of security training.
- Result Interpretation: A low click-through rate (CTR) validates that the training has succeeded in strengthening the
"human firewall."

---
## Phishing Configuration and Risk Assessment

Initial setup requires selecting the attack scope: single target versus mass deployment. This choice dictates infrastructure 
needs.

Next, the email routing is configured for maximum realism. This involves setting the From address and From name to 
impersonate a trusted source known to the target, capitalizing on Authority. The delivery method must be specified, 
typically choosing between a verified server or directing traffic to the target's SMTP server (address and Port 25).

Further psychological decisions include flagging the message as high priority or attaching files, invoking Urgency 
or Curiosity. A convincing Subject and message body are then drafted, incorporating the malicious URL that links to 
the credential trap.

Risk Outcome: Acquiring even one set of valid credentials is an alarming failure. This result proves an adversary 
can successfully gain unauthorized access, requiring immediate incident response and a critical review of defensive measures.

Tool Deployment: Phishing Email via SET
For this stage of the simulation, the solution is to deploy the Social-Engineer Toolkit (SET). This is a crucial, 
open-source framework developed by David Kennedy specifically for social engineering engagements.
SET offers a broad feature set, but the immediate requirement is its ability to build and dispatch custom phishing emails.
I'm using this tool to construct the deceptive email and send it directly to the target user. It handles the integration 
of the fake login page trap, simplifying the process of delivery for the credential harvesting objective.

Now, we would be asked to select between two options. One option allows us to send the phishing email to a single address, 
while the other option enables us to send an email to many people. Here, we would select option 1, i.e., E-Mail Attack 
Single Email Address.

Now, we will have several questions to answer and various fields to fill out. The first set of questions concerns 
the email addresses and how the email will be routed and delivered. After each input provided, we can press Enter to 
get to the next question.

- Send email to: Let’s begin by targeting ---
- How to deliver the email: We will choose Use your own server or open relay
- From address: We know that the guys at the toy factory communicate regularly with Flying Deer, the shipping company, 
so that we will use --- as the source email address
- From name: Let’s use the name Flying Deer
- Username for open-relay: We will leave it blank and just hit the Enter key
- Password for open-relay: We will leave it blank and just hit the Enter key
- SMTP email server address: We will deliver directly to the TBFC mail server by entering ---
- Port number for the SMTP server: We leave the default value of 25 and just hit the Enter key
- The next set of questions will ask if you want to send it as a high priority or attach a file.
- Flag this message as high priority: The choice is entirely up to you, depending on your knowledge of the circumstances, 
but we will answer with no
- Do you want to attach a file: We will answer with n
- Do you want to attach an inline file: Again, let’s answer with n
- Finally, we pick an email subject and enter the message contents in plaintext or HTML.
- Email subject: We need to think of something convincing, for example, “Shipping Schedule Changes”
- Send the message as HTML or plain: We will keep the default choice of plaintext and just hit the Enter key
- Enter the body of the message, and type END (capitals) when finished: Create and type any convincing message. Make sure 
to include the URL --- to check if the target will fall for this trick.
- Now, the phishing email has been sent to the target. The "Press <return> to continue" button is just the Enter button to 
restart the tool. Open the terminal where our server.py script is running to see if the user gets trapped and enters their 
credentials.

>[!Note] 
>You may have to wait for 1 - 2 minutes and observe the terminal for any credentials entered by the user.

>[!Tip]
>Outcome: Successfully captured the credentials 'username=admin&password=unranked-wisdom-anthem' from the phishing page. 
This demonstrates the effectiveness of my phishing setup, as it indicates that the phishing email led to credential
submission, which is a critical objective in this exercise. Opened the emailer and found “Urgent: Production & Shipping 
Request - 1984000 Units (Next 2 Weeks)”

>[!Note]
>You did it! Wareville is one step safer.  The townsfolk are counting on you to keep Christmas secure.  Head back to 
Wareville to continue your mission!
