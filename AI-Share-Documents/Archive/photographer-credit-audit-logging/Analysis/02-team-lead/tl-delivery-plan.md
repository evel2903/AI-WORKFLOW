# Team Lead Delivery Plan

## Role Running
`team-lead`

## Input Files
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `ai-docs/00-project/project-context.md`
- `ai-docs/00-project/constraints.md`
- `ai-docs/00-project/glossary.md`
- `ai-docs/01-business-analysis/ba-feature-spec.md`
- `ai-docs/01-business-analysis/ba-acceptance-criteria.md`
- `ai-docs/01-business-analysis/ba-open-questions.md`
- `ai-docs/01-business-analysis/ba-self-check.md`

## Allowed Output Directories
- `ai-docs/02-team-lead/`

## Completion Artifacts
- `ai-docs/02-team-lead/tl-delivery-plan.md`
- `ai-docs/02-team-lead/tl-task-breakdown.md`
- `ai-docs/02-team-lead/tl-risk-log.md`
- `ai-docs/02-team-lead/tl-handoff.md`
- `ai-docs/02-team-lead/tl-self-check.md`

## Preflight Result
Status: PASSED

`ai-docs/01-business-analysis/ba-open-questions.md` was read first. No item remains `Status: OPEN`; all identified ambiguities are documented as `ASSUMED`, so planning may proceed.

## Delivery Goal
Extend the existing photographer-credit feature so authorized admin credit changes for photographer users generate reliable audit records, while preserving the current photographer-only credit invariant, preserving admin-only update permissions, and avoiding any expansion into broader credit-history or generalized audit-platform scope.

## Confirmed Scope
- Reuse the existing business rule that `credit` applies only to `Photographer` users.
- Reuse the existing business rule that only `Admin` users may update photographer credit.
- Add audit-log creation only when a successful admin credit update results in an actual stored-value change.
- Do not create an audit log for no-op requests where the requested credit equals the current stored credit.
- Ensure required audit content is available for:
  - acting admin ID and name
  - target photographer ID and name
  - previous credit value
  - new credit value
  - action timestamp
- Preserve the rule that non-photographer users cannot become successful credit-update targets.

## Non-Goals
- No credit spending, recharge, expiration, automatic adjustment, or other broader credit workflows.
- No non-admin credit management.
- No generalized audit-history platform outside this feature boundary.
- No historical backfill logging unless later upstream artifacts explicitly add it.
- No business-facing audit-log reporting or browsing scope unless later upstream artifacts explicitly add it.

## Milestones

### M1: Validate Existing Credit-Update Baseline
Confirm the downstream design starts from the current photographer-only credit and admin-only update behavior rather than redesigning the feature.

### M2: Design Change-Detection And Audit Boundaries
Define exactly where the system detects a real credit change, when logging is required, and how no-op, denied, and invalid requests avoid false audit entries.

### M3: Design And Implement Audit Persistence
Define the audit storage model, required fields, any migration or persistence-registration work, and consistency expectations between the credit update and audit record creation.

### M4: Integrate API, Authorization, And Update Flow
Ensure the approved update path preserves existing permissions, preserves non-photographer rejection behavior, and aligns any no-op outcome behavior with the approved design.

### M5: Verify With Focused Backend Tests
Add tests that prove audit records are created for real admin credit changes only, not for no-op, denied, or invalid requests.

## Execution Order
1. Reconfirm existing credit update invariants and current valid actors.
2. Define change-detection rules and no-op handling expectations.
3. Define audit record content and storage responsibilities.
4. Define update-flow integration, authorization points, and consistency/transaction expectations.
5. Implement persistence and application flow changes in `src/`.
6. Add and execute validating tests in `test/`.

## Dependencies
- Existing photographer-credit update capability is the baseline that this enhancement extends.
- Existing user identity data must support required admin and target identifiers and names.
- System Design must decide the approved audit storage strategy and whether names are snapshotted or resolved later.
- Coding depends on `sd-data-design.md` being implementation-usable for the chosen audit persistence model.
- Testing depends on `coding-plan.md` aligning with the implemented audit scope.

## Repo Boundary Plan
- Planning artifacts stay in `ai-docs/02-team-lead/`.
- Production implementation belongs in `src/`.
- Test implementation belongs in `test/`.

## Assumptions Carried Forward From BA
- `Q1`: No-op response behavior is not yet fixed at the business level; downstream design may choose the exact response so long as no audit log is produced.
- `Q2`: Audit-log visibility is assumed to remain internal or admin-scoped unless later upstream artifacts broaden scope.
- `Q3`: Name snapshot policy remains a downstream design decision and must be made explicit in system design.
- `Q4`: Timestamp standard and source-of-truth details remain a downstream design decision so long as the timestamp is trustworthy and system-generated.
- `Q5`: Historical backfill logging is out of scope; only future successful admin credit changes require audit records.

## Key Risks
- Ambiguity around name snapshot policy can materially affect persistence design and long-term traceability quality.
- If change detection is placed in the wrong layer, the system could produce duplicate or misleading audit entries.
- If audit persistence is not kept consistent with the credit update, accountability records may drift from actual stored state.
- If no-op behavior is not handled explicitly, downstream implementation may log false updates or return inconsistent API outcomes.
- If a new audit store is introduced, migration and registration work may be required.

## Done Criteria
- Planning artifacts map work to `FR-*`, `NFR-*`, and `AC-*`.
- Assumptions from BA are carried into both risk and handoff artifacts.
- System Design receives a clear handoff for photographer-only credit invariants, admin-only update boundaries, change-only audit logging, no-op behavior, and audit persistence decisions.
