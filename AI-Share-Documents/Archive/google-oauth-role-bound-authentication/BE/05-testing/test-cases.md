# Testing Backend Test Cases

## Role Running
`testing-be`

## Test Cases

### TC-001: Admin Password Login Succeeds For Existing Admin
Maps to: `CD-006`, `SD-009`, `SD-018`, `TL-006`, `FR-001`, `AC-001`, `AC-012`

Verifies `LoginUseCase` lowercases email lookup, accepts an existing active `Admin` with valid password, signs token from server-side account data, and returns `Account.Role = Admin`.

Covered by:
- `test/Modules/Authentication/Auth.LoginUseCaseSpec.ts`
- `test/Modules/Authentication/E2E.AuthControllerSpec.ts`

### TC-002: Photographer Password Login Is Rejected
Maps to: `CD-006`, `SD-018`, `TL-006`, `FR-005`, `FR-011`, `AC-007`, `AC-013`

Verifies `LoginUseCase` rejects password login for `Photographer` accounts without verifying password or issuing a token.

Covered by:
- `test/Modules/Authentication/Auth.LoginUseCaseSpec.ts`

### TC-003: First Google Login Creates Photographer
Maps to: `CD-005`, `SD-004`, `SD-005`, `SD-016`, `TL-003`, `FR-005`, `FR-006`, `AC-003`, `AC-004`

Verifies first successful Google login creates a new active passwordless account with role `Photographer`, Google subject, verified email flag, and server-issued token.

Covered by:
- `test/Modules/Authentication/Auth.AuthenticateWithGoogleUseCaseSpec.ts`

### TC-004: Repeated Google Login Preserves Existing Role
Maps to: `CD-005`, `SD-005`, `SD-008`, `TL-004`, `FR-007`, `FR-008`, `FR-009`, `AC-005`, `AC-006`

Verifies existing Google-linked accounts authenticate without creating duplicates and without mutating stored role, including the manually linked Admin assumption carried from System Design.

Covered by:
- `test/Modules/Authentication/Auth.AuthenticateWithGoogleUseCaseSpec.ts`

### TC-005: Google OAuth Does Not Create Admin
Maps to: `CD-005`, `SD-003`, `SD-017`, `TL-002`, `FR-002`, `FR-003`, `FR-004`, `AC-002`, `AC-003`

Verifies an existing Admin email that is not linked to the Google subject causes conflict instead of Admin creation, conversion, or token issuance.

Covered by:
- `test/Modules/Authentication/Auth.AuthenticateWithGoogleUseCaseSpec.ts`

### TC-006: Disabled Accounts Cannot Login
Maps to: `CD-002`, `CD-005`, `CD-006`, `SD-007`, `TL-008`, `FR-015`, `NFR-006`, `AC-011`

Verifies disabled Admin password login and disabled Google-linked Photographer login are denied before token issuance.

Covered by:
- `test/Modules/Authentication/Auth.LoginUseCaseSpec.ts`
- `test/Modules/Authentication/Auth.AuthenticateWithGoogleUseCaseSpec.ts`

### TC-007: Google Token Validation Uses Server-Side Verification
Maps to: `CD-005`, `SD-021`, `SD-023`, `TL-007`, `NFR-001`, `NFR-003`, `NFR-004`, `AC-009`, `AC-010`

Verifies `GoogleTokenVerifier` calls Google tokeninfo, validates configured audience, requires verified email, and maps provider identity without trusting client-provided role data.

Covered by:
- `test/Modules/Authentication/Auth.GoogleTokenVerifierSpec.ts`

### TC-008: OAuth Callback Exchanges Code And Ignores Extra Client Query Identity Fields
Maps to: `CD-005`, `SD-021`, `SD-023`, `TL-007`, `NFR-002`, `NFR-004`, `AC-009`, `AC-010`

Verifies `AuthController` accepts Google callback fields `code` and `state`, tolerates provider-added query fields, exchanges the code server-side, and authenticates only with the resulting server-side ID token.

Covered by:
- `test/Modules/Authentication/E2E.AuthControllerSpec.ts`
- `test/Modules/Authentication/Auth.GoogleOAuthClientSpec.ts`

### TC-009: Public Registration Is Unsupported
Maps to: `CD-006`, `SD-027`, `TL-006`, `FR-012`, `FR-013`, `FR-014`, `AC-008`, `AC-013`

Verifies `RegisterUseCase` and `POST /auth/register` reject public registration without creating accounts.

Covered by:
- `test/Modules/Authentication/Auth.RegisterUseCaseSpec.ts`
- `test/Modules/Authentication/E2E.AuthControllerSpec.ts`

### TC-010: Manual Admin Provisioning Boundary Remains Admin-Only
Maps to: `CD-004`, `SD-030`, `SD-037`, `TL-002`, `FR-002`, `FR-004`, `AC-002`

Verifies `CreateUserUseCase` creates Admin accounts through the protected user-management boundary and rejects duplicate email.

Covered by:
- `test/Modules/Users/Users.CreateUserUseCaseSpec.ts`
- `test/Modules/Users/E2E.UserControllerSpec.ts`

### TC-011: Google Auth Start Returns Callback URL Contract
Maps to: `CD-005`, `SD-023`, `TL-003`, `AC-009`

Verifies `GET /auth/google` returns `AuthorizationUrl` and `CallbackUrl` JSON instead of a 302 browser redirect.

Covered by:
- `test/Modules/Authentication/E2E.AuthControllerSpec.ts`
- `test/Modules/Authentication/Auth.GoogleOAuthClientSpec.ts`
