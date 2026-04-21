# AI Forensics

---

In reviewing the AI Forensics material, digital forensics work routinely demands rapid assembly of disparate evidence fragments under 
pressure, a process many analysts have handled manually without borrowing tools from adjacent fields. The content explored whether 
artificial intelligence belongs in this domain, weighing its potential applications against the fresh challenges it introduces along 
with associated ethical and legal considerations. 

Background familiarity with artificial intelligence fundamentals from the AI Security Threats room, core digital forensics principles 
from the Introduction to Digital Forensics room, and typical operational obstacles from the Difficulties and Challenges room proved 
essential. The objectives centered on recognizing routine daily hurdles in the field, examining how artificial intelligence can mitigate 
them, identifying the new complications and implications it creates for investigations, and evaluating its overall effect on standard 
processes.

Artificial intelligence capabilities map directly onto common forensics pain points. Parallelized deep learning combined with transformer
models handles massive data volumes in milliseconds, delivering classifications and insights that cut through manual drudgery. Machine 
learning establishes baselines of normal user, system, and network activity then surfaces anomalies even in stealthy attacks designed to
blend in, revealing large-scale patterns that escape human comprehension. The technology also scales effortlessly across cloud, hybrid, 
and remote environments that now generate unprecedented forensic data volumes, allowing teams to manage growing workloads without 
proportional headcount increases.

Industry adoption has already embedded artificial intelligence into operational tools for tangible productivity gains. Anomaly 
detection and user entity behavior analytics rely on unsupervised techniques such as isolation forests and autoencoders to learn 
baselines and flag deviations without predefined rules. Email and communication monitoring applies transformer-based language models 
like BERT and RoBERTa to classify messages by tone, structure, and known attack indicators, catching impersonation attempts, urgency 
language, and malicious links. Malware and file classification analyzes static and dynamic features from large training corpora to 
label new samples as benign or malicious. Alert triage ranks and filters noise by learning from past incidents and analyst feedback, 
while timeline reconstruction clusters related events across logs, identifies causal links, and rebuilds attack sequences far faster 
than manual effort.

Limitations require equal attention before deployment. Traditional deterministic algorithms always return identical outputs for the 
same input, but artificial intelligence systems are probabilistic, producing variable results from statistical models that can shift 
with minor prompt changes or yield inconsistent timelines on identical data. This non-determinism clashes with forensics requirements 
for repeatable, defensible findings and demands rigorous prompt engineering. Model performance evaluation must combine accuracy, which 
can mislead on imbalanced datasets by simply predicting the dominant class, with precision to limit false positives and recall to 
ensure all true positives are caught; isolated metrics distort reality while the full set guides optimal tuning. Overfitting from 
training data further erodes reliability. The garbage-in-garbage-out principle applies forcefully here because poor input data produces
confident but false outputs that could undermine justice when evidence enters legal proceedings.

Across digital forensics and incident response, artificial intelligence has moved beyond theory into targeted use. Image and video 
analysis benefits from convolutional neural networks that learn spatial patterns, including frameworks pairing error level analysis 
with models to detect tampering at high accuracy and specialized detectors for deepfakes. Generative adversarial networks create an 
arms race by training defensive tools on synthetic fakes that expose subtle manipulations. Communication analysis leverages natural 
language processing transformers to classify phishing emails with reported 99 percent accuracy and scan chat logs or social media for 
threat patterns and sentiment. Timeline reconstruction automatically correlates sequenced logs from multiple sources to surface 
anomalies such as impossible simultaneous logins or atypical application behavior. Malware detection has advanced through neural 
network representations of files, behavioral profiling of API call sequences visualized as images, and widespread integration into 
endpoint detection and response products.

Ethical and legal implications cannot be ignored once artificial intelligence enters evidentiary workflows. Black-box models lack 
explainability, conflicting with courtroom standards such as the Daubert test and leading to exclusion of flagged evidence when 
rationale cannot be articulated. Bias embedded in historical training data risks unfair prioritization or misidentification, as 
demonstrated by facial recognition errors disproportionately affecting minority groups and resulting in documented wrongful arrests. 
Accountability demands unbroken chain of custody and audit trails that cloud-based tools often obscure, while privacy regulations 
like GDPR restrict processing of personal data and require offline or federated approaches to keep evidence admissible. In every case, 
artificial intelligence accelerates collection and insight but never supplants human judgment, validation, or responsibility.

