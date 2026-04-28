# TypeQL Guides (Deep Reference)

## Read Data

- Start with `match` and constrain types immediately.
- Keep relation patterns role-explicit for readability and correctness.
- Shape output with `fetch`; avoid over-returning unneeded bindings.
- Add `select` when you need variable-level control before projection.

## Insert / Update Data

- `insert` creates new facts from explicit patterns.
- `update` mutates matched bindings; treat it as "match then apply".
- `delete` removes matched facts; narrow matching aggressively.
- `put` is the idempotent "ensure exists" style primitive.

## Advanced Pipelines

- Pipelines are composable stages: match -> transform/filter -> mutate/fetch.
- Stage order matters for both correctness and performance.
- Keep narrowing stages early; shape and aggregate later.

## Debugging TypeQL

- Verify each variable has at least one binding-producing clause.
- Isolate failing subpatterns by temporary minimization.
- Confirm relation role names and subtype assumptions.
- Check whether optional/negation changed expected cardinality.
- Check comparison/value expressions for type mismatches.

## Optimizing TypeQL

- Prefer high-selectivity constraints early.
- Avoid unconstrained fanout variables.
- Use `distinct` and pagination strategically on broad traversals.
- Avoid building huge intermediate sets before reductions.
- Factor repeated logic into functions for consistency and maintainability.

## SQL vs TypeQL

- TypeQL models semantics first; SQL models normalized row sets first.
- Joins become shared variables in declarative graph-like patterns.
- Rich domain constraints can move into schema rather than app code.
- N-ary relation semantics are first-class, not workaround tables.
