---
layout: post
title: LLM4TS: Two-Stage Fine-Tuning for Time-Series Forecasting with Pre-Trained LLMs
date: 2023-05-18 00:13:14
description: 
tags: note
categories: fsc-note
tabs: true
---

# LLM4TS: Two-Stage Fine-Tuning for Time-Series Forecasting with Pre-Trained LLMs

## Method

![Overall Architecture](/assets/img/pic/llm4ts/structure.jpg)

## Time-Series Alignment
Perform autoregressive processing on time-series data to analyze the relationship between inputs and outputs.

### Instance Normalization
Normalize the entire data instance, which is part of the data preprocessing.

### Time-Series Tokenization
A typical data processing method using channel-independent strategies combined with patching.
![Time-Series Tokenization Diagram](/assets/img/pic/llm4ts/patch.jpg)

### Three Encodings for Patched Time-Series Data

- **Token Embedding Layer**: Use a 1D convolutional layer as the token embedding layer.
- **Positional Layer**: Use a trainable lookup table to identify the position of each patch.
- **Temporal Embedding Layer**: Achieve dual-level aggregation.
  - First, use a trainable lookup table for each temporal attribute (e.g., second, minute) to map each attribute to a high-dimensional space, then sum these mappings to form a unified temporal embedding representation.
  - Each patch may contain multiple timestamps, and pooling methods are used to extract the final temporal embedding, usually by taking the first timestamp in the patch as the representative of the entire patch.

### Partial Freezing and Tunable Layers
Freeze certain parts of the model while keeping some layers trainable to optimize model performance.
- **Layer Normalization Tuning**
- **Low-Rank Adaptation (LoRA)**

## Forecasting Fine-Tuning Strategy
Adopt the initial linear probing followed by full fine-tuning (LP-FT) strategy, whose superiority lies in its dual-phase approach:
- First, optimize the output layer to minimize adjustments during fine-tuning (preserving the effectiveness of the feature extractor in OOD (Out-Of-Distribution) scenarios).
- Then conduct full fine-tuning to adapt the model to specific tasks (improving ID (In-Distribution) accuracy).

