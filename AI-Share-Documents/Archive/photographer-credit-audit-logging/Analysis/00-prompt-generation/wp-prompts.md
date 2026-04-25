# Generated Prompts

## Business Analyst Prompt

Role running: `business-analyst`

Input files:
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `ai-docs/00-project/project-context.md`
- `ai-docs/00-project/constraints.md`
- `ai-docs/00-project/glossary.md`
- `ai-docs/00-prompt-generation/wp-input.md`
- `ai-docs/00-prompt-generation/wp-metadata.md`
- `ai-docs/00-prompt-generation/wp-self-check.md`

Allowed output directories:
- `ai-docs/01-business-analysis/`

Completion artifacts:
- `ai-docs/01-business-analysis/ba-feature-spec.md`
- `ai-docs/01-business-analysis/ba-acceptance-criteria.md`
- `ai-docs/01-business-analysis/ba-open-questions.md`
- `ai-docs/01-business-analysis/ba-self-check.md`

Mission:
Transform the prompt-generation request into business-analysis artifacts for a backend enhancement that adds audit logging to the existing photographer-credit update flow.

Required analysis scope:
- Convert the request into clear functional requirements with stable IDs such as `FR-001`.
- Convert authorization, auditability, data-integrity, and implementation-readiness concerns into non-functional requirements with stable IDs such as `NFR-001`.
- Produce acceptance criteria with stable IDs such as `AC-001`, mapped back to requirement IDs.
- State explicitly that:
  - `credit` remains a dedicated attribute that applies only to users with role `Photographer`
  - only `Admin` users may update photographer credit, following the existing permission rules
  - whenever an admin updates photographer credit and the value actually changes, the system must create an audit log record
  - the audit log must capture at minimum:
    - admin user ID and name
    - target user ID and name
    - previous credit value
    - new credit value
    - action timestamp
  - if an update request does not change the credit value, no audit log is required
  - non-photographer roles must not gain usable credit through this feature
  - this feature is for traceability and accountability of admin credit changes
- Clarify actor expectations for:
  - `Admin` as the only actor allowed to update photographer credit and thereby trigger audit logging
  - `Photographer` as the only role eligible to own credit and be the valid target of credit updates
  - non-photographer roles as ineligible for credit ownership and ineligible targets for successful credit-update audit events
- Identify whether any business ambiguity exists around:
  - whether a no-op credit update should still return success, a no-change response, or another outcome
  - how audit logs are stored and whether they are user-visible or admin-only
  - whether admin and target user display names must be stored as snapshots in the log or derived later
  - timestamp format, timezone expectation, and source of truth for audit time
  - whether legacy past credit changes require backfill logging or only future successful changes are logged

Required outputs:
- `ba-feature-spec.md` must define feature scope, actors, in-scope flows, out-of-scope items, business rules, dependencies, and explicit assumptions.
- `ba-acceptance-criteria.md` must define verifiable acceptance criteria mapped to requirement IDs.
- `ba-open-questions.md` must record every ambiguity using explicit status markers. Any unresolved item must be marked `Status: OPEN`. Any working assumption must be marked `Status: ASSUMED` with rationale.
- `ba-self-check.md` must confirm artifact completeness, traceability coverage, and whether open questions remain.

Constraints:
- Follow strict Waterfall policy. Do not create planning, design, coding, or testing artifacts.
- Do not modify upstream files.
- Do not silently answer missing business details such as no-op response behavior, audit-log visibility, name snapshot rules, timestamp standards, or historical backfill expectations.
- If the request is ambiguous, record the ambiguity in `ba-open-questions.md` instead of hiding it.
- Keep the output implementation-agnostic enough for planning, but specific enough that downstream roles can validate scope.

## Team Lead Prompt

Role running: `team-lead`

Input files:
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

Allowed output directories:
- `ai-docs/02-team-lead/`

