# 🏙️ Image Segmentation of Urban Environments (SIV Project)

This project was developed as part of the **Signal, Image and Video (SIV)** course at the **University of Trento**.  
It explores **two complementary approaches** to image segmentation on the **Cityscapes dataset** — a **traditional, class-agnostic region growing algorithm** and a **deep learning–based semantic segmentation model (U-Net)**.

---

## 📘 Project Overview

Accurate image segmentation is essential for applications like **autonomous driving**, **robotics**, and **urban scene understanding**.  
This project aims to:

- Implement and analyze a **region growing segmentation pipeline** using low-level image cues (color, texture, edges).  
- Design and train a **U-Net** architecture for **class-specific semantic segmentation**.  
- Compare their performance qualitatively and quantitatively on the **Cityscapes** dataset.

---

## 🧩 Part 1 — Region Growing Segmentation (Class-Agnostic)

### 🧠 Methodology
A classical **bottom-up segmentation pipeline** was implemented combining:
- **Color (LAB)** and **texture (LBP)** features  
- **Gradient magnitude (Sobel)** for edge localization  
- **Canny edges** to constrain region expansion  
- **Agglomerative clustering** for region merging  

Seeds are first placed in **low-gradient basins** (homogeneous areas), then expanded using **8-connectivity** while respecting edge boundaries.  
A **region merging step** reduces over-segmentation using a feature-weighted similarity matrix.

### 🧱 Implementation Outline
- `Preprocessor` – extracts gradient, edges, and LBP features  
- `RegionGrower` – performs multi-seed, iterative region growing  
- `RegionMerger` – merges visually similar segments  
- `SegmentationPostProcessor` – applies morphological refinements  
- `SegmentationEvaluator` – computes **ARI** and **NMI** clustering metrics

### 📊 Results
- Achieved **ARI: 0.5243**, **NMI: 0.5304**  
- Reasonably preserves region boundaries, though tends to **over-segment** complex areas  
- Iterative seeding and merging steps improve coherence but require manual tuning

---

## 🤖 Part 2 — Semantic Segmentation with U-Net

### 🧠 Methodology
A **U-Net** model was implemented for **pixel-wise semantic labeling** of 19 urban classes (road, building, car, pedestrian, etc.).  
The network adopts the **encoder-decoder** structure with **skip connections**, trained from scratch on **Cityscapes (2975 training / 500 validation images)**.

- **Encoder:** 5 downsampling blocks (64–1024 filters) with batch normalization and dropout  
- **Decoder:** 4 upsampling blocks with transposed convolutions and skip connections  
- **Loss:** Cross-Entropy with `ignore_index=250`  
- **Optimizer:** Adam (`lr=1e-4`, β₁=0.9, β₂=0.999)  
- **Metrics:** Pixel Accuracy and Mean IoU (mIoU)

### 📈 Training Overview
- 25 epochs, batch size 32  
- Achieved > 90% training accuracy and strong validation performance (≈ 88–90%)  
- mIoU steadily increased, confirming progressive learning of class boundaries  
- Mild overfitting observed — could be mitigated with data augmentation or early stopping

### 🏁 Results
- Strong segmentation for **large, well-defined classes** (road, building, vegetation)  
- Occasional boundary imprecision and partial recognition of small objects (pedestrians, signals)  
- Demonstrates significant improvement over classical segmentation in both accuracy and interpretability

---

## ⚖️ Comparison

| Aspect | Region Growing | U-Net |
|--------|----------------|-------|
| Approach | Bottom-up, class-agnostic | Supervised, data-driven |
| Input Cues | Color, texture, edges | Learned multi-scale features |
| Labels | None (unsupervised) | 19 semantic classes |
| Tuning | Manual thresholds | Learned parameters |
| Performance | ARI ≈ 0.52, NMI ≈ 0.53 | Pixel Acc ≈ 90%, mIoU ↑ steadily |
| Strengths | Interpretability, no labels needed | Robustness, accuracy, scalability |
| Limitations | Over-segmentation, parameter sensitivity | Needs labeled data, heavy compute |

---

## 📎 References

1. Arulananth, T. S. et al. (2024). *Semantic segmentation of urban environments: Leveraging U-Net deep learning model for cityscape image analysis.* **PLOS One, 19(4)**, e0300767.  
2. Cordts, M. et al. (2016). *The Cityscapes Dataset for Semantic Urban Scene Understanding.* **CVPR**, 3213–3223.  

---

**Author:** Nancy Kalaj  
**University of Trento – MSc in Artificial Intelligence Systems**  
📧 nancy.kalaj@studenti.unitn.it
