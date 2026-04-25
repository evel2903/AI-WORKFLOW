# Testing Frontend - Checklist

## Preflight Check
- [ ] Read `coding-self-check.md`.
- [ ] Confirm Coding is ready and not blocked, or write `Status: NOT_IN_SCOPE` if frontend coding is out of scope.
- [ ] Read `coding-plan.md`.
- [ ] Confirm `coding-plan.md` exists and is sufficient for testing scope validation.
- [ ] Stop immediately if coding artifacts are incomplete or coding plan is missing.

## Testing Work
- [ ] Read acceptance criteria and design artifacts.
- [ ] Read backend handoff documents only when required by System Design.
- [ ] Optionally read matching archived BE documents when useful for frontend integration test context.
- [ ] Review coding change log and validate `coding-self-check.md`.
- [ ] Define the test scope and levels.
- [ ] Write tests only in approved frontend test locations under `FE/<Project name>/`.
- [ ] Cover success, failure, loading, empty, and validation states where applicable.
- [ ] Cover routing, integration, and authorization cases where applicable.
- [ ] Record results, blocked items, and gaps.
- [ ] Complete `test-self-check.md`.
- [ ] Do not modify frontend production code.
