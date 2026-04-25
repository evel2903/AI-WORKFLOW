# Business Analysis Open Questions

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Blocking Question Summary
No blocking open questions are present at Business Analysis handoff.

## Questions And Assumptions

### BAQ-001: Exact Admin Login Request Field Names
- Status: `ASSUMED`
- Affected surface: `FE`
- Blocking: `No`
- Impact: `MEDIUM`
- Related requirements: `FR-003`, `FR-004`, `AC-002`, `AC-003`
- Question: What exact request field names does `/auth/login` expect for Admin username/password login?
- Assumption: The frontend will expose business labels `username` and `password`, while System Design/Coding FE will confirm and map the exact backend request field names from backend contract or source.
- Rationale: The archive confirms `/auth/login` authenticates existing Admin accounts only, but the supplied BA inputs do not prove the exact request DTO shape. This is an integration detail that should not block FE-only planning.

### BAQ-002: Frontend Framework And Package Tooling
- Status: `ASSUMED`
- Affected surface: `FE`
- Blocking: `No`
- Impact: `MEDIUM`
- Related requirements: `FR-001`, `FR-002`, `NFR-002`, `AC-001`
- Question: Which frontend framework, package manager, and test tooling should the initial project use?
- Assumption: System Design and Coding FE will select tooling based on existing repository conventions, project needs, and maintainability.
- Rationale: `FE/FE-EvelS` currently has no application files, so BA can define the business need without selecting implementation tooling.

### BAQ-003: Dashboard Homepage Content
- Status: `ASSUMED`
- Affected surface: `FE`
- Blocking: `No`
- Impact: `LOW`
- Related requirements: `FR-005`, `FR-010`, `AC-004`, `AC-009`
- Question: What business content must appear on Admin and Photographer dashboard/homepages?
- Assumption: This feature requires only enough dashboard/homepage experience to verify role-specific redirects and route protection.
- Rationale: The request focuses on authentication flows and routing, not dashboard business functionality.

### BAQ-004: Auth Persistence Storage Policy
- Status: `ASSUMED`
- Affected surface: `FE`
- Blocking: `No`
- Impact: `MEDIUM`
- Related requirements: `FR-012`, `FR-013`, `FR-021`, `AC-010`, `AC-015`
- Question: Should auth state be persisted in local storage, session storage, cookies, memory plus refresh, or another browser mechanism?
- Assumption: System Design will define the persistence approach after considering backend token behavior, security tradeoffs, and project conventions.
- Rationale: The business requirement is persistence and correct restoration/clearing; the exact browser mechanism is a design decision.

### BAQ-005: Google Callback Frontend Landing Shape
- Status: `ASSUMED`
- Affected surface: `FE`
- Blocking: `No`
- Impact: `MEDIUM`
- Related requirements: `FR-008`, `FR-009`, `FR-019`, `AC-007`, `AC-008`
- Question: What exact frontend route or callback landing behavior should receive the Google auth result?
- Assumption: System Design will map the frontend callback route to the existing backend callback flow and archived backend contract.
- Rationale: The archive confirms backend callback behavior exists, but frontend route naming and browser navigation behavior belong to design and implementation.

### BAQ-006: Backend Base URL And Runtime Environment Configuration
- Status: `ASSUMED`
- Affected surface: `FE`
- Blocking: `No`
- Impact: `LOW`
- Related requirements: `FR-004`, `FR-008`, `FR-009`, `NFR-006`
- Question: What runtime configuration should the frontend use for backend API base URL and environment-specific auth settings?
- Assumption: Coding FE will introduce or follow project-standard environment configuration.
- Rationale: This is required for implementation but does not affect business scope.
