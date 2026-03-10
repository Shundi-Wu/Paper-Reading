

## CoSeR

**CoSeR: Bridging Image and Language for Cognitive Super-Resolution.** *H Sun et al.* **CVPR, 2024 (Citation 94)** [(pdf)](https://openaccess.thecvf.com/content/CVPR2024/papers/Sun_CoSeR_Bridging_Image_and_Language_for_Cognitive_Super-Resolution_CVPR_2024_paper.pdf)

### Method

ControlNet: extract ref image's feature in correspond layers

Reference-Attention module: inject features 

- **Q**: LR
- **K, V**: Ref

![image-20260307160642983](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260307160642983.png)

## SFTGAN

**Recovering Realistic Texture in Image Super-resolution by Deep Spatial Feature Transform.** *X Wang et al.* **CVPR, 2018 (Citation 1507)** [(pdf)](https://openaccess.thecvf.com/content_cvpr_2018/papers/Wang_Recovering_Realistic_Texture_CVPR_2018_paper.pdf)

*[iRAG](../Papers/iRAG.md) use this method, but used in U-Net and another restoration module*

### Method

$$
\text{SFT}(\mathbf{F}|\gamma, \beta) = \gamma \odot \mathbf{F} + \beta
$$

![image-20260309220024255](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260309220024255.png)

## GenVE

**Aligning Global Semantics and Local Textures in Generative Video Enhancement.** *Z Chen et al.* **ICCV, 2025 (Citation 0)** [(pdf)](https://openaccess.thecvf.com/content/ICCV2025/papers/Chen_Aligning_Global_Semantics_and_Local_Textures_in_Generative_Video_Enhancement_ICCV_2025_paper.pdf)

### Method

![image-20260309223004555](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260309223004555.png)

- Ref Image *(LR key frame -> SR method -> Reference)* : $\gamma_t$ decreasing from 0.5 to 0 monotonically with t from $T$ to 0. $z_t$ originally is **$z$ + $T_0$ step noise + image diffuser ** at timestep t.
	$$
	\tilde{z}_t = \alpha_tz + \sigma_t\epsilon, \quad  z_t = (1 − \gamma_t)z_t + \gamma_t\tilde{z}_t
	$$

- Ref augmentation

	- Noise: n step noise (setting by users) to $y$ and $Z$
	- Temporal Augmentation: produce sequence y
	- Masking Condition: mask with probability r

- Local Ref Attention: B: bias matrix (d diagonal matrix)

![image-20260309232337349](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260309232337349.png)

## GPP-LLIE

**Low-Light Image Enhancement via Generative Perceptual Priors.** *H Zhou et al.* **AAAI, 2025 (Citation 27)** [(pdf)](https://arxiv.org/pdf/2412.20916)

### Method

- GPP-LN
- Cross-attention

![image-20260310154439380](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260310154439380.png)

## ControlNet

**Adding Conditional Control to Text-to-Image Diffusion Models**. *L Zhang et al.* **ICCV, 2023** **(Citation 7556)**  [(pdf)](https://openaccess.thecvf.com/content/ICCV2023/papers/Zhang_Adding_Conditional_Control_to_Text-to-Image_Diffusion_Models_ICCV_2023_paper.pdf)  [ControlNet](../Papers/ControlNet.md)

### Method

- zero convolution

![image-20260220162038827](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260220162038827.png)

## T2I-Adapter

**T2I-Adapter: Learning Adapters to Dig out More Controllable Ability for Text-to-Image Diffusion Models.** *C Mou et al.* **2023 (Citation 1702)** [(pdf)](https://arxiv.org/pdf/2302.08453)

### Method

- T2I Adapter: HQ unshuffled +  inject into encoder

![image-20260310155516406](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260310155516406.png)

## IP-Adapter

**IP-Adapter: Text Compatible Image Prompt Adapter for Text-to-Image Diffusion Models.** *H Ye et al.* **2023 (Citation 1612)** [(pdf)](https://arxiv.org/pdf/2308.06721)

### Method

- Decoupled Cross-Attention

![image-20260310160644039](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260310160644039.png)
