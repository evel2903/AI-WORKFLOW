# Frontend Test Cases

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Test Cases

### TC-001: Admin Login Sends EmailAddress And Maps Success
Maps to: `CD-003`, `CD-004`, `SD-024`, `SD-025`, `AC-003`, `AC-004`

Verifies `loginAdmin` calls `POST /auth/login` with `EmailAddress` and `Password`, then maps token and Admin account data into frontend auth session shape.

### TC-002: Admin Login Backend Error Mapping
Maps to: `CD-003`, `CD-004`, `SD-025`, `SD-029`, `AC-005`, `AC-014`

Verifies disabled-account backend errors become clear frontend error messages and do not return authenticated state.

### TC-003: Google Login Initiation Endpoint
Maps to: `CD-003`, `CD-004`, `SD-026`, `AC-006`, `AC-007`

Verifies Photographer Google auth starts with `GET /auth/google` and consumes `AuthorizationUrl` and `CallbackUrl`.

### TC-004: Google Callback Completion
Maps to: `CD-003`, `CD-004`, `SD-027`, `AC-008`, `AC-009`

Verifies callback completion forwards query string to `GET /auth/google/callback` and maps Photographer auth state.

### TC-005: Malformed Auth Response Fails Closed
Maps to: `CD-002`, `CD-003`, `SD-024`, `SD-031`, `SD-033`, `AC-016`

Verifies unknown roles such as `Customer` are rejected and do not produce a valid session.

### TC-006: Callback Query Auth Payload Parsing
Maps to: `CD-003`, `CD-004`, `SD-027`, `AC-008`, `AC-009`

Verifies callback query payload parsing can produce a Photographer auth session when backend redirects with auth values.

### TC-007: Persist Valid Auth Session
Maps to: `CD-002`, `SD-033`, `SD-038`, `AC-010`

Verifies a valid auth session is written to and read from `localStorage`.

### TC-008: Reject Malformed Persisted Auth State
Maps to: `CD-002`, `SD-039`, `SD-042`, `AC-010`, `AC-016`

Verifies malformed persisted state is discarded and removed from storage.

### TC-009: Reject Disabled Account State
Maps to: `CD-002`, `SD-032`, `SD-033`, `AC-014`, `AC-016`

Verifies disabled account state is invalid for restored sessions.

### TC-010: Clear Persisted Auth State
Maps to: `CD-002`, `SD-040`, `AC-015`

Verifies auth storage clearing removes persisted session data.

### TC-011: Block Unauthenticated Admin Route Access
Maps to: `CD-005`, `SD-013`, `SD-042`, `SD-050`, `AC-011`

Verifies an anonymous user attempting an Admin protected route is redirected to `/login/admin` and protected content does not render.

### TC-012: Block Wrong-Role Route Access
Maps to: `CD-005`, `SD-013`, `SD-014`, `SD-042`, `SD-050`, `AC-012`, `AC-013`

Verifies a Photographer session attempting an Admin route is redirected to `/unauthorized`.

### TC-013: Allow Matching Role Route Access
Maps to: `CD-005`, `SD-013`, `SD-042`, `AC-004`, `AC-016`

Verifies an Admin session can render Admin protected content.

### TC-014: Admin Form Success Redirect
Maps to: `CD-004`, `CD-005`, `SD-016`, `SD-025`, `AC-002`, `AC-004`

Verifies the Admin form submits email address/password, persists auth state, and redirects to `/admin`.

### TC-015: Admin Form Error Display
Maps to: `CD-004`, `SD-017`, `SD-051`, `AC-005`

Verifies failed Admin login displays a visible error and does not redirect to `/admin`.

### TC-016: Photographer Google Button Redirect
Maps to: `CD-004`, `SD-018`, `SD-026`, `AC-006`, `AC-007`

Verifies Photographer Google sign-in calls the initiation API and navigates to the returned `AuthorizationUrl`.

### TC-017: Photographer Google Initiation Error
Maps to: `CD-004`, `SD-018`, `SD-051`, `AC-008`

Verifies Google initiation failure displays a visible user-facing error and does not navigate away.
