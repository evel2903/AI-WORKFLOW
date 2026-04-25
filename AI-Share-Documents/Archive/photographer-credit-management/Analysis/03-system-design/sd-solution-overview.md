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

## Preflight Result
Status: PASSED

`ai-docs/02-team-lead/tl-self-check.md` was read first and confirms Team Lead readiness for System Design.

## Design Goal
Design a NestJS Clean Architecture solution that adds a photographer-only `Credit` attribute, initializes it to `10` on first successful photographer login, allows only admins to update it manually, prevents non-photographer roles from having usable credit, and keeps the feature strictly limited to storage and admin-managed updates.

## Proposed Module Boundary
Target module ownership:
- `src/Modules/Users`: owns persisted user/account state, including role and `Credit`.
- `src/Modules/Authentication`: owns successful photographer login flow and triggers one-time credit initialization when needed.

Layer alignment:
- `Presentation`: admin credit update endpoint and any response shaping for approved read surfaces.
- `Application`: orchestrates first-login credit initialization and admin-managed update use cases.
- `Domain`: owns role-based credit eligibility rules, one-time default initialization rule, and numeric constraints.
- `Infrastructure`: persists `Credit`, executes guarded updates, and wires migrations or ORM changes.

If the codebase uses equivalent module names or shared user/account abstractions, Coding must adapt to those names while preserving the ownership boundary above.

## Key Design Decisions
- `SD-001`: Reuse the existing authenticated user/account record as the owner of `Credit` instead of creating a standalone credit subsystem. Maps to `TL-001`, `TL-006`, `FR-001`, `FR-008`, `NFR-006`, `AC-001`, `AC-008`.
- `SD-002`: Treat `Credit` as a role-bound attribute that is eligible only for `Photographer` users. Maps to `TL-001`, `FR-001`, `FR-002`, `FR-010`, `NFR-001`, `AC-001`, `AC-002`, `AC-010`.
- `SD-003`: Represent non-photographer credit as non-usable by keeping persistence value `null` for non-photographer roles and omitting it from non-photographer API payloads. Maps to `TL-004`, `TL-005`, `FR-002`, `FR-007`, `FR-010`, `AC-002`, `AC-007`, `BAQ-001`, `BAQ-004`.
- `SD-004`: Initialize `Credit = 10` exactly once on first successful photographer login when the stored credit is still uninitialized. Maps to `TL-002`, `FR-005`, `NFR-003`, `AC-005`, `BAQ-002`.
- `SD-005`: Preserve existing stored credit on subsequent photographer logins and never reinitialize it when a value already exists. Maps to `TL-002`, `FR-006`, `NFR-003`, `AC-006`.
- `SD-006`: Allow only authenticated admins to manually update photographer credit through an admin-guarded backend operation. Maps to `TL-003`, `FR-003`, `FR-004`, `NFR-002`, `AC-003`, `AC-004`.
- `SD-007`: Reject credit assignment or update attempts for non-photographer targets, even when the caller is an admin. Maps to `TL-004`, `FR-007`, `FR-010`, `NFR-004`, `AC-007`.
- `SD-008`: Treat credit as a non-negative whole-number business value. Maps to `TL-003`, `TL-006`, `FR-003`, `FR-008`, `NFR-003`, `NFR-004`, `BAQ-003`.
- `SD-009`: Do not introduce spending, recharge, expiration, transaction history, or automated credit adjustments in this feature. Maps to `TL-008`, `FR-009`, `NFR-005`, `AC-009`.
- `SD-010`: Expose credit only in approved photographer-relevant response contexts and not as a universal field on every user payload. Maps to `TL-005`, `FR-008`, `FR-010`, `AC-008`, `BAQ-004`.

## Main Flows

### Photographer Login With Uninitialized Credit
1. Photographer successfully authenticates through the existing authentication flow.
2. Authentication module loads the persisted user/account record.
3. If role is `Photographer` and `Credit` is `null`, application layer initializes `Credit = 10`.
4. Updated user/account state is persisted before success response is finalized.
5. Response returns the authenticated result using approved photographer-visible payload rules.

### Photographer Login With Existing Credit
1. Photographer successfully authenticates.
2. Authentication module loads user/account record.
3. If role is `Photographer` and `Credit` already has a value, no reinitialization occurs.
4. Existing stored value remains unchanged.

### Admin Manual Credit Update
1. Admin sends an authenticated request to the admin-only credit update endpoint.
2. Presentation layer enforces admin authorization.
3. Application layer loads target user/account.
4. If target role is not `Photographer`, the request fails without mutation.
5. If requested credit is invalid, the request fails without mutation.
6. Otherwise, the new credit value is persisted and returned in the success response.

## Error And Rejection Cases
- Non-admin actor attempts manual credit update.
- Admin targets a non-photographer user for credit update.
- Admin submits invalid numeric credit input.
- Photographer login occurs with already initialized credit and must not overwrite it.
- Non-photographer response context attempts to expose usable credit.

## Security And Authorization
- Role eligibility and update permission must be decided using server-side persisted role data only.
- Client input must not be trusted to declare target eligibility or caller authorization.
- Admin-only manual update path must be guard-protected in Presentation and revalidated in Application.
- The feature does not create a self-service path for photographers to alter their own credit.

## Assumptions Carried Forward
- `BAQ-001`: Non-photographer credit may be hidden or omitted in API responses, but must remain non-usable.
- `BAQ-002`: Legacy photographers with `Credit = null` may receive the default `10` on their first successful login after rollout.
- `BAQ-003`: Credit is a non-negative whole-number business value.
- `BAQ-004`: Credit exposure may be limited to approved backend use cases rather than every response contract.

## Out Of Scope
- Credit spending behavior.
- Recharge, top-up, expiration, or scheduled reset behavior.
- Credit transaction history or ledger.
- Non-admin credit management.
- Unrelated redesign of authentication or user lifecycle.
