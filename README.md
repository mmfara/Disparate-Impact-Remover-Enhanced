# Disparate Impact Remover (Intersectional Version)

This repository provides a customized implementation of the **Disparate Impact Remover (DIR)** algorithm for preprocessing datasets to reduce algorithmic bias. It supports **intersectional fairness** by applying group-wise feature repair independently across combinations of protected attributes (e.g., race *and* gender).

## 🔍 Overview

Disparate Impact Remover is a fairness-aware preprocessing technique that modifies non-protected features to reduce disparate impact, while preserving within-group ranking. This implementation extends the original [AIF360]([https://github.com/IBM/AIF360](https://aif360.readthedocs.io/en/stable/modules/generated/aif360.algorithms.preprocessing.DisparateImpactRemover.html#id2)) version by:

- Supporting **multiple** protected attributes (intersectional groups)
- Skipping repair on **small groups** (to avoid distortion)
- Handling **exceptions gracefully**
- Offering **verbose logging** for transparency

## 📦 Features

- Intersectional group handling
- Repair level control (`repair_level` between 0.0 and 1.0)
- Group-size threshold (`min_group_size`)
- Robust error handling and logging
- Compatible with `aif360`'s `Transformer` API

## 🚀 Installation

Make sure you have the following dependencies installed:

```bash
pip install aif360
pip install git+https://github.com/algofairness/BlackBoxAuditing

````

> Note: `BlackBoxAuditing` is required for the underlying repair algorithm.

## 🧪 Usage

```python
from your_module import DisparateImpactRemover
from aif360.datasets import BinaryLabelDataset

# Load your dataset (BinaryLabelDataset)
dataset = ...

# Apply repair
dir = DisparateImpactRemover(
    repair_level=0.8,
    sensitive_attribute=['race', 'sex'],  # supports multiple!
    min_group_size=30,
    verbose=True
)
repaired_dataset = dir.fit_transform(dataset)
```

## ⚙️ Parameters

| Parameter             | Description                                        |
| --------------------- | -------------------------------------------------- |
| `repair_level`        | Degree of fairness repair (0.0 = none, 1.0 = full) |
| `sensitive_attribute` | Single or list of protected attribute names        |
| `min_group_size`      | Minimum size of a group to apply repair (int)      |
| `verbose`             | Enable/disable logging (bool)                      |

## 📚 Reference

This work builds on the method introduced in:

> Feldman, M., Friedler, S. A., Moeller, J., Scheidegger, C., & Venkatasubramanian, S. (2015).
> **Certifying and removing disparate impact.**
> *Proceedings of the 21st ACM SIGKDD International Conference on Knowledge Discovery and Data Mining.*

[https://doi.org/10.1145/2783258.2783311](https://doi.org/10.1145/2783258.2783311)

## 🛠 Notes

* Protected attributes are **restored unchanged** after repair.
* Repair is applied **per group**, defined by combinations of protected attribute values.
* Small groups are **skipped** to avoid noisy transformations.

## 📄 License

This project uses and extends open-source components from IBM's [AIF360](https://github.com/IBM/AIF360) and the [BlackBoxAuditing](https://github.com/algofairness/BlackBoxAuditing) toolkit.

## 📖 Citation

If you use this code in your research or applications, **please cite the following paper**:

> Farayola, Michael Mayowa, Malika Bendechache, Takfarinas Saber, Regina Connolly, and Irina Tal.  
> *Enhancing Algorithmic Fairness: Integrative Approaches and Multi-Objective Optimization Application in Recidivism Models*.  
> In **Proceedings of the 19th International Conference on Availability, Reliability and Security (ARES 2024)**, pages 1–10, ACM, 2024.  
> [https://doi.org/10.1145/3664476.3669978](https://doi.org/10.1145/3664476.3669978)

BibTeX:
```bibtex
@inproceedings{farayola2024enhancing,
  title={Enhancing Algorithmic Fairness: Integrative Approaches and Multi-Objective Optimization Application in Recidivism Models},
  author={Farayola, Michael Mayowa and Bendechache, Malika and Saber, Takfarinas and Connolly, Regina and Tal, Irina},
  booktitle={Proceedings of the 19th International Conference on Availability, Reliability and Security},
  pages={1--10},
  year={2024}
}
