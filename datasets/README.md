# datasets

This directory holds the curated datasets used to train and evaluate the zbal attribution and false-flag models.

## Contents (planned)

- `actors/` — Actor profile records including known TTPs, infrastructure patterns, and operating windows
- `baselines/` — TTP and indicator co-occurrence frequency tables used by the false-flag coherence scorer
- `incidents/` — Labeled historical incidents for backtesting attribution accuracy

## Versioning

Datasets are versioned with a date-stamped schema so that engine checkpoints can be linked to a specific dataset version. The schema definition will be published in `docs/` once the data model is finalized.
