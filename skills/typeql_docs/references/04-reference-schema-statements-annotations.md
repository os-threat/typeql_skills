# TypeQL Reference: Schema, Statements, Annotations

## Schema Lifecycle

- `define`: create schema structures and capabilities.
- `undefine`: remove schema elements.
- `redefine`: modify existing schema elements safely and explicitly.

## Statement Families

- Type declarations: `entity`, `relation`, `attribute`.
- Hierarchy: `sub`, `alias`, `label`.
- Relation behavior: `relates`, `plays`, `links`.
- Ownership/value: `owns`, `has`, `value`.
- Instance typing and identity: `isa`, `iid`, `is`.
- Local binding helpers: `let =` and `let in`.
- Comparisons: equality/order/set membership style constraints.

## Annotation Families

- Structural/cardinality: `card`, `distinct`, `independent`.
- Type semantics: `abstract`.
- Identity/uniqueness: `key`, `subkey`, `unique`.
- Value-domain controls: `values`, `range`, `regex`.

## Modeling Recommendations

- Use `key`/`unique` intentionally for canonical identity.
- Prefer explicit `relates`/`plays` constraints for relation correctness.
- Use value-domain annotations to shift validation from application code to schema.
- Keep subtype hierarchies meaningful and query-driven.
