# Uncertainty-Guided Slice Triage for Brain Tumor Segmentation under Missing MRI Modalities

[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://streamlit.io)
[![Python 3.12](https://img.shields.io/badge/python-3.12-blue.svg)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-Deep%20Learning-red.svg)](https://pytorch.org/)

Official repository for my B.Tech Biomedical Engineering Final-Year Thesis. This project introduces an uncertainty-guided clinical triage framework using a 2D ResUNet with **FiLM conditioning** for robust brain tumor segmentation under incomplete/missing MRI sequences.

## 🧠 Key Features
- **Multimodal Adaptability**: Handles missing MRI modalities (FLAIR, T1, T1ce, T2) dynamically using Feature-wise Linear Modulation (FiLM).
- **Epistemic Uncertainty Estimation**: Utilizes Monte Carlo (MC Dropout) during inference to compute predictive entropy.
- **Automated Slice Triage**: Converts pixel uncertainty into a **Review Priority Score (RPS)** to flag ambiguous slices for radiologists.
- **Interactive Streamlit Web App**: A real-time clinical deployment dashboard for live visualization and multi-class segmentation (NC, ED, ET).

## 🚀 Quick Start (Running the Web App)
1. Clone the repository:
   ```bash
   git clone [https://github.com/YOUR-USERNAME/your-repo-name.git](https://github.com/YOUR-USERNAME/your-repo-name.git)
   cd your-repo-name

## Results

Evaluated on a patient-wise held-out test set (56 patients) from BraTS 2020.
Train / val / test split: 257 / 56 / 56 patients (24,422 slice pairs in total).

### Segmentation under missing MRI modalities

| Input condition        | Mean Dice |
|------------------------|-----------|
| All modalities         | 0.7514    |
| T1 missing             | 0.7500    |
| T2 missing             | 0.7492    |
| FLAIR missing          | 0.7068    |
| T1ce missing           | 0.6117    |
| T1ce + FLAIR missing   | 0.5979    |

T1ce and FLAIR carry the most information. Losing T1 or T2 barely changes performance.

### Uncertainty-guided slice triage

| Review strategy                          | Errors captured in top 20% of slices |
|------------------------------------------|--------------------------------------|
| **Review Priority Score (RPS)**          | **64.10%**                           |
| Tumor-area ranking                       | 29.49%                               |
| Random order                             | 21.79%                               |

Reviewing only the top 20% of slices ranked by RPS catches about 3x more
errors than random review.

### Method summary
- 2D ResUNet with modality dropout and a 4-bit availability mask (FiLM conditioning at the bottleneck)
- Monte Carlo Dropout (T stochastic passes) for pixel-wise predictive entropy
- Review Priority Score: mean tumor-region entropy, weighted by the number of missing modalities

## Triage Curve 
<img width="1200" height="600" alt="triage_curve" src="https://github.com/user-attachments/assets/544a9158-42b0-42d7-8154-3d925d27ebfd" />
