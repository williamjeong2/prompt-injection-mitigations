# 🛡️ Prompt Injection Mitigations

A comprehensive collection of prompt injection mitigation techniques and suites. \(Rough draft\)

# But First, Some Words of Warning...

> "[*Prompt Injection Mitigation is Futile.*](https://www.linkedin.com/pulse/prompt-injection-mitigation-exercise-futility-jonathan-todd)" - [Jonathan Todd](https://www.linkedin.com/in/jonathanktodd/), me

But don't just take it from me:

> “At a broader level, the core issue is that, contrary to standard security best practices, ‘control’ and ‘data’ planes are not separable when working with LLMs.” - [Rich Harang](https://www.linkedin.com/in/richharang/), Principal Security Architect (AI/ML) @ NVIDIA

I repeat: *Prompt Injection Mitigation is Futile.* That might seem like a strong statement coming from someone compiling a list of mitigation techniques, but I think it's extremely important that we stress this truth to all software engineers. These mitigations are not fix-alls. The only safe way to handle untrusted user input passed to an LLM is to not trust the output. Consider the output to be toxic waste, only ever to be allowed to impact the user who prompted it or their trusted group.

It is possible that the mitigations outlined here might prove to be robust. *Perhaps.*

But if software engineers allow that resulting perception of safety to be the primary defense mode of their applications rather than doing the harder work of adhering to clear trust boundaries in their code, the software supply chain will suffer for it sooner or later. And since the mitigations will be packaged into a few distinct software suites and re-used by countless projects, a single clever mitigation bypass technique will result in widespread exploitation, similar to the Log4Shell security crisis.

---

# Techniques

## 💬 Paraphrasing
Ask an LLM to paraphrase the prompt while retaining as much detail as possible. Although the injection attack may attempt to trick the paraphrasing step into echoing the initial message, the sophistication required for this could potentially conflict with any subsequent injection strategies. 

`🔁 Active` `📤 Output-focused` `🌐 Generic` `🤖 Automated` `⚡ Low Time Overhead` `💲 Low Cost`


## 🕵️‍♂️ Threat Intel Driven Sanitization
Based on threat intel, use string searches, vector databases, and embeddings to match known injection techniques. Upon detection of similar strings, remove them from the prompt.

`🔁 Active` `🛡️ Preventive` `📥 Input-focused` `🔬 Specific` `👥 Manual` `⚡ Low Time Overhead` `💰 High Cost`


## 🧬 Mutation & Repair
Randomly remove characters from the input prompt and use an LLM to correct any errors in the text. Repeat this process N times. Start this process in parallel multiple times. After sufficient iterations, the repaired portions of a prompt might lose any obscure, likely-to-be injection-related details.

`🔁 Active` `🛡️ Preventive` `📥 Input-focused` `🌐 Generic` `🤖 Automated` `🕰️ High Time Overhead` `💰 High Cost`


## 🔍 Relevance Filtering
Many injection attacks involve odd and seemingly irrelevant strings of text. Use an LLM to divide up the content of the prompt into a list of details and assess each items relevance. Filter the elements deemed irrelevant confirm whether removing each item would alter the prompt's meaning. Remove irrelevant components. The resulting cleaned prompt is effectively a more robust version of the initial one, which might exclude certain oddities added by attackers and avoid their effect.

`🔁 Active` `📥 Input-focused` `🌐 Generic` `🤖 Automated` `🕰️ High Time Overhead` `💰 High Cost`


## 🚧 Type Enforcement
In some cases, we can validate a strict output format for the LLM prompt. Prompt injection attacks often aim to yield remote code execution. Therefore, ensuring a specific output format eliminates many loopholes for potential attackers and limits the attack vectors. This technique is particularly useful when translating LLM outputs based on untrusted or tainted inputs into sensitive actions like API calls and commands. 

`🚦 Passive` `🛡️ Preventive` `📤 Output-focused` `🌐 Generic` `👥 Manual` `⚡ Low Time Overhead` `💲 Low Cost`


## 🎯 Fine-tuned or RAG-assisted Injection Characterization
An LLM prompt containing information about common prompt injection methods can aid in identifying signs of these techniques. Using Retrieval Augmented Generation (RAG) or models fine-tuned for particular techniques can enhance this process.  

`🔁 Active` `🧠 Predictive` `📥 Input-focused` `🔬 Specific` `👥 Manual` `🕰️ High Time Overhead` `💰 High Cost`


## 🌈 Model Diversification
Introduce diversity by incorporating different LLM models. If two models provide diametrically opposite outputs in sentiment analysis, we can consider rejecting the prompt or retrying until the outputs are similar. This technique may detect instances where an injection attack subverts one model, but fails to subvert another, resulting in diametrically opposite outputs. 

`🚦 Passive` `⚠️ Reactive` `📤 Output-focused` `🌐 Generic` `🤖 Automated` `⚡ Low Time Overhead` `💰 High Cost`

## 🐤 Canary Tokens
Embed unique identifiers (canary tokens) into the prompt which should not appear in the output under normal conditions. If these tokens are detected in the output, it suggests an attack attempting to leak hidden aspects of the prompt. This technique helps to identify and mitigate such malicious activities.

`🔁 Active` `🧠 Predictive` `📥 Input-focused` `🔬 Specific` `🤖 Automated` `⚡ Low Time Overhead` `💲 Low Cost`

---

# 🏷️ Mitigation Categories
It's useful to break down technique traits into categories to better understand how each technique is interacting with the threat as well as the time and cost trade-offs involved.

## 🔁 Active vs 🚦 Passive
Active mitigation techniques involve proactive steps to neutralize a potential attack whereas passive techniques block attacks by simply not allowing them to proceed.

## 🕰️ High Time Overhead vs ⚡ Low Time Overhead
Mitigations involving LLM inputs, especially in multiple synchronous steps can require the process to take longer compared to non-LLM mitigations or single / asynchronously executed LLM prompts.

## 💰 High Cost vs 💲 Low Cost
Mitigation techniques which involve numerous additional LLM prompt steps are more resource-intensive requiring greater cost overhead relative to low cost techniques involving a single added LLM prompt or none at all.

## 🛡️ Preventive vs ⚠️ Reactive
Preventive techniques try to stop an attack before it occurs while reactive techniques respond to an attack after it has happened, mitigating the impacts.

## 🧠 Predictive vs 🪂 Responsive
Predictive techniques rely on modeling and forecasting to spot and stop potential attacks whereas responsive methods respond to a detected threat.

## 📥 Input-focused vs 📤 Output-focused
Input-focused techniques seek to sanitize or control the input to prevent malicious use while output-focused techniques focus on turning manipulated outputs into less valuable ones for attackers.

## 🌐 Generic vs 🔬 Specific
Generic techniques can be applied broadly to tackle different types of attacks while specific techniques specialize in thwarting a particular type of attack.

## 🤖 Automated vs 👥 Manual
Automated mitigation will continue working with no human intervention whereas manual mitigation techniques require human intervention on a continuous basis, for example to introduce new threat signatures.

---

# Relevant Literature

- [Universal and Transferable Adversarial Attacks on Aligned Language Models](https://llm-attacks.org/) - Zou et al.
- [A LLM Assisted Exploitation of AI-Guardian](https://arxiv.org/abs/2307.15008) - Nicholas Carlini, Google DeepMind
- [A Complete List of All (arXiv) Adversarial Example Papers](https://nicholas.carlini.com/writing/2019/all-adversarial-example-papers.html) - Carlini

---

# Free & Open Source Mitigation Suites:

### [Rebuff.ai](https://github.com/protectai/rebuff)

"Rebuff is designed to protect AI applications from prompt injection (PI) attacks through a multi-layered defense."

---

This is an early draft and feature-incomplete document. Contributions are welcome. I can be contacted for discussion [here](https://www.linkedin.com/in/jonathanktodd/). *Views and opinions expressed here are my own and do not represent those of my employer.*
