# Testing Frontend - Inputs and Outputs

## Allowed Inputs
- `AGENTS.md`
- `SKILLS/FE/shared/*`
- `AI-Share-Documents/Analysis/**`
- `AI-Share-Documents/FE/04-coding-fe/*`
- `AI-Share-Documents/BE/**` only when backend handoff is required for frontend testing
- `AI-Share-Documents/Archive/**/BE/**` as optional read-only reference material for matching archived backend documents
- approved frontend source files under `FE/<Project name>/`
- approved frontend test files under `FE/<Project name>/`
- frontend test config files
- `FE/<Project name>/package.json`

## Allowed Outputs
Write only to:
- `FE/<Project name>/test/**`
- `FE/<Project name>/tests/**`
- `FE/<Project name>/e2e/**`
- `FE/<Project name>/app/**/__tests__/**`
- `FE/<Project name>/src/**/__tests__/**`
- `FE/<Project name>/components/**/__tests__/**`
- `FE/<Project name>/features/**/__tests__/**`
- `FE/<Project name>/playwright.config.*`
- `FE/<Project name>/vitest.config.*`
- `FE/<Project name>/jest.config.*`
- `FE/<Project name>/package.json`
- `AI-Share-Documents/FE/05-testing-fe/test-plan.md`
- `AI-Share-Documents/FE/05-testing-fe/test-cases.md`
- `AI-Share-Documents/FE/05-testing-fe/test-results.md`
- `AI-Share-Documents/FE/05-testing-fe/test-gaps.md`
- `AI-Share-Documents/FE/05-testing-fe/test-self-check.md`
- `AI-Share-Documents/FE/**`

## Precondition
`coding-self-check.md` must exist before testing may start. If it records `Status: NOT_IN_SCOPE`, write `Status: NOT_IN_SCOPE` to `AI-Share-Documents/FE/05-testing-fe/test-self-check.md` and stop.
When frontend is in scope, `coding-plan.md` and `coding-self-check.md` must both exist and be usable before testing may start.
Matching archived BE documents under `AI-Share-Documents/Archive/**/BE/**` may be read for context, but they are optional and cannot block frontend testing when missing.
