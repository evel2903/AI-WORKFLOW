# Testing Backend Gaps

## Role Running
`testing-be`

## Gaps

### TG-001: Real Database Migration And Constraint Execution Not Covered
Status: KNOWN GAP

Reason:
This phase adds unit and controller-level tests only. The new `Credit` column migration, database check constraints, and runtime migration registration were not executed against a real database in the test suite.

Risk:
Constraint names, migration ordering, or database-specific check behavior could still fail outside the mocked test environment.

Mitigation:
Execute migrations and a small integration smoke test against a staging database before release.

### TG-002: Full JWT And Role Guard Wiring Not Executed End-To-End
Status: KNOWN GAP

Reason:
`UserController` tests override `JwtAuthGuard` and `RolesGuard` to focus on request validation and controller/use-case wiring for the new credit endpoint.

Risk:
An environment-specific issue in JWT strategy configuration or guard composition could block the admin credit path even though controller tests pass.

Mitigation:
Add authenticated integration coverage with a real test token and backing persistence when that harness exists.

### TG-003: No Separate Migration Backfill Test Beyond Login-Time Initialization
Status: PARTIALLY COVERED

Reason:
The approved design does not bulk backfill existing photographers in the migration. Coverage instead proves the supported behavior that a legacy photographer with `Credit = null` is initialized to `10` on successful login.

Risk:
Photographers who never log in after rollout will remain `null` until that first successful login.

Mitigation:
This matches the approved design and is documented in upstream coding and system-design artifacts. No additional test is required unless rollout strategy changes.
