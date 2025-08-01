<img width="720" height="163" alt="random_gen" src="https://github.com/user-attachments/assets/5e72881b-f57d-417b-8350-833d184990ff" /><h1 align="center">
  🔮 Patch-Based Variational Autoencoder for Tarot Card Generation
</h1>

<p align="center">
  <b>Local generative modeling of tarot-style art with contextual VAE and transformer encoding</b><br/>
  <i>Inspired by visual motifs on Pinterest, built for creative exploration and compactness</i>
</p>

<p align="center">
  <img src="assets/banner_tarot.png" alt="Banner Image" width="700"/>
</p>

<p align="center">
  <a href="https://www.tensorflow.org/">
    <img src="https://img.shields.io/badge/TensorFlow-2.12+-FF6F00?logo=tensorflow&logoColor=white" alt="TF Badge">
  </a>
  <img src="https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white" alt="Python Badge">
  <img src="https://img.shields.io/badge/License-MIT-green" alt="License Badge">
  <img src="https://img.shields.io/badge/Model%20Size-~15MB-lightgrey" alt="Model Size Badge">
</p>

---

## 🧠 Overview

This project presents a **lightweight patch-based Variational Autoencoder (VAE)** trained on tarot card-style artwork. The model learns local patterns by encoding and decoding **32×32 patches** with transformer-based context.  

Unlike traditional VAEs that process entire images, this method enables:
- 🧩 **Modular learning** of parts of the image  
- 🚀 **Smaller model size (~16MB)** vs original ~600MB  
- ✨ **Random latent sampling** for visual creativity  

---

## 🌈 Why Tarot Cards?

While browsing Pinterest, I was fascinated by the ornate, symbolic, and colorful visual language of tarot cards.  
This project started from the desire to **recreate and reimagine those forms**—through a machine that learns from the patterns and composition.

---

## 🗃 Dataset

- ✅ **Sources**:  
  - [Kaggle Tarot Dataset]([INSERT_KAGGLE_LINK](https://www.flickr.com/photos/sumadas-treasure-box/albums))  
  - [Flickr Creative Commons]([INSERT_FLICKR_LINK](https://www.flickr.com/photos/sumadas-treasure-box/albums))  
- 📐 **Preprocessing**:
  1. Resize & pad images to **256×256**
  2. Normalize to `[0,1]`
  3. Extract non-overlapping **32×32 patches** (8×8 grid → 64 patches per image)

---

## 🧪 Model Architecture

<p align="center">
  <img src="assets/model_architecture.png" alt="Model Architecture" width="600"/>
</p>

The architecture is composed of **three core components**:

### 1️⃣ Variational Transformer Encoder
- Accepts **embedded patches + context patches**
- Computes `z_mean` and `z_log_var` for VAE sampling
- Uses:
  - Multi-Head Attention for capturing spatial relationships
  - Positional Embeddings to maintain patch order
  - Feed-forward layers and LayerNorm for stability
- Output: Latent vector `z` sampled via reparameterization trick

---

### 2️⃣ Patch Decoder
- Accepts latent vector `z` and reconstructs a **32×32 patch**
- Architecture: Dense → Reshape → 3× Conv2DTranspose → Sigmoid
- Responsible for **learning local visual patterns** like shapes, colors, and borders

---

### 3️⃣ Chunked VAE System
- Orchestrates the encoding/decoding of **full images** in patch form:
  1. **Patch Extraction**: Split input into non-overlapping patches
  2. **Patch Embedding**: Embed patches into a compact feature space
  3. **Context Gathering**: Collect `context_n` previous patches for each new patch
  4. **Encoding & Sampling**: Transformer encoder outputs latent vector `z`
  5. **Patch Decoding**: Latent vector decoded back to patch
  6. **Image Reconstruction**: Patches stitched back into a 256×256 image
- Includes:
  - KL-divergence loss with **warm-up scheduling**
  - Mean Squared Error reconstruction loss

---

## 📊 Results

### 🎯 Reconstruction Examples
<p align="center">
  <img width="719" height="288" alt="reconstructions" src="https://github.com/user-attachments/assets/bea43c04-b6b2-45f8-a38b-ffd852f340f9" />
</p>

---

### 🌌 Random Latent Generations
<p align="center">
  <img width="720" height="163" alt="random_gen" src="https://github.com/user-attachments/assets/322716dc-4d79-4e61-8ee9-4cb86cb98d75" />
</p>



---

### 📉 Training Curves
| Full Training Curve | Zoomed-In (Y-Axis) |
|---------------------|----------------------------|
| ![loss](https://github.com/user-attachments/assets/fdd4c593-c008-42b2-92d2-3db374f82390) | ![zoom](https://github.com/user-attachments/assets/6230c47b-f6b7-4985-af96-c6d8cabd4fc2) |


---


