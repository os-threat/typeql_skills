# TypeQL Guides (Detailed Practical Reference)

This file tracks the guide-oriented workflow: read, write, pipelines, debug, optimize, SQL translation.

How to use this file: start with the workflow section closest to your task, copy a baseline pattern, then adapt variable names, role labels, and constraints to your schema.

## Mini TOC

- Read Data
- Insert / Update / Delete / Put
- Advanced Pipelines
- Debugging Playbook
- Optimization Playbook
- SQL vs TypeQL Translation Cheatsheet

## 1) Read Data

### 1.1 Baseline read pattern

```typeql
match
  $p isa person, has name $name;
fetch {
  "name": $name
};
```

### 1.2 Read by relation roles

```typeql
match
  $c isa company, has name "Acme";
  (employer: $c, employee: $p) isa employment;
  $p has name $employee_name;
fetch {
  "employee": $employee_name
};
```

### 1.3 Read with stream controls

```typeql
match
  $p isa person, has joined_at $ts, has name $n;
sort $ts desc;
offset 0;
limit 50;
fetch {
  "name": $n,
  "joined_at": $ts
};
```

## 2) Insert / Update / Delete / Put

### 2.1 Insert new instances

```typeql
insert
  $p isa person, has email "alice@example.com", has name "Alice";
```

### 2.2 Insert with pre-match linkage

```typeql
match
  $p isa person, has email "alice@example.com";
  $c isa company, has name "Acme";
insert
  (employee: $p, employer: $c) isa employment;
```

### 2.3 Update matched facts

```typeql
match
  $p isa person, has email "alice@example.com";
update
  $p has status "inactive";
```

### 2.4 Delete narrowly

```typeql
match
  $p isa person, has email "alice@example.com", has nickname $nick;
delete
  $p has $nick;
```

### 2.5 Idempotent writes with put

```typeql
put
  $p isa person, has email "alice@example.com", has name "Alice";
```

## 3) Advanced Pipelines

Key idea: use early stages to constrain cardinality, late stages to shape output.

```typeql
match
  $p isa person, has status $s, has name $n;
  { $s == "active"; } or { $s == "trial"; };
select $p, $n;
distinct;
sort $n asc;
limit 100;
fetch {
  "name": $n
};
```

## 4) Debugging Playbook

### 4.1 Bisect a failing match

```typeql
match
  $p isa person, has name $n;
  # (employee: $p, employer: $c) isa employment;
  # $c has name "Acme";
reduce
  $count = count;
```

If count > 0, commented region likely contains the issue.

### 4.2 Check role correctness

```typeql
# If schema says employment relates employer and employee:
match
  (employee: $p, employer: $c) isa employment;
```

Typos or wrong role labels are common root causes.

### 4.3 Check variable grounding in logical branches

```typeql
# Bad: $c may be unbound
match
  $p isa person;
  { (employee: $p, employer: $c) isa employment; } or { $p has status "freelancer"; };
  not { $c has name "BadCorp"; };
```

Fix by binding `$c` outside branch if needed downstream.

## 5) Optimization Playbook

- Add type + selective value constraints first.
- Avoid unconnected variable families (Cartesian growth).
- Use `select` to drop unused vars before heavy `fetch`.
- Use deterministic pagination for large scans.
- Reduce branch breadth in `or` where possible.
- Favor reusable functions for repeated heavy subpatterns.

## 6) SQL vs TypeQL Translation Cheatsheet

- SQL `JOIN` -> variable co-binding / relation pattern in `match`.
- SQL `WHERE` -> comparison + pattern constraints.
- SQL `GROUP BY` + aggregates -> `reduce ... groupby`.
- SQL projection -> `fetch`.
- SQL idempotent merge/upsert workflows -> `put` patterns.

## Common mistakes in this section

- Treating guide workflows as strict syntax rules instead of adaptable patterns.
- Running write patterns without validating candidate matches first.
- Translating SQL joins as disconnected variables instead of shared bindings.
