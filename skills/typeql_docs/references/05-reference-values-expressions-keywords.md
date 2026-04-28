# TypeQL Reference: Values, Expressions, Keywords (Detailed)

How to use this file: use it to check value typing, expression legality, and keyword safety before finalizing production queries.

## Mini TOC

- Value Types
- Value Type Guidance
- Comparison Operators
- Expression Operators
- Common Expression Functions
- Lists and Structs
- Keyword Safety
- Quick Type-Validation Checklist

## 1) Value Types

| Type | Typical Use | Example |
| --- | --- | --- |
| `boolean` | flags | `true` |
| `integer` | counts/ids | `42` |
| `double` | floating numeric | `3.14159` |
| `decimal` | precise financial-like values | `99.95dec` |
| `string` | text | `"alice@example.com"` |
| `date` | day precision | `2026-04-28` |
| `datetime` | timestamp | `2026-04-28T20:45:00` |
| `datetimetz` | timezone-aware timestamp | `2026-04-28T20:45:00 Australia/Sydney` |
| `duration` | elapsed interval | `PT4H30M` |

## 2) Value Type Guidance

- Use `decimal` when precision matters.
- Keep timezone semantics explicit in cross-region systems.
- Keep type families consistent inside expressions.
- Apply domain constraints at schema layer (`@range`, `@regex`, `@values`).

## 3) Comparison Operators

| Operator | Meaning |
| --- | --- |
| `==` | equality |
| `!=` | inequality |
| `<` | less than |
| `<=` | less than or equal |
| `>` | greater than |
| `>=` | greater than or equal |
| `like` | regex-like text matching |
| `contains` | substring/text containment |

Example:

```typeql
match
  $p isa person, has name $n, has age $a;
  $n like "^A.*";
  $a >= 18;
fetch { "name": $n };
```

## 4) Expression Operators

### Arithmetic

- `+`, `-`, `*`, `/`, `%`, `^`

```typeql
match
  $i isa invoice, has subtotal $sub, has tax_rate $tax;
let $total = $sub + ($sub * $tax);
fetch { "total": $total };
```

### Assignment and composition

```typeql
match
  $p isa person, has first_name $fn, has last_name $ln;
let $full_name = concat($fn, " ", $ln);
fetch { "full_name": $full_name };
```

## 5) Common Expression Functions

- `abs(...)`
- `ceil(...)`
- `floor(...)`
- `round(...)`
- `min(...)`, `max(...)`
- `len(...)`
- `concat(...)`
- `iid(...)`
- `label(...)` (on type variables)

Function notes:

- `label(...)` expects a type variable, not an instance variable.
- Validate numeric function behavior on your concrete numeric type (`double` vs `decimal`).

## 6) Lists and Structs

### List usage

```typeql
match
  $p isa person, has tag $tag;
fetch {
  "tags": [ $tag ]
};
```

### Struct note

Struct syntax is part of the language reference topics; verify runtime support for your deployed TypeDB version before relying on structs in production.

## 7) Keyword Safety

- Treat language keywords as reserved identifiers.
- Avoid naming schema labels with keywords.
- Keep consistent formatting for readability in multi-stage pipelines.

Common reserved tokens to avoid as identifiers:

`with`, `match`, `fetch`, `update`, `define`, `undefine`, `redefine`, `insert`, `put`, `delete`, `end`, `entity`, `relation`, `attribute`, `asc`, `desc`, `struct`, `fun`, `return`, `alias`, `sub`, `owns`, `as`, `plays`, `relates`, `iid`, `isa`, `links`, `has`, `is`, `or`, `not`, `try`, `in`, `true`, `false`, `of`, `from`, `first`, `last`

## 8) Quick Type-Validation Checklist

- Value literal format matches declared type.
- Comparison operands are type-compatible.
- Date/datetime/datetimetz choices are intentional.
- Decimal/double mixing is deliberate and tested.

## Common mistakes in this section

- Mixing decimal and floating-point semantics unintentionally.
- Using `label(...)` on instance variables instead of type variables.
- Choosing datetime variants without timezone intent.
