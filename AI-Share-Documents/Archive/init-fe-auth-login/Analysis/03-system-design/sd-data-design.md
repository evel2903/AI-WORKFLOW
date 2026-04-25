# System Design Data Design

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Data Design Case
Frontend data design is required. No backend data design, schema change, migration, entity registration, or DataSource update is in scope.

Existing backend data structures reused:
- Backend account role/status model from archived auth implementation.
- Backend Google identity mapping and callback auth behavior.
- Backend token issuance based on server-side account data.

## Data Sources

### Backend Auth API
Design ID: `SD-030`

Maps to: `TL-002`, `TL-003`, `TL-007`, `TL-008`, `FR-004`, `FR-008`, `FR-009`, `FR-012`, `NFR-006`, `AC-003`, `AC-007`, `AC-008`, `AC-010`

Data source:
- Runtime-configured backend base URL.

Consumed endpoints:
- `POST /auth/login`
- `GET /auth/google`
- `GET /auth/google/callback`

Response dependency:
- Envelope with `Success`, `Data`, and `Errors`.
- Auth result containing token and account data.

## Frontend Models

### AuthRole
Design ID: `SD-031`

Maps to: `TL-004`, `TL-010`, `FR-015`, `FR-016`, `FR-017`, `FR-022`, `NFR-004`, `AC-012`, `AC-013`, `AC-016`

Allowed values:
```ts
type AuthRole = "Admin" | "Photographer";
```

Validation:
- Any value outside this set is invalid.
- Invalid role clears or rejects auth state.

### AuthAccount
Design ID: `SD-032`

Maps to: `TL-003`, `TL-004`, `TL-009`, `FR-012`, `FR-013`, `FR-017`, `FR-022`, `AC-010`, `AC-016`

Shape:
```ts
type AuthAccount = {
  id: string;
  role: AuthRole;
  status: "Active" | "Disabled" | string;
  email?: string | null;
  displayName?: string | null;
};
```

Mapping:
- Backend `Account.Id` -> `id`
- Backend `Account.Role` -> `role`
- Backend `Account.Status` -> `status`
- Backend `Account.Email` -> `email`
- Backend `Account.DisplayName` -> `displayName`

Validation:
- `id` must be present.
- `role` must be valid.
- `status` must not represent disabled account on successful auth.

### AuthSession
Design ID: `SD-033`

Maps to: `TL-003`, `TL-009`, `FR-012`, `FR-013`, `FR-021`, `FR-022`, `NFR-003`, `NFR-004`, `AC-010`, `AC-015`, `AC-016`

Shape:
```ts
type AuthSession = {
  accessToken: string;
  account: AuthAccount;
  authenticatedAt: string;
};
```

Optional extension:
```ts
type AuthSession = {
  expiresAt?: string;
};
```

Validation:
- `accessToken` must be non-empty.
- `account` must pass `AuthAccount` validation.
- `authenticatedAt` must be parseable or recreated during mapping.

### ApiEnvelope
Design ID: `SD-034`

Maps to: `TL-002`, `FR-004`, `FR-009`, `NFR-006`, `AC-003`, `AC-008`

Shape:
```ts
type ApiEnvelope<T> = {
  Success: boolean;
  Data: T | null;
  Errors: Array<{ Code?: string; Message?: string }>;
};
```

Frontend client should normalize this into a local result:
```ts
type ApiResult<T> =
  | { ok: true; data: T }
  | { ok: false; code?: string; message: string };
```

## State Ownership

### AuthProvider State
Design ID: `SD-035`

Maps to: `TL-003`, `TL-009`, `FR-012`, `FR-013`, `FR-021`, `NFR-003`, `NFR-004`, `AC-010`, `AC-015`

Owner:
- Client-side `AuthProvider`.

State fields:
- `session: AuthSession | null`
- `status: "loading" | "authenticated" | "anonymous"`
- `lastError: string | null`

Responsibilities:
- hydrate persisted session during app startup;
- validate persisted session shape;
- expose `setAuthenticatedSession`;
- expose `logout`;
- expose `hasRole(role)`;
- keep route guards informed.

