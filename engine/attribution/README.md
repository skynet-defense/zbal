# engine/attribution

This module implements the belief lattice and Dempster-Shafer evidence combination logic that produces attribution hypotheses from correlated signal features.

## Planned functionality

- Build and update the actor hypothesis lattice from `datasets/`
- Accept feature vectors and coherence scores from upstream modules
- Apply false-flag trustworthiness discounts (`α`) before evidence combination
- Propagate belief and plausibility values through the lattice
- Emit ranked attribution hypotheses with confidence intervals

See [docs/lattice-design.md](../../docs/lattice-design.md) and [docs/attribution-model.md](../../docs/attribution-model.md) for design details.
