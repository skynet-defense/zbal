# Lattice Design

## Overview

The zbal belief lattice is the core data structure used to represent and reason over competing attribution hypotheses. It encodes partial-order relationships between actor groups and sub-groups, enabling evidence to propagate across related hypotheses rather than being evaluated in isolation.

## Motivation

Threat actor attribution is rarely binary. Observed indicators often match multiple actors simultaneously, and some actors share infrastructure, tooling, or tactics. A flat probability model discards this relational structure. The lattice preserves it, allowing evidence that rules out a parent group to automatically down-weight all child nodes, and evidence that implicates a sub-group to partially support its parent.

## Structure

### Nodes

Each node in the lattice represents an actor hypothesis at a specific granularity level:

- **Level 0 (top)**: Universal hypothesis — "some actor is responsible"
- **Level 1**: Nation-state affiliations or major criminal ecosystems
- **Level 2**: Specific threat actor groups (e.g., campaign clusters)
- **Level 3**: Individual operators or infrastructure segments
- **Level ⊥ (bottom)**: Contradiction node — no valid attribution

Nodes also carry metadata including known TTPs, historical activity windows, and geographic operating areas.

### Edges

Directed edges encode subsumption: an edge from node A to node B means "B is a specialization of A." Belief assigned to B is always ≤ belief assigned to A. Evidence inconsistent with A propagates down to reduce belief in all descendants.

### Belief Functions

Each node stores a pair `(belief, plausibility)` from Dempster-Shafer theory:
- `belief(H)` — the minimum probability that H is correct given current evidence
- `plausibility(H)` — the maximum probability that H could be correct if all ambiguous evidence were resolved in its favor

The interval `[belief, plausibility]` represents epistemic uncertainty. A narrow interval indicates high confidence; a wide interval indicates ambiguity.

## Combination Rules

When two independent evidence sources E₁ and E₂ arrive, their mass functions are combined using Dempster's rule:

```
m₁₂(A) = Σ_{B∩C=A} m₁(B)·m₂(C) / (1 - K)
```

where `K` is the conflict mass (probability assigned to empty intersections). High conflict (`K > 0.7`) triggers a false-flag warning to the `engine/correlation` module.

## Deception Handling

When the false-flag detector (see [false-flag-analysis.md](false-flag-analysis.md)) raises a deception flag for a set of indicators, the corresponding mass functions are discounted before combination:

```
mᵈ(A) = α·m(A)  for A ≠ Θ
mᵈ(Θ) = 1 - α·(1 - m(Θ))
```

where `α ∈ [0,1]` is the trustworthiness score assigned to those indicators.

## Persistence

The lattice is stored as a directed acyclic graph in the `datasets/` directory. Nodes and edges are serialized in JSON-Lines format. The graph is versioned alongside the training datasets so that model checkpoints remain reproducible.

## Example

```
Level 0:  [ all actors ]
             /        \
Level 1:  [Nation A]  [Nation B]
             |              |
Level 2: [Group X]      [Group Y]  [Group Z]
             |
Level 3: [Operator 1]  [Operator 2]
```

Evidence strongly implicating Group X raises belief at Group X, Nation A, and (to a lesser extent) Level 0. It lowers plausibility for Nation B and its descendants.
