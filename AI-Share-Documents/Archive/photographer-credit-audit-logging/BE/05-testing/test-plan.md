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

`ai-docs/04-coding/coding-self-check.md` was reviewed first. `ai-docs/04-coding/coding-plan.md` exists, is populated, and is consistent with the implemented audit-logging scope.

## Scope Under Test
- Admin credit updates for photographer users create an audit log when the stored value changes.
- Audit logs capture actor identifiers, target identifiers, previous credit, new credit, and timestamp.
- No audit log is created for no-op updates.
- Unauthorized or invalid credit update attempts do not create audit logs.
- Existing photographer-only credit rules remain intact.
- Broader credit-engine and generalized audit-history behavior remain out of scope.
- The coding nuance for legacy photographers with `Credit = null` is covered at the use-case level.

## Test Strategy
- Use unit tests for the credit update use case to validate changed-value detection, audit-log creation, no-op suppression, actor validation, target validation, and the legacy `null -> PreviousCredit = 0` behavior.
- Use controller/E2E-style tests without a database to validate request validation and actor-context wiring into the credit update use case.
- Update affected test doubles that implement `IUserRepository` so the new repository contract compiles across the test suite.
- Do not modify `src/` in this phase.

## Test Levels
- Unit:
  - `UpdateUserCreditUseCase`
  - existing auth and user use-case tests impacted by the `IUserRepository` contract change
- Controller/E2E without database:
  - `UserController`

## Dependencies
- `CD-001` dedicated audit log model
- `CD-002` coordinated repository write path
- `CD-003` update-flow integration and no-op suppression
- `CD-005` new migration and data source registration, covered only as a documented gap in this phase

## Exclusions
- No production code changes under `src/`
- No execution of the new migration against a live or test database
- No public audit-log read API coverage, because that scope was not added
- No broader credit engine behavior beyond the approved photographer-credit update surface

## Risks
- The migration and check constraints for `credit_audit_logs` are not exercised by a real database test in this phase.
- Transaction semantics are verified indirectly through use-case/repository expectations, not through an integration database run.
