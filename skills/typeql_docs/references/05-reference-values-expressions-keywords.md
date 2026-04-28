# TypeQL Reference: Values, Expressions, Keywords

## Value Types

- Boolean
- Integer
- Double
- Decimal
- String
- Date
- Datetime
- DatetimeTZ
- Duration

## Value Handling Guidance

- Choose the narrowest type that fits domain semantics.
- Keep timezone behavior explicit (`datetime` vs `datetimetz`).
- Avoid mixing floating-point and decimal semantics unintentionally.
- Use constrained value domains where possible.

## Expressions

- Literals: inline primitive constants.
- Operators: arithmetic, logical, and comparison operators.
- Function calls: invoke reusable computation/query logic.
- Structs: grouped structured values.
- Lists: ordered collections for expression-level operations.

## Keywords

- Treat keywords as reserved syntax tokens.
- Avoid naming schema labels that collide with keywords.
- Keep query formatting consistent to preserve readability on complex pipelines.
