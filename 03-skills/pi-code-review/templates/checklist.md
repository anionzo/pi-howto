# Code Review Checklist

## Correctness
- Does the logic match the intended behavior?
- Are edge cases handled?
- Are null / undefined cases safe?
- Are errors surfaced or swallowed?

## Security
- Is user input validated?
- Are secrets protected?
- Is auth / authorization enforced?
- Could shell / SQL / template injection happen?

## Performance
- Any obviously expensive loops or repeated work?
- Any unnecessary network / DB calls?
- Any unbounded memory growth?

## Maintainability
- Are responsibilities separated cleanly?
- Are names clear?
- Is duplication avoidable?
- Is the code too large or too nested?

## Testing
- Are new behaviors tested?
- Are failure paths tested?
- Do tests prove the important contract?
