# TypeQL Skills

This repository contains two TypeQL skill packages with different goals:

1. a direct baseline copy of the CaliLuke TypeDB skill
2. a generated and hardened documentation system built from official TypeQL docs

## Skill Packages

### 1) Baseline copy (`skills/typedb`)

- Main file: `skills/typedb/skill.md`
- Source: [CaliLuke TypeDB skill](https://github.com/CaliLuke/skills/blob/main/skills/typedb/SKILL.md)
- Generation instruction: `.generate/copy_caliluke_skill.md`
- Purpose: single-file, dense, practical reference

### 2) Official-docs generated pack (`skills/typeql_docs`)

- Main file: `skills/typeql_docs/skill.md`
- Entry README: `skills/typeql_docs/README.md`
- Generation instruction: `.generate/generate_From_typeql_docs.md`
- Purpose: quick-core plus deep references with stronger structure, navigation, and maintainability

## How They Compare

- **`skills/typedb` strengths**
  - Excellent standalone file
  - Fast copy/paste workflow
  - Dense practical examples in one place

- **`skills/typeql_docs` strengths**
  - Better information architecture (core + references)
  - Better task-based navigation (syntax cookbook, pattern cookbook, ops/debugging)
  - Easier to extend and maintain as docs evolve
  - Includes source-map traceability to official docs

- **Practical recommendation**
  - If you want one file quickly: start with `skills/typedb/skill.md`.
  - If you want a complete system for ongoing use: start with `skills/typeql_docs/README.md`.

## Regeneration

- Refresh the baseline copy with `.generate/copy_caliluke_skill.md`.
- Regenerate the hardened official-docs pack with `.generate/generate_From_typeql_docs.md`.
