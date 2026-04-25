# Business Analysis Feature Spec

## Role Running
`business-analyst`

## Input Files
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `ai-docs/00-project/project-context.md`
- `ai-docs/00-project/constraints.md`
- `ai-docs/00-project/glossary.md`
- `ai-docs/00-prompt-generation/wp-input.md`
- `ai-docs/00-prompt-generation/wp-metadata.md`
- `ai-docs/00-prompt-generation/wp-self-check.md`

## Allowed Output Directories
- `ai-docs/01-business-analysis/`

## Completion Artifacts
- `ai-docs/01-business-analysis/ba-feature-spec.md`
- `ai-docs/01-business-analysis/ba-acceptance-criteria.md`
- `ai-docs/01-business-analysis/ba-open-questions.md`
- `ai-docs/01-business-analysis/ba-self-check.md`

## Feature Name
Photographer Credit Audit Logging

## Business Goal
Extend the existing photographer-credit feature so every real admin-driven change to photographer credit is traceable through an audit log, improving accountability without changing the existing role-bound credit rules or expanding into a broader credit-history product.

## Problem Statement
The current feature allows admins to update photographer credit, but the request now requires those changes to be traceable. Without explicit business rules for when audit records are created and what they must contain, downstream implementation could log the wrong events, miss required accountability details, or create misleading records for no-op updates.

## Actors
- `Admin`: The only actor allowed to update photographer credit and the only actor whose successful credit-change actions can trigger audit logging.
- `Photographer`: The only role eligible to own credit and the only valid target role for successful credit updates.
- `Non-Photographer User`: Any user whose role is not `Photographer`; this actor is ineligible to own usable credit and is not a valid target for successful credit-update audit events.
- `System`: The backend service responsible for enforcing role rules, performing authorized credit updates, detecting real value changes, creating audit log records when required, and preventing false or misleading audit entries.

## In Scope
- Preserve the existing rule that `credit` is a dedicated attribute for users with role `Photographer`.
- Preserve the existing rule that only `Admin` users may update photographer credit.
- Create an audit log record whenever an admin updates photographer credit and the stored credit value actually changes.
- Require each audit log record to capture at minimum:
  - admin user ID
  - admin user name
  - target user ID
  - target user name
  - previous credit value
  - new credit value
  - action timestamp
- Do not require an audit log when the requested update does not change the stored credit value.
- Preserve the existing rule that non-photographer roles must not gain usable credit through this enhancement.
- Support traceability and accountability for future successful admin credit changes.

## Out Of Scope
- Credit spending, recharge, increase, decrease, expiration, or automatic adjustment workflows.
- Photographer self-service credit management.
- Non-admin credit update capability.
- General-purpose user activity logging beyond photographer-credit updates.
- Full audit-log browsing, reporting, analytics, or export capability unless a later artifact explicitly adds that scope.
- Reconstructing or backfilling audit records for historical credit changes that occurred before this enhancement, unless later upstream artifacts explicitly add that scope.
- Changing the core business rule that credit belongs only to `Photographer` users.

## Functional Requirements
- `FR-001`: The system must preserve `credit` as a dedicated attribute that applies only to users with role `Photographer`.
- `FR-002`: The system must preserve the rule that only `Admin` users may update photographer credit.
- `FR-003`: The system must create an audit log record whenever an admin successfully updates a photographer user's credit and the stored credit value changes.
- `FR-004`: The audit log record for a successful photographer credit change must capture at minimum the acting admin's user ID and name.
- `FR-005`: The audit log record for a successful photographer credit change must capture at minimum the target photographer's user ID and name.
- `FR-006`: The audit log record for a successful photographer credit change must capture the previous credit value and the new credit value.
- `FR-007`: The audit log record for a successful photographer credit change must capture the action timestamp.
- `FR-008`: If an admin submits a credit update request that does not change the stored credit value, the system must not create an audit log record for that request.
- `FR-009`: The system must prevent non-photographer users from becoming valid successful targets of credit-update audit events.
- `FR-010`: The system must preserve traceability and accountability for admin credit changes without expanding this feature into a broader credit-history or audit platform.

## Non-Functional Requirements
- `NFR-001`: Credit eligibility and credit-update authorization must continue to be enforced using server-side role data and server-side user state.
- `NFR-002`: Audit logging must be tied to successful admin credit changes only and must not create misleading records for denied, invalid, or no-op requests.
- `NFR-003`: Audit log content must be complete enough to identify who performed the action, who was affected, what changed, and when it changed.
- `NFR-004`: The enhancement must preserve data integrity between the approved credit update behavior and the approved audit record behavior.
- `NFR-005`: The feature must remain implementation-ready for downstream planning and design without silently deciding unresolved audit-policy details.
- `NFR-006`: The enhancement must stay scoped to the existing photographer-credit domain and must not broaden into general audit logging or broader credit lifecycle management.

## Business Rules
- `BR-001`: Only users with role `Photographer` are eligible to own usable credit.
- `BR-002`: Only `Admin` users may perform photographer credit updates.
- `BR-003`: Audit logging applies only when a successful admin credit update results in a real change to the stored credit value.
- `BR-004`: No audit log is required for a no-op credit update request where the requested value matches the current stored value.
- `BR-005`: Audit logs for this feature are accountability records for admin credit changes, not a full credit ledger or transaction engine.
- `BR-006`: Non-photographer roles must not gain usable credit or become successful credit-update targets through this enhancement.
- `BR-007`: The system is the source of truth for determining whether a change occurred and whether an audit log is required.

## Dependencies
- The existing photographer-credit feature remains the upstream business baseline for credit ownership and admin update permissions.
- The backend maintains user identities with stable identifiers and names that can support the required audit record content.
- The existing admin credit-update flow or approved downstream replacement remains available as the action point where audit logging will be attached.
- Downstream phases will define the approved storage strategy, consistency model, and visibility model for audit records without violating the business rules above.

## Assumptions
- `ASM-001`: This request enhances the existing photographer-credit feature rather than reopening prior decisions about who can own credit or who can update it.
- `ASM-002`: Only future successful admin credit changes need audit records unless a later upstream artifact explicitly requires historical backfill.
- `ASM-003`: A no-op credit update request is one where the requested value equals the current stored value.
- `ASM-004`: Audit log visibility is not required to be publicly exposed in this phase; downstream phases may keep it internal or admin-scoped unless business direction changes.
- `ASM-005`: Admin name and target user name may require an explicit downstream decision on whether they are stored as snapshots or resolved later, so that ambiguity remains documented in `ba-open-questions.md`.
- `ASM-006`: Timestamp standardization details remain a downstream design decision so long as the business invariant is that every required audit record has a trustworthy action time.

## Handoff Notes
- Team Lead must carry all `ASSUMED` items from `ba-open-questions.md` into planning, risks, and handoff artifacts.
- Downstream phases must preserve the existing photographer-only credit invariant and admin-only update authorization while adding audit logging for real value changes only.
- System Design must make audit storage, no-op outcome behavior, name-handling policy, timestamp handling, and change/update consistency implementation-usable without expanding feature scope.
