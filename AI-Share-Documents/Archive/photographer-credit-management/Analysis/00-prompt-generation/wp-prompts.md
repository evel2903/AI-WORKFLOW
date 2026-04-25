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
Transform the prompt-generation request into business-analysis artifacts for a backend feature that adds photographer-only credit storage and admin-managed credit updates.

Required analysis scope:
- Convert the request into clear functional requirements with stable IDs such as `FR-001`.
- Convert data integrity, authorization, and implementation-readiness concerns into non-functional requirements with stable IDs such as `NFR-001`.
- Produce acceptance criteria with stable IDs such as `AC-001`, mapped back to requirement IDs.
- State explicitly that:
  - `credit` is a dedicated attribute that applies only to users with role `Photographer`
  - users with other roles must not have usable credit
  - the exact non-photographer representation may be `null`, omitted, or hidden depending on approved downstream design, but the business invariant must be preserved
  - admins are allowed to manually update credit for users with role `Photographer`
  - the system must initialize `credit = 10` when a photographer logs in for the first time
  - this feature does not include credit increase, decrease, spending, recharge, expiration, or automatic adjustment workflows
  - credit exists for future feature use and must be stored safely now
- Clarify actor expectations for:
  - `Admin` as the only actor allowed to manually update photographer credit
  - `Photographer` as the only role eligible to own credit
  - other roles as ineligible for credit ownership
- Identify whether any business ambiguity exists around:
  - how credit should appear in responses
  - how legacy existing photographer records should be handled
  - whether admin updates can set any integer value or require additional validation rules

Required outputs:
- `ba-feature-spec.md` must define feature scope, actors, in-scope flows, out-of-scope items, business rules, dependencies, and explicit assumptions.
- `ba-acceptance-criteria.md` must define verifiable acceptance criteria mapped to requirement IDs.
- `ba-open-questions.md` must record every ambiguity using explicit status markers. Any unresolved item must be marked `Status: OPEN`. Any working assumption must be marked `Status: ASSUMED` with rationale.
- `ba-self-check.md` must confirm artifact completeness, traceability coverage, and whether open questions remain.

Constraints:
- Follow strict Waterfall policy. Do not create planning, design, coding, or testing artifacts.
- Do not modify upstream files.
- Do not silently answer missing business details such as response visibility rules, legacy photographer backfill behavior, or numeric validation constraints for admin updates.
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
Create the delivery plan for the photographer-credit feature after validating Business Analysis outputs.

Mandatory preflight:
- Read `ai-docs/01-business-analysis/ba-open-questions.md` first.
- If any item is marked `Status: OPEN`, stop immediately.
- When blocked, do not read other BA artifacts and do not produce planning artifacts.
- When blocked, write only the blocking note in `ai-docs/02-team-lead/tl-self-check.md`.
- Proceed only when every business question is `RESOLVED` or `ASSUMED`.

Required planning scope:
- Break the feature into execution tasks with stable IDs such as `TL-001`, mapped back to `FR-*`, `NFR-*`, and `AC-*`.
- Produce a sequencing-aware delivery plan covering:
  - photographer-only credit data rules
  - first-login credit initialization to `10`
  - admin-only credit update capability
  - non-photographer credit rejection or hiding behavior
  - migration or backfill implications for existing photographer records if required
  - API, authorization, persistence, and testing work needed for the approved scope
- Include dependencies, risks, assumptions carried forward from BA, and explicit non-goals.
- Ensure the plan respects repo boundaries:
  - production implementation belongs in `src/`
  - testing implementation belongs in `test/`
  - planning artifacts belong in `ai-docs/02-team-lead/`

Required outputs:
- `tl-delivery-plan.md` must state scope, milestones, dependency order, risks, and assumptions.
- `tl-task-breakdown.md` must define detailed tasks mapped to upstream requirement and acceptance IDs.
- `tl-risk-log.md` must record delivery, authorization, data-integrity, and ambiguity risks, including any `ASSUMED` BA items.
- `tl-handoff.md` must summarize what system design must solve, with explicit references to photographer-only credit invariants, first-login initialization, admin update authorization, and any legacy-data handling assumptions.
- `tl-self-check.md` must confirm preflight status, planning completeness, and traceability.

Constraints:
- Follow Waterfall order strictly.
- Do not write system design, production code, or tests.
- Do not invent answers to unresolved business questions.
- Preserve explicit out-of-scope items: credit spending logic, credit increase/decrease workflows, automated credit adjustments, and non-admin credit management.

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
Design the backend solution for a photographer-only credit attribute so coding can implement it safely in this NestJS Clean Architecture repository.

