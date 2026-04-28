---
title: 'Visual Instruction Tuning'
date: 2023-04-17
permalink: /posts/2023/04/visual-instruction-tuning/
categories:
  - readings
tags:
  - multimodal
  - instruction-tuning
  - llava
read_time: true
comments: false
share: false
related: false
---

*Paper: [Visual Instruction Tuning](https://arxiv.org/abs/2304.08485) (LLaVA, NeurIPS 2023) — [PDF](/files/llava.pdf)*

## Motivation

Large language models have demonstrated remarkable instruction-following capabilities, but these have been largely confined to the text domain. LLaVA asks: can we extend instruction tuning to the multimodal setting, creating a general-purpose visual assistant that can follow natural language instructions about images?

## Method

The core idea is **visual instruction tuning** — generating instruction-following data from image-text pairs using GPT-4/ChatGPT, then fine-tuning a vision-language model on this data.

**Data generation pipeline:**
1. Start with image-caption pairs (from COCO)
2. Feed the caption and image metadata (bounding boxes, object labels) to GPT-4
3. Prompt GPT-4 to generate diverse conversation data: questions, answers, detailed descriptions, and reasoning traces
4. This yields ~158K instruction-following samples spanning conversation, detailed description, and complex reasoning

**Model architecture:**
- Vision encoder: pre-trained CLIP ViT-L/14
- Language model: Vicuna (fine-tuned LLaMA)
- A single linear projection layer connects vision features to the language embedding space
- Only the projection layer is trained in stage 1; both the projection and LLM are fine-tuned in stage 2

## Key Insights

1. **Language-only instruction data is insufficient.** Simply using text-based instruction data degrades performance — multimodal instruction data is essential.

2. **GPT-4 generated data works remarkably well.** Despite being synthetic, the instruction-following data produces a capable assistant that can reason about images, answer follow-up questions, and even refuse to answer unanswerable queries.

3. **Simple architecture, strong results.** A single linear projection between CLIP and Vicuna is enough — no complex fusion mechanisms needed. The model achieves ~92% of GPT-4's performance on a multimodal instruction-following benchmark while being fully open-source.

4. **Two-stage training is important.** Pre-training the projection layer on image-caption alignment before fine-tuning on instruction data prevents catastrophic forgetting and leads to better convergence.

## Thoughts

LLaVA is an elegant demonstration that the instruction-tuning paradigm transfers well to multimodal settings. The data generation trick — using a strong LLM to convert existing image annotations into diverse instruction data — is cost-effective and scalable. The simplicity of the architecture is also a strength; it shows that with good data, even a linear projector suffices.

One limitation worth noting: the visual understanding is bottlenecked by the CLIP encoder, which has known failure modes (e.g., spatial reasoning, counting). Improving the vision backbone would likely yield significant gains.
