# System Design Implementation Guidelines

## Coding Direction

### SD-051: Reuse Existing Users Module
Maps to: `TL-001`, `TL-006`

- Keep implementation in `src/Modules/Users`.
- Do not create a new top-level audit module for this narrow feature.

## Files To Create
- Domain/persistence model for audit records:
  - domain entity or record model for `CreditAuditLogEntity`
  - repository interface `ICreditAuditLogRepository`
  - ORM entity for `credit_audit_logs`
  - repository implementation for audit writes
- Migration to create `credit_audit_logs`

## Files To Update
- Existing credit update use case
- Existing `UserModule`
- Existing database registration files:
  - `src/Shared/Database/TypeOrmDataSource.ts`
  - `src/App.module.ts` only if the current wiring pattern requires it

## Layer Responsibilities

### Presentation
- Keep request validation for `Credit` shape and role-guard enforcement at the controller/guard boundary.
- Do not create audit records in controllers.

### Application
- Orchestrate:
  - actor authorization recheck
  - target load
  - target-role validation
  - no-op comparison
  - credit mutation
  - audit record creation
  - coordinated persistence

### Domain
- Keep photographer-only credit invariant and audit record invariants explicit.
- No domain logic for generalized audit browsing or credit history summaries.

### Infrastructure
- Persist audit records in the dedicated table.
- Handle transactional coordination if using TypeORM transaction support.

## Guardrails

### SD-052: Order Of Operations
Maps to: `TL-002`, `TL-004`, `NFR-002`, `NFR-004`

Required order:
1. validate actor
2. load target
3. validate target eligibility
4. compare current and requested credit
5. short-circuit success for no-op
6. update credit and create audit record together

### SD-053: No Silent Scope Growth
Maps to: `TL-005`, `FR-010`

Do not add:
- audit list endpoints
- search/reporting
- event bus publication
- generalized audit abstractions
- historical replay/backfill

### SD-054: Snapshot Names At Write Time
Maps to: `TL-003`, `FR-004`, `FR-005`

- Use the actor and target names available at the successful update moment to populate the audit record.
- Do not rely on later user lookups to reconstruct names.

### SD-055: Preserve Existing Response Envelope
Maps to: `TL-005`

- Continue using `Success`, `Data`, and `Errors`.
- No schema change is required beyond the existing updated-user response payload for the credit update endpoint.

## Security Guardrails

### SD-056: Server-Side Actor Identity Only
Maps to: `TL-004`, `NFR-001`

- Actor ID and role must come from authenticated server-side context, not client request body.

### SD-057: No Audit Creation On Failed Attempts
Maps to: `TL-004`, `NFR-002`

- Failed authorization, validation, or target checks must not produce persisted audit rows.

## Testing Guidance For Downstream Coding And Testing

### SD-058: Required Test Focus
Maps to: `TL-007`, `AC-003`, `AC-008`, `AC-009`, `AC-010`

Downstream tests should prove:
- audit row created for real admin credit change
- audit row not created for no-op update
- audit row not created for unauthorized actor
- audit row not created for non-photographer target
- audit row contains actor snapshot, target snapshot, previous/new values, timestamp

## Explicit Out Of Scope
- Credit spending, recharge, expiration, automatic adjustment
- Non-admin credit management
- Historical audit backfill
- Public or generalized audit browsing APIs
- General-purpose audit platform abstractions
