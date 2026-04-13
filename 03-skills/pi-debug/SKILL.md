---
name: pi-debug
description: Debug failures systematically by reproducing the issue, isolating the cause, applying a fix, and verifying the outcome. Use when the user reports a bug, crash, flaky behavior, or failing test.
---

# pi-debug

Use this skill for investigation-first debugging.

## Debug Workflow

1. **Reproduce**
   - capture exact steps
   - identify expected vs actual behavior
   - create the smallest reliable reproduction possible
2. **Isolate**
   - narrow to a specific file, branch, request, or state transition
   - inspect logs, stack traces, and recent changes
3. **Fix**
   - choose the smallest fix that addresses the root cause
   - avoid papering over symptoms when possible
4. **Verify**
   - rerun reproduction steps
   - add or update tests if appropriate
   - check for nearby regressions

## Common Bug Categories

- null / undefined errors
- async race conditions
- stale state
- boundary and indexing issues
- wrong assumptions about input shape
- environment/config mismatches
- retry and timeout bugs

## Debugging Tactics

- read the failing code path first
- inspect stack traces carefully
- add temporary logging or assertions
- reduce the problem to one failing case
- compare working vs failing paths
- check recent diffs around the failure

## Output Expectations

- reproduction steps
- likely root cause
- fix summary
- verification steps/results

## Resources

See:
- `templates/debug-checklist.md`
- `templates/issue-template.md`
- `templates/common-fixes.md`
- `templates/error-codes.md`
