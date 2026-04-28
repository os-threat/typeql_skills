# TypeQL Reference: Schema, Statements, Annotations (Detailed)

How to use this file: use it while designing or evolving schema so constraints and capabilities are explicit before data loading or query tuning.

## Mini TOC

- Schema Lifecycle Commands
- Core Statement Families
- Example Schema Block
- Annotation Reference
- Modeling Pitfalls
- Schema Review Checklist

## 1) Schema Lifecycle Commands

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

## 2) Core Statement Families

### Type declarations

- `entity`
- `relation`
- `attribute`

### Hierarchy and labels

- `sub`: subtype relationship
- `alias`: alternate naming
- `label`: label-oriented references

### Relation/role capabilities

- `relates`: declare relation roles
- `plays`: allow a type to play a role
- `links`: relation-player links in data/query patterns

### Ownership/value capabilities

- `owns`: schema ownership capability
- `has`: instance-level ownership statement
- `value`: attribute value type declaration

### Identity/typing statements

- `isa`: instance-type relationship
- `iid`: internal identifier access/match
- `is`: identity equivalence check between variables

### Local binding statements

- `let ... = ...`
- `let ... in ...`

### Comparisons

- `==`, `!=`, `<`, `<=`, `>`, `>=`
- text operators such as `like`, `contains`

## 3) Example Schema Block (Integrated)

```typeql
define
  attribute email, value string;
  attribute name, value string;
  attribute tenure_years, value integer @range(0..80);

  relation employment,
    relates employer,
    relates employee;

  entity company,
    owns name @key,
    plays employment:employer;

  entity person,
    owns email @key,
    owns name,
    owns tenure_years,
    plays employment:employee;
```

## 4) Annotation Reference

### Structural

- `@card(...)`: cardinality limits
- `@distinct`: uniqueness behavior at ownership/relation scope
- `@independent`: independent lifecycle semantics

### Type semantics

- `@abstract`: type is not directly instantiable

### Identity and uniqueness

- `@key`: canonical identifier
- `@unique`: uniqueness without key semantics
- `@subkey(...)`: composite/subkey identity pattern

### Value-domain controls

- `@values(...)`: finite allowed set
- `@range(...)`: numeric/date range limits
- `@regex(...)`: string pattern constraints

## 5) Modeling Pitfalls

- Overusing attributes where role semantics should be modeled as relations.
- Declaring relations without clearly constrained role players.
- Omitting identity constraints for externally referenced entities.
- Using broad inheritance trees with little query or modeling value.

## 6) Schema Review Checklist

- Every externally addressable entity has key/unique strategy.
- Every relation has semantically named roles.
- Every high-value attribute has appropriate type-domain constraints.
- Cardinality constraints reflect real business invariants.

## Common mistakes in this section

- Adding types without identity strategy (`@key`/`@unique`) where needed.
- Using generic role names that reduce query clarity.
- Applying annotations at the wrong level (type vs capability).
