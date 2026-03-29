# engine/signal-processing

This module ingests raw signals from external sources and normalizes them into a shared feature embedding space consumed by the correlation and attribution modules.

## Planned functionality

- Ingest network telemetry, TTP records, binary artifact metadata, and temporal event logs
- Normalize heterogeneous indicator types into a unified vector representation
- Apply source reliability weighting before passing features downstream
- Stream or batch processing modes depending on signal volume

See [docs/attribution-model.md](../../docs/attribution-model.md) for the full pipeline description.
