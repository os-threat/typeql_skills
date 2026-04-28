# TypeQL Docs Skill Pack

Practical TypeQL skill system for day-to-day engineering work.

This folder is designed for two speeds:

- quick execution from `skill.md`
- deep, task-specific lookup from `references/`

## How to use this pack

1. Start with `skill.md` for the shortest path to a working query.
2. Jump to the relevant file in `references/` for deeper syntax, semantics, or troubleshooting.
3. Use `references/07-clause-by-clause-syntax.md` when you need exact clause templates.
4. Use `references/08-pattern-cookbook.md` when you need production-ready patterns.
5. Use `references/99-source-map.md` to verify source coverage from official docs.

## File map

- `skill.md`
  - Practical core: pipeline model, CRUD patterns, stream/aggregation basics, debugging/optimization checklists, common mistakes, IID/type-label, CLI notes.

- `references/01-guides.md`
  - Guide-oriented workflows: read/write/pipelines/debug/optimize and SQL-to-TypeQL mapping.

- `references/02-core-concepts.md`
  - Conceptual model and correctness: entities/relations/attributes, constraints, variable binding semantics, invalid pattern families.

- `references/03-reference-pipelines-functions-patterns.md`
  - Pipeline clause behavior, stage ordering, function definition/invocation patterns, and pattern safety rules.

- `references/04-reference-schema-statements-annotations.md`
  - Schema lifecycle, statement families, annotation behavior, and schema review checklists.

- `references/05-reference-values-expressions-keywords.md`
  - Value types, operators, expression rules, function notes, and keyword safety.

- `references/06-operations-debugging-pitfalls-cli.md`
  - Transaction lifecycle, debugging workflows, high-impact pitfalls, and CLI/script conventions.

- `references/07-clause-by-clause-syntax.md`
  - Copy-adapt syntax templates for each major clause.

- `references/08-pattern-cookbook.md`
  - Production patterns for upsert, relation linking, updates/deletes, nested fetch, pagination, and identity-safe traversal.

- `references/99-source-map.md`
  - Full URL map of official source material used to generate this pack.

## Suggested navigation by task

- **I need a query now** -> `skill.md`
- **I need exact syntax for a clause** -> `references/07-clause-by-clause-syntax.md`
- **I need a proven pattern** -> `references/08-pattern-cookbook.md`
- **My query is wrong or slow** -> `references/06-operations-debugging-pitfalls-cli.md`
- **I am designing or changing schema** -> `references/04-reference-schema-statements-annotations.md`
- **I need deeper conceptual grounding** -> `references/02-core-concepts.md`
- **I need source traceability** -> `references/99-source-map.md`

## Maintenance notes

- Keep examples runnable and schema-aware (role labels and variable names should be explicit).
- Keep `skill.md` compact; move deeper expansions into `references/`.
- Update `references/99-source-map.md` whenever sources change.
