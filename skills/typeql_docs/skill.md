# TypeQL Docs Skill (Practical Core)

This file is the fast, practical core for day-to-day TypeQL work.
Use it first; then open the deep references in `skills/typeql_docs/references/`.
Use this sequence consistently: start with core patterns here, then move to syntax and cookbook references for exact implementation details.

## 0) Quick Operations Reference

### Transaction types

| Type | Use For | Finalize |
| --- | --- | --- |
| `schema` | `define` / `redefine` / `undefine` and schema-level functions | `commit` |
| `write` | `insert` / `put` / `update` / `delete` | `commit` |
| `read` | `match` / `fetch` / `reduce` / stream operations | `close` |

### Query skeleton

```text
[with ...]
[define|undefine|redefine]
[match]
[insert|put|update|delete]
[select|require|distinct|sort|offset|limit]
[reduce ...]
[fetch ...]
;
```

## 1) Query Pipeline Mental Model

Think in ordered stages:

1. **Bind data**: `match`
2. **Mutate data**: `insert`, `put`, `update`, `delete`
3. **Filter/shape stream**: `select`, `require`, `distinct`, `sort`, `offset`, `limit`
4. **Aggregate**: `reduce`
5. **Project output**: `fetch`

Recommended operating sequence for most reads:

```typeql
match
  ...strict bindings...;
select ...needed vars...;
distinct;
sort ...;
offset ...;
limit ...;
fetch { ... };
```

## 2) Golden Rules (High ROI)

- Type variables early (`$p isa person;`) to avoid accidental fanout.
- Join facts through shared variables or relation roles; never leave variables unconnected.
- Use role-labeled relations for clarity and correctness.
- Prefer `put` for idempotent ingestion jobs.
- Use `require` after optional logic so downstream stages are guaranteed bound.
- Treat `sort` as expensive: it may require processing full candidate sets.
- Validate match-only results before running write clauses.

## 3) Schema Quickstart

```typeql
define
  attribute name, value string;
  attribute email, value string;
  attribute joined_at, value datetime;
  attribute status, value string @values("active", "inactive");

  relation employment,
    relates employer,
    relates employee;

  entity company,
    owns name @key,
    plays employment:employer;

  entity person,
    owns name,
    owns email @key,
    owns status,
    owns joined_at,
    plays employment:employee;
```

Schema evolution:

- `define`: add new schema.
- `redefine`: modify existing type/capability/annotations.
- `undefine`: remove schema components.

## 4) Core Read/Write Patterns

### Read (match + fetch)

```typeql
match
  $p isa person, has email "alice@example.com", has name $name;
fetch {
  "name": $name
};
```

### Insert

```typeql
insert
  $p isa person,
    has name "Alice",
    has email "alice@example.com",
    has status "active";
```

### Link existing entities via relation

```typeql
match
  $p isa person, has email "alice@example.com";
  $c isa company, has name "Acme";
insert
  (employee: $p, employer: $c) isa employment;
```

### Idempotent upsert-style write (`put`)

```typeql
put
  $p isa person,
    has email "alice@example.com",
    has name "Alice";
```

### Update matched entities

```typeql
match
  $p isa person, has email "alice@example.com";
update
  $p has status "inactive";
```

### Delete matched facts

```typeql
match
  $p isa person, has email "alice@example.com";
delete
  $p;
```

## 5) Pattern Logic You Will Use Constantly

### Disjunction (`or`)

```typeql
match
  $p isa person;
  { $p has status "active"; } or { $p has status "trial"; };
fetch { "person": $p };
```

### Negation (`not`)

```typeql
match
  $p isa person;
  not { $p has status "inactive"; };
fetch { "person": $p };
```

### Optional (`try`)

```typeql
match
  $p isa person, has name $n;
  try { $p has email $e; };
require $n;
fetch {
  "name": $n,
  "email": $e
};
```

## 6) Stream Controls + Aggregation

```typeql
match
  $p isa person, has name $n;
select $p, $n;
distinct;
sort $n asc;
offset 0;
limit 20;
fetch { "name": $n };
```

```typeql
match
  $c isa company;
  (employer: $c, employee: $e) isa employment;
reduce
  $count = count groupby $c;
```

