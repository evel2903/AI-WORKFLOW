# System Design Data Design

## Decision
- [x] New or changed data design is required
- [ ] No new data design is required; existing structures are reused

## Data Design Summary
The existing `users` persistence model remains the owner of photographer `Credit`. This enhancement adds a new append-only audit persistence structure for admin photographer-credit changes. The audit structure is separate from the user table because it models historical events, not current user state.

## Entity Or Aggregate Candidates

### SD-036: Existing User Aggregate Reused
Maps to: `TL-001`, `FR-001`, `FR-002`, `FR-009`, `AC-001`, `AC-002`, `AC-009`

- Name:
  `UserEntity` / existing user aggregate
- Responsibility:
  Own current user state, including current `Credit`
- Key fields reused:
  - `Id`
  - `FirstName`
  - `LastName`
  - `Role`
  - `Credit`

### SD-037: New CreditAuditLogEntity
Maps to: `TL-003`, `FR-003`, `FR-004`, `FR-005`, `FR-006`, `FR-007`, `AC-003`, `AC-004`, `AC-005`, `AC-006`, `AC-007`

- Name:
  `CreditAuditLogEntity`
- Responsibility:
  Persist immutable audit records for successful admin photographer-credit changes
- Key fields:
  - `Id: string`
  - `ActorUserId: string`
  - `ActorUserName: string`
  - `TargetUserId: string`
  - `TargetUserName: string`
  - `PreviousCredit: number`
  - `NewCredit: number`
  - `CreatedAt: Date`

## Relationships And Ownership

### SD-038: Ownership Boundary
Maps to: `TL-003`, `NFR-004`

- Relationship:
  many audit records to one target user over time
- Ownership boundary:
  audit records are historical children of the credit-update business operation, not embedded state inside the user aggregate

### SD-039: Actor/Target User Referential Link
Maps to: `TL-003`, `FR-004`, `FR-005`

- Both `ActorUserId` and `TargetUserId` should reference existing user identities by ID at write time.
- Names are stored as snapshots, not derived later, to preserve accountability even if user names change.

## Persistence Mapping

### Domain To Persistence Direction
- `UserEntity` continues mapping to the existing users ORM entity/table.
- `CreditAuditLogEntity` maps to a new ORM entity/table dedicated to audit records.

### Recommended Table
`credit_audit_logs`

### Recommended Columns
- `Id` (`uuid` or repo-standard string ID)
- `ActorUserId` (`varchar` / matching user ID type, not null)
- `ActorUserName` (`varchar`, not null)
- `TargetUserId` (`varchar` / matching user ID type, not null)
- `TargetUserName` (`varchar`, not null)
- `PreviousCredit` (`int`, not null)
- `NewCredit` (`int`, not null)
- `CreatedAt` (`timestamp with time zone` if available, otherwise repo-standard persisted timestamp type, not null)

### SD-040: Numeric Type Expectations
Maps to: `TL-003`, `FR-006`, `AC-006`

- `PreviousCredit` and `NewCredit` use the same integer domain as current user `Credit`.
- Values must be non-negative whole numbers.

## Constraints

### SD-041: Required Fields
Maps to: `FR-004`, `FR-005`, `FR-006`, `FR-007`

All audit record fields listed above are required and non-null.

### SD-042: Change-Only Integrity Constraint
Maps to: `FR-003`, `FR-008`, `AC-003`, `AC-008`

Prefer both:
- application invariant: only create `CreditAuditLogEntity` if `PreviousCredit != NewCredit`
- database check constraint if supported:
  - `PreviousCredit <> NewCredit`

### SD-043: Referential Integrity
Maps to: `FR-004`, `FR-005`

- Foreign keys to user IDs are recommended for `ActorUserId` and `TargetUserId` if the existing schema conventions allow it cleanly.
- If repo conventions avoid FK constraints in this area, IDs may still be stored as scalar references, but Coding must document the chosen approach.

### SD-044: Timestamp Source
Maps to: `FR-007`, `AC-007`

- `CreatedAt` should be generated server-side in UTC.
- Prefer using the same timestamp creation pattern already used by other ORM entities in the repository.

## Storage Strategy

### SD-045: Separate Audit Table
Maps to: `TL-003`, `FR-010`, `AC-010`

- Use a dedicated audit table instead of expanding the users table.
- Reason:
  - audit records are append-only history
  - current user state and historical admin actions are different persistence concerns
  - keeps the current user schema focused on present state

### SD-046: No Audit Read Model Required
Maps to: `TL-005`, `FR-010`

- No separate read model, projection table, or public API storage optimization is required in this phase.

## Consistency Strategy

### SD-047: Shared Transaction Preferred
Maps to: `TL-003`, `TL-006`, `NFR-004`

Preferred implementation path:
- persist the user credit update and the new audit record in one TypeORM transaction using the shared data source

Fallback only if needed:
- sequential writes with explicit failure handling and documented residual risk

Coding should not choose the fallback silently; if transaction wiring is impractical, `coding-self-check.md` must document the deviation and risk.

## Migration Notes

### SD-048: Migration Required
Maps to: `TL-003`, `TL-006`

- A new migration is required to create `credit_audit_logs`.
- The migration must not backfill historical records.
- The migration does not alter the existing photographer credit column semantics.

### Recommended Migration Shape
- create `credit_audit_logs`
- add required columns
- add optional FK constraints to users if aligned with repo convention
- add `PreviousCredit <> NewCredit` check if supported
- add indexes:
  - `TargetUserId`
  - `ActorUserId`
  - `CreatedAt`

## TypeOrmDataSource And Module Wiring Notes

### SD-049: TypeOrmDataSource Update Required
Maps to: `TL-003`, `TL-006`

- Register the new migration in `src/Shared/Database/TypeOrmDataSource.ts`
- Register the new ORM entity in the appropriate TypeORM entity list/module wiring

### SD-050: App Module Notes
Maps to: `TL-006`

- `src/App.module.ts` only needs changes if entity/module registration patterns require it.
- If the repository already follows feature-module registration through `UserModule`, prefer local module wiring there over broad app-level changes.

## Reuse Notes
- Existing user persistence structures are reused for current credit state.
- New data design is required because historical audit events are materially different from current user state and cannot be represented safely as another field on the user row.
