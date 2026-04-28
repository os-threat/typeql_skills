# TypeQL Reference: Pipelines, Functions, Patterns (Detailed)

How to use this file: treat it as the authoritative clause and composition reference when writing or reviewing executable TypeQL pipelines.

## Mini TOC

- Pipeline Clauses
- Stage Ordering Guidance
- Functions
- Pattern Reference
- Pattern-Safety Rules

## 1) Pipeline Clauses

### `match`

Binds variables using type/value/relation constraints.

```typeql
match
  $p isa person, has email $e;
  $e contains "@example.com";
```

### `fetch`

Projects binding results to object/list structures.

```typeql
match
  $p isa person, has name $n;
fetch {
  "name": $n
};
```

### `insert`

Adds new facts.

```typeql
insert
  $p isa person, has name "Alice";
```

### `delete`

Removes matched facts.

```typeql
match
  $p isa person, has email "alice@example.com";
delete
  $p;
```

### `update`

Mutates matched facts.

```typeql
match
  $p isa person, has email "alice@example.com";
update
  $p has status "inactive";
```

### `put`

Idempotent-oriented creation/ensure behavior.

```typeql
put
  $p isa person, has email "alice@example.com", has name "Alice";
```

### `select`

Restricts visible variable set.

```typeql
match
  $p isa person, has name $n, has email $e;
select $p, $n;
```

### `require`

Ensures selected vars are bound before continuing.

```typeql
match
  $p isa person;
  try { $p has email $e; };
require $e;
fetch { "email": $e };
```

### `distinct`

Deduplicates stream rows.

### `sort`

Orders rows by variable values, for example `sort $n asc;`.

### `offset` / `limit`

Pagination primitives.

### `reduce`

Aggregation stage.

```typeql
match
  $p isa person;
reduce
  $count = count;
```

Additional reducer examples:

```typeql
match
  $p isa person, has salary $s;
reduce
  $max = max($s),
  $min = min($s),
  $avg = mean($s);
```

```typeql
match
  $c isa company, has industry $ind;
  (employer: $c, employee: $e) isa employment;
reduce
  $count = count groupby $ind;
```

### `with` / `end`

Used for context and scoped composition (including inline function scenarios).

## 2) Stage Ordering Guidance

Recommended read order:

`match -> select -> require -> distinct -> sort -> offset -> limit -> fetch`

Recommended aggregate order:

`match -> reduce -> fetch` (or consume reduced output directly)

## 3) Functions

### 3.1 Function definition shape

```typeql
define
  fun active_user_emails() -> { string }:
    match
      $u isa user, has status "active", has email $e;
    return { $e };
```

### 3.2 Scalar function shape

```typeql
define
  fun active_user_count() -> integer:
    match
      $u isa user, has status "active";
    return count;
```

### 3.3 Function invocation pattern

```typeql
match
  let $emails in active_user_emails();
fetch { "emails": $emails };
```

### 3.4 Inline function with `with`

```typeql
with
  fun active_in_org($org_name: string) -> { person }:
    match
      $c isa company, has name == $org_name;
      (employer: $c, employee: $p) isa employment;
    return { $p };

match
  let $p in active_in_org("Acme");
fetch { "person": $p };
```

## 4) Pattern Reference

### Conjunction

All statements in `match` are conjunctive.

### Disjunction

```typeql
match
  $p isa person;
  { $p has status "active"; } or { $p has status "trial"; };
```

### Negation

```typeql
match
  $p isa person;
  not { $p has status "inactive"; };
```

### Optional

```typeql
match
  $p isa person;
  try { $p has email $e; };
```

## 5) Pattern-Safety Rules

- Do not assume branch-local variables are globally bound.
- Keep disjunction branches type-compatible when downstream logic expects common vars.
- Use `require` when later stages must consume optionally bound vars.
- Keep relation roles explicit to reduce ambiguity.
- Prefer anonymous relation syntax unless relation instance variable is needed.

## Common mistakes in this section

- Misordering stream clauses and getting unstable pagination/results.
- Using `label(...)` or function invocation patterns with incompatible variable kinds.
- Forgetting `require` after `try` when downstream clauses depend on optional vars.
