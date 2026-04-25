# Frontend Test Gaps

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Gaps

### GAP-001: No Live Backend Integration Test
- Related cases: `TC-001`, `TC-003`, `TC-004`
- Risk: Request/response behavior may differ from the isolated frontend contract mocks.
- Reason: Testing FE scope used frontend tests and no live backend server contract test was required.
- Recommendation: Run a later smoke test against `BE/BE-EvelS` with real `/auth/login`, `/auth/google`, and `/auth/google/callback` behavior.

### GAP-002: No Browser E2E Coverage
- Related cases: `TC-011` through `TC-017`
- Risk: Full browser navigation, localStorage, and callback behavior may differ from jsdom.
- Reason: This phase added unit/component coverage and build validation, not Playwright/Cypress E2E.
- Recommendation: Add browser E2E tests once backend/local auth fixtures are stable.

### GAP-003: Callback Redirect Variant Not Fully Exercised In Browser
- Related cases: `TC-004`, `TC-006`
- Risk: Backend may redirect to frontend with a shape that differs from the supported query payload.
- Reason: Tests cover backend callback API completion and query payload parsing separately, not every possible redirect shape.
- Recommendation: Confirm backend callback redirect behavior in integration testing.

### GAP-004: npm Audit Findings Remain
- Related cases: `TR-001` through `TR-004`
- Risk: Dependency vulnerabilities remain reported as moderate by npm audit.
- Reason: `npm audit fix --force` may introduce breaking changes and was not applied automatically.
- Recommendation: Review audit output separately and update dependencies deliberately.
