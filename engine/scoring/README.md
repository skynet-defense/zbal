# engine/scoring

This module converts raw lattice belief/plausibility intervals into final attribution scores and deception penalties exposed through the API.

## Planned functionality

- Rank candidate actors by posterior attribution probability
- Compute deception penalty from false-flag module output
- Calculate evidence breadth (number of independent source signals)
- Format results into the API response schema defined in `api/`

See [docs/attribution-model.md](../../docs/attribution-model.md) for the output schema.
