---
title: "AROMMA"
excerpt: "Aggregated Representation of Olfaction via Molecule and Mixture Alignment<br/> <strong>#KnowledgeDistillation #Psuedo-labeling #ICASSP2026</strong>"
collection: portfolio
---

Code: [https://github.com/DGIST-Distributed-AI-Lab/aromma](https://github.com/DGIST-Distributed-AI-Lab/aromma)

![](../../images/aromma-space.png)

AROMMA is a novel framework that learns a **unified embedding space for both single molecules and binary mixtures**, providing a generalized representation for olfactory perception.

The framework leverages a large-scale **chemical foundation model (e.g., SPMM)** for robust molecular representations, incorporates an **attention-based aggregator** to model permutation invariance and asymmetric interactions, and **addresses label sparsity** through knowledge distillation and pseudo-labeling.

AROMMA achieves **state-of-the-art** performance on both GS-LF (single-molecule) and BP (binary-mixture) benchmarks, with up to 19.1% improvement in AUROC, demonstrating effective bidirectional knowledge transfer and strong generalization ability.

## Architecture Overview
![](../../images/aromma-architecture.png)

### Embedder
- Individual molecular embeddings are obtained using the SMILES encoder of SPMM.
- SPMM is pre-trained on approximately 50 million molecules, providing robust molecule representations.
- The model is fine-tuned efficiently using LoRA (Low-Rank Adaptation).

### Aggregation Module
- The input consists of SPMM embeddings $$E \in \mathbb{R}^{2\times d_e}$$ (zero-padded in the case of a single molecule).
- The embeddings pass through a linear projection followed by a ReLU activation, producing $$E'$$.
- A masked multi-head self-attention layer generates $$H$$, which represents contextualized molecular embeddings.
- The contextualized representations $$H$$ are used as keys and values in a cross-attention layer, together with a global learnable query $$q \in \mathbb{R}^{1 \times d'_h}$$.
- Finally, Layer Normalization is applied to obtain the global embedding $$z$$.

## Training Strategy
### Loss Function
- Initial training is conducted on the unified 152-descriptor set, where the 78 missing BP labels are treated as zeros.
- For single molecules, we combine:
    - Multi-Label Logit Distillation (MLD) loss for knowledge distillation
    - Binary Cross-Entropy (BCE) loss for supervised learning
    - The POM, a state-of-the-art model for single-molecule odor prediction, serves as the teacher to alleviate label sparsity.
- For binary mixtures, only the BCE loss is used.

The total loss balances single-molecule and mixture objectives with loss balancing coefficient.

### Latent Label Augmentation & Re-training
To address label sparsity and incompleteness in the BP dataset, we apply class-aware pseudo-labeling.
- We first compute the positive label ratio for each class $$c$$ in GS-LF $$\gamma_c = \frac{1}{n} \sum_{j=1}^n \mathbf{1}(y_{jc}=1)$$.
- For each class $$c$$, we select a threshold $$\tau_c$$ such that the proportion of predicted probabilities exceeding the threshold matches the class prior $$\frac{1}{n} \sum_{j=1}^n \mathbf{1}(p_{jc} \ge \tau_c)=\gamma_c$$.
- Using this strategy, we construct two augmented datasets Pseudo-78 and Pseudo-152. The model is then re-trained as AROMMA-P78 and AROMMA-P152.

![](../../images/aromma-results.png)
