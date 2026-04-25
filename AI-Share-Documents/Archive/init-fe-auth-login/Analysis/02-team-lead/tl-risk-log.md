# Team Lead Risk Log

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Risks

### RISK-001: Uninitialized Frontend Project
- Related items: `TL-001`, `TL-006`, `FR-001`, `FR-002`, `AC-001`, `BAQ-002`
- Severity: `MEDIUM`
- Risk: `FE/FE-EvelS` currently has no app files, so missing baseline conventions can lead to inconsistent tooling or architecture choices.
- Mitigation: System Design must define conservative frontend architecture and tooling selection criteria before Coding FE initializes the project.
- Status: `OPEN`

### RISK-002: Admin Login Contract Field Ambiguity
- Related items: `TL-002`, `TL-007`, `FR-003`, `FR-004`, `AC-003`, `BAQ-001`
- Severity: `MEDIUM`
- Risk: BA confirms `/auth/login` exists but does not prove exact request field names for username/password.
- Mitigation: System Design must identify the frontend-consumed request shape or explicitly document the integration assumption; Coding FE should isolate mapping in the API client.
- Status: `OPEN`

### RISK-003: Archived Google Contract Has Mixed Endpoint Evidence
- Related items: `TL-002`, `TL-008`, `FR-008`, `FR-009`, `AC-007`, `AC-008`, `BAQ-005`
- Severity: `HIGH`
- Risk: Archived `sd-api-contract.md` documents `POST /auth/google/login`, while archived BE coding/testing and the current request state that the backend uses `GET /auth/google` and `GET /auth/google/callback`.
- Mitigation: System Design must prefer the current request and later archived BE coding/testing evidence for callback-based flow, and document the older `POST` contract as historical or non-authoritative for this feature.
- Status: `OPEN`

### RISK-004: Auth Persistence Tradeoff
- Related items: `TL-003`, `TL-009`, `FR-012`, `FR-013`, `FR-021`, `AC-010`, `AC-015`, `BAQ-004`
- Severity: `MEDIUM`
- Risk: Poor persistence choice can cause stale sessions, insecure token handling, or users being logged out unexpectedly.
- Mitigation: System Design must define persistence behavior, bootstrap validation, malformed-state handling, and logout clearing.
- Status: `OPEN`

### RISK-005: Wrong-Role Access Leakage
- Related items: `TL-004`, `TL-010`, `FR-015`, `FR-016`, `FR-017`, `FR-022`, `AC-012`, `AC-013`, `AC-016`
- Severity: `HIGH`
- Risk: If guards are incomplete or role checks trust client-controlled data, users may access pages outside their assigned role.
- Mitigation: Use backend-returned account role only, fail closed for missing/malformed role data, and test wrong-role navigation.
- Status: `OPEN`

### RISK-006: Google Callback User Experience Failure
- Related items: `TL-004`, `TL-005`, `TL-008`, `FR-009`, `FR-019`, `AC-008`, `BAQ-005`
- Severity: `MEDIUM`
- Risk: Callback errors, state mismatch, or unexpected navigation may leave users stuck without clear recovery.
- Mitigation: System Design must define callback landing behavior, loading state, success transition, error state, and retry path.
- Status: `OPEN`

### RISK-007: Dashboard Scope Creep
- Related items: `TL-004`, `TL-010`, `FR-005`, `FR-010`, `AC-004`, `AC-009`, `BAQ-003`
- Severity: `LOW`
- Risk: Dashboard content could expand beyond what is needed to prove role redirects and route protection.
- Mitigation: Keep dashboard/homepage work to route shells unless System Design finds existing requirements elsewhere.
- Status: `OPEN`

### RISK-008: Environment Configuration Gap
- Related items: `TL-002`, `TL-006`, `FR-004`, `FR-008`, `FR-009`, `NFR-006`, `BAQ-006`
- Severity: `LOW`
- Risk: Missing backend base URL or auth callback configuration can block local verification.
- Mitigation: System Design/Coding FE should provide a clear environment configuration approach and document defaults.
- Status: `OPEN`

### RISK-009: Error Message Quality
- Related items: `TL-005`, `TL-007`, `TL-008`, `FR-018`, `FR-019`, `FR-020`, `NFR-005`, `AC-005`, `AC-014`
- Severity: `MEDIUM`
- Risk: Raw backend errors or vague messages can confuse users or expose unnecessary details.
- Mitigation: System Design should define error categories and user-facing message expectations.
- Status: `OPEN`

### RISK-010: Accidental Backend Scope Expansion
- Related items: `TL-002`, `TL-012`, `NFR-008`, `AC-017`
- Severity: `MEDIUM`
- Risk: Contract uncertainty could lead downstream agents to modify backend code despite `FE_ONLY` scope.
- Mitigation: System Design and Coding FE must treat backend as read-only unless a blocking contradiction is formally documented in Analysis.
- Status: `OPEN`