The practical lab illustrated these concepts through a simulated breach at RobbCo, where proprietary firmware and operating system code
in RETROS, the MF Boot Agent, and Unified Operating System appeared compromised following suspicious off-hours activity reported to 
founder Robb House. Working inside an isolated virtual environment to preserve evidence integrity, I first activated the forensic 
environment and ran the scikit-learn-based classify_logs.py script against auth.log to surface anomalous entries showing a failed 
administrative login followed by successful access as j.morgan and escalation to r.house. The file_anomalies.py script then flagged 
items in high-priority directories based on name, path, size, extension, entropy, permissions, and creation time. Manual review of 
each artefact reconstructed the attack: a phishing-delivered malicious spreadsheet macro harvested bash history, SSH keys, sessions, 
and passwd data before exfiltrating to an external address; dropper and reverse-shell files enabled remote access; sudo commands 
planted an SSH key for privilege escalation; disguised persistence binaries with fabricated logs maintained control; and proprietary 
source code plus an encoded archive staged in shared memory confirmed intellectual property theft. Decoding and extraction commands 
verified the exfiltrated payload while revealing the artificial intelligence model's occasional misclassification of legitimate files, 
reinforcing that human insight must always validate machine output.

This exercise drove home how artificial intelligence functions as a guiding light for pattern detection and triage yet depends entirely
on analyst oversight to connect dots, correct errors, and uphold investigative integrity.

---

| Description | Code/Command |
|-------------|--------------|
| Activate the forensic virtual environment | source /opt/dfir-env/bin/activate |
| Classify logs for anomalies using the AI script | python3 /opt/dfir-lab/classify_logs.py /var/log/auth.log |
| Identify file anomalies with the ML script | python3 /opt/dfir-lab/file_anomalies.py |
| Decode the base64-encoded stolen archive | base64 -d /dev/shm/.core_dump_2025.tgz.enc > /tmp/stolen.tar.gz |
| Extract the tar archive containing source code | tar -xzvf /tmp/stolen.tar.gz -C /tmp/stolen_source |

---

**Extracted Tables**

| Task | What AI Enables | Example Tools / Platforms | How AI Solves It |
|------|-----------------|---------------------------|------------------|
| Anomaly Detection / UEBA | Flags unusual user/system behavior compared to learned “normal” | UEBA, Elastic, Exabeam | Uses unsupervised learning techniques (like Isolation Forests and Autoencoders) to learn baseline behaviour for users and systems. Deviations from this baseline are flagged as potential threats, even without predefined rules. |
| Email & Communication | Detects emails and flags risky language in chat/email logs | Microsoft Defender for O365, NLP | Transformer-based language models (e.g., BERT, RoBERTa) classify messages as malicious or benign based on tone, structure, and known attack patterns. These models help detect impersonation, urgency phrases, and malicious links. |
| Malware / File Classification | Classifies files as malicious or benign based on extracted static/dynamic features | Microsoft Defender (STAMINA), Cylance, VirusTotal integrations | Systems analyse file metadata, code signatures, and behaviour to detect threats. These models are trained on large malware corpora to classify new files based on patterns learned from known threats. |
| Alert Triage & Prioritisation | Automatically scores, ranks, and filters alerts to reduce analyst workload | Cortex XSOAR/XSIAM, IBM QRadar Advisor, CrowdStrike Falcon + Charlotte | Analyses past alert data, analyst feedback, and incident outcomes to rank alerts by severity and relevance. This reduces noise and surfaces the most urgent issues first, saving analysts time. |
| Timeline & Event Correlation | Reconstructs attack timelines by clustering and linking logs across sources | Timesketch, Velociraptor, Jupyter-based analysis | Clusters similar log events, identifies causal relationships, and aligns activity across systems. This helps analysts visualise attack chains and reconstruct incident timelines faster. |

