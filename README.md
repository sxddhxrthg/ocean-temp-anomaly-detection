# 🌊 ML-Based Detection of Ocean Temperature Anomalies
### Early Climate Change Detection via CNN-RF Parallel Ensemble

![Python](https://img.shields.io/badge/Python-3.10-blue)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.x-green)
![License](https://img.shields.io/badge/License-MIT-yellow)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

---

## 🎯 Problem Statement

Traditional ML models suffer from **Smoothing Bias** — Huber/MAE loss functions
treat critical thermal spikes as noise and miss them completely.
For deep-sea submersibles like **MATSYA-6000**, a missed **-0.3846°C** thermal
spike is not a metric failure — it is a mission failure.

---

## ⚙️ Architecture — Spatiotemporal Residual Mapping

| Stage | Method |
|-------|--------|
| **Noise Removal** | STL Decomposition → isolates residual anomaly signal |
| **Feature Engineering** | 67 raw attributes pruned to 41 core features |
| **Tensor Encoding** | 3-day windows → RGB spatial tensors (Day1=R, Day2=G, Day3=B) |
| **Oversampling** | Cost-sensitive 3× augmentation on top-10% extreme spikes |
| **CNN Backbone** | ResNet-101 (ImageNet pretrained, last 20 layers unfrozen) |
| **RF Gate** | 500-tree Random Forest on 41 engineered features |
| **Fusion** | Late fusion weighted ensemble: 61% CNN + 39% RF |

> **Architecture Type:** Parallel Late-Fusion — CNN and RF run independently,
> outputs fused post-inference. Not serial. Not stacked.

---

## 📊 Results — Table II (60th Percentile Threshold)

| Model | MAE | Spike MAE | Accuracy | Recall | F1 |
|-------|-----|-----------|----------|--------|----|
| CNN (ResNet-101) | 0.1007°C | 0.2089°C | 54.60% | 12.63% | — |
| Random Forest | 0.1279°C | 0.0589°C | 68.50% | 100% | — |
| MLP Baseline | 2.5874°C | — | — | — | — |
| **Ensemble (61/39)** | **0.0700°C** | **0.1114°C** | **84.03%** | **80%** | **80%** |

### 🏆 Key Metrics
- **MAE: 0.0700°C** — 30.45% improvement over CNN alone
- **Accuracy: 84.03%** — 45.24% improvement over RF alone
- **RMSE: 0.0849°C**
- **Precision: 80% | Recall: 80% | F1: 80%**
- **Deepest anomaly detected:** −0.13°C (mid-November 2025, winter onset)

---

## 🌏 Dataset

| Parameter | Value |
|-----------|-------|
| **Source** | Copernicus CMEMS Satellite SST |
| **Region** | North Indian Ocean (4.83°N–24.58°N, 67.08°E–89.33°E) |
| **Period** | July 2022 – February 2026 |
| **Resolution** | 0.083° (~9km), 238×268px grid |
| **Observations** | 1,190 daily snapshots |
| **Training samples** | 949 → 1,234 (after oversampling) |

---

## 🔑 Key Design Decisions

| Decision | Choice | Reason |
|----------|--------|--------|
| **Loss Function** | MSE over Huber | Huber forgives outliers — counterproductive for anomaly detection |
| **Optimizer** | Adam over AdamW | AdamW L2 decay caused recall collapse to 0% |
| **RF Input** | 41 engineered features | Raw pixel RF (63,784 features) gave MAE 0.126°C — noise dominated |
| **Fusion** | Weighted average | Ridge stacking and residual correction both underperformed |

---

## 🚀 Future Work — Phase 2

- [ ] K-Fold Multi-Basin Cross-Validation
- [ ] Bayesian Hyperparameter Optimization
- [ ] Uncertainty Quantification
- [ ] Long-term Model Drift Monitoring Pipeline
- [ ] Onboard compute optimization for MATSYA-6000 deployment

---

## 📁 Repository Structure
ocean-temp-anomaly-detection/
├── MinorProject.ipynb                          # Full ML pipeline
├── Architecture_Document.docx                  # System architecture
├── Functional_Document_Module_Description.docx # Module breakdown
├── Functional_Test_Case_and_Result_Analysis.xlsx # Test results
├── Result_Analysis.docx                        # Result discussion
├── Sprint_Retrospective_Document.docx          # Agile retrospective
├── Daily_Scrum_Log.docx                        # Sprint logs
├── Review2_PPT.pptx                            # Review presentation
└── Minor_Project_Paper (2).pdf                 # Research paper

---

## 👥 Team

| Name | Register Number |
|------|----------------|
| Sawant Aarya Rajesh | RA2311003010827 |
| Siddharth Ganesh | RA2311003010826 |

**Guide:** Dr. Divya Mohan
**Panel Head:** Dr. Rajalakshmi M
**Institution:** SRM Institute of Science and Technology, Kattankulatham
**Course:** 21CSP302L — Minor Project | Batch B834

---

## 🏛️ Real-World Application

This system is designed to protect **MATSYA-6000** — India's indigenously built
deep-sea submersible capable of reaching **6,000m depth** — from critical
thermal shock events in the North Indian Ocean.

---

## 📜 Publications

- **WOSC 2026** — Poster presented at CSIR-NIO Goa (Feb 23–26, 2026)
- **DSM Journal** — Submission currently under review
