# Personalized Healthcare Recommendations (PHR) via Knowledge Graph Link Prediction (KGLP)

This repository provides a reproducible Python implementation for the paper:

Personalized Healthcare Recommendations for Diabetic Patients using Knowledge Graph Link Prediction

It trains a GNN-based Knowledge Graph Embedding (KGE) model for link prediction and reports
filtered MRR and Hits@K (K = 1, 3, 10), aggregated across multiple seeds as mean ± 95% confidence interval.

Note on data licensing: If you use licensed datasets (e.g., SNOMED-CT, DrugBank), do not upload raw files to GitHub.
This repo includes sample toy triples and expects you to download and preprocess the original sources locally.

## Repository structure

```
PHR-KGLP/
  phrkg/                 # core library (data, model, training, evaluation)
  configs/               # YAML configs for HKG and KEM
  data/                  # sample toy triples + data instructions
  scripts/               # helper run scripts
  outputs/               # generated reports/models (gitignored)
  refs.bib               # BibTeX (paper)
  CITATION.cff           # GitHub citation metadata
  LICENSE
  requirements.txt
```

## Installation

Create a virtual environment and install dependencies:

```bash
python -m venv .venv
source .venv/bin/activate   # Linux/Mac
# .venv\Scripts\activate    # Windows
pip install -r requirements.txt
```

## Quickstart (toy example)

### Run link prediction on HKG toy triples
```bash
python -m phrkg.run_experiments --config configs/hkg.yaml
```

### Run link prediction on KEM toy triples
```bash
python -m phrkg.run_experiments --config configs/kem.yaml
```

Results are written to:
- `outputs/hkg/report.yaml`
- `outputs/kem/report.yaml`

Each report contains per-seed metrics and the aggregate mean ± 95% CI.

## Using your real datasets

1. Convert your KG to a CSV file with columns:
   - `head, relation, tail`

2. Update the config (e.g., `configs/hkg.yaml`) to point to your file:
   - `dataset.triples_path: data/<your_triples>.csv`

3. Run:
```bash
python -m phrkg.run_experiments --config configs/hkg.yaml
```

## Reproducibility settings

- Multiple seeds are configured in `configs/*.yaml` (`training.seeds`)
- Results are reported as **mean ± 95% CI** (Student's t distribution)
- Early stopping uses validation MRR (`training.patience`)

## How to reproduce paper tables

- Link prediction: use `outputs/*/report.yaml` for MRR and HR@K
- Recommendation tables (P@K/R@K/NDCG): integrate your downstream ranking module and run evaluation similarly across seeds.

## Citation

See `CITATION.cff` (preferred) or `refs.bib`.

## License

This project is released under the MIT License (see `LICENSE`).