### Form State
Design ID: `SD-036`

Maps to: `TL-005`, `TL-007`, `FR-003`, `FR-018`, `NFR-005`, `AC-002`, `AC-005`

Admin login form owns:
- username value;
- password value;
- field validation state;
- submit loading state;
- submit error state.

Validation:
- username is required;
- password is required;
- failed submission must not clear username automatically;
- password may be cleared after failure if Coding FE chooses.

### Callback State
Design ID: `SD-037`

Maps to: `TL-005`, `TL-008`, `FR-009`, `FR-019`, `AC-008`

Google callback page owns:
- processing state;
- callback error state;
- retry/back-to-login action state.

Behavior:
- show loading while callback is being processed;
- avoid duplicate callback processing;
- on success, immediately persist session and redirect;
- on failure, show a visible error and link back to `/login/photographer`.

## Persistence Strategy

### Browser Storage
Design ID: `SD-038`

Maps to: `TL-003`, `TL-009`, `FR-012`, `FR-013`, `FR-021`, `NFR-003`, `NFR-004`, `AC-010`, `AC-015`

Recommended storage:
- `localStorage` key: `evels.auth.session`

Rationale:
- No backend refresh/session endpoint is in scope.
- The requirement asks for persisted browser auth state.
- `localStorage` is simple for initial FE-only implementation and testable.

Constraints:
- Store only the token and account data required for routing/API auth.
- Validate stored JSON before use.
- Remove the key on logout, malformed state, wrong-role auth result, or disabled-account result.
- If Coding FE discovers backend cookie-based auth is already required, document the discovery and adapt the auth provider while preserving route guard behavior.

### Bootstrap Hydration
Design ID: `SD-039`

Maps to: `TL-003`, `TL-009`, `FR-013`, `NFR-004`, `AC-010`

Startup sequence:
1. AuthProvider starts with `status = "loading"`.
2. Read `evels.auth.session`.
3. Parse JSON.
4. Validate `AuthSession`.
5. If valid, set `status = "authenticated"`.
6. If missing or invalid, remove stored value and set `status = "anonymous"`.

No backend revalidation endpoint is required for this feature.

### Logout Clearing
Design ID: `SD-040`

Maps to: `TL-003`, `TL-009`, `FR-021`, `AC-015`

Logout sequence:
1. Remove `evels.auth.session`.
2. Clear in-memory session.
3. Clear auth error state.
4. Redirect to `/login/admin`, `/login/photographer`, or `/` based on current context.

## Cache And Revalidation
Design ID: `SD-041`

Maps to: `TL-003`, `TL-009`, `NFR-003`, `NFR-004`, `AC-010`

Auth API calls should not be cached:
- Admin login: no cache.
- Google initiation: no cache.
- Google callback completion: no cache.

Dashboard route shells for this feature require no remote data cache.

## Route Guard Data Dependencies
Design ID: `SD-042`

Maps to: `TL-004`, `TL-010`, `FR-014`, `FR-015`, `FR-016`, `FR-017`, `FR-022`, `NFR-004`, `AC-011`, `AC-012`, `AC-013`, `AC-016`

`RequireAuth` depends on:
- `AuthProvider.status`
- `AuthProvider.session`

`RequireRole("Admin")` depends on:
- valid session;
- `session.account.role === "Admin"`.

`RequireRole("Photographer")` depends on:
- valid session;
- `session.account.role === "Photographer"`.

Guard loading behavior:
- while auth status is `loading`, show a compact loading state and do not render protected content.

Guard failure behavior:
- anonymous user: redirect to login;
- wrong-role user: redirect to `/unauthorized`;
- malformed state: clear state and redirect to login.

## Backend Data Out Of Scope
Design ID: `SD-043`

Maps to: `TL-012`, `NFR-008`, `AC-017`

No backend schema, persistence model, migration, data source registration, or backend entity change is in scope. Existing backend account, role, status, identity, and token data structures are reused.
