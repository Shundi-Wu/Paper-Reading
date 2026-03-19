---
year: 2026
venue: ICLR
citation: 0
tags:
  - diffusion
  - ISR
  - reference
pdf: https://openreview.net/pdf?id=02mgFnnfqG
status: true
---

**LiveMoments: Reselected Key Photo Restoration in Live Photos via Reference-guided Diffusion.** *Clara Xue et al.* **ICLR, 2026 (Citation 0)** [(pdf)](https://openreview.net/pdf?id=02mgFnnfqG)

## TLDR

Introduce new task: **Reselected Key Photo Restoration in Live Photos** and Live Photo Dataset

## Motivation

Live Photo https://support.apple.com/en-sg/104966

### Difficulty

- Key photo is processed through the complete ISP pipeline with enhancement, other frames are degraded by motion blur, etc.

- diffusion-based RefSR produce unnatural textures

### Insight

- a unified Motion Alignment module is introduced to inject the motion guidance
- a comprehensive benchmark consisting of three datasets

## Method

![image-20260317204914109](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260317204914109.png)

### 1. ReferenceNet

$$
\text{Cross\_attn} = \text{Softmax}(\frac{Q[K, K_{ref}]^T}{\sqrt{d}}) [V, V_{ref}]
$$

### 2. Motion Alignment

- **Latent Space:** RAFT(LQ, HQ+Degradation) +  Motion Encoder(**convolutional layers and SiLU activations**)
	$$
	\text{Cross\_attn}_{opt} = \text{Softmax}(\frac{Q[K, K_{ref}+E_{Lo \rightarrow Ls}]^T}{\sqrt{d}}) [V, V_{ref}]
	$$

- **Image Space:** 

	![image-20260317212436865](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260317212436865.png)

	1. estimate whole image flow $ O_{Lo \rightarrow Ls} $
	1. calculate pixel-level average displacement $(\Delta x_i, \Delta y_i) = \left( \frac{1}{p^2} \sum_{j=1}^{p \times p} f_{x_i}^j, \frac{1}{p^2} \sum_{j=1}^{p \times p} f_{y_i}^j \right)$
	1. ref patch $$(\hat{x}_i, \hat{y}_i) = (x_i + \Delta x_i, y_i + \Delta y_i)$$

## Experiment

### Setting 

- Base Model : SD3-medium

#### Training Datasets

- 2K-resolution DL3DV (frame interval 5) -> 50400 image pairs 1024 *1024
- collect 4K-video on internet -> 25000 image pairs 1024 * 1024
- Real-ESRGAN specifically adjusted parameters

#### Test Datasets

- construct a synthesized dataset, **SynLive260 (182 videos -> 260 image pairs+degradation)**, along with two real-world Live Photo datasets, **vivoLive144 (Vivo X200 Pro)** and **iPhoneLive90 (iPhone 15 Pro)**  

- no-reference metrics + relative no-reference form 
	$$
	\text{metric}_{re}(\tilde{I}_{Hs}, I_{Ho}) =  \frac{|\text{metric}(\tilde{I}_{Hs}) − \text{metric}(I_{Ho})|}{\text{metric}(I_{Ho})}
	$$

### Result

![image-20260317215119469](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260317215119469.png)

![image-20260317215204923](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260317215204923.png)

### Ablation

- ReferenceNet

![image-20260317215426199](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260317215426199.png)

- PCR
