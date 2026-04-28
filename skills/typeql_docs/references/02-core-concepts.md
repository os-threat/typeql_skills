# TypeQL Core Concepts (Deep Reference)

## Entities, Relations, Attributes

- **Entity**: identity-bearing concept instance.
- **Relation**: association instance with role semantics.
- **Attribute**: typed value-bearing instance.
- Model meaning in relations and roles, not in ad-hoc attribute blobs.

## Constraining Data

- Use statement-level and annotation constraints to enforce quality.
- Typical dimensions: ownership, role-playing validity, value domain limits, uniqueness.
- Keep constraints near schema definitions to centralize invariants.

## Schema and Data

- Schema defines permissible structure and constraints.
- Data must conform to schema; schema evolution should be explicit and reviewed.
- Use `redefine` for deliberate shape change instead of silent drift.

## Query Variables and Patterns

- Variables are co-binding anchors across patterns.
- Conjunctions tighten matches.
- Disjunctions broaden matches.
- Negations exclude matches; use with clear grounding.
- Optionals preserve partial bindings when some facts are absent.

## Query Clauses

- Build from strict matching into transformation/projection/mutation.
- Keep clause intent obvious: discovery, filtering, mutation, shaping.

## Queries as Functions

- Encapsulate repeated query logic.
- Prefer stable inputs/outputs over hidden assumptions.
- Use stream functions for set-like outputs and scalar for computed singular values.

## Invalid Patterns (Common Failure Modes)

- Unbound variables used in downstream clauses.
- Contradictory type/value constraints.
- Role usage incompatible with relation declaration.
- Unsupported pattern nesting or malformed negation/optional combinations.

## Glossary (Operational)

- **Binding**: concrete assignment to a variable.
- **Pattern**: logical condition over variables.
- **Pipeline**: ordered query stages.
- **Projection**: selected/output subset and shape.
- **Cardinality**: count of returned bindings/results.
