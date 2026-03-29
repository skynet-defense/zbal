# Attribution Model

## Overview

The zbal attribution model identifies the origin of adversarial signals by combining multi-source evidence into a unified probability distribution over threat actors. It operates on normalized feature vectors produced by the signal-processing and correlation subsystems, and outputs a ranked list of candidate actors with associated confidence scores.

## Goals

- Attribute observed signals to known or novel threat actors with calibrated confidence
- Distinguish direct attacks from false-flag operations through deception scoring
- Provide interpretable evidence chains that support human analyst review

## Architecture

The attribution pipeline has three stages:

### 1. Feature Extraction

Raw signals (network telemetry, TTPs, timing metadata, linguistic markers) are ingested from the `engine/signal-processing` module. Each signal is projected into a shared embedding space so that cross-domain indicators can be compared.

### 2. Lattice-Based Evidence Aggregation

Extracted features are mapped onto the belief lattice (see [lattice-design.md](lattice-design.md)). The lattice represents partial orderings of actor hypotheses. Evidence updates propagate upward and downward through the lattice using Dempster-Shafer combination rules, resolving conflicts produced by ambiguous or potentially fabricated indicators.

### 3. Confidence Scoring

For each candidate actor node in the lattice, the attribution engine emits:
- **Attribution score** — posterior probability that the actor is responsible
- **Deception penalty** — reduction applied when false-flag indicators are detected (see [false-flag-analysis.md](false-flag-analysis.md))
- **Evidence breadth** — number of independent signal sources supporting the attribution

The final ranked list is produced by the `engine/scoring` module and exposed through the `api` layer.

## Inputs

| Field | Type | Description |
|---|---|---|
| `signal_id` | string | Unique identifier for the raw signal |
| `feature_vector` | float[] | Normalized embedding from signal processing |
| `timestamp` | ISO 8601 | Time of signal observation |
| `source_confidence` | float [0,1] | Reliability weight of the originating sensor |

## Outputs

| Field | Type | Description |
|---|---|---|
| `actor_id` | string | Identifier of the candidate threat actor |
| `attribution_score` | float [0,1] | Posterior attribution probability |
| `deception_penalty` | float [0,1] | False-flag adjustment applied |
| `evidence_sources` | string[] | List of contributing signal IDs |

## Limitations

- Attribution confidence degrades when observed TTPs are widely shared across actor groups
- The model assumes the feature embedding space is current; stale embeddings reduce accuracy
- Deception penalty heuristics are calibrated on historical false-flag datasets and may not generalize to novel techniques
