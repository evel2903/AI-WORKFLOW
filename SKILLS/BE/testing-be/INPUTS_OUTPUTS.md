# Testing Backend - Inputs and Outputs

## Allowed Inputs
- `AGENTS.md`
- `SKILLS/BE/shared/*`
- `AI-Share-Documents/Analysis/**`
- `AI-Share-Documents/BE/04-coding-be/*`
- `BE/<Project name>/src/**`
- `BE/<Project name>/test/**`
- Target backend test configuration needed to run or add tests

## Allowed Outputs
Write only to:
- `BE/<Project name>/test/**`
- `AI-Share-Documents/BE/05-testing-be/test-plan.md`
- `AI-Share-Documents/BE/05-testing-be/test-cases.md`
- `AI-Share-Documents/BE/05-testing-be/test-results.md`
- `AI-Share-Documents/BE/05-testing-be/test-gaps.md`
- `AI-Share-Documents/BE/05-testing-be/test-self-check.md`
- `AI-Share-Documents/BE/**` for backend handoff documents when frontend depends on backend changes

## Precondition
`coding-self-check.md` must exist before testing may start. If it records `Status: NOT_IN_SCOPE`, write `Status: NOT_IN_SCOPE` to `AI-Share-Documents/BE/05-testing-be/test-self-check.md` and stop.
When backend is in scope, `coding-plan.md` and `coding-self-check.md` must both exist and be usable before testing may start.