| Artefact | Behaviour | Impact/Analysis | Explanation |
|----------|-----------|-----------------|-------------|
| /tmp/invoice_dump.txt | Stores collected recon data | Reveals prior SSH usage, usernames, and active sessions | Output of the macro’s system recon. Provided the attacker insight into viable accounts and access paths. |

| Artefact | Behaviour | Impact/Analysis | Explanation |
|----------|-----------|-----------------|-------------|
| /home/j.morgan/Documents/Invoices/invoice_Q1_2075.ods | Embedded macro executes shell commands | Harvests .bash_history, SSH keys, user sessions, and dumps /etc/passwd. Attempts exfiltration to 192.168.0.100 | Likely the result of a phishing lure masquerading as a legitimate invoice but runs an embedded payload when opened. |

| Artefact | Behaviour | Impact/Analysis | Explanation |
|----------|-----------|-----------------|-------------|
| /tmp/.syncd | Connects to http://10.0.0.66/payload.sh and executes it | Initiates second-stage download | First-stage dropper used to quietly retrieve additional malicious tooling. |
| /tmp/.x | Reverse shell stub | Establishes remote shell to 10.0.0.66:4444 | Gave the attacker live access under j.morgan’s user context. Likely deployed immediately after the phishing document was opened. |

| Artefact | Behaviour | Impact/Analysis | Explanation |
|----------|-----------|-----------------|-------------|
| /home/j.morgan/.bash_history | Reveals use of sudo to modify SSH keys | SSH key planted in r.house’s authorized_keys | Subtle privilege escalation — no exploits used. Demonstrates abuse of legitimate permissions for escalation. |

| Artefact | Behaviour | Impact/Analysis | Explanation |
|----------|-----------|-----------------|-------------|
| /usr/local/bin/sysmon | Outbound connection to 10.0.0.66:5555 | Reverse shell disguised as system monitoring tool | Persistence through deception — masks itself as a legitimate binary to evade detection. |
| /opt/robbco/sys/boot_monitor.log | Fake boot telemetry logs | Justifies sysmon's presence | Fabricated log file used to support the ruse of a legitimate monitoring utility. |

| Artefact | Behaviour | Impact/Analysis | Explanation |
|----------|-----------|-----------------|-------------|
| /opt/robbco/engineering/MFBootAgent/mfboot_main.c /opt/robbco/firmware/RETROS_BIOS/core.asm | Flagged by ML model | Not malicious, but classified as suspicious | AI mistakenly flagged these — they are RobbCo’s proprietary source code. |
| /dev/shm/.core_dump_2025.tgz.enc | Base64-encoded stolen archive | Exfiltration-ready package containing RobbCo IP | Stored in shared memory — stealthy staging location for data theft. |

---

### Key Takeaways
- Know the basics of artificial intelligence covered in the AI Security Threats room.
- Know the basics of digital forensics from the Introduction to Digital Forensics room.
- Know the challenges faced in digital forensics from the Difficulties and Challenges room.
- Understand the day-to-day challenges faced in digital forensics.
- Understand how artificial intelligence can be used to address those challenges.
- Understand the challenges that arise when artificial intelligence is used for forensics investigations and the ethical and legal
  implications this has.
- Understand how artificial intelligence impacts the investigation process.
- Accuracy measures overall correct predictions but can mislead with imbalanced data by favoring the majority class.
- Precision measures how many positive predictions are actually correct, reducing false positives.
- Recall measures success at identifying all actual positives in the dataset.
- Phishing email sent to j.morgan, malicious .ods file opened triggering data harvesting, data saved to /tmp/invoice_dump.txt and
  exfiltrated, attacker logs into j.morgan using collected credentials, access escalates from there.
- Artificial intelligence enhances tasks like anomaly detection, communication analysis, and timeline reconstruction.
- Models used in forensics exhibit probabilistic behaviour and precision-recall trade-offs.
- Legal and ethical challenges include explainability, bias, accountability, and privacy.
- Human insight combined with scikit-learn scripts uncovered the targeted breach at RobbCo while validating all machine-generated
  findings.

---






