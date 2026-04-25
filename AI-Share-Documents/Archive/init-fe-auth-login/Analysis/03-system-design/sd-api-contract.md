# System Design API Contract

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Contract Source And Reconciliation
Design ID: `SD-022`

Maps to: `TL-002`, `TL-008`, `FR-008`, `FR-009`, `NFR-006`, `AC-007`, `AC-008`, `RISK-003`

This file defines the frontend-consumed backend contracts for this FE-only feature. It does not request backend changes.

Contract reconciliation:
- The current request requires reusing the backend callback-based Google auth flow.
- Archived BE coding and testing confirm `GET /auth/google` and `GET /auth/google/callback`.
- Archived `sd-api-contract.md` also documents an older `POST /auth/google/login` ID-token flow.
- For this feature, `GET /auth/google` and `GET /auth/google/callback` are authoritative. `POST /auth/google/login` is not used by the frontend unless Coding FE discovers it is the only currently deployed endpoint, in which case Coding FE must document the contradiction and stop for workflow guidance rather than modifying backend code.

## Response Envelope
Design ID: `SD-023`

Maps to: `TL-002`, `FR-004`, `FR-009`, `NFR-006`, `AC-003`, `AC-008`

Expected backend envelope:
```json
{
  "Success": true,
  "Data": {},
  "Errors": []
}
```

Frontend rules:
- Treat `Success: true` and valid `Data` as success.
- Treat `Success: false`, HTTP 4xx/5xx, missing `Data`, or malformed payload as failure.
- Error messages displayed to users should be mapped to clear frontend messages, not raw stack traces.

## Shared Auth Result Shape
Design ID: `SD-024`

Maps to: `TL-002`, `TL-003`, `TL-007`, `TL-008`, `TL-009`, `FR-012`, `FR-017`, `FR-022`, `AC-004`, `AC-009`, `AC-010`, `AC-016`

Expected success data:
```json
{
  "AccessToken": "application-auth-token",
  "Account": {
    "Id": "account-id",
    "Role": "Admin",
    "Status": "Active",
    "Email": "user@example.com",
    "DisplayName": "Display Name"
  }
}
```

Frontend mapping:
- `AccessToken` maps to `authSession.accessToken`.
- `Account.Id` maps to `authSession.account.id`.
- `Account.Role` maps to `authSession.account.role`.
- `Account.Status` maps to `authSession.account.status`.
- `Account.Email` maps to `authSession.account.email`.
- `Account.DisplayName` maps to `authSession.account.displayName`.

Accepted token field aliases:
- Prefer `AccessToken`.
- If backend currently returns `accessToken`, `Token`, or another known auth-token field, Coding FE may support the discovered field through the isolated API client and document it in `coding-plan.md`.

Role validation:
- `Admin` and `Photographer` are the only accepted frontend roles.
- Missing or unknown role means auth failure.

## Admin Login
Design ID: `SD-025`

Maps to: `TL-002`, `TL-007`, `FR-003`, `FR-004`, `FR-005`, `FR-006`, `FR-018`, `FR-020`, `NFR-005`, `NFR-006`, `AC-002`, `AC-003`, `AC-004`, `AC-005`, `AC-014`, `BAQ-001`

Method and path:
- `POST /auth/login`

Frontend form labels:
- `Username`
- `Password`

Request body integration assumption:
```json
{
  "Username": "admin-username",
  "Password": "admin-password"
}
```

Assumption note:
- Current artifacts prove `/auth/login` exists for Admin password login but do not prove exact request field names.
- Coding FE must confirm request DTO shape from backend source or runtime contract before finalizing the API client.
- If backend expects a different field such as `Email`, the UI can still show business label `Username` while the API client maps to the backend field.

Success handling:
- On `Success: true`, map auth result.
- Require `Account.Role = Admin`.
- Persist auth state.
- Redirect to `/admin`.

