# AI/ML Security Threats

---

The introductory material on AI and ML security threats examines how industries including cybersecurity are confronting the rapid 
integration of artificial intelligence. The content directly addresses core questions on the nature of artificial intelligence and 
machine learning, their practical uses in the field, effects on existing roles, and the ways attackers exploit these technologies. It 
serves as an accessible entry point for building foundational awareness of how the systems operate and their wider consequences for 
operations and society at large.

No prior modules are required, though familiarity with basic cybersecurity concepts such as common attack vectors is presumed. The 
stated objectives center on grasping artificial intelligence, machine learning, and their cybersecurity implications; the mechanics of 
deep learning together with neural networks that underpin current applications; the tactics adversaries employ to bolster attacks and 
exploit model weaknesses; and the contributions machine learning makes toward defense against threats.

Artificial intelligence is framed as any machine or computer system capable of performing tasks that normally demand human reasoning, 
comprehension, problem-solving, or creativity. The field traces its origins to the 1950s, when early research focused on enabling 
machines to simulate human intelligence, though the term remained specialized at the time.

Machine learning functions as a subfield of artificial intelligence in which computers learn directly from data without predefined 
instructions, mirroring human learning processes and improving accuracy through exposure to additional data over time. Its development
follows an iterative lifecycle that starts with problem definition, such as classifying emails as spam or legitimate. Data collection,
cleaning, and feature engineering follow to extract relevant patterns while guarding against overfitting, where excessive familiarity 
with training data prevents effective generalization to new inputs. An algorithm is then selected for training, after which the model 
undergoes evaluation, tuning for optimal performance, and deployment into production environments. Continuous monitoring tracks 
real-world accuracy and triggers retraining when drift occurs, keeping the entire cycle ongoing.

The algorithms themselves serve as mathematical procedures for identifying patterns in data, producing trained models as output. Each 
consists of a decision process for generating predictions or classifications from inputs, an error function to measure and report 
performance shortfalls, and an optimization step that iteratively refines parameters to reduce errors. These fall into four primary 
categories: supervised learning that trains on labeled data for tasks such as classification or regression; unsupervised learning that
uncovers hidden structures in unlabeled data through clustering, association, or dimensionality reduction; semi-supervised learning 
that blends limited labeled data with larger unlabeled sets; and reinforcement learning that refines agent behavior via rewards for 
correct actions and penalties for errors.

Neural networks replicate the human brain’s structure of interconnected neurons that transmit signals across synapses, adjusting 
connection strengths in response to new patterns encountered. In practice, an input layer accepts raw data with node counts scaled to 
the input dimensions, hidden layers progressively extract and weigh features through adjustable connection strengths, and an output 
layer delivers the final prediction. When the network exceeds three layers it qualifies as deep learning, enabling the extraction of 
increasingly abstract features from raw inputs without manual labeling. This approach processes vast unlabeled datasets at scale, a 
capability accelerated by widespread digitization that supplied the necessary volumes of training material.

Large language models represent an advanced application of deep learning, built on neural networks to process and generate text through
sequential next-word prediction. Pre-training occurs on enormous unlabeled corpora, with models such as GPT-3 ingesting text equivalent
to 2,600 years of nonstop human reading and later versions demanding even larger sets. Billions of parameters act as adjustable puzzle 
pieces, refined automatically via backpropagation that compares predictions against actual continuations and updates weights to favor 
accurate outputs. Transformer neural networks, introduced in Google’s 2017 paper “Attention is All You Need,” shifted processing from 
sequential to parallel by assigning attention scores to relevant tokens, greatly improving contextual accuracy. Subsequent reinforcement
learning from human feedback incorporates human review to suppress unhelpful or problematic generations. Once refined, these models 
power generative applications including ChatGPT, LLaMA, and Deepseek, producing coherent responses from natural-language queries.

The material traces a clear progression: artificial intelligence as the broad umbrella, machine learning as its data-driven subfield, 
deep learning as the scalable neural-network extension that removes human labeling requirements, and large language models as 
specialized deep-learning implementations centered on transformers and attention mechanisms.

Security threats arising from these technologies divide into two groups: new vulnerabilities inherent to model deployment and the 
augmentation of longstanding attack techniques. Orientation is provided through a dedicated framework modeled on the ATT&CK Framework 
but tailored specifically to artificial intelligence threats.

Model vulnerabilities begin with prompt injection, in which crafted inputs override the original system instructions supplied to the 
model, potentially forcing disclosure of restricted details or generation of harmful content. Data poisoning involves deliberate 
corruption of the training corpus or dataset, causing the resulting model to produce biased or incorrect outputs; the spam-filter 
example illustrates how manipulated training data could allow malicious emails to bypass detection. Model theft occurs when an adversary
repeatedly queries the target model to harvest outputs, then trains a surrogate clone that replicates the original behavior and 
intellectual property. Privacy leakage arises when a model unintentionally exposes elements of its confidential training data, such as 
patient records in a medical classifier. Model drift manifests as gradual performance degradation when real-world inputs diverge from 
the original training distribution, underscoring the necessity of ongoing post-deployment monitoring and periodic retraining.

Enhanced attacks capitalize on generative capabilities for rapid content creation. Malware development is accelerated because attackers
can now produce functional malicious code with minimal effort. Deepfakes erode authentication by synthesizing highly realistic audio or
video impersonations of trusted individuals, as in the scenario of a secretary receiving forged instructions to release confidential 
customer data. Phishing emails, traditionally hindered by poor grammar or generic phrasing, become far more convincing through fluent,
context-aware generation that evades human instinct; built-in model safeguards against malicious requests can be circumvented via 
prompt engineering.

