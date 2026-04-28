# Generate Hardened TypeQL Skill Pack (from Official Docs)

First you must copy skill file from https://github.com/CaliLuke/skills/blob/main/skills/typedb/SKILL.md to the file `skills\typedb\skill.md` in the skills folder.

The, read the official TypeDB/TypeQL docs listed below and generate a hardened, production-grade skill pack in `skills/typeql_docs/`.

Your output must be better than the copied document in `skills/typedb/skill.md` in:

- practical usability
- completeness of coverage
- debugging/ops guidance
- navigability and maintainability

## Core objective

Produce a two-layer documentation system:

1. `skills/typeql_docs/skill.md` as a practical quick-core for daily usage
2. deep references in `skills/typeql_docs/references/` for exhaustive lookup and implementation details

The docs must be executable and operational, not abstract summaries.

## Required output files

Create/update these files:

- `skills/typeql_docs/skill.md`
- `skills/typeql_docs/references/01-guides.md`
- `skills/typeql_docs/references/02-core-concepts.md`
- `skills/typeql_docs/references/03-reference-pipelines-functions-patterns.md`
- `skills/typeql_docs/references/04-reference-schema-statements-annotations.md`
- `skills/typeql_docs/references/05-reference-values-expressions-keywords.md`
- `skills/typeql_docs/references/06-operations-debugging-pitfalls-cli.md`
- `skills/typeql_docs/references/07-clause-by-clause-syntax.md`
- `skills/typeql_docs/references/08-pattern-cookbook.md`
- `skills/typeql_docs/references/99-source-map.md`

## Content and quality requirements

### Global standards

- Use clear, direct language suitable for engineers executing real tasks.
- Include runnable TypeQL snippets throughout.
- Prefer concrete "bad vs good" examples where ambiguity is common.
- Flag version-sensitive behavior with concise notes.
- Keep advice aligned to official docs; do not invent undocumented syntax.
- Avoid hand-wavy prose and generic filler.

### `skill.md` requirements (quick core)

Must include:

- transaction type quick reference (`schema`, `write`, `read`)
- query/pipeline mental model and ordered skeleton
- schema quickstart example
- CRUD core patterns (`insert`, `put`, `update`, `delete`)
- pattern logic (`or`, `not`, `try`) with examples
- stream controls and aggregation basics (`select`, `require`, `distinct`, `sort`, `offset`, `limit`, `reduce`)
- debugging checklist and optimization checklist
- SQL-to-TypeQL translation shortcuts
- common mistakes and fixes
- IID/type-label identity examples
- CLI/script conventions
- reference index linking all deep docs

### Deep reference file standards

For `01` through `08`:

- start with a `Mini TOC`
- include a `How to use this file:` line near the top
- end with `## Common mistakes in this section`
- include practical examples with schema-aware variable/role naming
- be precise and implementation-oriented

### File-specific requirements

- `01-guides.md`: workflow guidance across read/write/pipelines/debug/optimize/sql mapping
- `02-core-concepts.md`: semantic model, constraints, variable binding, pattern semantics, invalid patterns
- `03-reference-pipelines-functions-patterns.md`: clause-by-clause behavior, ordering, function definitions/invocations, safety rules
- `04-reference-schema-statements-annotations.md`: schema lifecycle, statement families, annotation behavior, modeling checklists
- `05-reference-values-expressions-keywords.md`: value types, operators, expressions, function notes, reserved keyword guidance
- `06-operations-debugging-pitfalls-cli.md`: transaction lifecycle, debugging workflow, high-impact pitfalls, CLI/script patterns
- `07-clause-by-clause-syntax.md`: copy-adapt templates for all major clauses
- `08-pattern-cookbook.md`: production-ready patterns (upsert, relation linking, updates/deletes, nested fetch, pagination, identity checks, anti-cartesian patterns)
- `99-source-map.md`: exact URL list from this document with no omissions

## Refinement loop (required)

After drafting:

1. Compare the generated set against `skills/typedb/skill.md`.
2. Identify missing practical capability areas.
3. Patch gaps until the generated set is stronger overall.
4. Ensure consistency of voice, section naming, and navigation.

The final result should read as one coherent, hardened doc system.

## TypeQL Guide

- https://typedb.com/docs/guides/typeql/read-data/
- https://typedb.com/docs/guides/typeql/insert-update-data/
- https://typedb.com/docs/guides/typeql/advanced-pipelines/
- https://typedb.com/docs/guides/typeql/debugging-typeql/
- https://typedb.com/docs/guides/typeql/optimizing-typeql/
- https://typedb.com/docs/guides/typeql/sql-vs-typeql/

## Core Concepts of TypeQL

- https://typedb.com/docs/core-concepts/typeql/entities-relations-attributes/
- https://typedb.com/docs/core-concepts/typeql/constraining-data/
- https://typedb.com/docs/core-concepts/typeql/schema-data/
- https://typedb.com/docs/core-concepts/typeql/query-variables-patterns/
- https://typedb.com/docs/core-concepts/typeql/query-clauses/
- https://typedb.com/docs/core-concepts/typeql/queries-as-functions/
- https://typedb.com/docs/core-concepts/typeql/invalid-patterns/
- https://typedb.com/docs/core-concepts/typeql/glossary/

