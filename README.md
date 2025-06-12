# Disparate Impact Remover (Intersectional Version)

This repository provides a customized implementation of the **Disparate Impact Remover (DIR)** algorithm for preprocessing datasets to reduce algorithmic bias. It supports **intersectional fairness** by applying group-wise feature repair independently across combinations of protected attributes (e.g., race *and* gender).

## 🔍 Overview

Disparate Impact Remover is a fairness-aware preprocessing technique that modifies non-protected features to reduce disparate impact, while preserving within-group ranking. This implementation extends the original [AIF360](https://github.com/IBM/AIF360) version by:

- Supporting **multiple** protected attributes (intersectional groups)
- Skipping repair on **small groups** (to avoid distortion)
- Handling **exceptions gracefully**
- Offering **verbose logging** for transparency

## 📦 Features

- ✅ Intersectional group handling
- ✅ Repair level control (`repair_level` between 0.0 and 1.0)
- ✅ Group-size threshold (`min_group_size`)
- ✅ Robust error handling and logging
- ✅ Compatible with `aif360`'s `Transformer` API

## 🚀 Installation

Make sure you have the following dependencies installed:

```bash
pip install aif360
pip install git+https://github.com/algofairness/BlackBoxAuditing
