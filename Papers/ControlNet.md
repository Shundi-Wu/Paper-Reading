---
year: 2023
venue: ICCV
citation: 7556
tags:
  - diffusion
pdf: https://openaccess.thecvf.com/content/ICCV2023/papers/Zhang_Adding_Conditional_Control_to_Text-to-Image_Diffusion_Models_ICCV_2023_paper.pdf
status: true
---
**Adding Conditional Control to Text-to-Image Diffusion Models**. *L Zhang et.al.* 
**ICCV, 2023** **(Citation 7556)**  [(PDF)](https://openaccess.thecvf.com/content/ICCV2023/papers/Zhang_Adding_Conditional_Control_to_Text-to-Image_Diffusion_Models_ICCV_2023_paper.pdf)

## Contribution

- Add spatially localized input conditions to a pretrained text-to-image diffusion model
- Zero Convolution

## Method

![image-20260220194957079](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260220194957079.png)

**Zero Convolution:** 1 × 1 convolution with both weight and bias initialized to zero.

**Starts**: harmful noise cannot influence the hidden states
$$
y_{c}=y
$$
**Formula:**
$$
y_c = F (x; Θ) + Z(F (x + Z(c; Θ_{z1}); Θc); Θ_{z2})
$$
![image-20260220162038827](https://raw.githubusercontent.com/Shundi-Wu/Typora/main/assets/img/image-20260220162038827.png)



## Train & Inference

- **Prompt: **randomly replace 50% text prompts $c_t$ with empty strings.
- **Loss:** like Diffusion Model - predict noise
- **The sudden convergence phenomenon.** Due to the zero convolutions, ControlNet always predicts high-quality images during the entire training. At a certain step in the training process (e.g., the 6133 steps marked in bold), the model suddenly learns to follow the input condition.

---

- **CFG(Classifier-free Guidance)**: Add to $ε_c$, decrease $β_{cfg}$, $z_t =  ϵ_θ(x,c)+w_i⋅r_i(c_{control})$

$$
ε_{prd} = ε_{uc} + β_{cfg}(ε_c − ε_{uc})
$$

