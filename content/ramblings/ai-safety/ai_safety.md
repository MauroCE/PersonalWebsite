---
title: Literature Review
linktitle: Sets
toc: true
type: docs
date: "2024-06-04T00:00:00+01:00"
draft: false
menu:
  ai-safety:
    parent: 1) Usure
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

1. [An AI System Evaluation Framework for Advancing AI Safety:
Terminology, Taxonomy, Lifecycle Mapping](https://arxiv.org/abs/2404.05388): Tries to bring together the AI, Software Engineering and Governance communities by creating a common terminology, a taxonomy of components and actions on AI systems and how these relate to the life-cycle of such a system.
2. [Managing extreme AI risks amid rapid progress](https://arxiv.org/abs/2310.17688). Progress in AI development is very fast, but only 1-3% of AI publications are on Safety and we cannot wait decades as for climate change. AI systems are hard to control and understand and have often shown emerging abilities that can be deceiving. Prompts governments, companies and research institutions to re-orient. Governments should have enforceable consequences **if-else** to avoid corner-cutting. **White-box audit** should become the standard, and key companies should **invest more** in AI safety. Some R&D challenges below:
Some R&D challenges
   - Honesty (AI systems can cheat to reach an objective)
   - Robustness (unpredictability in new situations)
   - Interpretability/Transparency
   - Evaluating emerging cpabilities and AI alignment
   
3. [Black-Box Access is Insufficient for Rigorous AI Audits](https://arxiv.org/abs/2401.14446): AI Auditing can happen in 4 ways:
    - Black box: Can design an input, query the system and analyze output.
    - Grey box: Limited access to inner workings (e.g. embeddings, activations, logits). 
    - White box: Full access to the system (e.g. weights, gradients, and fine-tuning)
    - Outside-the-box: Info about dev and deployment of the system (e.g. docs, data, code)
    
    Limitations of black-box access include: limited understanding of the model, components can't be studied separately, can produce misleading results (e.g. affected by Auditor bias). Here is a cool table describing the type of attacks/evaluation techniques that can be done by Auditors with different access. In Table 2 of the paper they list various ways in which white-box access allows for more thorough and safer auditing.
    ![some](/table_audit.png)
  