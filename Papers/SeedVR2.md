---
year: 2026
venue: ICLR
citation: 11
tags:
  - diffusion
  - VSR
  - adversarial
pdf: https://arxiv.org/pdf/2506.05301
status: false
---

**SeedVR2: One-Step Video Restoration via Diffusion Adversarial Post-Training.** *J Wang et al.* **ICLR, 2026 (Citation 11)** [(pdf)](https://arxiv.org/pdf/2506.05301)

## TLDR

## Motivation

### Difficulty

- nowadays, one-step diffusion model rely on distillation from a pre-trained teacher model, suffering from an undesired upper bound constrained by the teacher model.
- a performance drop when handling heavy degradations; training unstable; compute LPIPS loss unaffordable

### Insight

- one-step video restoration via adversarial training without frozen priors, focusing on the loss function and progressive distillation (APT, **Diffusion adversarial post-training for one-step video generation.** ICML, 2025)
- propose an adaptive window attention mechanism - dynamically adjust the window size

## Method

![image-20260303133855362](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260303133855362.png)

