# Rigorous QSAR Modeling for CDK2 Inhibitor Bioactivity Prediction

[![Python 3.10+](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![RDKit](https://img.shields.io/badge/Chemoinformatics-RDKit-green.svg)](https://www.rdkit.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Project Overview
This repository contains a complete Machine Learning QSAR pipeline designed to classify human **Cyclin-Dependent Kinase 2 (CDK2)** inhibitors ($IC_{50} \le 1\ \mu\text{M}$) using binding bioactivity data curated from ChEMBL (`CHEMBL301`).

The core scientific focus compares **Validation Splitting Strategies** (Random vs. Bemis–Murcko Scaffold vs. Prospective Time-Based) to demonstrate benchmark performance overestimation in drug discovery workflows.

---

## Key Findings

1. **Scaffold Split Inflation:** Bemis–Murcko scaffold splitting ($0.935$ ROC-AUC) performs identically to random splitting ($0.929$ ROC-AUC), failing to reflect prospective screening difficulty.
2. **Prospective Recall Collapse:** Under prospective temporal splitting (pre-2021 train | 2021–2025 test), model performance drops to **$0.726$ ROC-AUC** and Recall collapses to **$62.3\%$**, proving that temporal chemical shifts introduce novel motifs that standard splits conceal.
3. **Mechanistic SHAP Drivers:** TreeSHAP interpretability mapped top active feature contributions to polar surface area (`TPSA`) and specific structural bits (`Bit_1152`, `Bit_74`), aligning with known CDK2 hinge-region hydrogen bonding requirements (Glu81/Leu83).
4. **Applicability Domain Filtering:** Nearest-neighbor Tanimoto distance filtering ($T_{\text{cutoff}} = 0.60$) successfully flagged **$12.7\%$** of test compounds as Out-of-Domain (OOD), recovering model trust.

---

## Multi-Regime Validation Comparison

| Split Regime | ROC-AUC | Accuracy | Precision | Recall | Test Samples |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Random Split** | **0.929** | **85.3%** | **86.6%** | **85.8%** | **566** |
| **Scaffold Split** | **0.935** | **87.8%** | **92.5%** | **88.8%** | **565** |
| **Time-Based Split** | **0.726** | **66.2%** | **81.7%** | **62.3%** | **458** |

---

## Applicability Domain Assessment

* **Total Test Set:** 566 compounds
* **In-Domain (Safe Predictions):** 494 compounds ($87.3\%$)
* **Out-of-Domain (OOD Flagged):** 72 compounds ($12.7\%$)

---

## Repository Contents
* `cdk2_curated_bioactivity.csv`: Raw cleaned ChEMBL301 assay dataset ($N = 2,829$).
* `cdk2_featurized_dataset.csv`: Concatenated feature matrix ($2,053$ descriptors + bits).
* `shap_summary_plot.png`: SHAP beeswarm interpretability plot.
* `applicability_domain_distribution.png`: Nearest-neighbor Tanimoto similarity distribution.
