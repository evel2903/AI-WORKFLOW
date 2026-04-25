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
Transform the prompt-generation request into business-analysis artifacts for an authentication feature that supports only two authenticated roles: `Admin` and `Photographer`.

Required analysis scope:
- Convert the request into clear functional requirements with stable IDs such as `FR-001`.
- Convert security and architectural readiness concerns into non-functional requirements with stable IDs such as `NFR-001`.
- Produce acceptance criteria with stable IDs such as `AC-001`, mapped back to requirement IDs.
- State explicitly that:
  - `Admin` accounts are created manually only
  - `Admin` accounts must not be created through Google OAuth
  - `Admin` accounts must not be created through any public registration flow
  - `Photographer` accounts are created only through Google OAuth
  - first successful Google login creates a new account if absent with default role `Photographer`
  - subsequent Google logins authenticate the existing account and must preserve the existing role
  - roles are immutable after account creation
  - email/password authentication is out of scope
  - customer users are anonymous in this phase and do not authenticate
  - backend decisions must rely on server-side validated identity and account data only
  - disabled accounts must not be allowed to log in
  - the design must remain ready for later authorization integration

Required outputs:
- `ba-feature-spec.md` must define feature scope, actors, in-scope flows, out-of-scope items, business rules, dependencies, and explicit assumptions.
- `ba-acceptance-criteria.md` must define verifiable acceptance criteria mapped to requirement IDs.
- `ba-open-questions.md` must record every ambiguity using explicit status markers. Any unresolved item must be marked `Status: OPEN`. Any working assumption must be marked `Status: ASSUMED` with rationale.
- `ba-self-check.md` must confirm artifact completeness, traceability coverage, and whether open questions remain.

Constraints:
- Follow strict Waterfall policy. Do not create planning, design, coding, or testing artifacts.
- Do not modify upstream files.
- Do not silently answer missing business details such as the exact Admin provisioning mechanism or the exact session/token format.
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
Create the delivery plan for the authentication feature after validating Business Analysis outputs.

Mandatory preflight:
- Read `ai-docs/01-business-analysis/ba-open-questions.md` first.
- If any item is marked `Status: OPEN`, stop immediately.
- When blocked, do not read other BA artifacts and do not produce planning artifacts.
- When blocked, write only the blocking note in `ai-docs/02-team-lead/tl-self-check.md`.
- Proceed only when every business question is `RESOLVED` or `ASSUMED`.

Required planning scope:
- Break the feature into execution tasks with stable IDs such as `TL-001`, mapped back to `FR-*`, `NFR-*`, and `AC-*`.
- Produce a sequencing-aware delivery plan covering authentication flow, account lookup/creation rules, immutable-role enforcement, disabled-account checks, OAuth token validation, and readiness for later authorization integration.
- Include dependencies, risks, assumptions carried forward from BA, and explicit non-goals.
- Ensure the plan respects repo boundaries:
  - production implementation belongs in `src/`
  - testing implementation belongs in `test/`
  - planning artifacts belong in `ai-docs/02-team-lead/`

Required outputs:
- `tl-delivery-plan.md` must state scope, milestones, dependency order, risks, and assumptions.
- `tl-task-breakdown.md` must define detailed tasks mapped to upstream requirement and acceptance IDs.
- `tl-risk-log.md` must record delivery, security, and ambiguity risks, including any `ASSUMED` BA items.
- `tl-handoff.md` must summarize what system design must solve, with explicit references to immutable role handling, server-side trust boundaries, and disabled-account login prevention.
- `tl-self-check.md` must confirm preflight status, planning completeness, and traceability.

Constraints:
- Follow Waterfall order strictly.
- Do not write system design, production code, or tests.
- Do not invent answers to unresolved business questions.
- Preserve explicit out-of-scope items: email/password authentication, Admin public registration, Admin Google onboarding, and customer authentication.

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
Design the backend authentication solution for the Google OAuth role-bound access feature so coding can implement it safely in this NestJS Clean Architecture repository.

