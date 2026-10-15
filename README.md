# SolubilityAI 🧪

**A reproducible, CPU-friendly molecular machine learning project for aqueous solubility prediction.**

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/RIDDHI1624/SolubilityAI/blob/main/SolubilityAI_GitHub.ipynb)

## Objective
Predict experimental logS from molecular SMILES using RDKit Morgan fingerprints (radius 2, 2,048 bits) and a scikit-learn Random Forest regressor. Dataset: [AqSolDB curated-solubility-dataset.csv](https://github.com/semmatoninn/AqSolDB).

## Results from the original Colab run

| Split | Training molecules | Test molecules | MAE (logS) | RMSE (logS) | R² |
|---|---:|---:|---:|---:|---:|
| Random 80/20 | 7,984 | 1,996 | 1.1357 | 1.5112 | 0.6108 |
| Bemis–Murcko scaffold-group | 8,760 | 1,220 | 1.4686 | 1.8331 | 0.5091 |

One scaffold split had **zero scaffold-group overlap**. Because the two evaluations use different training and test compositions, the performance difference cannot be attributed solely to scaffold novelty. These are recorded experimental metrics; the clean notebook has not been re-executed by this packaging step.

### Error analysis

- The random-split model showed high MAE for poorly soluble compounds (logS below −6: MAE 2.458).
- On the 1,220 scaffold-held-out molecules, LogP above 4 had MAE **2.203**, versus **1.095** for LogP 2–4. This is an observational association, not a causal explanation.
- The notebook evaluates ten largest held-out errors and showcases predictions for aspirin, caffeine, and paracetamol (not externally validated).

## Run

1. Open `SolubilityAI_GitHub.ipynb` in Google Colab and select a **CPU** runtime.
2. Run the notebook cells top to bottom. The first code cell installs requirements.
3. The notebook downloads AqSolDB from the public GitHub source. It writes figures, result CSVs, and a model artifact into `outputs/` (which is ignored in Git).

No Google Drive mounting or paid resources are needed. Internet access is required for the initial dataset download and dependency installation. Training time and metrics can vary with library/runtime versions.

## Methods

- Read curated AqSolDB CSV; validate SMILES with RDKit, canonicalize structures and average same-canonical-SMILES measurements.
- Build Morgan fingerprints as `uint8` arrays.
- Train mean baseline and 200-tree Random Forest (random split).
- Evaluate with MAE, RMSE and R², predicted-versus-experimental and error-range plots.
- Train fresh Random Forest using a `GroupShuffleSplit` on Bemis–Murcko scaffolds; evaluate held-out molecules.
- Inspect outliers with RDKit MolLogP and molecular-weight descriptors.

## Files

- `SolubilityAI_GitHub.ipynb`: self-contained notebook
- `requirements.txt`: Python dependencies
- `.gitignore`: ignores generated models/results and local files

## Caveats

This is a **research/portfolio demonstration**, not validated drug-development guidance. The data-cleaning procedure does not fully standardize salts, tautomers, stereochemistry, ionization or experimental conditions. Single-split results do not estimate performance variance, and example-molecule inference is not external validation. Morgan fingerprints can hash different environments to the same position. Read LogP-error associations as exploratory.

## Dataset attribution

Source: [AqSolDB repository](https://github.com/semmatoninn/AqSolDB). Please consult the original data repository and its accompanying publication/license before redistributing data. Original software code in this repository may be licensed separately.
