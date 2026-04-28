# TypeQL Docs Skill (Quick Core)

Use this skill as a practical, high-signal operating guide for writing, debugging, and optimizing TypeQL.  
For deeper detail, use the companion docs in `skills/typeql_docs/references/`.

## What TypeQL Is Best At

- Modeling domain meaning directly (entities, relations, attributes, roles, constraints).
- Querying connected facts with logical patterns instead of manual joins.
- Evolving schema and data with explicit, composable pipelines.
- Expressing reusable logic with functions and schema-level constraints.

## Workflow To Solve Real Tasks

1. Define or inspect schema shape (`define`, `redefine`, `undefine`).
2. Start query with a strict `match` block and explicit types.
3. Add retrieval / mutation stages (`fetch`, `insert`, `update`, `delete`, `put`).
4. Reduce result volume early (`select`, `require`, `distinct`, `sort`, `limit`, `offset`).
5. Validate assumptions with small slices, then scale.

## Golden Rules

- Always type your variables early (`$x isa person;`) to avoid wide scans.
- Prefer relation-role modeling over duplicating edge attributes.
- Use `fetch` to shape response payloads cleanly.
- Use `require` for mandatory bindings before mutation.
- Keep writes idempotent when possible (`put`).
- Push expensive logic later in the pipeline only when required.

## Core Query Patterns

### Read Pattern

```typeql
match
  $p isa person, has name $name;
  $name == "Alice";
fetch
  $p: { name: $name };
```

### Insert Pattern

```typeql
insert
  $p isa person, has name "Alice";
```

### Update Pattern (match then mutate)

```typeql
match
  $p isa person, has name "Alice";
update
  $p has email "alice@example.com";
```

### Safe Upsert Pattern

```typeql
put
  $p isa person, has name "Alice";
```

## Debugging Checklist

- Confirm each variable is bound where you expect.
- Remove clauses until result appears; then add constraints back incrementally.
- Check role labels in relations (`(employee: $p, employer: $c) isa employment;`).
- Validate value type compatibility (date vs datetime, decimal vs double, etc.).
- Check optional / negation usage for accidental broadening.

## Optimization Checklist

- Add type and attribute constraints as early as possible.
- Avoid unbounded disjunctions on high-cardinality variables.
- Limit result shape with `select` before heavy `fetch`.
- Use pagination (`sort` + `offset` + `limit`) for large traversals.
- Avoid repeated expensive subpatterns; factor with functions.

## SQL vs TypeQL Mental Mapping

- Table rows -> typed things (entities / relations / attributes).
- Foreign keys -> explicit relation instances and roles.
- JOIN conditions -> `match` pattern co-binding variables.
- Views / UDFs -> TypeQL functions.
- Constraints -> schema annotations and statement constraints.

## Schema Design Heuristics

- Use entities for independent identity-bearing things.
- Use relations for n-ary meaning and role semantics.
- Use attributes for values; constrain with `value`, `range`, `regex`, `values`.
- Mark identity attributes using key/uniqueness annotations.
- Keep subtype trees intentional; avoid deep inheritance without query value.

## Companion Deep References

- `references/01-guides.md`: read/insert/update pipelines, debugging, optimization, SQL mapping.
- `references/02-core-concepts.md`: model primitives, variables/patterns/clauses, invalid patterns, glossary.
- `references/03-reference-pipelines-functions-patterns.md`: execution pipeline and logic building blocks.
- `references/04-reference-schema-statements-annotations.md`: schema statements, constraints, annotations.
- `references/05-reference-values-expressions-keywords.md`: value system, expressions, operators, keywords.
- `references/99-source-map.md`: complete source URL map from the instruction set.
