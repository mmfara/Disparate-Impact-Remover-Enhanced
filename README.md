# Enhanced Disparate Impact Remover (DIR⁺)

This repository provides a customized implementation of the **Disparate Impact Remover (DIR)** algorithm for preprocessing datasets to reduce algorithmic bias. This enhanced version—**DIR⁺**—supports **intersectional fairness** by applying group-wise feature repair across combinations of protected attributes (e.g., race *and* gender), with added controls for selective repair and group-level transparency.


## 🔍 Overview

Disparate Impact Remover is a fairness-aware preprocessing technique that modifies non-protected features to reduce disparate impact, while preserving within-group rank ordering. This implementation extends the original [AIF360 Disparate Impact Remover](https://aif360.readthedocs.io/en/stable/modules/generated/aif360.algorithms.preprocessing.DisparateImpactRemover.html) by introducing several key improvements:

**DIR⁺ extends this method by:**
- ✅ Supporting **intersectional repair** over multiple protected attributes (e.g., `race + gender`)
- ✅ Allowing **selective group repair** using the `groups_to_repair` parameter
- ✅ Skipping repair for **statistically small or fragile groups** via `min_group_size`
- ✅ Providing **transparent group-level logging** and diagnostics
- ✅ Maintaining compatibility with AIF360’s `Transformer` API

DIR⁺ is ideal for fairness research where intersectional group distinctions (e.g., Black women, Latino men) are crucial and statistical dignity must be preserved.

---

## 📦 Features

- Intersectional group handling via multiple protected attributes
- Adjustable repair intensity (`repair_level`)
- Minimum group size filtering (`min_group_size`)
- Transparent repair control (`groups_to_repair`)
- Robust error handling and verbose logging
- Drop-in compatible with `aif360`'s `Transformer`-based pipelines

---

## 🚀 Installation

Install the necessary dependencies:

bash
pip install aif360
pip install git+https://github.com/algofairness/BlackBoxAuditing

---

> Note: `BlackBoxAuditing` is required for the underlying repair algorithm.

## 🧪 Usage

python
from your_module import DisparateImpactRemover
from aif360.datasets import BinaryLabelDataset

# Load your dataset (BinaryLabelDataset)
dataset = ...

# Apply intersectional fairness repair
dir_plus = DisparateImpactRemover(
    repair_level=0.8,
    sensitive_attribute=['race', 'sex'],   # supports intersectional attributes
    min_group_size=30,
    groups_to_repair={'race=1|sex=0', 'race=1|sex=1'},
    verbose=True
)

repaired_dataset = dir_plus.fit_transform(dataset)

## ⚙️ Parameters

| Parameter             | Type               | Description |
|-----------------------|--------------------|-------------|
| `repair_level`        | `float`            | Degree of fairness repair. `0.0` means no repair; `1.0` means full repair. |
| `sensitive_attribute` | `str` or `list`    | Single or list of protected attribute names to define intersectional groups (e.g., `["race", "gender"]`). |
| `min_group_size`      | `int`              | Minimum number of instances required for a group to be eligible for repair. Smaller groups are skipped. |
| `groups_to_repair`    | `set` or `list`    | (Optional) Specific group labels to repair (e.g., `{"race=1|gender=0"}`). All other groups are skipped. |
| `verbose`             | `bool`             | If `True`, logs repair status for each group (repaired, skipped, excluded). Default is `True`. |


## 📚 Reference

This work builds on the method introduced in:

> Feldman, M., Friedler, S. A., Moeller, J., Scheidegger, C., & Venkatasubramanian, S. (2015).
> **Certifying and removing disparate impact.**
> *Proceedings of the 21st ACM SIGKDD International Conference on Knowledge Discovery and Data Mining.*

[https://doi.org/10.1145/2783258.2783311](https://doi.org/10.1145/2783258.2783311)

## 🛠 Notes

- 🛡️ Protected attributes are **restored unchanged** after repair to maintain dataset integrity.
- 🧬 Repairs are applied **independently to each group**, where groups are defined by combinations of protected attribute values (e.g., `race=1|gender=0`).
- 🚫 Groups that do not meet the `min_group_size` threshold are **automatically skipped** to prevent unreliable or noisy transformations.
- 🎯 If `groups_to_repair` is specified, only those exact groups will be repaired; all others are excluded—even if large.
- 📊 Verbose logging provides per-group repair status (`repaired`, `skipped`, `excluded`) for auditing and reproducibility.


## 📄 License

This project uses and extends open-source components from IBM's [AIF360](https://github.com/IBM/AIF360) and the [BlackBoxAuditing](https://github.com/algofairness/BlackBoxAuditing) toolkit.

## 📖 Citation

If you use this code in your research or applications, **please cite the following paper**:

> Farayola, Michael Mayowa, Malika Bendechache, Takfarinas Saber, Regina Connolly, and Irina Tal.  
> *Enhancing Algorithmic Fairness: Integrative Approaches and Multi-Objective Optimization Application in Recidivism Models*.  
> In **Proceedings of the 19th International Conference on Availability, Reliability and Security (ARES 2024)**, pages 1–10, ACM, 2024.  
> [https://doi.org/10.1145/3664476.3669978](https://doi.org/10.1145/3664476.3669978)

BibTeX:
bibtex
@inproceedings{farayola2024enhancing,
  title={Enhancing Algorithmic Fairness: Integrative Approaches and Multi-Objective Optimization Application in Recidivism Models},
  author={Farayola, Michael Mayowa and Bendechache, Malika and Saber, Takfarinas and Connolly, Regina and Tal, Irina},
  booktitle={Proceedings of the 19th International Conference on Availability, Reliability and Security},
  pages={1--10},
  year={2024}
}
