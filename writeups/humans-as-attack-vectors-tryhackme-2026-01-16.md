# Humans as Attack Vectors

---
Humans remain the primary attack surface in modern intrusions, often exploited through psychological manipulation rather than 
technical exploits. Social engineering tactics succeed by crafting messages that appear legitimate and trigger emotional responses 
like urgency or curiosity, prompting victims to grant access, disclose credentials, or execute malicious actions. Attackers 
frequently target accounts with broad privileges—HR for employee data, IT admins for network entry, finance for financial 
transfers—then pivot to data theft, ransomware deployment, or further lateral movement. Phishing via email persists as the 
dominant vector, with billions of malicious messages circulating daily, though volumes fluctuate while sophistication rises 
through AI assistance.

Delivery methods extend beyond traditional links to include malware disguised as legitimate software downloads, often via fake 
CAPTCHA challenges, malicious QR codes, or search engine optimization poisoning that elevates malicious sites. Deepfake 
technology amplifies impersonation risks, generating convincing audio or video to mimic executives or trusted contacts during 
calls or meetings, leading to large unauthorized transfers in documented cases. Phone-based impersonation, such as fraudsters 
posing as IT support to request remote access or account changes, frequently initiates ransomware chains.

Defensive posture requires dual focus: proactive mitigation through layered controls and reactive detection when prevention 
fails. Effective measures include email filtering to block phishing attempts upstream, endpoint detection and response tools 
to interrupt malware execution, and user education emphasizing verification of suspicious requests—particularly those 
involving urgent financial actions or account takeovers. Security awareness programs, reinforced with simulated phishing 
exercises, reduce click rates over time. In practice, I've seen the most impact from combining technical blocks with cultural 
reinforcement of "trust but verify" habits.

SOC analysts play a critical role in spotting bypassed controls—unusual login patterns, anomalous file executions, or 
behavioral deviations that indicate compromised credentials or malware activity. The work extends beyond monitoring to 
proposing improvements that lighten the alert burden, such as better filtering or policy updates approved through change 
management.

---

Key Takeaways
- Prioritize mitigation through anti-phishing email gateways, robust EDR deployment, and mandatory verification protocols
  for high-risk requests
- Reinforce user awareness via ongoing training and realistic phishing simulations
- Maintain vigilance for evolving vectors including deepfakes, malicious QR codes, and impersonation calls
- Detect intrusions that evade prevention by analyzing anomalous behaviors and credential misuse indicators
- Propose and advocate for security enhancements to reduce routine alert fatigue

---
