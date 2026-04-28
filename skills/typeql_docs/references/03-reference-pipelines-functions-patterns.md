# TypeQL Reference: Pipelines, Functions, Patterns

## Pipelines

- `match`: bind variables using logical patterns.
- `fetch`: project structured output payloads.
- `insert`: add new facts.
- `delete`: remove matched facts.
- `update`: mutate matched facts.
- `put`: ensure facts exist (idempotent-oriented writes).
- `select`: keep only required variables.
- `require`: enforce variable presence before subsequent stages.
- `distinct`: deduplicate results.
- `sort`: order result bindings.
- `limit` / `offset`: pagination control.
- `reduce`: aggregate or fold bindings.
- `with`: configure options or contextual modifiers.
- `end`: terminate pipeline scope where applicable.

## Functions

- Define reusable query logic with explicit inputs/outputs.
- Stream functions: return multiple results/bindings.
- Scalar functions: return singular computed values.
- Prefer functions for repeatable logic; use rules for inference-style domain axioms.

## Patterns

- Conjunctions: all constraints must hold.
- Disjunctions: any branch may hold.
- Negations: exclude branch matches with care.
- Optionals: allow partial enrichment while keeping base bindings.

## Practical Composition Rules

- Start strict: type + key/value constraints first.
- Expand only when necessary (optional/disjunction).
- Keep mutation steps after verification steps (`require`, narrow `match`).
- For large traversals, pair deterministic `sort` with pagination.
