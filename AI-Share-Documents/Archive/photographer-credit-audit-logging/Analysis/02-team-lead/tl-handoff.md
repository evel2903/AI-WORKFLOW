# Team Lead Handoff To System Design

## Role Running
`team-lead`

## Handoff Status
Ready for `system-design`.

Preflight result: PASSED. `ba-open-questions.md` was read first. No business question entry is marked `Status: OPEN`.

## Confirmed Scope
- Preserve the existing invariant that `credit` is a dedicated attribute for `Photographer` users only.
- Preserve the existing invariant that only `Admin` users may update photographer credit.
- Add audit logging only when an admin successfully changes a photographer's stored credit value.
- Do not create an audit log for a no-op request where the requested credit equals the current stored credit.
- Ensure every required audit record captures:
  - acting admin ID and name
  - target photographer ID and name
  - previous credit value
  - new credit value
  - action timestamp
- Preserve the rule that non-photographer users cannot become successful credit-update targets and must not produce successful credit-change audit events.

## Required System Design Focus
- Define where change detection happens in the existing admin credit-update flow.
- Define exactly when audit creation is triggered and when it is explicitly suppressed.
- Define how audit logging integrates with the existing photographer-credit update use case instead of creating a disconnected subsystem.
- Define the storage strategy for audit records, including whether a new persistence structure, migration, or DataSource registration is required.
- Define how the system preserves consistency between the credit update and the audit record.
- Define authorization and invalid-target behavior so denied or invalid requests do not create misleading audit records.
- Define any request/response implications for no-op updates without violating the BA assumption that no audit log is created.

## Data Design Requirements
`sd-data-design.md` must be implementation-usable and must not be placeholder-only.

It must cover:
- the entity or aggregate candidate that owns audit records
- required audit fields for actor identity, target identity, previous value, new value, and timestamp
- numeric type expectations for logged credit values
- timestamp handling and source of truth
- whether admin and target names are stored as snapshots or resolved later
- nullability and constraint expectations, if any
- consistency strategy between the credit update and audit record creation
- migration or DataSource implications if a new audit store is introduced

## API And Flow Requirements
System Design must define behavior for:
- successful admin update of photographer credit with a real value change
- admin no-op update where the requested value matches the current stored value
- non-admin credit update attempt
- admin update attempt against a non-photographer target
- invalid input that fails before a successful update occurs
- any approved audit visibility or response-surface implications while keeping scope narrow

## Assumptions To Carry Forward
- `Q1`: No-op update response semantics are not fixed at the business level; downstream design may choose the exact outcome so long as no audit log is created.
- `Q2`: Audit-log visibility is assumed to remain internal or admin-scoped in this phase unless later upstream artifacts expand scope.
- `Q3`: Name snapshot policy is unresolved at the business level and must be made explicit in design.
- `Q4`: Timestamp standard and formatting remain a design decision so long as the action timestamp is trustworthy and system-generated.
- `Q5`: Historical backfill logging is out of scope; logging applies only to future successful admin credit changes after rollout.

## Traceability Expectations
- Use `SD-*` IDs.
- Map each `SD-*` item to relevant `TL-*`, `FR-*`, `NFR-*`, and `AC-*` IDs where practical.
- Preserve the trace chain `FR -> AC -> TL -> SD -> CD -> TC`.

## Hard Non-Goals
- Do not design credit spending, recharge, expiration, or automatic adjustment behavior.
- Do not design non-admin credit management.
- Do not design a generalized audit-history platform outside this feature boundary.
- Do not reintroduce historical backfill logging unless a later upstream artifact explicitly changes scope.
- Do not redesign the existing photographer-only credit model.

## Downstream Gate
Coding must not start unless System Design produces all required artifacts and `sd-data-design.md` is complete enough to implement audit persistence safely.
