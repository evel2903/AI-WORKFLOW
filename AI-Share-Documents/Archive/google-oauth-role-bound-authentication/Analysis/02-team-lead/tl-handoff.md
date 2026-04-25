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
Design an implementation-ready backend authentication solution for Google OAuth role-bound authentication while preserving every BA business rule and Team Lead constraint.

## Required Design Focus
- Define how the system supports exactly two authenticated roles: `Admin` and `Photographer`.
- Define how Admin account creation remains manual-only and never public or Google OAuth-based.
- Define how Photographer account creation happens only through Google OAuth.
- Define first successful Google login behavior when no matching account exists.
- Define subsequent Google login behavior when a matching account exists.
- Define role immutability as a domain invariant that prevents upgrade, downgrade, and login-time role mutation.
- Define backend-only Google OAuth token validation and identity trust boundaries.
- Define how client-provided role and unvalidated identity data are ignored for authentication and authorization decisions.
- Define disabled-account login denial timing and response behavior.
- Define authentication output needed for later authorization integration without implementing full authorization policy rules.

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
- account/entity candidates needed for authenticated users
- role representation for exactly `Admin` and `Photographer`
- account status or disabled-state representation
- Google provider identity mapping and account matching direction
- uniqueness constraints or equivalent rules needed to prevent duplicate provider identities
- immutability constraints for role after account creation
- persistence implications for first-login account creation
- migration or DataSource notes when applicable
- explicit reuse explanation if existing structures are sufficient

## API And Flow Requirements
System Design must define behavior for:
- valid first Google login with no matching account
- valid Google login with an existing matching account
- valid Google login for a disabled matching account
- invalid or untrusted Google OAuth token
- client-supplied role spoofing attempt
- email/password authentication attempt if such route exists or is requested
- public Admin registration attempt if such route exists or is requested
- Customer login or registration attempt if such route exists or is requested

## Assumptions To Carry Forward
- `BAQ-001`: Admin provisioning is internal, manual, and non-public. System Design must define boundaries but does not need to implement an operational Admin-management product feature unless required to preserve the auth rules.
- `BAQ-002`: Post-auth token/session format is not specified. System Design must choose or define enough contract detail for Coding and Testing.
- `BAQ-003`: Google account matching fields are unspecified. System Design must define matching rules using validated Google identity data and server-side persistence.
- `BAQ-004`: Disabled-account error details are unspecified. System Design must define response behavior.
- `BAQ-005`: Existing non-Photographer account behavior on Google login is unspecified. System Design must resolve behavior without mutating role and without allowing Google OAuth to create or convert Admin accounts.

## Traceability Expectations
- Use `SD-*` IDs.
- Map each `SD-*` item to relevant `TL-*`, `FR-*`, `NFR-*`, and `AC-*` IDs where practical.
- Preserve the trace chain `FR -> AC -> TL -> SD -> CD -> TC`.

## Hard Non-Goals
- Do not design email/password authentication.
- Do not design public Admin registration.
- Do not design Google OAuth creation of Admin accounts.
- Do not design Customer account creation or Customer authentication.
- Do not design role upgrade or downgrade flows.
- Do not implement full authorization policy rules beyond authorization-readiness.

## Downstream Gate
Coding must not start unless System Design produces all required artifacts and `sd-data-design.md` is complete enough to implement persistence safely.
