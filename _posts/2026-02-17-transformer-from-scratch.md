---
title: "Implementing a Transformer Encoder From Scratch"
date: 2026-02-17
categories: research
layout: single
tags:
  - transformers
  - pytorch
  - implementation
author_profile: true
excerpt_separator: "<!--more-->"
---

Re-implementing the Transformer encoder from *Attention Is All You Need* was one of the most instructive deep learning exercises I’ve done so far.

Reading the paper gives conceptual clarity.  
Implementing it builds architectural intuition.

<!--more-->

---

## Transformer Encoder Architecture

![Transformer Encoder Architecture](/images/attention.webp)

The encoder replaces recurrence with attention mechanisms and feed-forward networks, stacked with residual connections and normalization layers.

---

## Core Attention Mechanism

At the heart of the model lies scaled dot-product attention:

$$
Attention(Q,K,V)=softmax\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

That small scaling term \( \sqrt{d_k} \) is critical.

Without it:
- Dot products grow large  
- Softmax saturates  
- Gradients vanish  
- Training becomes unstable  

What looks like a minor detail in the paper turns out to be essential in practice.

---

## What I Implemented From Scratch

Instead of using `torch.nn.Transformer`, I manually built:

- Token embedding layer  
- Sinusoidal positional encoding  
- Scaled dot-product attention  
- Multi-head attention  
- Feed-forward network  
- Residual connections  
- Layer normalization  
- Encoder stacking  
- Custom training loop  

The goal was not just replication, but understanding system behavior.

---

## The Real Difficulty: Tensor Shapes

Multi-head attention requires reshaping tensors carefully:

From:

(batch_size, seq_len, d_model)

To:

(batch_size, num_heads, seq_len, head_dim)

A single incorrect `view()` or `transpose()` silently corrupts attention computation.

Most debugging time was spent here.

---

## Masking Is Subtle

Incorrect masking does not crash the model.  
It quietly damages training.

Proper broadcasting across:

- Batch dimension  
- Attention heads  
- Sequence length  

is crucial.

---

## Architectural Observations

After stacking multiple encoder layers:

- Representation depth increased significantly  
- Different attention heads captured distinct token relationships  
- Training stability depended heavily on residual + LayerNorm structure  

The architecture is elegant — but fragile if implemented carelessly.

---

## Key Takeaways

- Mathematical details directly affect optimization stability  
- Manual implementation exposes hidden architectural dependencies  
- Attention mechanisms require careful tensor reasoning  
- Reading papers builds vocabulary; implementation builds intuition  

---

## Code

Full implementation available here:

https://github.com/devesssi/dl-playground/blob/main/transformer-implementation