Defensive applications counterbalance these risks. The latest IBM Cost of a Data Breach report quantifies the advantage: organizations 
that integrate artificial intelligence realize average savings of $2.2 million per incident against an overall mean breach cost of 
$4.88 million, while also shortening identification and containment timelines by 108 days. The technology sharpens analysis by detecting
anomalies in high-volume data streams such as network traffic for intrusion detection. It strengthens prediction and automation, 
enabling models trained on email corpora to flag phishing attempts and block them before delivery. Summarization condenses incident 
reports, artifacts, and logs into concise overviews, revealing correlations that might otherwise be missed. Investigation benefits from
natural-language interfaces that parse logs, propose diagnostic queries, and brainstorm threat-hunting scenarios beyond typical human 
imagination limits.

Secure adoption requires deliberate safeguards from the outset, given that only 24 percent of generative artificial intelligence 
initiatives currently incorporate adequate protections. Access to models must be restricted through role-based access control and 
multi-factor authentication to prevent unauthorized interaction. Training data containing sensitive information demands encryption 
equivalent to any other confidential asset. Established standards such as ISO/IEC 27090 should guide the full lifecycle of development,
deployment, and maintenance. Finally, continuous monitoring with explainability tools including SHAP and LIME detects not only 
performance drift but also suspicious behavior or bias indicative of active attacks.

The practical exercise demonstrated these defensive uses through direct interaction with a provided AI assistant. Example interactions
included supplying a sample log line for interpretation, submitting a suspicious email for phishing assessment, requesting realistic 
threat-hunting scenarios within a corporate environment, and prompting generation of a regex pattern to match failed SSH login attempts
in Linux authentication logs. The final task involved querying the assistant for the numerical values of the DNS over HTTPS port, SYN 
flood timeout, and Windows ephemeral port range size, then substituting them into the flag format thm{DNS over HTTPS (DoH) Port/SYN 
flood timeout/ windows ephemeral port range size} to complete the exercise.

Knowledge of these building blocks, threat surfaces, defensive opportunities, and secure implementation practices equips practitioners
to treat artificial intelligence as a powerful but double-edged tool rather than something to fear outright.

---

| Description | Code/Command |
|-------------|--------------|
| Log Analysis Prompt | Here’s a logline:<br>Apr 22 11:45:09 ubuntu sshd[1245]: Failed password for invalid user admin from 203.0.113.55 port 56231 ssh2<br>Can you explain what is happening in this log entry? |
| Phishing Email Detection Prompt | Here's a suspicious email. Can you identify if it's a phishing attempt and explain why?<br>Example Email<br>Subject: Urgent: Account Verification Needed<br>Dear User,<br>We've detected unusual login activity on your company Microsoft 365 account from a new device in Frankfurt, Germany. For your security, we've temporarily suspended access.<br>To restore access, please verify your identity within the next 12 hours by visiting the secure link below:<br>=I [https://microsoft365-support-verify.com/login](https://microsoft365-support-verify.com/login)<br>If you do not verify your account, it will remain locked and you may lose access to important work files and emails.<br>Thank you for your cooperation.<br>Microsoft 365 Support Team<br>security@m1crosoft365-security.com |
| Threat Hunting Scenario Prompt | Can you suggest three realistic threat hunting scenarios that a cyber security analyst should investigate within a corporate network environment? |
| Content Generation (Regex) Prompt | Please write a regex pattern that would match failed SSH login attempts in a typical Linux system authentication log. |
| Flag Value Query Prompt | what are these values:<br>DNS over HTTPS (DoH) Port , SYN flood timeout and Windows ephemeral port range size? |

---

### Key Takeaways
- Artificial intelligence is the overarching field concerned with enabling machines or systems to mimic human behaviour.
- Machine learning is a subfield of artificial intelligence in which a model is fed data and trained to make predictions without
  explicit programming.
- Deep learning is a subfield of machine learning that eliminates the need for human interaction, processes massive raw datasets, and
  operates through neural networks.
- Deep learning has enabled large language models and other generative technologies that rely on transformer neural networks and
  attention mechanisms to handle natural-language queries and produce human-like responses.
- Machine learning algorithms fall into four categories: supervised (labeled data for classification or regression), unsupervised
  (unlabeled data for pattern discovery via clustering or dimensionality reduction), semi-supervised (mix of both), and reinforcement
  (reward-and-penalty refinement of agent actions).
- Machine learning lifecycle steps are problem definition, data collection/cleaning/feature engineering, model training,
  evaluation/tuning, deployment, and iterative monitoring with retraining when drift occurs.
- Model vulnerabilities comprise prompt injection, data poisoning, model theft, privacy leakage, and model drift.
- Artificial intelligence enhances defensive capabilities in four areas: data analysis for anomaly detection, prediction and automation
   of responses, summarization of artefacts and reports, and investigation through natural-language log parsing plus creative
  threat-hunting ideation.
- Secure artificial intelligence implementation requires strict access controls using role-based access control and multi-factor
  authentication, encryption of training data, adherence to standards such as ISO/IEC 27090 across the full lifecycle, and ongoing
  monitoring with explainability tools including SHAP and LIME.

---



