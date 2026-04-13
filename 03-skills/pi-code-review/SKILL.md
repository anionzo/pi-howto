---
name: pi-code-review
description: Review code for correctness, security, performance, maintainability, and test quality. Use when the user asks for a review, audit, or change assessment.
---

# pi-code-review

Use this skill when reviewing source code, pull requests, diffs, or architecture changes.

## Goals

- identify correctness and reliability issues
- flag security risks and unsafe assumptions
- spot performance problems and scalability concerns
- improve readability, maintainability, and test coverage
- give actionable, prioritized feedback

## Review Workflow

1. **Understand scope**
   - identify files, module boundaries, and intended behavior
   - determine whether you are reviewing a whole codebase, a diff, or a single file
2. **Read before judging**
   - read the changed code and nearby context
   - look for tests, config, migrations, and docs that affect behavior
3. **Check the critical dimensions**
   - correctness
   - security
   - performance
   - maintainability
   - testing
4. **Prioritize findings**
   - critical: likely bug, vulnerability, or major failure mode
   - high: strong chance of production pain
   - medium: design or maintainability issue with real cost
   - low: polish, consistency, style, or minor cleanup
5. **Report clearly**
   - explain what is wrong
   - explain why it matters
   - point to concrete locations
   - suggest a fix or safer direction

## Review Areas

### Correctness

- logic errors
- edge cases
- null / undefined handling
- off-by-one mistakes
- broken invariants
- error handling
- async flow bugs

### Security

- auth / authorization gaps
- injection risks
- unsafe shell usage
- secret leakage
- insecure defaults
- missing validation / sanitization

### Performance

- unnecessary repeated work
- poor algorithmic complexity
- N+1 database or API calls
- memory growth
- excessive I/O
- missed caching opportunities

### Maintainability

- confusing naming
- long functions
- duplicated logic
- tight coupling
- mixed responsibilities
- weak module boundaries

### Testing

- missing coverage for new behavior
- untested failure paths
- brittle tests
- assertions that do not prove the behavior

## Output Format

Use the templates in `templates/`.

### Minimum structure

1. **Summary**
2. **Findings by severity**
3. **Recommendations**
4. **Optional praise / strengths**

## Review Rules

- prefer concrete findings over generic style comments
- do not invent bugs without evidence
- if unsure, mark the issue as a question or risk, not a fact
- keep findings scoped to the actual code under review
- focus on the highest-impact issues first

## Quick Checklist

See:
- `templates/checklist.md`
- `templates/severity-guide.md`
- `templates/review-template.md`

## Example

See `examples/sample-review.md` for an example output.
