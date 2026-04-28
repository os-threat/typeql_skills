# TypeQL Core Concepts (Detailed)

How to use this file: use it to validate modeling decisions and query reasoning before implementing or debugging syntax-level details.

## Mini TOC

- Data Model Primitives
- Constraints as First-Class Semantics
- Schema vs Data Separation
- Variables and Binding Semantics
- Pattern Semantics
- Clause Semantics
- Queries as Functions
- Invalid Pattern Families
- Glossary

## 1) Data Model Primitives

- **Entity**: independent concept identity (for example, person, company).
- **Relation**: semantic connection with roles (for example, employment with employer/employee).
- **Attribute**: typed value concept (for example, email, created_at).

Design heuristic:

- If something has standalone identity -> entity.
- If something describes a connection among participants -> relation.
- If something is a value with a domain/type -> attribute.

## 2) Constraints as First-Class Semantics

Constraints should be pushed into schema whenever possible:

- Ownership constraints (`owns`)
- Role participation constraints (`plays`, `relates`)
- Value-domain constraints (`@range`, `@regex`, `@values`)
- Identity/uniqueness constraints (`@key`, `@unique`, `@subkey`)
- Cardinality and independence controls (`@card`, `@independent`, `@distinct`)

This reduces duplicated validation in application code.

## 3) Schema vs Data Separation

- **Schema layer**: types, inheritance, capabilities, constraints.
- **Data layer**: concrete instances conforming to schema.

Evolution commands:

- `define`: introduce schema
- `redefine`: modify existing schema behavior
- `undefine`: remove schema behavior/types

## 4) Variables and Binding Semantics

Variables bind concepts/values and become the "join keys" of TypeQL.

```typeql
match
  $p isa person;
  (employee: $p, employer: $c) isa employment;
  $c has name "Acme";
```

`$p` and `$c` are bound through relation participation, not through foreign-key fields.

## 5) Pattern Semantics

### Conjunction

Multiple statements in `match` are conjunctive constraints over the same binding set.

### Disjunction (`or`)

Any branch can satisfy the disjunctive region.

### Negation (`not`)

Excludes bindings for which nested pattern exists.

### Optional (`try`)

Allows partial enrichment without rejecting a base match.

## 6) Clause Semantics

- `match`: discover and constrain bindings.
- write clauses: mutate matched/new facts.
- stream clauses: reduce/shape output stream.
- `reduce`: aggregate over bindings.
- `fetch`: serialize chosen values in output structure.

## 7) Queries as Functions

Functions package reusable logic with explicit interfaces.

- Stream return: multiple results.
- Scalar return: single computed result.

Prefer functions for repeated query fragments and domain computations.

## 8) Invalid Pattern Families (Common)

- Using variables downstream that are not guaranteed bound upstream.
- Contradictory constraints in the same binding scope.
- Role labels not valid for a relation type.
- Logical branch variable leakage (`or`/`try`) into guaranteed context.
- Type/value mismatch in comparisons and assignments.

## 9) Glossary

- **Binding**: assignment of a variable to concept/value.
- **Pattern**: constraint expression over variables.
- **Pipeline**: ordered stage execution.
- **Projection**: selected and shaped output.
- **Cardinality**: number of bindings/results at a stage.
- **Selectivity**: how strongly a constraint shrinks candidate bindings.

## Common mistakes in this section

- Modeling semantic relations as plain attributes.
- Leaving invariants in app code instead of schema constraints.
- Assuming branch-local variables are globally available.