Required design scope:
- Produce stable design IDs such as `SD-001` mapped to `TL-*`, `FR-*`, and `AC-*`.
- Define solution boundaries for:
  - manual-only Admin account creation
  - Google OAuth-only Photographer onboarding
  - first-login account creation with default role `Photographer`
  - subsequent-login authentication of existing accounts without role mutation
  - immutable role enforcement after account creation
  - disabled-account login denial
  - backend validation of Google OAuth tokens
  - server-side-only identity and authorization data trust
  - readiness for later authorization rules
- Align the design to module-based Clean Architecture with clear separation across `Presentation`, `Application`, `Domain`, and `Infrastructure`.

Required outputs:
- `sd-solution-overview.md` must explain the chosen architecture and flow at a high level.
- `sd-domain-design.md` must define aggregates/entities, value objects, domain rules, invariants, and use cases.
- `sd-api-contract.md` must define endpoints, request/response contracts, auth flow boundaries, error cases, and response envelope usage with `Success`, `Data`, and `Errors`.
- `sd-data-design.md` is mandatory and must be implementation-usable. It must define entity or aggregate candidates, persistence mapping direction, key constraints, uniqueness rules, role immutability constraints, disabled-account state handling, and any migration or DataSource implications.
- `sd-implementation-guidelines.md` must provide coding guidance, integration points, security guardrails, and explicit out-of-scope items.
- `sd-self-check.md` must confirm design completeness, data-design usability, and traceability.

Constraints:
- Follow the Waterfall gating rules.
- Do not write production code or tests.
- Do not leave `sd-data-design.md` as placeholder content.
- Do not rely on client-provided role data anywhere in the design.
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
Implement the system-design-approved authentication feature in the NestJS backend.

Mandatory preflight:
- Validate that `sd-data-design.md` exists and is implementation-usable before writing any production code.
- If `sd-data-design.md` is missing, empty, placeholder-only, or insufficient, stop immediately.
- When blocked, write only a blocking note in `ai-docs/04-coding/coding-self-check.md`.
- Create `ai-docs/04-coding/coding-plan.md` before making production code changes.

Required implementation scope:
- Implement only the approved feature scope from system design.
- Ensure only `Admin` and `Photographer` are authenticated roles in this phase.
- Enforce that Admin accounts are not created via Google OAuth or any public registration path.
- Enforce that first successful Google login creates a new account only when absent and assigns role `Photographer`.
- Enforce that subsequent Google logins authenticate the existing account and preserve the stored role unchanged.
- Enforce immutable role behavior after account creation.
- Ensure the backend validates Google OAuth tokens and never trusts role or identity input from the client.
- Ensure disabled accounts cannot log in.
- Keep the authentication implementation ready for later authorization integration.
- Do not add email/password authentication.
- Do not add customer login or customer account creation.

Required documentation outputs:
- `coding-plan.md` must list target module, files to create or update, implementation order, constraints, and out-of-scope items.
- `coding-change-log.md` must record concrete code changes using `CD-*` IDs mapped to upstream `SD-*` or `TL-*` IDs.
- `coding-self-check.md` must record preflight results, completed changes, known gaps, and basic verification status.

Constraints:
- Write production code only under `src/`.
- Do not write tests in this phase.
- Do not expand scope beyond approved system design.
- Preserve repository conventions and response envelope format.

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
Design and implement backend tests for the completed authentication feature.

Mandatory preflight:
- Validate `ai-docs/04-coding/coding-self-check.md` before starting.
- Validate that `ai-docs/04-coding/coding-plan.md` exists and is usable.
- If `coding-plan.md` is missing, empty, placeholder-only, or inconsistent with the implemented scope, stop immediately.
- When blocked, write only a blocking note in `ai-docs/05-testing/test-self-check.md`.

Required testing scope:
- Produce test traceability using stable IDs such as `TC-001` mapped to `CD-*`, `SD-*`, `AC-*`, and `FR-*` where practical.
- Cover at minimum:
  - Google first login creates a Photographer account when none exists
  - repeated Google login authenticates the same stored account without role changes
  - Admin accounts are not created through Google OAuth
  - role mutation paths are rejected or absent according to the approved design
  - disabled accounts are denied login
  - server-side validation rejects invalid or untrusted Google token data
  - client-supplied role or identity values do not control authorization decisions
  - unsupported flows remain out of scope or rejected, including email/password authentication and customer authentication

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