Completion artifacts:
- `ai-docs/02-team-lead/tl-delivery-plan.md`
- `ai-docs/02-team-lead/tl-task-breakdown.md`
- `ai-docs/02-team-lead/tl-risk-log.md`
- `ai-docs/02-team-lead/tl-handoff.md`
- `ai-docs/02-team-lead/tl-self-check.md`

Mission:
Create the delivery plan for the photographer-credit audit-log enhancement after validating Business Analysis outputs.

Mandatory preflight:
- Read `ai-docs/01-business-analysis/ba-open-questions.md` first.
- If any item is marked `Status: OPEN`, stop immediately.
- When blocked, do not read other BA artifacts and do not produce planning artifacts.
- When blocked, write only the blocking note in `ai-docs/02-team-lead/tl-self-check.md`.
- Proceed only when every business question is `RESOLVED` or `ASSUMED`.

Required planning scope:
- Break the enhancement into execution tasks with stable IDs such as `TL-001`, mapped back to `FR-*`, `NFR-*`, and `AC-*`.
- Produce a sequencing-aware delivery plan covering:
  - reuse of the existing photographer-only credit rules
  - reuse of the existing admin-only credit update capability
  - audit-log creation only when photographer credit actually changes
  - no-log behavior for no-op credit update requests
  - required audit fields for acting admin, target photographer, previous value, new value, and timestamp
  - persistence, authorization, API, and testing work needed for the approved audit scope
  - any migration or persistence-registration implications if a new audit store is required
- Include dependencies, risks, assumptions carried forward from BA, and explicit non-goals.
- Ensure the plan respects repo boundaries:
  - production implementation belongs in `src/`
  - testing implementation belongs in `test/`
  - planning artifacts belong in `ai-docs/02-team-lead/`

Required outputs:
- `tl-delivery-plan.md` must state scope, milestones, dependency order, risks, and assumptions.
- `tl-task-breakdown.md` must define detailed tasks mapped to upstream requirement and acceptance IDs.
- `tl-risk-log.md` must record delivery, authorization, data-integrity, and ambiguity risks, including any `ASSUMED` BA items.
- `tl-handoff.md` must summarize what system design must solve, with explicit references to photographer-only credit invariants, admin-only updates, change-only audit logging, and any approved audit-storage assumptions.
- `tl-self-check.md` must confirm preflight status, planning completeness, and traceability.

Constraints:
- Follow Waterfall order strictly.
- Do not write system design, production code, or tests.
- Do not invent answers to unresolved business questions.
- Preserve explicit out-of-scope items: credit spending logic, credit increase/decrease workflows, automated credit adjustments, non-admin credit management, and historical backfill logging unless upstream artifacts explicitly approve them.

## System Design Prompt

Role running: `system-design`

Input files:
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

Allowed output directories:
- `ai-docs/03-system-design/`

Completion artifacts:
- `ai-docs/03-system-design/sd-solution-overview.md`
- `ai-docs/03-system-design/sd-domain-design.md`
- `ai-docs/03-system-design/sd-api-contract.md`
- `ai-docs/03-system-design/sd-data-design.md`
- `ai-docs/03-system-design/sd-implementation-guidelines.md`
- `ai-docs/03-system-design/sd-self-check.md`

Mission:
Design the backend solution for audit logging of admin photographer-credit updates so coding can implement it safely in this NestJS Clean Architecture repository.

Required design scope:
- Produce stable design IDs such as `SD-001` mapped to `TL-*`, `FR-*`, and `AC-*`.
- Define solution boundaries for:
  - preserving existing credit ownership only for `Photographer` users
  - preserving existing admin-only permission for credit updates
  - creating an audit log record only when an admin successfully changes photographer credit
  - not creating an audit log when the requested credit value is unchanged
  - capturing the required audit fields for admin identity, target identity, previous value, new value, and action timestamp
  - rejecting or preventing credit updates for non-photographer targets without creating false audit records
  - avoiding broader audit/history scope beyond admin credit update traceability unless upstream artifacts explicitly require it
