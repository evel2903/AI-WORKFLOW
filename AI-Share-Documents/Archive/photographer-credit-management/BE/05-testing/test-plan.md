# Testing Backend Plan

## Role Running
`testing-be`

## Input Files
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `ai-docs/04-coding/coding-plan.md`
- `ai-docs/04-coding/coding-change-log.md`
- `ai-docs/04-coding/coding-self-check.md`
- updated production code under `src/`
- existing tests under `test/`

## Allowed Output Directories
- `test/`
- `ai-docs/05-testing/`

## Completion Artifacts
- `ai-docs/05-testing/test-plan.md`
- `ai-docs/05-testing/test-cases.md`
- `ai-docs/05-testing/test-results.md`
- `ai-docs/05-testing/test-gaps.md`
- `ai-docs/05-testing/test-self-check.md`
- test code changes under `test/`

## Preflight Result
Status: PASSED

`ai-docs/04-coding/coding-self-check.md` was reviewed before testing work. `ai-docs/04-coding/coding-plan.md` exists, is populated, and matches the implemented photographer-credit scope closely enough to validate coverage.

## Scope Under Test
- First successful photographer login initializes `Credit = 10`.
- Subsequent photographer logins preserve existing stored credit and do not reset it.
- Admin-only manual update of photographer credit through `PATCH /users/:id/credit`.
- Non-admin update attempts are rejected.
- Non-photographer targets cannot receive usable credit through the update path.
- Non-photographer DTOs do not expose usable credit.
- Broader credit-engine routes and behaviors remain absent.
- Legacy handling assumption from System Design is covered at the use-case level: photographers with stored `Credit = null` initialize to `10` on successful login.

## Test Strategy
- Cover credit initialization and preservation in authentication use-case tests where the login rule actually executes.
- Cover admin authorization and non-photographer rejection in the dedicated credit-update use case.
- Cover API contract and request validation for the admin credit endpoint in controller tests with guards overridden.
- Cover DTO projection rules separately so photographer visibility and non-photographer omission are explicit and stable.
- Keep testing bounded to `test/` and avoid production-code edits in this phase.

## Test Levels
- Unit:
  - `AuthenticateWithGoogleUseCase`
  - `UpdateUserCreditUseCase`
  - `UserDtoMapper`
- Controller/E2E without database:
  - `UserController`

## Dependencies
- Implemented `Credit` domain and persistence behavior from `CD-001`.
- Implemented first-login initialization behavior from `CD-002`.
- Implemented admin-only update path from `CD-003`.
- Migration and legacy handling notes from `CD-004` for gap assessment.

## Exclusions
- No production code changes under `src/`.
- No migration execution against a real database in this phase.
- No end-to-end JWT guard or database-backed authorization integration.
- No testing of spending, recharge, expiration, automatic increase/decrease, or other broader credit-engine behavior because those flows remain out of scope.

## Risks
- Real database constraint behavior for the new `Credit` column is not exercised by these tests.
- Guard overrides in controller tests do not prove full JWT/role guard wiring.
- Migration registration and rollout behavior are validated indirectly through coding artifacts, not by executing migrations here.
