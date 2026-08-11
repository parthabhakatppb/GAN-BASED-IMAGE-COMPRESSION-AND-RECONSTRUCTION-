# 📸 Object-Centric Image Compression Pipeline
### *YOLOv8-Seg + Object Stacking Atlas + Neural Refinement*

<p align="center">
  <img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white" alt="PyTorch" />
  <img src="https://img.shields.io/badge/YOLOv8-000000?style=for-the-badge&logo=ultralytics&logoColor=white" alt="YOLOv8" />
  <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white" alt="OpenCV" />
  <img src="https://img.shields.io/badge/Google_Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white" alt="Colab" />
  <img src="https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge" alt="License" />
</p>

<p align="center">
  <a href="#-key-features">Key Features</a> •
  <a href="#-architecture--pipeline">Architecture</a> •
  <a href="#-configuration">Configuration</a> •
  <a href="#-notebooks--benchmarks">Notebooks</a> •
  <a href="#-getting-started">Getting Started</a>
</p>

---

## 💡 Overview

This repository provides an end-to-end framework for **Object-Centric Image Compression**. Traditional lossy compression methods treat all pixels uniformly, leading to equal degradation across critical foreground targets and non-essential background regions. 

By utilizing **YOLOv8-Seg** segmentation and an efficient **2D Bin-Packing (Object Stacking)** technique, our pipeline isolates critical regions, packs them into a void-free spatial atlas, downsamples non-essential background details, and uses a **Neural/GAN Refinement Network** to reconstruct high-fidelity visuals while drastically reducing file size (bpp).

---

## ✨ Key Features

* 🎯 **Smart Instance Segmentation**: Powered by `YOLOv8m-seg` for high-precision mask and bounding box detection, with fallbacks for class-agnostic FFT spectral residual saliency.
* 📦 **Compact Object Packing**: Stacks extracted foreground objects tightly into an atlas, eliminating empty pixel voids to maximize compression efficiency.
* ⚡ **Asymmetric Quality Encoding**: Separately adjusts JPEG/WebP compression levels and downscaling factors for background textures versus packed object patches.
* 🛠️ **Seamless Reconstruction**: Re-stitches cropped object elements precisely onto upscaled backgrounds using lightweight JSON coordinate metadata.
* 📈 **Comprehensive Metrics Suite**: Measures quantitative and perceptual quality across PSNR, SSIM, LPIPS, and VMAF metrics.

---

## 🔄 Architecture & Pipeline

```text
               ┌───────────────────────┐
               │    Input Raw Image    │
               └───────────┬───────────┘
                           │
             ┌─────────────┴─────────────┐
             ▼                           ▼
  ┌──────────────────────┐    ┌──────────────────────┐
  │ Foreground Detection │    │ Background Separation│
  │     (YOLOv8-Seg)     │    │  (Downscaled x2/x4)   │
  └──────────┬───────────┘    └──────────┬───────────┘
             │                           │
             ▼                           │
  ┌──────────────────────┐               │
  │ Object Crop Stacking │               │
  │    (Compact Atlas)   │               │
  └──────────┬───────────┘               │
             │                           │
             └─────────────┬─────────────┘
                           │
                           ▼
              ┌────────────────────────┐
              │ Joint Compression/Pack │
              └────────────┬───────────┘
                           │
                           ▼
              ┌────────────────────────┐
              │ Reconstruction & GAN   │
              │   Neural Refinement    │
              └────────────────────────┘
