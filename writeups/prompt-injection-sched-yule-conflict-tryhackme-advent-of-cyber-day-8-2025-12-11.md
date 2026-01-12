# Agentic AI Hack 

---
## The evolution from static language prediction to independent decision-making systems 
defines the shift to Agentic AI. Traditional Large Language Models (LLMs) are restricted to processing and generating text based on 
their fixed training corpus, which makes them susceptible to classic weaknesses like fact hallucination, outdated knowledge, and 
prompt injection. Agentic AI, conversely, possesses agency: the ability to autonomously plan multi-step processes, execute external 
actions via tools and APIs, and adapt its strategy based on real-time observations, demanding a new security focus on workflow 
integrity and tool interaction.

---
## This autonomy is facilitated by advanced prompt engineering techniques such as ReAct (Reason + Act). 
ReAct overcomes the shortcomings of static Chain-of-Thought (CoT) reasoning—which operates exclusively within the model’s internal 
knowledge—by forcing the model to interleave verbal reasoning traces with explicit, external actions. This sequence of Thought → 
Action → Observation → Refinement allows the agent to dynamically ground its decisions in reality, integrate real-time data, and 
significantly reduce logical errors and factual inconsistencies.

---
## The system's reliance on structured Tool Use—where developers register external functions (e.g., `reset_holiday`, `get_logs`) 
that the LLM infers and calls—introduces novel attack vectors. The critical security risk arises when the agent's internal reasoning 
process, or CoT log, is accessible or can be manipulated. Attackers exploit this by using carefully crafted prompts to coerce the 
agent into revealing sensitive information from its execution context. This form of token leakage can involve tricking the agent 
into listing its own available functions or, more critically, forcing it to execute internal logging functions (e.g., `calling get_logs`) 
and then outputting hidden parameters, such as the required authorization token (`TOKEN_SOCMAS`), which are typically 
intended to be protected internal credentials. Successful exploitation allows an adversary to bypass application-level policy 
checks and execute restricted, high-privilege administrative functions.

---

>[!Note]
>### You did it! Wareville is one step safer.
>The townsfolk are counting on you to keep Christmas secure.
>Head back to Wareville to continue your mission!


