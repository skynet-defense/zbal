# Roadmap

## Current Status

zbal is in early development. The core documentation and directory structure are in place. The following roadmap tracks planned milestones toward a functional attribution engine.

## Milestones

### Milestone 1 — Foundation (current)

- [x] Define directory structure
- [x] Document attribution model (`docs/attribution-model.md`)
- [x] Document lattice design (`docs/lattice-design.md`)
- [x] Document false-flag analysis (`docs/false-flag-analysis.md`)
- [ ] Define data schemas for signals, indicators, and actor profiles

### Milestone 2 — Engine Core

- [ ] Implement signal ingestion and normalization (`engine/signal-processing/`)
- [ ] Implement cross-signal correlation (`engine/correlation/`)
- [ ] Implement Dempster-Shafer lattice with evidence combination (`engine/attribution/`)
- [ ] Implement attribution scoring and ranking (`engine/scoring/`)
- [ ] Unit and integration tests for all engine components (`tests/`)

### Milestone 3 — Datasets and Baselines

- [ ] Curate initial actor profile dataset (`datasets/`)
- [ ] Build TTP co-occurrence baseline for false-flag detection
- [ ] Establish versioning and update cadence for datasets
- [ ] Publish methodology notes alongside reference datasets (`papers/`)

### Milestone 4 — API Layer

- [ ] Design REST API schema for signal submission and attribution queries (`api/`)
- [ ] Implement authentication and rate limiting
- [ ] Build configuration endpoints for false-flag thresholds and scoring weights
- [ ] Publish OpenAPI specification

### Milestone 5 — Validation and Red-Teaming

- [ ] Backtest attribution accuracy against labeled historical incidents
- [ ] Red-team false-flag detection with synthetic deception scenarios
- [ ] Evaluate lattice performance on ambiguous/shared-TTP cases
- [ ] Publish evaluation results (`papers/`)

### Milestone 6 — Production Readiness

- [ ] Performance benchmarking and latency optimization
- [ ] Observability: metrics, logging, tracing
- [ ] Deployment documentation
- [ ] Security review of API and data handling

## Design Principles

- **Calibrated uncertainty** — every output includes confidence bounds; no binary verdicts
- **Interpretability** — attribution results are traceable to specific evidence sources
- **Deception awareness** — the model treats planted evidence as a first-class concern, not an afterthought
- **Dataset transparency** — training data and versioning are tracked alongside model artifacts

## Contributing

Contributions are tracked through GitHub issues and pull requests. Before opening a PR for engine code, please review the relevant design document in `docs/` and ensure new code is covered by tests in `tests/`.