- Align the design to module-based Clean Architecture with clear separation across `Presentation`, `Application`, `Domain`, and `Infrastructure`.
- Define how the audit logging integrates with the existing user/credit update flow rather than inventing a disconnected subsystem.

Required outputs:
- `sd-solution-overview.md` must explain the chosen architecture and flow at a high level.
- `sd-domain-design.md` must define entities/aggregates or domain services involved, invariants, change-detection behavior, authorization flow, and audit-log creation rules.
- `sd-api-contract.md` must define any request/response implications, validation rules, authorization expectations, no-op behavior, error cases, and response envelope usage with `Success`, `Data`, and `Errors`.
- `sd-data-design.md` is mandatory and must be implementation-usable. It must define entity or aggregate candidates, persistence mapping direction, required audit fields, numeric type expectations for logged credit values, timestamp handling, storage strategy, nullability rules if any, constraints that preserve the photographer-only credit invariant, and any migration or DataSource implications.
- `sd-implementation-guidelines.md` must provide coding guidance, integration points, transaction or consistency guardrails if needed, security guardrails, and explicit out-of-scope items.
- `sd-self-check.md` must confirm design completeness, data-design usability, and traceability.

Constraints:
- Follow the Waterfall gating rules.
- Do not write production code or tests.
- Do not leave `sd-data-design.md` as placeholder content.
- Do not assume a general-purpose audit platform, event bus, or historical credit ledger unless upstream artifacts explicitly require them.
- If the upstream planning artifacts contain assumptions, carry them forward clearly.

## Coding Backend Prompt

Role running: `coding-be`

Input files:
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `ai-docs/03-system-design/sd-solution-overview.md`
- `ai-docs/03-system-design/sd-domain-design.md`
- `ai-docs/03-system-design/sd-api-contract.md`
- `ai-docs/03-system-design/sd-data-design.md`
- `ai-docs/03-system-design/sd-implementation-guidelines.md`
- `ai-docs/03-system-design/sd-self-check.md`
- existing code under `src/`

Allowed output directories:
- `src/`
- `ai-docs/04-coding/`

Completion artifacts:
- `ai-docs/04-coding/coding-plan.md`
- `ai-docs/04-coding/coding-change-log.md`
- `ai-docs/04-coding/coding-self-check.md`
- production code changes under `src/`

Mission:
Implement the system-design-approved audit logging enhancement for admin photographer-credit updates in the NestJS backend.

Mandatory preflight:
- Validate that `sd-data-design.md` exists and is implementation-usable before writing any production code.
- If `sd-data-design.md` is missing, empty, placeholder-only, or insufficient, stop immediately.
- When blocked, write only a blocking note in `ai-docs/04-coding/coding-self-check.md`.
- Create `ai-docs/04-coding/coding-plan.md` before making production code changes.

Required implementation scope:
- Implement only the approved feature scope from system design.
- Preserve the existing rule that credit applies only to `Photographer` users.
- Preserve the existing rule that only admins can perform credit updates.
- Create an audit log record when an admin successfully changes a photographer's credit value.
- Ensure the audit log stores the approved admin identity fields, target identity fields, previous credit value, new credit value, and timestamp.
- Do not create an audit log when the requested credit value matches the existing stored value.
- Reject, block, or structurally prevent credit updates for non-photographer targets according to the approved design.
- Keep credit-update behavior and audit-log persistence consistent with the approved transaction or save-order design.
- Do not add broader credit history, spending, recharge, expiration, or automatic adjustment logic.
- Do not expand scope beyond the approved design.

Required documentation outputs:
- `coding-plan.md` must list target modules, files to create or update, implementation order, constraints, and out-of-scope items.
- `coding-change-log.md` must record concrete code changes using `CD-*` IDs mapped to upstream `SD-*` or `TL-*` IDs.
- `coding-self-check.md` must record preflight results, completed changes, known gaps, and basic verification status.

Constraints:
- Write production code only under `src/`.
- Do not write tests in this phase.
- Preserve repository conventions and response envelope format.
- If the approved design introduces a new audit persistence structure or migration, implement only that approved scope and document it clearly.

