# System Design Solution Overview

## Role Running
`system-design`

## Input Files
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `ai-docs/00-project/project-context.md`
- `ai-docs/00-project/constraints.md`
- `ai-docs/00-project/glossary.md`
- `ai-docs/02-team-lead/tl-delivery-plan.md`
- `ai-docs/02-team-lead/tl-task-breakdown.md`
- `ai-docs/02-team-lead/tl-risk-log.md`
- `ai-docs/02-team-lead/tl-handoff.md`
- `ai-docs/02-team-lead/tl-self-check.md`

## Allowed Output Directories
- `ai-docs/03-system-design/`

## Completion Artifacts
- `ai-docs/03-system-design/sd-solution-overview.md`
- `ai-docs/03-system-design/sd-domain-design.md`
- `ai-docs/03-system-design/sd-api-contract.md`
- `ai-docs/03-system-design/sd-data-design.md`
- `ai-docs/03-system-design/sd-implementation-guidelines.md`
- `ai-docs/03-system-design/sd-self-check.md`

## Feature Name
Photographer Credit Audit Logging

## Architecture Summary
This enhancement extends the existing photographer-credit update flow inside the existing Users feature boundary. The design keeps the credit update request path in the current `Users` module and adds an audit-record write as part of the same application operation when, and only when, a valid admin-driven photographer credit update results in a real change to the stored credit value.

The solution does not introduce a disconnected audit subsystem, event bus, or general-purpose history platform. It adds a narrow audit record model dedicated to admin photographer-credit changes and keeps all broader audit/history concerns out of scope.

## Main Design Decision
- `SD-001` maps to `TL-001`, `FR-001`, `FR-002`, `AC-001`, `AC-002`:
  Reuse the existing `Users` module and existing admin credit update flow as the orchestration boundary for this enhancement.
- `SD-002` maps to `TL-002`, `FR-003`, `FR-008`, `AC-003`, `AC-008`:
  Perform change detection inside the credit update use case after authorization and target validation, before persistence of the audit record.
- `SD-003` maps to `TL-003`, `FR-004`, `FR-005`, `FR-006`, `FR-007`, `AC-004`, `AC-005`, `AC-006`, `AC-007`:
  Persist a dedicated audit record containing actor identity, target identity, previous credit, new credit, and action timestamp.
- `SD-004` maps to `TL-004`, `FR-009`, `AC-009`:
  Denied, invalid, or non-photographer-targeted requests must terminate before any audit record is created.
- `SD-005` maps to `TL-005`, `TL-006`, `FR-010`, `AC-010`:
  Keep API and persistence scope narrow: no audit-read API is required in this phase, and no generalized audit-history abstraction is introduced.

## Module Placement
- Primary module: `src/Modules/Users`
- Layers involved:
  - `Presentation`: existing credit update controller/request boundary
  - `Application`: credit update orchestration plus audit persistence coordination
  - `Domain`: invariants around photographer-only credit, admin-only update authorization, and audit record semantics
  - `Infrastructure`: ORM entities, repository implementations, and database registration/migration

## High-Level Flow
1. Admin calls the existing or approved `PATCH /users/:id/credit` endpoint.
2. Presentation validates request shape and passes actor identity plus target ID and new credit value into the application layer.
3. Application loads the target user and validates:
   - actor is admin
   - target exists
   - target role is `Photographer`
4. Application compares requested credit with current stored credit.
5. If values are equal:
   - no audit record is created
   - response returns success with unchanged current state
6. If values differ:
   - update target credit
   - create audit record
   - persist both operations within one approved consistency boundary
   - return success with updated current state

## Error And No-Op Boundaries
- `SD-006` maps to `TL-002`, `FR-008`, `AC-008`:
  No-op updates are successful no-change operations for API purposes and do not create audit records.
- `SD-007` maps to `TL-004`, `FR-002`, `FR-009`, `AC-002`, `AC-009`:
  Unauthorized actors, invalid payloads, missing users, and non-photographer targets fail before audit persistence.

## Security And Authorization
- `SD-008` maps to `TL-001`, `TL-004`, `NFR-001`, `NFR-002`, `AC-002`:
  Authorization continues to rely on server-side actor role from the authenticated request context.
- `SD-009` maps to `TL-004`, `NFR-002`, `AC-009`:
  Audit record creation is contingent on successful authorization and valid photographer target checks.

## Assumptions Carried Forward
- `Q1`: No-op updates return a normal success response with current state, while still suppressing audit creation.
- `Q2`: Audit records are internal persistence artifacts in this phase; no read API is required.
- `Q3`: Admin and target names are stored as snapshots in the audit record to preserve accountability over time.
- `Q4`: Action timestamp is generated server-side in UTC using the application/database time source used for persisted timestamps.
- `Q5`: Historical backfill logging is not part of this design; only future successful admin credit changes are logged.