Failure handling:
- Invalid credentials: show "Username or password is incorrect."
- Disabled account: show "This account is disabled. Contact an administrator."
- Network failure: show "Unable to sign in. Check your connection and try again."
- Malformed response: show "Sign in could not be completed. Try again."
- Non-Admin role returned: clear auth state and show "This sign-in method is only for Admin users."

## Google Auth Initiation
Design ID: `SD-026`

Maps to: `TL-002`, `TL-008`, `FR-007`, `FR-008`, `FR-011`, `FR-019`, `NFR-005`, `NFR-006`, `AC-006`, `AC-007`, `AC-008`, `BAQ-005`

Method and path:
- `GET /auth/google`

Expected success data:
```json
{
  "AuthorizationUrl": "https://accounts.google.com/o/oauth2/v2/auth?...",
  "CallbackUrl": "https://backend.example.com/auth/google/callback"
}
```

Frontend handling:
- Call this endpoint when Photographer selects Google sign-in.
- Require `AuthorizationUrl`.
- Navigate the browser to `AuthorizationUrl`.
- `CallbackUrl` may be logged or used only for diagnostics/config validation; frontend should not construct the Google URL itself.

Failure handling:
- Missing `AuthorizationUrl`: show "Google sign-in could not be started. Try again."
- Network failure: show "Unable to start Google sign-in. Check your connection and try again."
- Backend error: show "Google sign-in is unavailable. Try again later."

## Google Callback Completion
Design ID: `SD-027`

Maps to: `TL-002`, `TL-008`, `FR-009`, `FR-010`, `FR-017`, `FR-019`, `FR-020`, `NFR-005`, `NFR-006`, `AC-008`, `AC-009`, `AC-014`, `BAQ-005`

Method and path:
- `GET /auth/google/callback`

Expected callback inputs:
- `code`
- `state`
- Optional error query fields from Google/backend.

Frontend route:
- `/auth/google/callback`

Integration pattern:
- Preferred: frontend callback page forwards the current callback query string to backend `GET /auth/google/callback`, then maps the backend auth result.
- Acceptable if backend redirects to frontend with an auth payload or token exchange result: Coding FE must keep the parsing isolated in the auth API/callback module and document the discovered callback behavior.

Success handling:
- On valid auth result, require `Account.Role = Photographer`.
- Persist auth state.
- Redirect to `/photographer`.

Failure handling:
- Google/backend error query: show "Google sign-in was cancelled or failed."
- Invalid callback response: show "Google sign-in could not be completed. Try again."
- Disabled account: show "This account is disabled. Contact an administrator."
- Non-Photographer role returned: clear auth state and show "This sign-in method is only for Photographer users."

## Authenticated API Calls
Design ID: `SD-028`

Maps to: `TL-003`, `TL-009`, `FR-012`, `FR-013`, `FR-021`, `NFR-003`, `AC-010`, `AC-015`

For future authenticated frontend calls, the API client should attach the stored token as:
- `Authorization: Bearer <accessToken>`

This feature only needs authenticated state for route access and dashboard route shells unless Coding FE discovers an existing backend user/session validation endpoint. Do not add backend endpoints.

## Error Code Mapping
Design ID: `SD-029`

Maps to: `TL-005`, `FR-018`, `FR-019`, `FR-020`, `NFR-005`, `AC-005`, `AC-008`, `AC-014`

Known backend error codes from archive:
- `AUTH_INVALID_GOOGLE_TOKEN`: Google sign-in could not be completed.
- `AUTH_ACCOUNT_DISABLED`: This account is disabled. Contact an administrator.
- `AUTH_UNTRUSTED_CLIENT_AUTH_DATA`: Sign in could not be completed.
- Unsupported auth/register/customer errors: Sign in method is not available.

Frontend must also handle:
- HTTP 401/403 as auth failure.
- HTTP 5xx as service unavailable.
- Network timeout/failure as connection issue.
- Unknown/malformed error as generic failure.
