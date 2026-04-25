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

`coding-self-check.md` confirms Coding Backend is ready for Testing Backend. `coding-plan.md` is present and usable, including the updated implemented scope that existing Admin accounts may authenticate with email/password while Photographer password login, email/password account creation, public registration, Admin Google creation, and Customer authentication remain unsupported.

## Test Strategy
- Unit-test authentication application rules where account creation, role preservation, disabled-account denial, and unsupported registration are enforced.
- Unit-test Google infrastructure adapters with mocked `fetch` and mocked configuration to avoid live Google calls.
- Controller-test auth endpoints without a database using mocked use cases and OAuth client boundaries.
- Keep user-management controller tests scoped to controller behavior with guards overridden, because authorization guard internals are outside this feature test boundary.
- Preserve existing non-auth test coverage and update only tests that were invalidated by the approved auth scope.

## Test Levels
- Unit: `LoginUseCase`, `RegisterUseCase`, `AuthenticateWithGoogleUseCase`, `GoogleTokenVerifier`, `GoogleOAuthClient`, `CreateUserUseCase`.
- Controller/E2E without DB: `AuthController`, `UserController`.
- Full integration with real Google OAuth, real database migrations, and live JWT guard authorization is excluded from this phase.

## Scope
- Google first login creates `Photographer`.
- Repeated Google login authenticates existing account without role changes.
- Google OAuth does not create Admin accounts.
- Role mutation paths are absent or rejected through use-case behavior.
- Disabled Admin and Photographer accounts are denied login.
- Google token validation rejects invalid, unverified, or wrong-audience provider data.
- Client-supplied callback query extras such as `iss`, `authuser`, and `prompt` do not control identity or role.
- Public registration remains rejected.
- Admin password login is covered as an implemented Coding Backend scope update.

## Exclusions
- No production code changes.
- No tests under `src/`.
- No live Google OAuth browser flow.
- No real database migration execution.
- No Customer authentication tests beyond confirming unsupported scope through registration/auth boundaries.
