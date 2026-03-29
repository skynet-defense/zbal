# False Flag Analysis

## Overview

False flag operations are deliberate efforts by a threat actor to make their activities appear to originate from a different actor. The zbal false-flag analysis module identifies indicators of deception within observed signals and adjusts attribution confidence accordingly.

## Threat Model

Adversaries may plant false flags by:

1. **TTP imitation** — adopting tools, techniques, and procedures (TTPs) publicly associated with another group
2. **Infrastructure borrowing** — routing traffic through IP ranges or ASNs associated with a known actor
3. **Language/encoding artifacts** — embedding character sets, comments, or error messages from a different language community
4. **Timing manipulation** — scheduling operations to coincide with known working hours of another actor's time zone
5. **Metadata injection** — inserting compiler paths, PDB strings, or build metadata from another actor's known toolchain

## Detection Pipeline

### Stage 1: Indicator Extraction

The signal-processing module produces typed indicators for each signal:
- `ttp_indicators` — mapped to MITRE ATT&CK technique IDs
- `infra_indicators` — IP, ASN, domain, certificate fingerprint
- `artifact_indicators` — binary metadata, string artifacts, encoding
- `temporal_indicators` — timestamps, inter-event timing distributions

### Stage 2: Coherence Scoring

A coherence score is computed for each candidate attribution by measuring the internal consistency of indicators:

- **Intra-source coherence**: Do the TTPs, infrastructure, and artifacts from a single intrusion cluster form a consistent operational profile?
- **Cross-source coherence**: Do multiple independent signals point to the same actor using the same combination of indicators?

Low intra-source coherence (indicators that are inconsistent with each other) is a strong deception signal. For example, Chinese-language artifacts combined with Russian working-hour timing has low coherence.

### Stage 3: Historical Baseline Comparison

Indicator combinations are compared against the historical baseline stored in `datasets/`. The baseline records the co-occurrence frequencies of TTPs, infrastructure choices, and artifact types for each known actor group. A combination that is highly unusual for the alleged actor but common in the datasets of another group triggers a false-flag score increase.

### Stage 4: Deception Flag Emission

If the false-flag score exceeds a configurable threshold, a deception flag is emitted with:

| Field | Type | Description |
|---|---|---|
| `flag_id` | string | Unique identifier for this deception assessment |
| `suspected_plant` | string[] | Indicators believed to be planted |
| `planting_actor` | string | Hypothesized actual actor |
| `deception_score` | float [0,1] | Confidence that deception is occurring |
| `affected_signals` | string[] | Signal IDs whose attribution is affected |

## Integration with Attribution Model

Deception flags feed back into the lattice (see [lattice-design.md](lattice-design.md)) through the trustworthiness discount factor `α`. A high deception score for an indicator set reduces the weight that set carries in Dempster-Shafer combination, preventing planted evidence from dominating the attribution result.

The final attribution output always includes a `deception_penalty` field so that analysts can see how much false-flag discounting was applied.

## Limitations and Known Weaknesses

- The baseline comparison requires a comprehensive historical dataset; novel actors or first-use indicators produce inconclusive results
- Sophisticated adversaries can achieve high intra-source coherence by meticulously studying and replicating another actor's full operational style
- The module can only flag deception where planted indicators are detectable; covert false flags that leave no anomalous signals are not covered
- Threshold tuning involves a precision-recall tradeoff: lower thresholds reduce missed false flags at the cost of discounting legitimate evidence

## Configuration

Key parameters are controlled via `api/` configuration endpoints:

| Parameter | Default | Description |
|---|---|---|
| `deception_threshold` | 0.6 | Minimum deception score to emit a flag |
| `coherence_window` | 30 days | Lookback window for baseline comparison |
| `discount_floor` | 0.1 | Minimum trustworthiness (`α`) after discounting |
