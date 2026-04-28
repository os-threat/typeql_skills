# TypeQL Pattern Cookbook (Production-Oriented)

This file provides ready-to-adapt patterns for common workloads.

How to use this file: choose the nearest workload pattern, validate it with a read pass, then execute the write variant in the correct transaction type.

## Mini TOC

- Idempotent upsert
- Relation linking
- Attribute replacement patterns
- Nested fetch
- Optional + require flow
- Symmetric dedupe
- Aggregation and pagination
- Identity and type lookup
- Safe mutation workflows

## 1) Idempotent entity upsert

```typeql
put
  $p isa person, has email "alice@example.com", has name "Alice";
```

## 2) Create relation between existing entities

```typeql
match
  $p isa person, has email "alice@example.com";
  $c isa company, has name "Acme";
insert
  (employee: $p, employer: $c) isa employment;
```

## 3) Replace single-cardinality attribute

```typeql
match
  $p isa person, has email "alice@example.com";
update
  $p has status "inactive";
```

## 4) Replace multi-valued attribute

```typeql
match
  $p isa person, has email "alice@example.com", has tag $old;
delete
  $p has $old;
insert
  $p has tag "priority";
```

## 5) Fetch nested related data

```typeql
match
  $c isa company, has name "Acme";
fetch {
  "company": $c.name,
  "employees": [
    match
      (employer: $c, employee: $p) isa employment;
    fetch { "name": $p.name, "email": $p.email }
  ]
};
```

## 6) Optional enrichment with required downstream field

```typeql
match
  $p isa person, has name $n;
  try { $p has email $e; };
require $e;
fetch { "name": $n, "email": $e };
```

## 7) Unique pairs for symmetric relation

```typeql
match
  friendship (friend: $p1, friend: $p2);
  $p1 has email $e1;
  $p2 has email $e2;
  $e1 < $e2;
fetch { "pair": [$e1, $e2] };
```

## 8) Grouped aggregate report

```typeql
match
  $c isa company, has industry $ind;
  (employer: $c, employee: $e) isa employment;
reduce
  $count = count groupby $ind;
```

## 9) Anti-Cartesian pattern

```typeql
# Prefer this:
match
  $p isa person;
  (employee: $p, employer: $c) isa employment;
  $c isa company;
```

Avoid unconstrained `$p` + `$c` in the same match scope.

## 10) Identity-safe non-self relation traversal

```typeql
match
  friendship (friend: $p1, friend: $p2);
  not { $p1 is $p2; };
fetch { "a": $p1, "b": $p2 };
```

## 11) Exact-type filtering

```typeql
match
  $p isa! person;
fetch { "person": $p };
```

## 12) Paginated deterministic search

```typeql
match
  $p isa person, has name $n;
sort $n asc;
offset 100;
limit 25;
fetch { "name": $n };
```

## 13) Validate candidate delete before mutation

```typeql
match
  $u isa user, has status "inactive";
fetch { "candidate": $u.email };
```

Then execute delete in a separate write transaction.

## 14) Type + IID lookup pattern

```typeql
match
  $x iid 0x1f0005000000000000012f;
  $x isa! $t;
fetch {
  "iid": iid($x),
  "type": label($t)
};
```

## 15) Keyword-safe naming convention

Prefer suffixes/prefixes if labels may collide with language keywords.
Example: use `match_` instead of `match`.

## Common mistakes in this section

- Applying cookbook snippets without checking schema role/type names.
- Skipping deterministic sort before offset/limit pagination.
- Performing multi-step write patterns in the wrong transaction type.