Required design scope:
- Produce stable design IDs such as `SD-001` mapped to `TL-*`, `FR-*`, and `AC-*`.
- Define solution boundaries for:
  - storing credit only for `Photographer` users
  - ensuring non-photographer roles do not have usable credit
  - initializing `credit = 10` on first successful photographer login
  - allowing admins to manually update photographer credit
  - rejecting or preventing credit updates for non-photographer targets
  - avoiding any credit increase/decrease business engine beyond simple storage and admin update
- Align the design to module-based Clean Architecture with clear separation across `Presentation`, `Application`, `Domain`, and `Infrastructure`.
- Define how the feature integrates with the existing user/authentication flow rather than inventing a disconnected credit subsystem.

Required outputs:
- `sd-solution-overview.md` must explain the chosen architecture and flow at a high level.
- `sd-domain-design.md` must define aggregates/entities, invariants, role-based credit eligibility rules, first-login initialization behavior, and admin-only update use cases.
- `sd-api-contract.md` must define any needed endpoints, request/response contracts, validation rules, authorization expectations, error cases, and response envelope usage with `Success`, `Data`, and `Errors`.
- `sd-data-design.md` is mandatory and must be implementation-usable. It must define entity or aggregate candidates, persistence mapping direction, numeric type expectations, nullability or hiding strategy for non-photographer roles, constraints that preserve the photographer-only credit invariant, first-login default initialization behavior, and any migration or DataSource implications.
- `sd-implementation-guidelines.md` must provide coding guidance, integration points, security guardrails, and explicit out-of-scope items.
- `sd-self-check.md` must confirm design completeness, data-design usability, and traceability.

Constraints:
- Follow the Waterfall gating rules.
- Do not write production code or tests.
- Do not leave `sd-data-design.md` as placeholder content.
- Do not assume a credit ledger, transaction history, or automated credit rules unless upstream artifacts explicitly require them.
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
Implement the system-design-approved photographer-credit feature in the NestJS backend.

Mandatory preflight:
- Validate that `sd-data-design.md` exists and is implementation-usable before writing any production code.
- If `sd-data-design.md` is missing, empty, placeholder-only, or insufficient, stop immediately.
- When blocked, write only a blocking note in `ai-docs/04-coding/coding-self-check.md`.
- Create `ai-docs/04-coding/coding-plan.md` before making production code changes.

Required implementation scope:
- Implement only the approved feature scope from system design.
- Add persisted credit storage only where the approved design says it belongs for `Photographer` users.
- Ensure non-photographer roles do not end up with usable credit values.
- Initialize `credit = 10` during first successful photographer login according to the existing authentication/onboarding flow.
- Implement an admin-only manual credit update path for photographer users if approved in system design.
- Reject, block, or structurally prevent credit updates for non-photographer targets according to the approved design.
- Preserve existing role and authorization boundaries.
- Do not add credit spending, recharge, history, expiration, or automatic adjustment logic.
- Do not expand scope beyond the approved design.

Required documentation outputs:
- `coding-plan.md` must list target module, files to create or update, implementation order, constraints, and out-of-scope items.
- `coding-change-log.md` must record concrete code changes using `CD-*` IDs mapped to upstream `SD-*` or `TL-*` IDs.
- `coding-self-check.md` must record preflight results, completed changes, known gaps, and basic verification status.

Constraints:
- Write production code only under `src/`.
- Do not write tests in this phase.
- Preserve repository conventions and response envelope format.
- If legacy photographer data needs migration or backfill, implement only what is approved by system design and document it clearly.

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
Design and implement backend tests for the completed photographer-credit feature.

Mandatory preflight:
- Validate `ai-docs/04-coding/coding-self-check.md` before starting.
- Validate that `ai-docs/04-coding/coding-plan.md` exists and is usable.
- If `coding-plan.md` is missing, empty, placeholder-only, or inconsistent with the implemented scope, stop immediately.
- When blocked, write only a blocking note in `ai-docs/05-testing/test-self-check.md`.

Required testing scope:
- Produce test traceability using stable IDs such as `TC-001` mapped to `CD-*`, `SD-*`, `AC-*`, and `FR-*` where practical.
- Cover at minimum:
  - first successful photographer login initializes `credit = 10`
  - subsequent photographer logins do not incorrectly reinitialize or corrupt existing credit
  - admins can manually update credit for photographer users
  - non-admin actors cannot update photographer credit
  - non-photographer users cannot receive or expose usable credit according to the approved design
  - invalid update attempts against non-photographer targets fail safely
  - broader credit engine behavior remains out of scope and was not added by accident
- If coding includes migration or backfill behavior for existing photographer records, cover it or document why it is only partially tested.

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
