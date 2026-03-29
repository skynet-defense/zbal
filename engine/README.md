# engine

The engine directory contains the core processing modules of zbal. Signals flow through these modules in order:

1. **[signal-processing/](signal-processing/)** — Ingest and normalize raw signals into feature vectors
2. **[correlation/](correlation/)** — Cluster related signals and compute coherence scores
3. **[attribution/](attribution/)** — Apply Dempster-Shafer evidence combination over the belief lattice
4. **[scoring/](scoring/)** — Rank hypotheses and produce final attribution scores

See the `docs/` directory for design documentation covering each stage.
