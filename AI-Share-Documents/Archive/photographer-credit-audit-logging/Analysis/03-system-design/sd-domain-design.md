# System Design Domain Design

## Domain Scope
This enhancement extends the existing photographer-credit update domain. It does not introduce a credit ledger. It introduces a dedicated audit record whose only domain purpose is to capture traceable admin photographer-credit changes.

## Domain Elements

### SD-010: UserEntity Remains Credit Owner
Maps to: `TL-001`, `FR-001`, `FR-009`, `AC-001`, `AC-009`

- Existing `UserEntity` remains the business owner of `Credit`.
- `Credit` remains valid only for users whose role is `Photographer`.
- Non-photographer users remain ineligible to hold usable credit.

### SD-011: Credit Update Actor Rule
Maps to: `TL-001`, `TL-004`, `FR-002`, `AC-002`

- Only actors with role `Admin` may execute the photographer credit update use case.
- Authorization is evaluated before change detection and before any audit creation.

### SD-012: Credit Update Audit Record
Maps to: `TL-003`, `FR-003`, `FR-004`, `FR-005`, `FR-006`, `FR-007`, `AC-003`, `AC-004`, `AC-005`, `AC-006`, `AC-007`

Introduce a domain record/entity concept named `CreditAuditLogEntity` or equivalent domain model with these fields:
- `Id`
- `ActorUserId`
- `ActorUserName`
- `TargetUserId`
- `TargetUserName`
- `PreviousCredit`
- `NewCredit`
- `CreatedAt`

This entity is append-only in this feature scope.

## Domain Invariants

### SD-013: Change-Only Audit Invariant
Maps to: `TL-002`, `FR-003`, `FR-008`, `NFR-002`, `AC-003`, `AC-008`

- An audit record is valid only if:
  - actor is authorized
  - target is a valid photographer
  - `PreviousCredit` and `NewCredit` are both known
  - `PreviousCredit != NewCredit`

### SD-014: No False Audit Invariant
Maps to: `TL-004`, `FR-009`, `NFR-002`, `AC-002`, `AC-009`

- No audit record may be created for:
  - non-admin actor attempts
  - non-photographer targets
  - validation failures
  - missing target users
  - no-op updates

### SD-015: Snapshot Identity Invariant
Maps to: `TL-003`, `FR-004`, `FR-005`, `NFR-003`, `AC-004`, `AC-005`

- Admin name and target user name are stored as snapshots at the time of the successful change.
- The audit record must remain self-describing even if user names change later.

## Application Use Case Design

### SD-016: Extend Existing UpdateUserCreditUseCase
Maps to: `TL-001`, `TL-006`, `FR-003`, `FR-008`, `AC-003`, `AC-008`

Recommended approach:
- Extend the existing `UpdateUserCreditUseCase` rather than creating a second orchestration entry point.
- Responsibilities:
  1. Validate actor role
  2. Load target user
  3. Validate target is `Photographer`
  4. Compare current and requested credit
  5. Return unchanged result with no audit write if equal
  6. Apply credit update if changed
  7. Build audit record snapshot
  8. Persist update and audit record within one consistency boundary
  9. Return updated user DTO

### SD-017: No New Public Audit Use Case Required
Maps to: `TL-005`, `FR-010`, `AC-010`

- No separate audit browsing/listing use case is required in this phase.
- Audit persistence is a side effect of successful credit changes only.

## Repository Contracts

### SD-018: Reuse IUserRepository
Maps to: `TL-001`, `TL-006`, `FR-001`, `FR-002`

- Existing `IUserRepository` remains responsible for loading and persisting user credit state.

### SD-019: Add ICreditAuditLogRepository
Maps to: `TL-003`, `FR-003`, `FR-004`, `FR-005`, `FR-006`, `FR-007`

Define a new repository contract for append-only audit writes, for example:
- `Create(auditLog: CreditAuditLogEntity): Promise<CreditAuditLogEntity | void>`

No read methods are required in this phase unless needed for internal verification during coding or testing.

### SD-020: Add Coordinated Save Boundary
Maps to: `TL-003`, `TL-006`, `NFR-004`, `AC-003`

The design needs one of these coordination options:
- preferred: transaction-capable application service or repository coordination wrapper that persists both user update and audit write atomically
- fallback: sequential persistence with documented failure handling if the repo stack cannot support cross-entity transaction wiring cleanly

Preferred decision:
- Use one database transaction if the existing TypeORM setup can coordinate both writes in the same data source.

## Error Cases

### SD-021: Unauthorized Actor
Maps to: `TL-004`, `FR-002`, `AC-002`

- Reject with existing forbidden/authorization error.
- No audit record created.

### SD-022: Missing Target User
Maps to: `TL-004`, `NFR-002`

- Reject with existing not-found error.
- No audit record created.

### SD-023: Non-Photographer Target
Maps to: `TL-004`, `FR-009`, `AC-009`

- Reject with existing business-rule error used for invalid credit targets.
- No audit record created.

### SD-024: No-Op Update
Maps to: `TL-002`, `FR-008`, `AC-008`

- Return success with unchanged user state.
- No audit record created.

### SD-025: Audit Persistence Failure
Maps to: `TL-003`, `NFR-004`

- If transaction support is used, entire operation rolls back.
- If fallback sequential persistence is required, implementation guidelines must call out that Coding needs explicit failure handling and must document residual risk.
