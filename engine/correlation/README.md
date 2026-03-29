# engine/correlation

This module groups related signals into coherent intrusion clusters and detects cross-source indicator overlap before evidence is passed to the attribution engine.

## Planned functionality

- Temporal and infrastructure-based signal clustering
- Compute intra-cluster coherence scores used by false-flag analysis
- Raise conflict warnings when Dempster-Shafer combination mass exceeds `K > 0.7`
- Produce correlation matrices exported to `datasets/` for baseline updates

See [docs/lattice-design.md](../../docs/lattice-design.md) for how conflict mass feeds into the lattice.
