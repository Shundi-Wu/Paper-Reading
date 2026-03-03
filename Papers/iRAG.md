---
year: 2025
venue: ICCV
citation: 0
tags:
  - diffusion
  - ISR
  - RAG
pdf: https://openaccess.thecvf.com/content/ICCV2025/papers/Lee_Reference-based_Super-Resolution_via_Image-based_Retrieval-Augmented_Generation_Diffusion_ICCV_2025_paper.pdf
status: true
---
**Reference-based Super-Resolution via Image-based Retrieval-Augmented Generation Diffusion.** *B Lee et al.* **ICCV, 2025 (Citation 0)** [(pdf)](https://openaccess.thecvf.com/content/ICCV2025/papers/Lee_Reference-based_Super-Resolution_via_Image-based_Retrieval-Augmented_Generation_Diffusion_ICCV_2025_paper.pdf)

## TLDR

Use **generation** to expend the reference database, use **hash** method to search relavant image. (**RAG** method)

Reconstruct by using intermediate image and **spatial feature transform**. 

Improve real-world metrics (MUSIQ), but underperform at PSNR etc.

**Question:**

- [Figure 2](##Method): why $F_{lq}$ trainable?

	

## Motivation

### Difficulties

- **Find Reference**: Identifying suitable references in real-world scenarios remains a critical challenge.
- **Use Reference:** (1) matching difficulty, especially when the reference image differs substantially from the input LR image in illumination or pose; (2) robust texture transfer, to ensure that only relevant high-frequency details are mapped to the LR input while minimizing artifacts from mismatched regions.

### Insight

- **RAG**
- **Restoration Module**

## Method

![image-20260223130710526](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260223130710526.png)

- augmenting the existing dataset by enriching it with auxiliary HR images
- retrieving a relevant HR image from a large database to match the target LR image
- generating a high-quality HR image by integrating LR and reference features into a diffusion model

### 1. Data Augmentation

- **Generate**

$$
\mathbf{I}_{gen} = \mathcal{G}(\tilde{z}_{HR}), \quad \tilde{z}_{HR} \triangleq z_{HR} + \alpha \cdot \sigma
$$

- **Filter Hallucinations** 	

​	Understanding hallucinations in diffusion models through mode interpolation. **NIPS, 2025**

### 2. Data Retrieval

- **Select**
	$$
	\mathbf{I}_{ref} = \mathop{\text{argmin}}_{\mathbf{I}_{DB} \in ref.DB } (dist(\mathbf{h}_{LR}, H_{\phi}(\mathbf{I}_{DB})))
	$$

- **Train $H_{\phi}$**

	1. producing two correlated views for the $k$-th image $x^{(k)}$

	2. VGG encoder + hash learnable function $g_{\phi}$ -> binary hash code

		**Goal:** minimize the distance between same image.

		**Loss:** cosine similarity + log-likelihood. (sum of $\ell_i^{(k)}$ )
		$$
		S(h_1^{(k)}, h_2^{(k)}) := e^{C(h_1^{(k)}, h_2^{(k)})} / \tau, \quad \ell_1^{(k)} := -\log \frac{S(h_1^{(k)}, h_2^{(k)})}{S(h_1^{(k)}, h_2^{(k)}) + \sum_{i, n \neq k} S(h_1^{(k)}, h_i^{(n)})}.
		$$

### 3. Ref SR

- **Intermediate Ref SR:** employ a reference-based SR network -> $\mathbf{I}_{inter}$
- **Concatenate Feature**: $F_{cond}\triangleq \text{concat}(\varepsilon_\varphi(\mathbf{I}_{LR}), \varepsilon_\varphi(\mathbf{I}_{inter}))$
- **Spatial Feature Transform:**  $n$-th residual block -> $\alpha, \beta = \mathop{K}_{\theta}(\mathbf{F}_{LR}, \mathbf{F}_{inter})$ -> $\mathbf{\hat{F}}_{\mathrm{dif}} = \alpha(\mathbf{F}_{\mathrm{LR}}, \mathbf{F}_{\mathrm{inter}}) \odot \mathbf{F}_{\mathrm{dif}} + \beta(\mathbf{F}_{\mathrm{LR}}, \mathbf{F}_{\mathrm{inter}})$

**Loss: ** inter loss (pixel + GAN loss) + final loss

## Experiment

### Setting

- **RefDB:** DIV2K + Flicker2K + OST (25%) + Generation (25%)
- **Train:** DIV2K + Flicker2K + OST (75%)
- **Validation:** DIV2K Validation, RealSR, DRealSR 

<img src="https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260223151538592.png" alt="image-20260223151538592" style="zoom: 50%;" />

### Result

<img src="https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260223152838213.png" alt="image-20260223152838213" style="zoom: 50%;" />

