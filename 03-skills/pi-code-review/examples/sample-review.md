# Sample Review

## Summary
- Scope: auth middleware and login flow
- Overall assessment: good structure, but one security issue and one missing error path
- Risk level: High

## Findings

### High
- **Title:** Missing authorization check on admin route
- **Location:** `src/routes/admin.ts:42`
- **Why it matters:** authenticated non-admin users can reach protected actions
- **Suggested fix:** enforce role check before handler execution

### Medium
- **Title:** Login retry path is not tested
- **Location:** `src/auth/login.test.ts`
- **Why it matters:** regressions in lockout behavior may slip through
- **Suggested fix:** add tests for repeated failure attempts and reset behavior

## Recommendations
1. Add a role-based authorization guard
2. Add failure-path tests for login throttling
3. Audit similar routes for the same missing check
