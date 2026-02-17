---
title: "Attention Is All You Need — Transformer Encoder Implementation"
collection: publications
permalink: /research/attention-transformer/
layout: single
author_profile: true
---

## Overview

This project re-implements the core components of the Transformer encoder architecture introduced in *Attention Is All You Need* (Vaswani et al., 2017). The objective was to reconstruct the attention mechanism and encoder stack from first principles in PyTorch to understand training dynamics, architectural behavior, and gradient stability beyond high-level abstractions.

---

## Motivation

Transformers replaced recurrence with attention, enabling parallel sequence modeling and large-scale training. Rather than using `torch.nn.Transformer`, I implemented the core modules manually to:

- Understand attention computation mathematically  
- Examine the role of scaling in gradient behavior  
- Study how residual connections and LayerNorm stabilize training  
- Observe attention weight distributions across heads  

---

## Architecture Implemented

The following modules were implemented from scratch:

- Token Embedding Layer  
- Sinusoidal Positional Encoding  
- Scaled Dot-Product Attention  
- Multi-Head Attention  
- Feedforward Network (2-layer MLP)  
- Residual Connections  
- Layer Normalization  
- Encoder Block  
- Stacked Encoder  

<details>
  <summary>Implementation Structure</summary>

  - `MultiHeadAttention` class  
  - `ScaledDotProductAttention` function  
  - `PositionalEncoding` module  
  - `EncoderLayer` module  
  - `TransformerEncoder` stack  
  - Custom training loop  

</details>

---

## Attention Computation

The attention mechanism is computed as:

$$
Attention(Q,K,V)=softmax\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

The scaling factor \( \sqrt{d_k} \) prevents large dot products from pushing the softmax function into saturation, improving gradient stability during training.

---

## Positional Encoding

Since the architecture lacks recurrence, sinusoidal encodings inject sequence order:

$$
PE_{(pos,2i)} = \sin\left(\frac{pos}{10000^{2i/d_{model}}}\right)
$$

This enables the model to generalize to sequence lengths beyond those seen during training.

---

## Training Evaluation

The encoder was trained on a synthetic sequence task to validate learning behavior. Observations included:

- Stable convergence when scaling factor was applied  
- Gradient instability when masking was incorrectly broadcast  
- Distinct attention distribution patterns across heads  
- Improved representation depth with stacked layers  

---

## Architectural Observations

- Increasing depth improved representation capacity but required careful initialization.  
- Masking errors significantly affected gradient flow.  
- Multi-head attention enabled diverse contextual relationships across tokens.  

---

## Challenges

- Managing tensor reshaping across multiple attention heads  
- Ensuring correct masking behavior  
- Debugging shape mismatches in batched matrix multiplication  

---

## Key Learnings

- Attention scaling is critical for stable optimization  
- Residual connections + LayerNorm are essential for deep stacking  
- Manual implementation reveals architectural trade-offs hidden by high-level APIs  

---

## Code

GitHub Repository:  
https://github.com/devesssi/dl-playground/blob/main/transformer-implementation
