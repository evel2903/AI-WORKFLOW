# System Design Domain Design

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Frontend Domain Concepts

### Authenticated Account
Design ID: `SD-008`

Maps to: `TL-003`, `TL-004`, `TL-009`, `FR-012`, `FR-013`, `FR-017`, `FR-022`, `NFR-003`, `NFR-004`, `AC-010`, `AC-016`

Frontend representation of the backend-returned account.

Required fields:
- `id`
- `role`
- `status`
- `email`
- `displayName`

Rules:
- `role` must be accepted only when it is exactly `Admin` or `Photographer`.
- `status` must be treated as active only when it is compatible with backend success response. If status is missing or indicates disabled/inactive, fail closed.
- Account identity and role must come from backend auth response or validated persisted auth state derived from that response.

### Auth Session
Design ID: `SD-009`

Maps to: `TL-003`, `TL-009`, `FR-012`, `FR-013`, `FR-021`, `FR-022`, `NFR-003`, `NFR-004`, `AC-010`, `AC-015`, `AC-016`

Frontend-owned authenticated state.

Required fields:
- `accessToken`
- `account`
- `authenticatedAt`

Optional fields if backend response provides them:
- `expiresAt`
- `tokenType`

Rules:
- Session is valid only when token exists and account role is valid.
- Session restoration must validate shape before marking the user authenticated.
- Logout removes the stored session and resets in-memory state.
- Malformed persisted state must be discarded.

### Role
Design ID: `SD-010`

Maps to: `TL-004`, `TL-010`, `FR-015`, `FR-016`, `FR-017`, `FR-022`, `NFR-004`, `AC-012`, `AC-013`, `AC-016`

Allowed values:
- `Admin`
- `Photographer`

Rules:
- Only the backend-returned account role authorizes frontend route access.
- UI route role checks must fail closed for missing, unknown, or malformed role values.
- Admin-only pages require `Admin`.
- Photographer-only pages require `Photographer`.

### Auth Flow State
Design ID: `SD-011`

Maps to: `TL-005`, `TL-007`, `TL-008`, `FR-018`, `FR-019`, `FR-020`, `NFR-005`, `AC-005`, `AC-008`, `AC-014`

Auth flows should model at least:
- `idle`
- `submitting`
- `redirecting`
- `processingCallback`
- `authenticated`
- `error`

Rules:
- Submitting/redirecting states disable duplicate user actions.
- Error states show clear recovery text and do not persist auth state.
- Callback processing must show an explicit loading state.

## Route Access Invariants

### Public Auth Routes
Design ID: `SD-012`

Maps to: `TL-004`, `FR-003`, `FR-007`, `FR-017`, `AC-002`, `AC-006`, `AC-016`

Rules:
- `/login/admin` is accessible without auth and shows username/password login only.
- `/login/photographer` is accessible without auth and shows Google login only.
- Authenticated users visiting login pages are redirected according to current role.

### Admin Protected Routes
Design ID: `SD-013`

Maps to: `TL-004`, `TL-010`, `FR-005`, `FR-014`, `FR-016`, `FR-017`, `FR-022`, `AC-004`, `AC-011`, `AC-013`, `AC-016`

Rules:
- `/admin` requires authenticated state and role `Admin`.
- Unauthenticated access redirects to `/login/admin`.
- Authenticated Photographer access redirects to `/unauthorized`.

### Photographer Protected Routes
Design ID: `SD-014`

Maps to: `TL-004`, `TL-010`, `FR-010`, `FR-014`, `FR-015`, `FR-017`, `FR-022`, `AC-009`, `AC-011`, `AC-012`, `AC-016`

Rules:
- `/photographer` requires authenticated state and role `Photographer`.
- Unauthenticated access redirects to `/login/photographer`.
- Authenticated Admin access redirects to `/unauthorized`.

### Unauthorized And Fallback Routes
Design ID: `SD-015`

Maps to: `TL-004`, `TL-005`, `FR-014`, `FR-015`, `FR-016`, `NFR-004`, `NFR-005`, `AC-011`, `AC-012`, `AC-013`

Rules:
- `/unauthorized` explains access is not available for the current role and provides an action to return to the correct dashboard or log out.
- Unknown routes use a standard not-found experience.
- Unauthorized route access must not render protected page content before redirect/denial.

## State Transitions

### Admin Login Success
Design ID: `SD-016`

Maps to: `TL-007`, `FR-003`, `FR-004`, `FR-005`, `FR-017`, `AC-002`, `AC-003`, `AC-004`

Transition:
`idle -> submitting -> authenticated -> redirect(/admin)`

Validation:
- Auth response must contain token and account.
- Account role must be `Admin`.
- If role is not `Admin`, transition to `error` and clear auth state.

### Admin Login Failure
Design ID: `SD-017`

Maps to: `TL-005`, `TL-007`, `FR-018`, `FR-020`, `NFR-005`, `AC-005`, `AC-014`

Transition:
`idle -> submitting -> error`

Failure inputs:
- invalid credentials;
- disabled account;
- network failure;
- malformed response.

### Photographer Google Initiation
Design ID: `SD-018`

Maps to: `TL-008`, `FR-007`, `FR-008`, `FR-011`, `AC-006`, `AC-007`

Transition:
`idle -> redirecting -> external Google/backend auth`

Validation:
- Backend initiation response must include `AuthorizationUrl`.
- Missing `AuthorizationUrl` produces an error and does not leave the app.

### Photographer Callback Success
Design ID: `SD-019`

Maps to: `TL-008`, `FR-009`, `FR-010`, `FR-017`, `AC-008`, `AC-009`

Transition:
`processingCallback -> authenticated -> redirect(/photographer)`

Validation:
- Auth response must contain token and account.
- Account role must be `Photographer`.
- If role is not `Photographer`, transition to `error` and clear auth state.

### Logout
Design ID: `SD-020`

Maps to: `TL-003`, `TL-009`, `FR-021`, `AC-015`

Transition:
`authenticated -> idle -> redirect(public login route)`

Rules:
- Clear browser persistence.
- Clear in-memory auth state.
- Remove auth token from future API requests.

## Backend Domain Reuse
Design ID: `SD-021`

Maps to: `TL-002`, `NFR-008`, `AC-017`

No backend domain design is in scope. Existing backend account, role, status, Google identity, and token issuance behavior from the archived backend implementation is reused as a dependency.