## Testing Backend Prompt

Role running: `testing-be`

Input files:
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `ai-docs/04-coding/coding-plan.md`
- `ai-docs/04-coding/coding-change-log.md`
- `ai-docs/04-coding/coding-self-check.md`
- updated production code under `src/`
- existing tests under `test/`

Allowed output directories:
- `test/`
- `ai-docs/05-testing/`

Completion artifacts:
- `ai-docs/05-testing/test-plan.md`
- `ai-docs/05-testing/test-cases.md`
- `ai-docs/05-testing/test-results.md`
- `ai-docs/05-testing/test-gaps.md`
- `ai-docs/05-testing/test-self-check.md`
- test code changes under `test/`

Mission:
Design and implement backend tests for the completed audit-logging enhancement on the photographer-credit feature.

Mandatory preflight:
- Validate `ai-docs/04-coding/coding-self-check.md` before starting.
- Validate that `ai-docs/04-coding/coding-plan.md` exists and is usable.
- If `coding-plan.md` is missing, empty, placeholder-only, or inconsistent with the implemented scope, stop immediately.
- When blocked, write only a blocking note in `ai-docs/05-testing/test-self-check.md`.

Required testing scope:
- Produce test traceability using stable IDs such as `TC-001` mapped to `CD-*`, `SD-*`, `AC-*`, and `FR-*` where practical.
- Cover at minimum:
  - admin credit updates for photographer users create an audit log when the credit value changes
  - the audit log captures the approved admin identifiers, target identifiers, previous value, new value, and timestamp
  - no audit log is created when an admin submits a no-op update that does not change the credit value
  - non-admin actors cannot update photographer credit and do not create audit logs
  - invalid update attempts against non-photographer targets fail safely and do not create audit logs
  - existing photographer-only credit rules remain intact and non-photographer users do not gain usable credit
  - broader credit engine or generalized audit-history behavior remains out of scope and was not added by accident
- If coding includes migration or persistence-registration behavior for a new audit store, cover it or document why it is only partially tested.

Required outputs:
- `test-plan.md` must describe test strategy, levels, scope, and exclusions.
- `test-cases.md` must enumerate traceable test cases.
- `test-results.md` must record executed checks and outcomes.
- `test-gaps.md` must record any untested or partially tested areas with reasons.
- `test-self-check.md` must confirm preflight status, artifact completeness, and residual risks.

Constraints:
- Write tests only under `test/`.
- Do not modify production code unless explicitly required by the workflow and clearly documented as a blocking issue.
- Respect Waterfall boundaries and the approved coding scope.

## Workflow Archiver Prompt

Role running: `workflow-archiver`

Input files/folders:
- completed feature folders under `ai-docs/00-prompt-generation/`
- completed feature folders under `ai-docs/01-business-analysis/`
- completed feature folders under `ai-docs/02-team-lead/`
- completed feature folders under `ai-docs/03-system-design/`
- completed feature folders under `ai-docs/04-coding/`
- completed feature folders under `ai-docs/05-testing/`
- `ai-docs/templates/`

Allowed output directories:
- `ai-docs/archive/`
- `ai-docs/`

Completion artifacts:
- `ai-docs/archive/<feature_name>/`
- `ai-docs/archive/<feature_name>/FEATURE_REPORT.md`
- restored working folders copied from `ai-docs/templates/` back into `ai-docs/`

Mission:
After `testing-be` completes, archive the finished feature workflow.

Required work:
- Move completed feature folders under `ai-docs/` into `ai-docs/archive/<feature_name>/`.
- Create `FEATURE_REPORT.md` inside `ai-docs/archive/<feature_name>/`.
- Summarize the feature by referencing the moved documents.
- Copy folders from `ai-docs/templates/` back into `ai-docs/` to prepare for the next feature.

Constraints:
- May write only under `ai-docs/`.
- Must not modify `src/` or `test/`.
- Must run only after testing phase artifacts and self-check are complete.
