---
title: "AROMMA Agent"
excerpt: "Bidirectional olfactory AI converter (olfaction-language)<br/> <strong>#LangChain #RAG</strong>"
collection: portfolio
---

Code: [https://github.com/DGIST-Distributed-AI-Lab/aromma-agent](https://github.com/DGIST-Distributed-AI-Lab/aromma-agent)

AROMMA Agent is an LLM-powered odor prediction and recommendation system built on top of AROMMA (ICASSP 2026).

It supports the following capabilities:
- Odor prediction from a molecule or a binary mixture given SMILES strings
- Molecule recommendation for composing a target scent described in natural language

## Olfaction to Language: odor prediction
When a user requests the scent of a specific molecule or mixture, the trained AROMMA predicts odor descriptors, and the large language model transforms them into natural-language sentences.

![](../../images/aromma-agent-prediction.png)

## Language to Olfaction: molecule recommendation
When a user enters a description of a desired scent, the large language model selects suitable labels from 152 odor descriptors. It then searches the database to identify and present molecules that produce that scent.

![](../../images/aromma-agent-recommendation.png)
