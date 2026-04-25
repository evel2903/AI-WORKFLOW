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

`ai-docs/01-business-analysis/ba-open-questions.md` was read before other BA artifacts. No question entry is marked `Status: OPEN`. All unresolved details are explicitly marked `Status: ASSUMED` and are carried into `tl-risk-log.md` and `tl-handoff.md`.

## Delivery Goal
Prepare System Design, Coding, and Testing for a backend feature that introduces photographer-only credit storage, initializes `credit = 10` on first successful photographer login, allows only admins to manually update photographer credit, prevents non-photographer roles from having usable credit, and keeps the implementation scoped to storage and admin-managed updates only.

## Scope Summary
In scope:
- dedicated `credit` attribute for users with role `Photographer`
- enforcement that non-photographer roles do not have usable credit
- first-login photographer credit initialization to `10`
- preservation of existing stored credit on later logins
- admin-only manual update capability for photographer credit
- server-side eligibility and authorization enforcement
- handling for legacy photographers with no initialized credit according to carried BA assumption
- planning for API, authorization, persistence, and testing work required to support the approved business rules

Out of scope:
- credit spending logic
- credit increase or decrease workflows driven by business events
- recharge, top-up, purchase, or expiration behavior
- transaction history or credit ledger behavior
- self-service photographer credit management
- manual credit updates by non-admin roles
- unrelated user-profile redesign or unrelated authentication changes

## Milestones

### Milestone 1: System Design Readiness
Objective: Produce implementation-usable design artifacts that define role-bound credit behavior without expanding the feature into a broader credit engine.

Expected System Design outcomes:
- define photographer-only credit invariant
- define non-photographer credit representation boundary
- define first-login default initialization behavior and legacy-photographer handling
- define admin-only manual credit update rules
- define persistence constraints, nullability rules, and any migration or backfill direction
- define API and authorization expectations for approved update and read contexts
- produce mandatory implementation-usable `sd-data-design.md`

Primary tasks: `TL-001` through `TL-008`

### Milestone 2: Backend Implementation Readiness
Objective: Enable Coding Backend to implement only the approved photographer-credit scope after System Design has provided usable data and API guidance.

Expected Coding outcomes:
- create `coding-plan.md` before production code changes
- implement storage, initialization, and admin-managed update behavior only under approved design scope
- preserve explicit non-goals and photographer-only eligibility rules
- document changes and verification in coding artifacts

Primary tasks: `TL-009`, `TL-010`

### Milestone 3: Backend Test Readiness
Objective: Enable Testing Backend to verify the implemented photographer-credit behavior against BA acceptance criteria and coding scope.

Expected Testing outcomes:
- validate `coding-plan.md` and `coding-self-check.md`
- test first-login initialization, subsequent-login preservation, admin-only update behavior, and non-photographer rejection paths
- document executed results, coverage gaps, and residual risks

Primary tasks: `TL-011`, `TL-012`

## Dependency Order
1. `team-lead` completes this planning package.
2. `system-design` consumes Team Lead artifacts and produces all required design artifacts.
3. `system-design` must produce a non-placeholder, implementation-usable `sd-data-design.md`.
4. `coding-be` preflights System Design artifacts and creates `coding-plan.md` before code changes.
5. `coding-be` implements production changes only under `src/`.
6. `testing-be` preflights Coding artifacts and writes tests only under `test/`.
7. `workflow-archiver` may run only after Testing Backend completes.

## Assumptions Carried Forward
- `BAQ-001`: Non-photographer credit representation may be `null`, omitted, hidden, or otherwise non-usable, but the business invariant is that non-photographer roles do not have usable credit.
- `BAQ-002`: Legacy photographers without initialized credit may receive default `10` on their first successful login after rollout.
- `BAQ-003`: Credit should be treated as a non-negative whole-number value unless a later upstream clarification changes that business rule.
- `BAQ-004`: Credit must be stored and available to approved backend use cases, but BA does not require it to appear in every response context.

## Key Risks
- Ambiguous response visibility can cause inconsistent API exposure or accidental leakage of credit in irrelevant contexts.
- Ambiguous legacy-photographer handling can lead to duplicate initialization logic, missed initialization, or unsafe bulk backfill choices.
- Ambiguous numeric validation can lead to negative or invalid credit values if System Design does not set explicit constraints.
- Admin-update authorization may be implemented too broadly if role checks and target eligibility checks are not both specified.
- Scope creep can turn a simple storage feature into a broader credit engine unless explicit non-goals remain enforced through design and coding.

## Phase Boundaries
- Team Lead writes only planning artifacts under `ai-docs/02-team-lead/`.
- System Design writes only design artifacts under `ai-docs/03-system-design/`.
- Coding Backend writes production code only under `src/` and coding artifacts under `ai-docs/04-coding/`.
- Testing Backend writes tests only under `test/` and testing artifacts under `ai-docs/05-testing/`.
