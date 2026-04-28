---
title: 'Improved Baselines with Visual Instruction Tuning'
date: 2023-10-05
permalink: /posts/2023/10/improved-baselines-visual-instruction-tuning/
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

*Paper: [Improved Baselines with Visual Instruction Tuning](https://arxiv.org/abs/2310.03744) (LLaVA 1.5, CVPR 2024) — [PDF](/files/llava_improved.pdf)*

## Motivation

LLaVA demonstrated that visual instruction tuning works. But the original model lagged behind proprietary systems on standard benchmarks. This follow-up asks: how much of that gap can be closed through better data and design choices, without fundamentally changing the architecture?

## Improvements

LLaVA 1.5 introduces several targeted changes, each ablated carefully:

1. **MLP connector replaces linear projection.** A two-layer MLP with GELU activation replaces the single linear layer connecting vision and language features. This small change consistently improves performance across all benchmarks.

2. **Academic-task-oriented VQA data.** The training mixture is augmented with task-specific VQA datasets (VQAv2, GQA, OKVQA, OCR-VQA, etc.), formatted as instruction-following conversations. This injects structured visual knowledge that synthetic GPT-4 data alone doesn't capture.

3. **Better base models.** CLIP-ViT-L/14 is upgraded to CLIP-ViT-L/14@336px (higher resolution), and Vicuna v0 is replaced with Vicuna v1.5 (trained on more and better data).

4. **Simple scaling.** Training resolution is increased to 336×336, and the model is trained for a single epoch on the expanded dataset mix (~665K samples).

## Key Results

| Benchmark | LLaVA | LLaVA 1.5 | Improvement |
|-----------|-------|-----------|-------------|
| MMBench | 36.2 | 64.3 | +28.1 |
| MME | 502 | 1511 | +1009 |
| SEED-Bench | 25.4 | 58.6 | +33.2 |
| POPE | 49.9 | 85.9 | +36.0 |

On several benchmarks, LLaVA 1.5 matches or exceeds the performance of GPT-4V and other proprietary systems, despite being fully open-source and significantly smaller.

## Key Insights

1. **Data diversity matters more than data scale.** The biggest gains came from adding diverse VQA datasets formatted as conversations — not from generating more synthetic data.

2. **Architecture tweaks compound.** The MLP connector alone helps modestly, the VQA data alone helps more, but together with resolution scaling they produce dramatic gains. Ablation studies show the effects are largely orthogonal and additive.

3. **Simple recipes win.** No architectural redesign, no novel training objectives, no RLHF. Just better data curation, a slightly better connector, and scale. This is a testament to how much low-hanging fruit remains in multimodal model design.

## Thoughts

LLaVA 1.5 is a refreshingly honest paper — no tricks, just careful engineering. The message is clear: the multimodal community should spend more effort on data quality and less on architectural novelty. The strong open-source performance also makes LLaVA 1.5 a practical baseline for future work.

My main critique: the paper doesn't deeply analyze *why* the MLP connector helps. Is it strictly about capacity, or does the nonlinearity enable better feature alignment? A probing analysis would have been valuable.
