# TypeQL Operations, Debugging, Pitfalls, and CLI

How to use this file: use it when queries misbehave in runtime, when performance regresses, or when CLI/script execution differs from expectations.

## Mini TOC

- Transaction Lifecycle
- Debugging Workflow
- High-Impact Pitfalls
- CLI/Console Patterns

## 1) Transaction Lifecycle

- `schema` and `write` transactions end with `commit`.
- `read` transactions should be closed without commit.
- Validate read results before applying writes.

## 2) Debugging Workflow

### Stepwise isolation

```typeql
match
  $p isa person, has name $n;
reduce
  $count = count;
```

Add constraints one-by-one until row count drops unexpectedly.

### Prefix testing for multi-stage pipelines

Run successive prefixes:

1. `match` only
2. `match -> reduce`
3. `match -> reduce -> filter`
4. full write/fetch stage

This pinpoints where data flow disappears.

## 3) High-Impact Pitfalls

### 3.1 Cartesian growth from unconnected variables

```typeql
# Bad
match
  $p isa person;
  $c isa company;
```

Fix by connecting them through relation or shared variable constraints.

### 3.2 Match is conjunctive, not sequential

Every statement in the same `match` must be true for one binding.

### 3.3 Scope hazards in disjunction/optional

Do not assume a variable bound in one logical branch is guaranteed downstream.
Use `require` for mandatory downstream variables.

### 3.4 Symmetric relation duplicates

Apply deterministic asymmetry for unique pair output.

### 3.5 Role-label mismatches

Role labels must match schema relation role definitions exactly.

### 3.6 Expensive sorting

`sort` can require full candidate processing before `offset`/`limit`.
Constrain as much as possible before sorting.

## 4) CLI/Console Patterns

```bash
# Server command
typedb console --command "database list"

# Open transaction and run schema query
typedb console --command "transaction mydb schema" --command "define entity person;"
```

### Script shape reminder

```text
transaction mydb write
insert
  $p isa person, has email "alice@example.com";
commit
```

Keep query syntax and transaction-control syntax clearly separated in scripts.

## Common mistakes in this section

- Running destructive writes before validating candidate sets with read queries.
- Mixing transaction-control commands with query syntax conventions.
- Treating poor performance symptoms without first checking cardinality growth.
