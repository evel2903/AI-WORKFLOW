# Testing Backend - Checklist

## Preflight Check
- [ ] Read `coding-self-check.md`.
- [ ] Confirm Coding is ready and not blocked, or write `Status: NOT_IN_SCOPE` if backend coding is out of scope.
- [ ] Read `coding-plan.md`.
- [ ] Confirm `coding-plan.md` exists and is sufficient for testing scope validation.
- [ ] Stop immediately if coding artifacts are incomplete or coding plan is missing.

## Testing Work
- [ ] Read acceptance criteria and design artifacts.
- [ ] Review coding change log and validate `coding-self-check.md`.
- [ ] Define the test scope and levels.
- [ ] Write tests only in `BE/<Project name>/test/`.
- [ ] Cover success, failure, and authorization cases where applicable.
- [ ] Record results, blocked items, and gaps.
- [ ] Complete `test-self-check.md`.
- [ ] Publish BE handoff documents to `AI-Share-Documents/BE/` only when frontend work depends on backend changes.
- [ ] Do not modify backend production code.
