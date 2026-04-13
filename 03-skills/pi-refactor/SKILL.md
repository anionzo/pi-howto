---
name: pi-refactor
description: Refactor code safely using small steps, tests, and proven patterns. Use when the user asks to clean up code, reduce duplication, or improve structure without changing behavior.
---

# pi-refactor

Use this skill when improving internal code quality while preserving behavior.

## Goals

- reduce duplication
- improve naming and readability
- simplify control flow
- separate responsibilities
- preserve behavior with tests or verification

## Refactor Workflow

1. **Understand the current behavior**
   - read nearby code and existing tests
   - identify the current contract before touching structure
2. **Choose the smallest safe refactor**
   - start with one clear improvement
   - avoid mixing feature work with cleanup when possible
3. **Preserve behavior**
   - add or run tests before large changes
   - prefer small, reversible steps
4. **Apply a named pattern**
   - make the refactor explainable
5. **Verify**
   - run tests, lint, or targeted validation

## Common Code Smells

- duplicate code
- long function / long parameter list
- unclear naming
- giant conditional blocks
- mixed responsibilities
- feature envy / tight coupling
- temporary variables hiding intent

## Common Refactoring Patterns

- Extract Method
- Inline Method
- Rename Variable / Function / Class
- Move Method
- Move Field
- Extract Class
- Inline Class
- Replace Temp with Query
- Introduce Parameter Object
- Preserve Whole Object

## Rules

- do not change behavior unless the user explicitly asked for it
- prefer many small refactors over one giant rewrite
- keep public interfaces stable unless a migration plan exists
- when uncertain, stop and explain the risk

## Output Expectations

- name the refactor pattern(s) used
- explain why the code is better after the change
- mention validation performed

## Resources

See:
- `templates/refactor-checklist.md`
- `templates/pattern-examples.md`
- `templates/smell-catalogue.md`
