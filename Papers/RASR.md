---
year: 2025
venue: arXiv
citation: 0
tags:
  - diffusion
  - ISR
  - RAG
pdf: https://arxiv.org/pdf/2508.09449
status: true
---
**RASR: Retrieval-Augmented Super Resolution for Practical Reference-based Image Restoration**. *J Yan et al.* **arxiv, 2025 (Citation 0) ** [(pdf)](https://arxiv.org/pdf/2508.09449)

## TLDR

Create RASR-Flicker30 Dataset, use normal [[ControlNet]] method. 

Lack of support in setting and comparison. e.g. Inference setting without ablation, WR-SR comparison.

**Question:**

- How to read result (Figure 1)

## Motivation

### Difficulty

- their reliance on manually curated target reference image pairs

### Insight

- **database:** category-specific reference data - RASR-Flicker30

## Method

<img src="https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260224223159439.png" alt="image-20260224223159439" style="zoom: 200%;" />

### 1. RASR Network

- **RAG - vision encoder:** select high-level semantic feature (DINOv2) -> embeddings -> cosine similarity

- **ControlNet:** 

  - semantic feature: inject reference into shallow layer **(first three decoder blocks)**

  $$
  \hat{f}_{\text{UNet}}^i = f_{\text{UNet}}^i + f_{\text{ControlNet}}^i, \quad i \in \{0, 1, 2\}
  $$

  - texture feature: inject prompt (**except** first three decoder blocks)
  - Question: effectiveness?

- **Loss:** MSE + LPIPS + GAN + Gram Loss  ($ \mathcal{L}_{Gram} = \sum\limits_{l \in \mathcal{L}} \lambda_{l} \frac{1}{C_l H_l W_l} \| G_l(\hat{I_{HR}}) - G_l({I_{GT}}) \|_{2}^{2}$) , $G_l$: Gram matrix in layer l

- **Inference:**  enhance perceptual quality
	$$
	\hat{f}_{\text{UNet}}^i = f_{\text{UNet}}^i + 0.5 \times f_{\text{ControlNet}}^i, \quad i \in \{0, 1, 2\}
	$$

### 2. RASR-Flicker30 Dataset

- Source: [Flicker](https://www.flickr.com)
- **Data:** 30 animal species, 40 train, 5 test, approximately100 reference per species



## Experiment

### Setting

- **Dataset:** RASR-Flicker30 + LSDIR, 768 * 768, Real-ESRGAN
- **Train:**
	- RASR-Flicker30: 512*512, (LR, random top 5 REF)
	- LSDIR: 512*512, (LR, another region in same image)

- **Test:** RASR-Flicker30 + WR-SR

### Result

**Baseline:** OSEDiff

![image-20260227161403658](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260227161403658.png)

<img src="https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260227164351714.png" alt="image-20260227164351714" style="zoom: 67%;" />

p.s. tab 2 use already paired image without retrieval

### Ablation

- text prompt
- loss components
- vision encoder

