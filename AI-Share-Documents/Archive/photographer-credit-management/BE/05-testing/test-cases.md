# Testing Backend Test Cases

## Role Running
`testing-be`

## Test Cases

### TC-001: First Successful Photographer Login Initializes Credit To 10
Maps to: `CD-002`, `SD-004`, `SD-005`, `TL-002`, `FR-005`, `AC-005`

Verifies a first successful photographer Google login creates a photographer account with `Credit = 10`.

Covered by:
- `test/Modules/Authentication/Auth.AuthenticateWithGoogleUseCaseSpec.ts`

### TC-002: Legacy Photographer With Null Credit Is Initialized On Successful Login
Maps to: `CD-002`, `CD-004`, `SD-005`, `SD-028`, `TL-005`, `TL-009`, `FR-005`, `FR-008`, `AC-005`, `AC-008`

Verifies the approved legacy assumption that an existing photographer with stored `Credit = null` is set to `10` on successful login instead of requiring migration backfill.

Covered by:
- `test/Modules/Authentication/Auth.AuthenticateWithGoogleUseCaseSpec.ts`

### TC-003: Subsequent Photographer Logins Preserve Existing Credit
Maps to: `CD-002`, `SD-005`, `TL-002`, `FR-006`, `AC-006`

Verifies later successful photographer logins do not reset an existing stored credit value to the default.

Covered by:
- `test/Modules/Authentication/Auth.AuthenticateWithGoogleUseCaseSpec.ts`

### TC-004: Admin Can Manually Update Photographer Credit
Maps to: `CD-003`, `SD-006`, `SD-015`, `TL-003`, `FR-003`, `AC-003`

Verifies an admin actor can update a photographer target and that the stored and returned credit values reflect the approved new value.

Covered by:
- `test/Modules/Users/Users.UpdateUserCreditUseCaseSpec.ts`
- `test/Modules/Users/E2E.UserControllerSpec.ts`

### TC-005: Non-Admin Actors Cannot Update Photographer Credit
Maps to: `CD-003`, `SD-006`, `SD-020`, `TL-003`, `FR-004`, `AC-004`

Verifies non-admin actors are denied before the credit update is applied.

Covered by:
- `test/Modules/Users/Users.UpdateUserCreditUseCaseSpec.ts`

### TC-006: Non-Photographer Targets Cannot Receive Credit
Maps to: `CD-001`, `CD-003`, `SD-002`, `SD-007`, `TL-001`, `TL-004`, `FR-002`, `FR-007`, `AC-002`, `AC-007`

Verifies the admin update path fails safely when the target is not a photographer and leaves credit unusable.

Covered by:
- `test/Modules/Users/Users.UpdateUserCreditUseCaseSpec.ts`

### TC-007: Non-Photographer Responses Do Not Expose Usable Credit
Maps to: `CD-001`, `SD-003`, `TL-004`, `FR-002`, `FR-010`, `AC-002`, `AC-010`

Verifies DTO mapping includes `Credit` for photographers and omits it for non-photographer users.

Covered by:
- `test/Modules/Users/Users.UserDtoMapperSpec.ts`

### TC-008: Admin Credit Endpoint Validates Request And Uses Server-Side Actor Role
Maps to: `CD-003`, `SD-022`, `SD-023`, `SD-024`, `TL-007`, `NFR-002`, `NFR-004`, `AC-003`, `AC-010`

Verifies `PATCH /users/:id/credit` accepts a valid non-negative whole-number payload, forwards the admin actor role from the authenticated request context, and rejects invalid negative input.

Covered by:
- `test/Modules/Users/E2E.UserControllerSpec.ts`

### TC-009: Broader Credit Engine Routes Remain Out Of Scope
Maps to: `CD-003`, `SD-025`, `TL-008`, `FR-009`, `AC-009`

Verifies unsupported routes such as `POST /users/:id/credit/increase` are absent, demonstrating that no broader credit engine was introduced accidentally.

Covered by:
- `test/Modules/Users/E2E.UserControllerSpec.ts`
