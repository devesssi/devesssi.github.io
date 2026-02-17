---
title: "Transformer Encoder — Detailed Notes"
excerpt_separator: "<!--more-->"
---

Personal technical notes while implementing the Transformer encoder.

<!--more-->

## Scaled Dot Product

The attention mechanism computes:

$$
Attention(Q,K,V)=softmax\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

Key observation:
Scaling stabilizes gradients when dimension increases.

---

## Multi-Head Attention

Each head learns different relational structures.
Parallel projections increase representation capacity.
