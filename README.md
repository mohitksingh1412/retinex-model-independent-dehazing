# 🌙 Model-Independent Retinex-Based Single Image Dehazing with Dynamic Color Correction

<p align="center">
  <img src="assets/figures/chapter4_framework.png" alt="Retinex-Based Dehazing Framework" width="95%"/>
</p>

<h3 align="center">
  An Illumination-Reflectance Decomposition Framework for Robust Image Restoration Under Non-Uniform and Varicolored Haze
</h3>



<p align="center">
  <img src="https://img.shields.io/badge/Domain-Computer%20Vision-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Research-Image%20Restoration-purple?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Framework-Retinex%20Theory-success?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Approach-Model%20Independent-orange?style=for-the-badge"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.8%2B-3776AB?style=flat-square&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Color%20Space-YCbCr-informational?style=flat-square"/>
  <img src="https://img.shields.io/badge/Application-Nighttime%20%7C%20Daytime%20%7C%20Underwater-blueviolet?style=flat-square"/>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Non--Commercial%20Research-lightgrey?style=flat-square"/></a>
</p>

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Research Motivation](#-research-motivation)
- [Proposed Contributions](#-proposed-contributions)
- [Architecture and Methodology](#-architecture-and-methodology)
  - [Luminance Separation](#1-luminance-separation-in-ycbcr-color-space)
  - [Retinex Decomposition](#2-illumination-and-reflectance-decomposition)
  - [Multi-Exposure Fusion](#3-multi-exposure-fusion-and-contrast-enhancement)
  - [Dynamic Color Correction](#4-dynamic-color-correction)
  - [Image Reconstruction](#5-color-reconstruction-and-final-image-restoration)
- [Experimental Results](#-experimental-results)
- [Installation and Usage](#-installation-and-usage)
- [Project Structure](#-project-structure)
- [Citation](#-citation)
- [License](#-license)

---

## 🔬 Overview

Conventional physics-based **Model Restoration Techniques (MRT)**, particularly those relying on the Atmospheric Scattering Model (ASM), depend heavily on accurate estimation of transmission maps and atmospheric light.

Although effective under relatively uniform atmospheric conditions, these techniques often struggle in complex real-world environments characterized by:

- Non-uniform atmospheric haze.
- Spatially varying and varicolored illumination.
- Nighttime scenes with artificial light sources.
- Strong color casts and chromatic distortion.
- Underwater imagery with wavelength-dependent attenuation.

This repository presents a **Model-Independent Retinex-Based Single Image Dehazing Framework with Dynamic Color Correction**, developed as part of Chapter 4 of the Ph.D. thesis *Visibility Enhancement for Hazy Environment*.

Unlike conventional atmospheric scattering-based approaches, the proposed framework exploits **Retinex Theory**, representing an image as the product of illumination and reflectance.

The central idea is to enhance scene visibility by separating broad illumination variations from intrinsic surface reflectance, without explicitly estimating atmospheric transmission and global atmospheric light.

The framework operates primarily on the luminance component in the **YCbCr color space**, combining multi-scale structural information, illumination enhancement, multi-exposure fusion, and adaptive chromatic correction.

### 🎯 Research Objective

To develop a robust image restoration framework capable of enhancing visibility and preserving natural color appearance across diverse degraded environments, including daytime haze, nighttime illumination, and underwater scenes.

---

## 💡 Research Motivation

Traditional atmospheric scattering-based dehazing can be expressed as:

$$
I(x,y)=R(x,y)t(x,y)+A(1-t(x,y))
$$

where:

- \(I(x,y)\): Observed degraded image.
- \(R(x,y)\): Original scene radiance.
- \(t(x,y)\): Transmission map.
- \(A\): Atmospheric light.

Reliable estimation of \(t(x,y)\) and \(A\) becomes challenging under non-uniform illumination and spatially varying haze.

The proposed framework instead adopts a Retinex representation:

$$
I(x,y)=L(x,y)\cdot R(x,y)
$$

where:

- \(L(x,y)\): Illumination component.
- \(R(x,y)\): Reflectance component.

By separating illumination and reflectance, the framework focuses on recovering scene details and enhancing visibility while reducing dependence on atmospheric parameter estimation.

### Key Research Challenges

| Challenge | Conventional Limitation | Proposed Direction |
|---|---|---|
| Non-uniform haze | Unreliable transmission estimation | Illumination-reflectance decomposition |
| Nighttime glare | Artificial light contamination | Illumination enhancement |
| Varicolored haze | Unwanted color casts | Dynamic color correction |
| Low contrast | Loss of local structural details | Multi-exposure fusion |
| Underwater degradation | Strong color imbalance | Adaptive chromatic correction |

---

## ✨ Proposed Contributions

The proposed framework introduces the following major processing components:

**1. Model-Independent Restoration**

Employs Retinex-based image decomposition to enhance visibility without explicitly estimating atmospheric transmission and global atmospheric light.

**2. Luminance-Guided Processing**

Performs primary restoration on the Y channel of the YCbCr color space, separating structural enhancement from chrominance information.

**3. Illumination-Reflectance Decomposition**

Separates broad illumination variations from high-frequency reflectance information to recover structural and textural details.

**4. Multi-Exposure Fusion**

Combines complementary gamma-corrected representations using exposedness and local contrast information.

**5. Dynamic Color Correction**

Introduces adaptive chromatic correction to address color distortions caused by artificial illumination and varicolored haze.

**6. Cross-Environment Restoration**

Targets challenging degradation scenarios including daytime, nighttime, and underwater imagery.

---

## ⚙️ Architecture and Methodology

<p align="center">
  <img src="assets/figures/chapter4_framework.png" alt="Complete Retinex-Based Dehazing Architecture" width="100%"/>
</p>

<p align="center">
  <em>Figure 1. Complete architectural pipeline of the proposed model-independent Retinex-based dehazing framework with dynamic color correction.</em>
</p>

The proposed methodology consists of five major processing stages.

### 1. Luminance Separation in YCbCr Color Space

The input RGB image is transformed into the YCbCr color space to separate brightness information from chrominance information.

The transformation can be represented as:

$$
I_{RGB}\longrightarrow (Y,Cb,Cr)
$$

The individual components serve different purposes:

| Component | Description |
|---|---|
| \(Y\) | Luminance information containing brightness and structural details |
| \(Cb\) | Blue-difference chrominance component |
| \(Cr\) | Red-difference chrominance component |

The primary restoration operations are performed on the luminance channel to minimize unnecessary modifications to the original color information.

### 2. Illumination and Reflectance Decomposition

According to Retinex Theory, an observed image can be represented as the product of illumination and reflectance.

For the luminance component:

$$
Y(x,y)=L(x,y)\cdot R(x,y)
$$

where:

- \(Y(x,y)\): Input luminance.
- \(L(x,y)\): Estimated illumination.
- \(R(x,y)\): Reflectance representing intrinsic surface information.

#### A. Illumination Estimation

The illumination component is estimated by smoothing the luminance representation using Gaussian filtering:

$$
L(x,y)=G_\sigma * Y(x,y)
$$

where:

- \(G_\sigma\): Gaussian kernel with standard deviation \(\sigma\).
- \(*\): Convolution operation.

The smoothing operation captures broad illumination variations while suppressing high-frequency details.

#### B. Reflectance Recovery

The reflectance component is recovered by normalizing luminance with the estimated illumination:

$$
R(x,y)=\frac{Y(x,y)}{L(x,y)+\epsilon}
$$

where \(\epsilon\) is a small constant used to prevent numerical instability.

This decomposition separates the low-frequency illumination component from the finer structural and textural information.

### 3. Multi-Exposure Fusion and Contrast Enhancement

To recover structural details in dense haze, multiple gamma-corrected representations are generated.

A gamma transformation can be expressed as:

$$
I_{\gamma}^{k}(x,y)=C\cdot Y(x,y)^{\gamma_k}
$$

where:

- \(Y(x,y)\): Original luminance.
- \(\gamma_k\): Gamma transformation parameter.
- \(C\): Scaling constant.

Different exposure representations provide complementary information about scene brightness and local contrast.

The representations are fused using exposure and contrast-based weighting.

A general weighted fusion formulation is:

$$
F_{fused}(x,y)=
\frac{
\sum_{k=1}^{N}W_k(x,y)I_\gamma^k(x,y)
}{
\sum_{k=1}^{N}W_k(x,y)+\epsilon
}
$$

where:

- \(W_k(x,y)\): Weight map for the \(k\)-th exposure representation.
- \(N\): Number of exposure representations.
- \(I_\gamma^k\): Gamma-corrected luminance representation.

The fusion operation combines complementary exposure information while maintaining local contrast.

### 4. Dynamic Color Correction

A major challenge in nighttime and underwater image restoration is the presence of spatially varying color distortions.

Examples include:

- Yellowish illumination from artificial streetlights.
- Greenish and bluish color casts in underwater scenes.
- Non-uniform chromatic distortions caused by colored haze.

To address these challenges, the framework incorporates a dynamic color correction module operating on chrominance components.

The correction process adjusts the color distribution using adaptive channel statistics.

A generalized channel correction formulation is:

$$
C_{corrected}(x,y)=
\alpha\left(C(x,y)-\mu_C\right)+\mu_{ref}
$$

where:

- \(C(x,y)\): Original chrominance component.
- \(\mu_C\): Estimated mean chrominance.
- \(\mu_{ref}\): Reference chrominance mean.
- \(\alpha\): Adaptive color correction factor.

The correction is intended to reduce unwanted chromatic shifts while maintaining a natural color appearance.

The module can be applied independently to the Cb and Cr channels.

### 5. Color Reconstruction and Final Image Restoration

After luminance enhancement and chrominance correction, the restored components are combined:

$$
I_{restored}(x,y)=
\mathcal{T}^{-1}
\left(
Y_{enhanced},
Cb_{corrected},
Cr_{corrected}
\right)
$$

where \(\mathcal{T}^{-1}\) denotes the inverse YCbCr-to-RGB transformation.

The resulting image aims to provide:

- Improved scene visibility.
- Enhanced local contrast.
- Better structural and textural preservation.
- Reduced color distortion.
- Natural-looking image reconstruction.

---

## 📊 Experimental Results

The proposed framework is evaluated on challenging image restoration scenarios, including nighttime, varicolored haze, and underwater degradation.

### 1. Nighttime and Varicolored Haze Restoration

<p align="center">
  <img src="assets/results/night_varicolored_comparison.png" alt="Nighttime and Varicolored Dehazing Comparison" width="100%"/>
</p>

<p align="center">
  <em>Figure 2. Visual comparison on nighttime and varicolored hazy scenes. (a) Input, (b) Traditional DCP, and (c) Proposed Retinex-Based Method. The results demonstrate improved visibility and reduced artificial color glare.</em>
</p>

### 2. Underwater Image Restoration

<p align="center">
  <img src="assets/results/underwater_comparison.png" alt="Underwater Image Restoration Comparison" width="100%"/>
</p>

<p align="center">
  <em>Figure 3. Restoration results on underwater degraded imagery, illustrating improvements in contrast and chromatic balance.</em>
</p>

---

## 📈 Quantitative Evaluation

The proposed method is evaluated using image quality assessment metrics suitable for degraded real-world images.

### 1. Natural Image Quality Evaluator (NIQE)

NIQE measures perceptual image quality using statistical characteristics of natural images without requiring a reference ground-truth image.

**Interpretation:** Lower NIQE values generally indicate better perceptual naturalness.

### 2. Color Distortion Index (CDI)

CDI is used to assess chromatic distortion and color fidelity.

**Interpretation:** Lower color distortion values indicate better color consistency, subject to the exact definition and implementation of the metric.

### Evaluation Summary

| Evaluation Metric | Purpose | Desired Direction |
|---|---|---|
| NIQE | Perceptual image quality | Lower |
| CDI | Color distortion assessment | Lower |

The reported experiments indicate improvements over traditional prior-based approaches in the evaluated nighttime and varicolored scenes.

*Note: Numerical benchmark values and dataset-specific results should be added when the corresponding evaluation tables are available.*

---

## 🚀 Installation and Usage

### 1. Prerequisites

- Python 3.8+
- NumPy
- OpenCV
- SciPy
- Matplotlib

### 2. Clone the Repository

```bash
git clone YOUR_REPOSITORY_URL
cd retinex-model-independent-dehazing
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run Dehazing on a Nighttime Image

```bash
python dehaze_retinex.py \
    --input data/samples/night_haze.png \
    --output data/results/night_haze_clear.png \
    --color_correct True
```

The `--color_correct` argument enables the dynamic color correction module.

---

## 📁 Project Structure

```text
.
├── assets/
│   ├── figures/
│   │   └── chapter4_framework.png
│   │
│   └── results/
│       ├── night_varicolored_comparison.png
│       └── underwater_comparison.png
│
├── data/
│   ├── samples/
│   └── results/
│
├── dehaze_retinex.py
├── requirements.txt
├── LICENSE
└── README.md
```

---

## 📖 Research Reference

This implementation corresponds to Chapter 4 of the Ph.D. thesis:

**Visibility Enhancement for Hazy Environment**

**Author:** Dr. Mohit Kumar Singh  
**Institution:** Malaviya National Institute of Technology (MNIT), Jaipur  
**Department:** Computer Science & Engineering

### Citation

If you use this research or implementation in your academic work, please cite the thesis:

```bibtex
@phdthesis{singh2025visibility,
  author    = {Mohit Kumar Singh},
  title     = {Visibility Enhancement for Hazy Environment},
  school    = {Malaviya National Institute of Technology (MNIT) Jaipur},
  year      = {2025},
  address   = {Jaipur, India},
  chapter   = {4: Model-Independent Retinex-Based Single Image Dehazing with Dynamic Color Correction}
}
```

---

## 📜 License

This repository is intended for non-commercial academic and research purposes.

Please refer to the [LICENSE](LICENSE) file for complete licensing terms and conditions.

---

## 👨‍🔬 About the Author

**Dr. Mohit Kumar Singh**  
Ph.D. in Computer Engineering  
Department of Computer Science & Engineering  
Malaviya National Institute of Technology (MNIT), Jaipur, India

**Research Interests**

`Computer Vision` · `Image Dehazing` · `Retinex Theory` · `Image Restoration` · `Deep Learning` · `Pattern Recognition` · `Generative AI`


<p align="center">
  <b>Advancing visual intelligence through research-driven image restoration and computer vision.</b>
</p>
