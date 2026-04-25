# Team Lead Handoff To System Design

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

## Handoff Status
Ready for `system-design`.

Preflight result: PASSED. `ba-open-questions.md` was read first. No business question entry is marked `Status: OPEN`.

## System Design Mission
Design an implementation-ready backend solution for photographer credit management that preserves every BA business rule and Team Lead constraint without turning the feature into a broader credit engine.

## Required Design Focus
- Define how `credit` exists as a dedicated attribute for `Photographer` users only.
- Define how non-photographer users remain ineligible for usable credit, including a clear representation strategy for read surfaces and update paths.
- Define first successful photographer login behavior that initializes `credit = 10`.
- Define subsequent-login behavior that preserves existing stored credit and does not reinitialize it incorrectly.
- Define admin-only manual credit update behavior, including both caller authorization and target-user eligibility checks.
- Define how invalid update attempts against non-photographer targets fail safely.
- Define how this feature integrates with the existing user and authentication flow rather than as a standalone subsystem.
- Define how credit remains available for future feature use without adding spending, recharge, or ledger behavior now.

## Mandatory System Design Artifacts
- `sd-solution-overview.md`
- `sd-domain-design.md`
- `sd-api-contract.md`
- `sd-data-design.md`
- `sd-implementation-guidelines.md`
- `sd-self-check.md`

## Data Design Requirements
`sd-data-design.md` must be implementation-usable and must not be placeholder-only.

It must cover:
- the user or account entity/aggregate that owns photographer credit
- numeric type expectations for `credit`
- nullability or equivalent handling for non-photographer roles
- constraints that preserve the photographer-only credit invariant
- one-time default initialization direction for first successful photographer login
- legacy-photographer handling for records that predate this feature and do not yet have initialized credit
- migration notes, DataSource implications, or explicit reuse guidance if existing structures are extended rather than replaced

## API And Flow Requirements
System Design must define behavior for:
- first successful photographer login when no credit has been initialized yet
- later photographer login when credit already exists
- admin manual credit update for a photographer target
- non-admin manual credit update attempt
- admin update attempt against a non-photographer target
- read visibility rules where credit may or may not appear
- invalid numeric update input according to the chosen design constraints

## Assumptions To Carry Forward
- `BAQ-001`: Non-photographer credit may be `null`, omitted, hidden, or otherwise non-usable, but non-photographer roles must never have usable credit.
- `BAQ-002`: Legacy photographers with no initialized credit may receive the default `10` on their first successful login after rollout.
- `BAQ-003`: Credit should be treated as a non-negative whole-number business value unless an upstream clarification changes that rule.
- `BAQ-004`: Credit must be stored and available to approved backend use cases, but BA does not require universal exposure in every response contract.

## Traceability Expectations
- Use `SD-*` IDs.
- Map each `SD-*` item to relevant `TL-*`, `FR-*`, `NFR-*`, and `AC-*` IDs where practical.
- Preserve the trace chain `FR -> AC -> TL -> SD -> CD -> TC`.

## Hard Non-Goals
- Do not design credit spending behavior.
- Do not design recharge, top-up, expiration, or automatic adjustment workflows.
- Do not design a transaction ledger or audit-history subsystem unless later upstream artifacts explicitly change scope.
- Do not design non-admin credit management.
- Do not broaden the feature into unrelated user-management or authentication redesign.

## Downstream Gate
Coding must not start unless System Design produces all required artifacts and `sd-data-design.md` is complete enough to implement persistence safely.
