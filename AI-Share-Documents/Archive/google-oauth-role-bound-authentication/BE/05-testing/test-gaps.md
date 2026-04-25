# Testing Backend Gaps

## Role Running
`testing-be`

## Gaps

### TG-001: Live Google OAuth Flow Not Executed
Status: KNOWN GAP

Reason:
Tests mock Google tokeninfo and token exchange responses. This validates backend logic and contracts without depending on live Google services.

Risk:
Runtime Google configuration or provider behavior could still fail in an environment-specific way.

Mitigation:
Run an environment smoke test with real `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, and `GOOGLE_REDIRECT_URI` before release.

### TG-002: Real Database Migration Not Executed In Test Suite
Status: KNOWN GAP

Reason:
Current tests are unit and controller-level tests without a real database.

Risk:
Role/status check constraints, unique Google subject constraints, or migration registration could fail against a real database.

Mitigation:
Add migration/integration tests or execute migrations in a staging database pipeline.

### TG-003: Authorization Guard Integration Is Partially Tested
Status: KNOWN GAP

Reason:
`UserController` tests override guards to validate controller behavior and request validation. Full JWT and role-guard behavior is not exercised end-to-end.

Risk:
Guard wiring or JWT strategy integration defects may require separate integration coverage.

Mitigation:
Add authenticated request integration tests once test JWT setup and database fixtures are available.

### TG-004: Admin Password Login Differs From Original BA Unsupported Email/Password Criterion
Status: DOCUMENTED SCOPE UPDATE

Reason:
Coding Backend was updated per user direction to support existing Admin password login. Testing follows `coding-plan.md`, which now says email/password auth may authenticate existing Admin accounts only and must not authenticate Photographers or create accounts.

Risk:
Traceability to original `AC-007` must be interpreted through the later Coding scope update.

Mitigation:
Tests explicitly cover Admin-only password login, Photographer password rejection, disabled Admin denial, and public registration rejection.
