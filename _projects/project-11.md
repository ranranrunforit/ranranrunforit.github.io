---
title: "TinyLLaMa"
excerpt: "Implemented LLaMa model loading and training. <br/><img src='/images/Llama.png'>"
collection: projects
date: 2024-11-22
category: python
description: "Implemented LLaMa model loading and training."
tags:
  - LLM
  - LlaMa
  - Attention
  - RMSNorm
  - Python
  - PyTorch
  - tqdm
  - RoPE
  - Adam
---

## Llama2 Implementation

This project focuses on implementing the Llama2 model, which utilizes a modified attention mechanism known as Grouped-Query Attention. The main components of the Llama2 architecture include:

**LlamaLayer**: Each transformer layer consists of:
   - Root Mean Square (RMS) layer normalization.
   - Self-attention mechanism with a residual connection.
   - Feed-forward network with another residual connection.

**Llama Model Structure**:
   - An embedding layer for token representations.
   - A stack of `LlamaLayer` instances based on the specified number of hidden layers.
   - A projection layer for predicting token IDs.
   - A generation function that employs temperature sampling for text continuation without nucleus/top-k sampling.

### Implementation Tasks:
The project includes several components marked with `#todo`, which require implementation:
- Attention mechanism.
- RMS normalization.
- Forward pass for the Llama model.
- Generation function.
- Rotary positional embeddings.
- AdamW optimizer with decoupled weight decay.

### Additional Components:
- **LlamaSentClassifier**: A class for encoding sentences and classifying them using the Llama2 model.
- **Optimizer**: Implementation of the AdamW optimizer with specific adaptations for weight decay and bias correction.

**[Build-my-own LLaMa (Live Preview)](https://github.com/ranranrunforit/tinyllama/blob/main/tinyLlama.ipynb)**

**Keywords**: LLM, LlaMa, Attention, RMSNorm, RoPE, Adam
**Packages**: Python, PyTorch, tqdm
