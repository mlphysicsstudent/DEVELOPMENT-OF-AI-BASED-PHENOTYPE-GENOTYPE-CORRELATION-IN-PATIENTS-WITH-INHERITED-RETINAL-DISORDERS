# DEVELOPMENT-OF-AI-BASED-PHENOTYPE-GENOTYPE-CORRELATION-IN-PATIENTS-WITH-INHERITED-RETINAL-DISORDERS

Code for the paper **"Development and Evaluation of AI-Based Phenotype–Genotype Correlation in Patients with Inherited Retinal Disorders"** (submitted to IEEE ISBI 2027).

This repository studies whether ultra-widefield (UWF) retinal images can be used to (1) detect inherited retinal dystrophies (IRDs), (2) classify the clinical phenotype, and (3) predict the causative gene, under strict patient-level evaluation.

## Overview

- **Backbone:** frozen DINOv2 ViT-S/14 features extracted from masked UWF images at 980 px
- **Detection:** one-class Gaussian mixture model (K = 8) fitted on normal images only, with a threshold calibrated on held-out normal images
- **Classification:** dual-moment pooling (mean ‖ std of patch features), PCA (q = 15) and nested leave-one-patient-out classifier selection (LogReg, SVM, LDA, RF, kNN)
- **End-to-end baseline:** linear head on the [CLS] token, with the last four transformer blocks fine-tuned
- **Analyses:** five-backbone comparison, per-gene learning curves, and offline augmentation with and without patient leakage

## Results

| Task | Metric | Frozen-feature | End-to-end |
|---|---|---|---|
| IRD vs normal (566 images) | AUC | 0.961 | 0.974 |
| Phenotype (7 classes) | Top-1 | 64.8% | 56.8% |
| Genotype (4 genes) | Top-1 | 62.9% | 45.7% |
| Genotype (39 genes) | Top-1 | 21.6% | 12.5% |

- Genotype accuracy rose from 0% to 71.4% as the number of training patients per gene increased.
- A deliberately leaked split reached 92.1% on the 39-gene task, compared with 21.6% under clean patient-level cross-validation. This shows how patient leakage can inflate reported accuracy.

## Repository structure

```
├── preprocessing/     # retinal mask (ellipse fit, annotation removal)
├── features/          # DINOv2 feature extraction and dual-moment pooling
├── detection/         # one-class GMM detector and calibration
├── classification/    # nested CV classifiers and end-to-end fine-tuning
├── analysis/          # learning curves, backbone comparison, augmentation/leakage
└── figures/           # scripts for the paper figures
```

## Requirements

```
python >= 3.10
torch
scikit-learn
numpy
opencv-python
```

## Data

The UWF images were collected at the IRD clinic of Dr. R.P. Centre for Ophthalmic Sciences, AIIMS, New Delhi, under institutional ethics approval. **The patient data are not included in this repository** and cannot be shared publicly.

## Citation

If you use this code, please cite:

```
R. K. Rathor and V. C, "Development and Evaluation of AI-Based
Phenotype-Genotype Correlation in Patients with Inherited Retinal
Disorders," submitted to IEEE ISBI, 2027.
```

## Acknowledgments

This study was funded by the Indian Council of Medical Research (ICMR).
