# Testing Backend Test Cases

## Role Running
`testing-be`

## Test Cases

### TC-001: Changed Admin Credit Update Creates Audit Log
Maps to: `CD-001`, `CD-002`, `CD-003`, `SD-003`, `SD-016`, `AC-003`, `FR-003`

Verifies a valid admin credit update for a photographer changes credit and creates exactly one audit record.

Covered by:
- `test/Modules/Users/Users.UpdateUserCreditUseCaseSpec.ts`

### TC-002: Audit Log Captures Required Admin And Target Identity Fields
Maps to: `CD-001`, `CD-003`, `SD-012`, `AC-004`, `AC-005`, `FR-004`, `FR-005`

Verifies the audit record contains admin ID/name and target ID/name snapshots.

Covered by:
- `test/Modules/Users/Users.UpdateUserCreditUseCaseSpec.ts`

### TC-003: Audit Log Captures Previous And New Credit Plus Timestamp
Maps to: `CD-001`, `CD-003`, `SD-012`, `AC-006`, `AC-007`, `FR-006`, `FR-007`

Verifies the audit record stores before/after credit values and a created timestamp.

Covered by:
- `test/Modules/Users/Users.UpdateUserCreditUseCaseSpec.ts`

### TC-004: No-Op Update Does Not Create Audit Log
Maps to: `CD-003`, `SD-024`, `AC-008`, `FR-008`

Verifies that when requested credit equals current stored credit, the use case returns success without audit persistence.

Covered by:
- `test/Modules/Users/Users.UpdateUserCreditUseCaseSpec.ts`

### TC-005: Non-Admin Actors Cannot Update Credit Or Create Audit Logs
Maps to: `CD-003`, `SD-021`, `AC-002`, `FR-002`

Verifies non-admin actors are rejected before any audit side effect is created.

Covered by:
- `test/Modules/Users/Users.UpdateUserCreditUseCaseSpec.ts`

### TC-006: Invalid Actor Identity Does Not Create Audit Logs
Maps to: `CD-003`, `SD-021`, `NFR-001`, `NFR-002`

Verifies a caller with `ActorRole = Admin` but no persisted admin user record is rejected and does not create audit logs.

Covered by:
- `test/Modules/Users/Users.UpdateUserCreditUseCaseSpec.ts`

### TC-007: Non-Photographer Targets Fail Safely Without Audit Logs
Maps to: `CD-003`, `SD-023`, `AC-009`, `FR-009`

Verifies invalid update attempts against non-photographer targets fail without mutating credit or creating audit logs.

Covered by:
- `test/Modules/Users/Users.UpdateUserCreditUseCaseSpec.ts`

### TC-008: Legacy Photographer With Null Credit Uses Approved PreviousCredit Fallback
Maps to: `CD-003`, `AC-003`, `FR-006`

Verifies the coded legacy behavior where a photographer with `Credit = null` produces an audit record with `PreviousCredit = 0` on the first successful admin credit change.

Covered by:
- `test/Modules/Users/Users.UpdateUserCreditUseCaseSpec.ts`

### TC-009: Controller Forwards Server-Side Actor Identity Into Credit Update Use Case
Maps to: `CD-003`, `SD-026`, `SD-028`, `NFR-001`

Verifies `PATCH /users/:id/credit` passes `ActorUserId`, `ActorRole`, target ID, and credit value into the use case from authenticated request context.

Covered by:
- `test/Modules/Users/E2E.UserControllerSpec.ts`

### TC-010: Invalid Credit Payloads Fail Before Credit Update Logic
Maps to: `CD-003`, `SD-027`, `SD-033`, `NFR-001`

Verifies invalid request payloads are rejected before the use case runs.

Covered by:
- `test/Modules/Users/E2E.UserControllerSpec.ts`

### TC-011: Broader Credit Engine Routes Remain Out Of Scope
Maps to: `CD-003`, `AC-010`, `FR-010`

Verifies unsupported routes such as `POST /users/:id/credit/increase` remain absent.

Covered by:
- `test/Modules/Users/E2E.UserControllerSpec.ts`

### TC-012: Repository Contract Change Does Not Break Existing Auth And User Use Cases
Maps to: `CD-002`, `NFR-004`

Verifies the new `IUserRepository` shape is absorbed by existing auth and user tests without behavioral regressions.

Covered by:
- `test/Modules/Authentication/Auth.LoginUseCaseSpec.ts`
- `test/Modules/Authentication/Auth.AuthenticateWithGoogleUseCaseSpec.ts`
- `test/Modules/Users/Users.CreateUserUseCaseSpec.ts`
