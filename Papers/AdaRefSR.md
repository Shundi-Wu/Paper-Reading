---
year: 2026
venue: ICLR
citation: 0
tags:
  - diffusion
  - ISR
  - reference
pdf: https://arxiv.org/pdf/2602.01864
status: true
---

**Trust but Verify: Adaptive Conditioning for Reference-Based Diffusion Super-Resolution via Implicit Reference Correlation Modeling**. *Y Wang et al.* **ICLR, 2026 (Citation 0)** [(pdf)](https://arxiv.org/pdf/2602.01864)

## TLDR

Leverage Ref Attention, create implicit gating module - extract summary to align. **Solid** works and very inspiring.

## Motivation

### Difficulty

- real-world degradations make correspondences between low-quality (LQ) inputs and reference (Ref) images unreliable

- traditional ref: weighting mechanism may result in excessive or insufficient reliance on references (green: over-injecting, red: under-utilizing)![image-20260228141059795](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260228141059795.png)

### Insight

- Trust but Verify: attenuates unreliable regions -> Adaptive Implicit Correlation Gating (AICG)

## Method

![image-20260228154614126](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260228154614126.png)

### 1. Direct Reference Feature Injection

- Extract Reference: Reference-Net instantiated with SD-Turbo  (timestep = 1)

- Inject

	- RA module (Reference Attention) : Initialized by copying SA module

	$$
	\mathrm{Q} = \mathrm{H}_{src} \mathrm{W}_Q, \quad  \mathrm{K} = \mathrm{H}_{ref} \mathrm{W}_K, \quad \mathrm{V} = \mathrm{H}_{ref} \mathrm{W}_V, \\
	\mathrm{\mathbf{H}}_{out} = \text{ZeroLinear}(\text{Softmax}(\frac{\mathbf{QK}^\mathrm{T}}{\sqrt{d}})\mathbf{V}) + \mathrm{\mathbf{H}}_{src}
	$$

### 2. AICG: Adaptive Implicit Correlation Gating

![image-20260228143409005](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260228143409005.png)

*PFStorer’s global gating fails to adapt to alignment quality： disregarding LQ–Ref feature correlations*

*ReFIR’s explicit correlation maps are disrupted by noise, leading to unreliable gating： token-wise similarity unreliable.*

- Summarize via Learnable Tokens ($M \ll L_{ref}$)

$$
\mathbf{S} = \mathbf{T}_S \mathbf{W}_K,\quad \mathbf{T}_S \in \R^{M \times d} \\ \mathrm{\mathbf{K}}_{sum} = \text{Softmax}(\frac{\mathbf{SK}^\mathrm{T}}{\sqrt{d}})\mathbf{K} \in \R^{M \times d}
$$

- Gate

$$
\mathrm{\mathbf{S}}_{map} = \text{Softmax}(\frac{\mathbf{QK}_{sum}^\mathrm{T}}{\sqrt{d}}) \in \R^{L_q \times M} \\
\mathbf{G} = \sigma(\frac{1}{M}\sum\limits_{j=1}^{M}[\mathbf{S}_{map}]_{:,j}) \in \R^{L_q \times 1}, \quad \sigma(\cdot) \ \text{is} \ \text{sigmoid} \ \text{function}
$$

- Fuse

$$
\mathrm{\mathbf{H}}_{out} = \text{ZeroLinear}(\mathbf{G} \odot \text{RA}(\mathbf{H}_{src}, \mathbf{H}_{ref})) + \mathrm{\mathbf{H}}_{src}
$$

## Experiment

### Setting

- **Train:** RealSRGAN degradation, 20% randomly replaced with irrelevant samples
	- DIV2K, DIV8K, Flickr2K 1024 × 1024 -> two 512 × 512 sub-regions (HQ and Ref)，Ref rotated
	- a face reference matching dataset with predefined HQ-Ref triplets： **Learning dual memory dictionaries for blind face restoration.** *X Li et al.* **TPAMI, 2022**
	- **Loss**: ** L2 + GAN
- 

### Result

**Baseline:** S3Diff

*P.S. Following the experimental settings of PFStorer, we test 40 human identities: 5 non-overlapping high-quality (HQ) pairs per identity, yielding 162 test pairs (all 512×512).*

*To verify category-based retrieval enhancement, we construct a self-collected bird dataset ( 8,460 HQ images; 66 as HQ images, others as retrieval database, all 512×512)*

![image-20260302232716310](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260302232716310.png)

<img src="https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260302232739184.png" alt="image-20260302232739184" style="zoom: 75%;" />

### Ablation

- ALCG

![image-20260302233424665](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260302233424665.png)

<img src="https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260302233518371.png" alt="image-20260302233518371" style="zoom: 60%;" />

- learnable token