Common reducers: `count`, `sum`, `min`, `max`, `mean`, `median`, `std`, `list`.

## 7) High-Value Debugging Procedure

1. Run the `match` clause alone.
2. Add `reduce $count = count;` to verify cardinality.
3. Add one suspicious constraint at a time.
4. Confirm relation role names exactly match schema.
5. Confirm value types (`date` vs `datetime`, `double` vs `decimal`).
6. Re-run as read query before each write mutation.

Minimal debug scaffold:

```typeql
match
  ...candidate pattern...
reduce
  $count = count;
```

## 8) High-Value Optimization Procedure

- Start with highest selectivity constraints.
- Keep high-cardinality variables tightly bound.
- Avoid broad `or` branches before narrowing.
- Paginate deterministic result sets (`sort` + `offset` + `limit`).
- Reduce payload size with `select` and focused `fetch`.
- Move repeated heavy patterns into functions.

## 9) SQL to TypeQL Translation Shortcuts

- SQL table row -> TypeQL thing instance.
- SQL foreign key -> relation role participation.
- SQL JOIN -> shared variable bindings in `match`.
- SQL projection -> `fetch` object/list shaping.
- SQL aggregation -> `reduce`.
- SQL UDF/view-like reuse -> TypeQL functions.

## 10) Common Mistakes (And Fixes)

### Mistake: Cross join by unconnected vars

```typeql
# Bad
match
  $p isa person;
  $c isa company;
```

```typeql
# Good
match
  $p isa person;
  (employee: $p, employer: $c) isa employment;
  $c isa company;
```

### Mistake: Assuming `match` is sequential

All `match` statements are conjunctive constraints on one logical binding set.

### Mistake: Using optional branch vars as guaranteed

Use `require` when a later stage depends on a variable potentially bound in `try`.

### Mistake: Symmetric relation duplicates

For symmetric role structures, enforce a stable asymmetric filter when you need unique pairs.

```typeql
match
  friendship (friend: $p1, friend: $p2);
  $p1 has email $e1;
  $p2 has email $e2;
  $e1 < $e2;
fetch { "pair": [$e1, $e2] };
```

### Mistake: Wrong relation syntax form

If no relation variable is needed, prefer anonymous relation syntax:

```typeql
(employee: $p, employer: $c) isa employment;
```

If relation variable is needed:

```typeql
$rel isa employment, links (employee: $p, employer: $c);
```

## 11) IID, Type Labels, and Identity Checks

```typeql
match
  $p iid 0x1f0005000000000000012f;
fetch { "person": { $p.* } };
```

```typeql
match
  $p isa! $t, has email "alice@example.com";
  $t sub person;
fetch {
  "iid": iid($p),
  "type": label($t)
};
```

Use `is` when checking identity equivalence between variables in pattern logic.

## 12) CLI and Script Conventions

```bash
typedb console --command "database list"
typedb console --command "transaction mydb schema" --command "define entity person;"
```

- Console control commands do not use TypeQL semicolon style.
- Query statements inside transactions should terminate correctly.
- In scripts, follow explicit transaction lifecycle (`transaction` -> query -> `commit`/`close`).

## 13) Version and Inference Note

- Validate syntax/feature support against your deployed TypeDB version.
- If your environment has deprecated rules in favor of functions, model reusable logic with functions.

## 14) Deep References

- `references/01-guides.md`: practical workflows from official guide topics.
- `references/02-core-concepts.md`: conceptual model and correctness rules.
- `references/03-reference-pipelines-functions-patterns.md`: pipeline, function, and pattern syntax detail.
- `references/04-reference-schema-statements-annotations.md`: schema statements and annotation behavior.
- `references/05-reference-values-expressions-keywords.md`: value types, operators, expressions, and keyword rules.
- `references/06-operations-debugging-pitfalls-cli.md`: transaction lifecycle, debugging workflow, pitfalls, and CLI patterns.
- `references/07-clause-by-clause-syntax.md`: copy-adapt syntax templates for every major clause.
- `references/08-pattern-cookbook.md`: production-ready query and mutation pattern library.
- `references/99-source-map.md`: canonical source URL map.
