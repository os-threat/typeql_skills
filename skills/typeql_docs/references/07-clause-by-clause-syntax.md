# TypeQL Clause-by-Clause Syntax Cookbook

This file is a compact "how to write it" reference by clause.

How to use this file: copy the closest clause template, then adapt variables, labels, and constraints to your schema and transaction intent.

## Mini TOC

- Schema Clauses
- Data Query/Write Clauses
- Stream/Projection Clauses
- Function Clauses
- Pattern Operators
- Ordering and Safety Notes

## Schema Clauses

### `define`

```typeql
define
  attribute email, value string;
  entity person, owns email @key;
```

### `redefine`

```typeql
redefine
  person owns email @unique;
```

### `undefine`

```typeql
undefine
  owns email from person;
```

## Data Query/Write Clauses

### `match`

```typeql
match
  $p isa person, has email $e;
  $e contains "@example.com";
```

### `insert`

```typeql
insert
  $p isa person, has email "a@example.com";
```

### `put`

```typeql
put
  $p isa person, has email "a@example.com", has name "A";
```

### `update`

```typeql
match
  $p isa person, has email "a@example.com";
update
  $p has status "active";
```

### `delete`

```typeql
match
  $p isa person, has email "a@example.com";
delete
  $p;
```

## Stream/Projection Clauses

### `select`

```typeql
match
  $p isa person, has name $n, has email $e;
select $p, $n;
```

### `require`

```typeql
match
  $p isa person;
  try { $p has email $e; };
require $e;
```

### `distinct`

```typeql
match
  $p isa person, has name $n;
distinct;
```

### `sort`

```typeql
match
  $p isa person, has name $n;
sort $n asc;
```

### `offset` / `limit`

```typeql
match
  $p isa person, has name $n;
sort $n asc;
offset 20;
limit 20;
```

### `reduce`

```typeql
match
  $p isa person;
reduce
  $count = count;
```

### `fetch`

```typeql
match
  $p isa person, has name $n;
fetch {
  "name": $n
};
```

## Function Clauses

### `fun ... -> { ... }` (stream)

```typeql
define
  fun active_emails() -> { string }:
    match $u isa user, has status "active", has email $e;
    return { $e };
```

### `fun ... -> scalar`

```typeql
define
  fun user_count() -> integer:
    match $u isa user;
    return count;
```

### `with` inline function

```typeql
with
  fun org_users($org: string) -> { person }:
    match
      $c isa company, has name == $org;
      (employer: $c, employee: $p) isa employment;
    return { $p };
match
  let $p in org_users("Acme");
fetch { "p": $p };
```

## Pattern Operators

### `or`

```typeql
match
  $p isa person;
  { $p has status "active"; } or { $p has status "trial"; };
```

### `not`

```typeql
match
  $p isa person;
  not { $p has status "inactive"; };
```

### `try`

```typeql
match
  $p isa person, has name $n;
  try { $p has email $e; };
fetch { "name": $n, "email": $e };
```

## Ordering and Safety Notes

- Keep reads deterministic with `sort` before pagination.
- Keep writes preceded by sufficiently narrow `match`.
- Use `require` when later clauses depend on optionally bound vars.
- Use relation instance variables only when needed; otherwise use anonymous relation syntax.

## Common mistakes in this section

- Copying a clause template without adapting variable binding context.
- Using query skeletons from different transaction intents without adjustment.
- Forgetting final statement termination where required by execution context.
