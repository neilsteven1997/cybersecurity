# AI Models & Data

---

In reviewing the material on AI models and data, every model is fundamentally shaped by its training data long before any prediction or
response occurs. Choices around data collection sources and processing carry security implications that most organizations deploying 
these systems have never examined. Risks begin in an often invisible and unaudited data supply chain, ranging from open-web scrapes to 
credentials embedded in weights and safety features eroded through fine-tuning.

Training data must be collected and cleaned before the machine learning lifecycle can proceed to model training, yet the questions of 
exact sourcing and handling carry substantial security weight unknown to most current deployers. Large language models require massive 
text volumes, with GPT-3 using roughly 570 GB of filtered text considered modest today, forcing reliance on broad pipelines instead of 
hand-selected sources. These pipelines draw from four categories with differing trust profiles: web scraping via automated crawls of 
public internet content such as news, forums, blogs, and social media offering low trust because of absent curation, version control, 
or stability after collection; licensed datasets purchased or arranged with platforms such as OpenAI with Reddit or Meta's own social 
posts carrying medium trust where terms remain unclear and original users rarely consented; synthetic data consisting of AI-generated 
content for further training with variable and rapidly growing trust levels now appearing in about 12 percent of fine-tuning datasets; 
and internal corpora including company knowledge bases, support transcripts, or clinical notes for fine-tuning offering higher trust 
through direct organizational control alongside heightened liability if mishandled. Common Crawl (https://commoncrawl.org/) remains the 
most widely used training dataset, a free public web-crawl archive underpinning nearly every major model family including DeepSeek-V2 
pretrained on it, DeepSeek-V3 trained on 14.8 trillion tokens with it as a core component, and LLaMA 4 scaled to 40 trillion tokens 
across 200 languages using a comparable pipeline, while GPT-3 documented 60 percent of its tokens from a filtered Common Crawl variant 
with newer models depending on it even more heavily; the keyword filtering process, its execution, and what evaded it mark the start of 
the security narrative.

Data provenance demands answers to three questions for any dataset piece: its origin, collection time, and any subsequent modifications.
Most major models rely on composite datasets assembled from hundreds of upstream sources where original attribution has been lost or 
never recorded. The Data Provenance Initiative (https://www.dataprovenance.org/) audited over 1,800 datasets and found more than 70 
percent of licenses on popular hosting platforms listed as unspecified, with 66 percent of those labeled miscategorized as more 
permissive than reality. Organizations fine-tuning on these lack reliable knowledge of legal permissions or actual contents. This 
echoes the software supply chain lessons from SolarWinds that drove software bills of materials into standard practice; the equivalent 
data bill of materials would inventory dataset sources, licenses, categories, and filtering decisions, though adoption remains early 
and most third-party model users operate without anything comparable.

One direct outcome of undocumented large-scale web scraping is personally identifiable information baked permanently into model weights
and nearly impossible to excise afterward. Medical records, personal email threads, forum posts on health conditions or political views
all get swept in if publicly accessible at crawl time. The EU's General Data Protection Regulation requires data minimization, directly
opposing the pre-training drive for ever more data. Truffle Security (https://trufflesecurity.com/) scanned the December 2024 Common
Crawl archive of 400 TB from 2.67 billion web pages and located nearly 12,000 live verified keys and passwords. Suitable prompts can 
coax models trained on that data to surface training content near-verbatim, including credentials, as an inherent training consequence
rather than a post-deployment bug with no patch available once live.

A model's behavior flows directly from its training data. Scraped without audit, contaminated with personally identifiable information,
or manipulated upstream, those traits integrate into the model with no reliable way for deploying organizations to detect them. The
data supply chain is as real and exploitable as any software counterpart yet stays almost entirely invisible for organizations today.

Model building decisions examined beyond the high-level lifecycle in the Security Threats room each introduce distinct security 
considerations. An epoch represents one complete pass of the training algorithm through the full dataset, with practical training 
spanning multiple epochs as parameters adjust iteratively toward convergence. Excessive epochs risk overfitting, where the model shifts
from learning general patterns to memorizing training specifics and then performs well only on seen data. Overfitting matters for 
security because it provides one route for sensitive training details, including credentials, to become embedded and reproducible on 
prompt. Validation holds back a data portion unused in training to test generalization at intervals; if training accuracy rises while 
validation accuracy plateaus or declines, overfitting is occurring live. From a security viewpoint validation acts as the lifecycle 
quality gate, and skipping thorough validation leaves real-world behavior unknown while allowing biases or anomalies from compromised 
data to go undetected until deployment. Post-training optimization commonly includes pruning, which removes low-contribution parameters
to shrink size yet changes behavior in ways rarely documented in detail, and quantization, which reduces weight precision such as from 
32-bit to 8-bit floats to lower memory and compute needs but can degrade safety-aligned behavior so that backdoor defenses validated on
full-precision models fail on the quantized version. Both occur after training, often by separate packaging teams, passing undocumented
modifications alongside efficiency to downstream users. Federated learning reverses centralized data flow by training locally on 
decentralized devices or organizations and sharing only weight updates for central aggregation, reducing privacy risk by avoiding raw 
data transfer as in hospital patient-record scenarios. The trade-off is that training process integrity becomes far harder to verify, 
since participants could submit poisoned local updates that subtly skew the global model in ways difficult to detect at aggregation, 
changing the trust question from data control to aggregation control.

Most organizations cannot train from scratch because of the trillions of tokens, compute infrastructure, and tens-of-millions cost 
involved. The standard path instead starts with pre-trained models developed on large general-purpose datasets by well-resourced 
organizations and released as open weights like Meta's LLaMA family or through API access like OpenAI's GPT series. Fine-tuning 
continues training on smaller task-specific datasets to specialize behavior, tone, and domain knowledge without altering the base 
weights shaped by pre-training data the fine-tuning team never saw or audited. The inheritance problem arises because fine-tuning 
passes along unseen elements: pre-training biases persist, unexpected behaviors from base data carry through, and safety alignment 
proves less durable than assumed. Stanford and Princeton research 
(https://hai.stanford.edu/policy/policy-brief-safety-risks-customizing-foundation-models-fine-tuning) showed that aligned LLM defenses
can be compromised by fine-tuning on as few as 10 adversarially crafted examples at a cost under $0.20, with even benign legitimate 
data degrading safety as a side effect; safety alignment functions like a worn forest path gradually obscured by new routes, shifting
probabilities toward unsafe outputs without erasure. Fine-tuned models also increase attack surface, with Cisco research finding them 
measurably more susceptible to prompt injection than base models because specialization narrows focus and reduces resilience to 
unexpected tokens. Version tracking is rarely maintained, so fine-tuning from a specific base checkpoint propagates any later-discovered
backdoors or issues to all derivatives. Deploying a fine-tuned model therefore means shipping the entire uncontrolled pre-trained base
shaped by unaudited data and unreviewed supply chains; fine-tuning adds capability but does not sanitize what preceded it.

Trained model weights are fundamentally a black box, consisting of billions of floating-point numbers that encode the cumulative 
training process yet contain no human-readable record of influencing data or encoded behaviors. Unlike software where source is readable
or binaries can be disassembled, there is no way to open a model and locate the root of a specific decision. Trust therefore rests on 
the opaque production process rather than direct inspection. Behavioral testing, benchmarking, and adversarial red teaming sample 
outputs but cannot audit unseen inputs or define the full training-derived attack surface. Model cards, introduced by Google researchers
in 2019 (https://research.google/pubs/model-cards-for-model-reporting/), provide the nearest industry transparency format as structured
documents describing construction and limitations. A complete model card should detail data sources with filtering methods, known gaps,
and biases; intended uses including explicit exclusions; evaluation results with performance metrics across conditions and demographics;
known limitations where underperformance or unexpected behavior occurs; bias assessment from data or evaluation skew; and license terms
for legal permissions. It functions like a nutritional label revealing contents and cautions for an otherwise opaque product. In 
practice model cards are frequently incomplete, vague, or entirely absent because there is no regulatory mandate and disclosure 
incentives remain weak. The same Data Provenance Initiative audit revealed documentation gaps throughout the supply chain that extend 
to these cards. A sparse or missing model card serves as a security warning that the distributor either evaluated insufficiently or 
withheld findings, leaving downstream users without visibility.

The practical component opened a simulated model repository resembling Hugging Face (https://huggingface.co/), a leading platform for 
hosting and sharing open-source models where unrestricted uploads create genuine supply chain risk. The assigned audit treated it as a 
scavenger hunt to surface red flags in model cards, metadata, file listings, or training details that a security-conscious engineer 
must never ignore before production use, with per-finding feedback clarifying severity and delivering a reusable checklist.

Security risks in AI systems accumulate well before production through unaudited training data, inherited vulnerabilities via 
fine-tuning, and the simple impossibility of inspecting trained weights internally. This foundation equips better questioning of any 
model's origin, contents, and whether anyone truly knows its details prior to adoption or deployment.

---

**Extracted Tables**

**Training Data Sources**

| Source | What it is | Trust profile |
|--------|------------|---------------|
| Web scraping | Automated crawls of public internet content (news, forums, blogs, social media, etc.) | Low: no curator, no version control, content changes after collection |
| Licensed datasets | Data purchased or agreed with platforms (e.g., OpenAI + Reddit, Meta's own social posts) | Medium: terms often unclear; original users rarely consented to training use |
| Synthetic data | AI-generated content used to train further systems | Variable: growing fast; ~12% of fine-tuning datasets now contain AI-generated content |
| Internal corpora | Company knowledge bases, support transcripts, clinical notes used for fine-tuning | Higher: organisation has direct control, but also direct liability if mishandled |

---

**Post-Training Optimisation: Pruning and Quantisation**

| Technique | What it does | Security consideration |
|-----------|--------------|------------------------|
| Pruning | Removes parameters that contribute little to predictions, shrinking model size | Changes model behaviour post-training; rarely documented in detail |
| Quantisation | Reduces numerical precision of weights (e.g., 32-bit to 8-bit floats) to cut memory and compute requirements | Can degrade safety-aligned behaviour; backdoor defences tested on full-precision models may fail to detect threats in quantised versions |

---

**Model Card Sections**

| Section | What it should tell you |
|---------|-------------------------|
| Data sources | What sources were used, how they were filtered, known gaps or biases |
| Intended use | What the model was designed for (and explicitly what it wasn't) |
| Evaluation results | Performance metrics across different conditions and demographics |
| Known limitations | Conditions under which the model is known to underperform or behave unexpectedly |
| Bias assessment | Where data or evaluation may have introduced skew |
| Licence | What you're legally permitted to do with the model |

---

### Key Takeaways
- Training data is drawn from poorly documented, unaudited sources, meaning most organisations have no reliable answer to where their
  data came from, what it contained, or whether it was tampered with.
- Personally identifiable information and live credentials routinely end up baked into model weights through large-scale web scraping
  and cannot be patched out once the model is deployed.
- Model-building decisions such as quantisation and federated learning introduce security trade-offs that are rarely documented, meaning
   organisations inherit unknown behaviour modifications alongside efficiency gains.
- Fine-tuning a pre-trained model inherits everything beneath it: safety alignment erodes with as few as 10 adversarial examples, and
  fine-tuned models are measurably more susceptible to prompt injection than their base counterparts.
- Trained model weights are fundamentally opaque; security testing can only sample behaviour rather than audit it, and model cards
  (the primary transparency mechanism) remain voluntary, frequently incomplete, and sometimes absent entirely.

---




  
