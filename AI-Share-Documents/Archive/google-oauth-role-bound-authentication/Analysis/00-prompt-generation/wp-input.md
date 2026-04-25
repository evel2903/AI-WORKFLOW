# Raw Request

## Role Running
`write-prompt`

## Input Files
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `.codex/skills/write-prompt/SYSTEM_PROMPT.md`
- `.codex/skills/write-prompt/CHECKLIST.md`
- `.codex/skills/write-prompt/INPUTS_OUTPUTS.md`
- `ai-docs/00-project/project-context.md`
- `ai-docs/00-project/constraints.md`
- `ai-docs/00-project/glossary.md`

## Allowed Output Directories
- `ai-docs/00-prompt-generation/`

## Completion Artifacts
- `ai-docs/00-prompt-generation/wp-input.md`
- `ai-docs/00-prompt-generation/wp-prompts.md`
- `ai-docs/00-prompt-generation/wp-metadata.md`
- `ai-docs/00-prompt-generation/wp-self-check.md`

## Description
Generate full Waterfall prompts for this feature request:

- The system supports ONLY two authenticated roles: Admin and Photographer.
- Admin accounts are created manually.
- Admin accounts must NOT be created through Google OAuth or any public registration flow.
- Photographer accounts are created through Google OAuth only.
- On first successful Google login:
  - If the account does not exist, the system must create a new account.
  - The default role must be `Photographer`.
- On subsequent Google logins:
  - If the account already exists, the system must authenticate the existing account.
  - The role must remain unchanged.
- Role is immutable:
  - Once an account is created, its role must NOT be changed.
  - No role upgrade or downgrade is allowed.
- The system does NOT support email/password authentication.
- The system does NOT support self-registration for Admin accounts.
- Customer users do not authenticate:
  - They do not have accounts in this phase.
  - They do not go through any login or registration flow.
- Security requirements:
  - Backend must validate Google OAuth tokens.
  - Backend must never trust role or identity data from client input.
  - Authentication and authorization decisions must rely on server-side data only.
  - Disabled accounts must not be allowed to log in.
- The authentication design must be ready for integration with authorization rules in later phases.

## Context
- This is a NestJS backend using module-based Clean Architecture.
- Feature modules live in `src/Modules/<FeatureName>`.
- Layers are `Presentation`, `Application`, `Domain`, and `Infrastructure`.
- API response envelope uses `Success`, `Data`, and `Errors`.
- Strict Waterfall order is `write-prompt -> business-analyst -> team-lead -> system-design -> coding-be -> testing-be`.
- All inter-phase handoff artifacts must be stored under `ai-docs/`.
- `write-prompt` writes only to `ai-docs/00-prompt-generation/`.