## TypeQL Reference

- https://typedb.com/docs/typeql-reference/data-model/
- https://typedb.com/docs/typeql-reference/schema/
- https://typedb.com/docs/typeql-reference/schema/define/
- https://typedb.com/docs/typeql-reference/schema/undefine/
- https://typedb.com/docs/typeql-reference/schema/redefine/
- https://typedb.com/docs/typeql-reference/pipelines/
- https://typedb.com/docs/typeql-reference/pipelines/match/
- https://typedb.com/docs/typeql-reference/pipelines/fetch/
- https://typedb.com/docs/typeql-reference/pipelines/insert/
- https://typedb.com/docs/typeql-reference/pipelines/delete/
- https://typedb.com/docs/typeql-reference/pipelines/update/
- https://typedb.com/docs/typeql-reference/pipelines/put/
- https://typedb.com/docs/typeql-reference/pipelines/select/
- https://typedb.com/docs/typeql-reference/pipelines/require/
- https://typedb.com/docs/typeql-reference/pipelines/distinct/
- https://typedb.com/docs/typeql-reference/pipelines/sort/
- https://typedb.com/docs/typeql-reference/pipelines/limit/
- https://typedb.com/docs/typeql-reference/pipelines/offset/
- https://typedb.com/docs/typeql-reference/pipelines/reduce/
- https://typedb.com/docs/typeql-reference/pipelines/with/
- https://typedb.com/docs/typeql-reference/pipelines/end/
- https://typedb.com/docs/typeql-reference/functions/
- https://typedb.com/docs/typeql-reference/functions/writing/
- https://typedb.com/docs/typeql-reference/functions/stream/
- https://typedb.com/docs/typeql-reference/functions/scalar/
- https://typedb.com/docs/typeql-reference/functions/functions-vs-rules/
- https://typedb.com/docs/typeql-reference/patterns/
- https://typedb.com/docs/typeql-reference/patterns/conjunctions/
- https://typedb.com/docs/typeql-reference/patterns/disjunctions/
- https://typedb.com/docs/typeql-reference/patterns/negations/
- https://typedb.com/docs/typeql-reference/patterns/optionals/
- https://typedb.com/docs/typeql-reference/statements/
- https://typedb.com/docs/typeql-reference/statements/entity/
- https://typedb.com/docs/typeql-reference/statements/relation/
- https://typedb.com/docs/typeql-reference/statements/attribute/
- https://typedb.com/docs/typeql-reference/statements/sub/
- https://typedb.com/docs/typeql-reference/statements/relates/
- https://typedb.com/docs/typeql-reference/statements/plays/
- https://typedb.com/docs/typeql-reference/statements/value/
- https://typedb.com/docs/typeql-reference/statements/owns/
- https://typedb.com/docs/typeql-reference/statements/alias/
- https://typedb.com/docs/typeql-reference/statements/isa/
- https://typedb.com/docs/typeql-reference/statements/links/
- https://typedb.com/docs/typeql-reference/statements/has/
- https://typedb.com/docs/typeql-reference/statements/is/
- https://typedb.com/docs/typeql-reference/statements/let-eq/
- https://typedb.com/docs/typeql-reference/statements/let-in/
- https://typedb.com/docs/typeql-reference/statements/label/
- https://typedb.com/docs/typeql-reference/statements/iid/
- https://typedb.com/docs/typeql-reference/statements/comparisons/
- https://typedb.com/docs/typeql-reference/annotations/
- https://typedb.com/docs/typeql-reference/annotations/card/
- https://typedb.com/docs/typeql-reference/annotations/independent/
- https://typedb.com/docs/typeql-reference/annotations/abstract/
- https://typedb.com/docs/typeql-reference/annotations/key/
- https://typedb.com/docs/typeql-reference/annotations/subkey/
- https://typedb.com/docs/typeql-reference/annotations/unique/
- https://typedb.com/docs/typeql-reference/annotations/values/
- https://typedb.com/docs/typeql-reference/annotations/range/
- https://typedb.com/docs/typeql-reference/annotations/regex/
- https://typedb.com/docs/typeql-reference/annotations/distinct/
- https://typedb.com/docs/typeql-reference/values/
- https://typedb.com/docs/typeql-reference/values/boolean/
- https://typedb.com/docs/typeql-reference/values/integer/
- https://typedb.com/docs/typeql-reference/values/double/
- https://typedb.com/docs/typeql-reference/values/decimal/
- https://typedb.com/docs/typeql-reference/values/string/
- https://typedb.com/docs/typeql-reference/values/date/
- https://typedb.com/docs/typeql-reference/values/datetime/
- https://typedb.com/docs/typeql-reference/values/datetimetz/
- https://typedb.com/docs/typeql-reference/values/duration/
- https://typedb.com/docs/typeql-reference/expressions/
- https://typedb.com/docs/typeql-reference/expressions/literals/
- https://typedb.com/docs/typeql-reference/expressions/operators/
- https://typedb.com/docs/typeql-reference/expressions/function-calls/
- https://typedb.com/docs/typeql-reference/expressions/structs/
- https://typedb.com/docs/typeql-reference/expressions/lists/
- https://typedb.com/docs/typeql-reference/keywords/