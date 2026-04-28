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
link: /files/llava_improved.pdf
read_time: false
comments: false
share: false
related: false
---

Building upon LLaVA, this work (LLaVA 1.5) achieves significant improvements through simple yet effective design choices: replacing the linear projection layer with an MLP connector, incorporating academic-task-oriented VQA data, and scaling up by utilizing better base models (CLIP-ViT-L and Vicuna v1.5). The result is a strong open-source multimodal model that matches or exceeds several proprietary alternatives on a range of benchmarks, demonstrating that careful data and architecture choices can close the gap with commercial systems.
