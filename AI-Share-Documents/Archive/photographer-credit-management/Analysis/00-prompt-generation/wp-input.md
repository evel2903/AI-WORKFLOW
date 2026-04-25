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

- Add a `credit` field for users.
- Credit is a property that applies only to users with the `photographer` role.
- Users with other roles must not have usable credit. Representation may be `null`, omitted, or hidden depending on the approved downstream design.
- Admins have permission to update the credit of users with the `photographer` role.
- When a photographer logs in for the first time, the system must automatically initialize `credit = 10`.
- There is no need in this feature to implement credit increase logic, credit decrease logic, consumption logic, recharge logic, or any broader credit workflow.
- For now, the system only needs to store the credit value and allow manual credit updates by admins.
- Credit is a dedicated photographer attribute intended for future features.

## Context
- This is a NestJS backend using module-based Clean Architecture.
- Feature modules live in `src/Modules/<FeatureName>`.
- Layers are `Presentation`, `Application`, `Domain`, and `Infrastructure`.
- PascalCase is used for classes, methods, and DTO properties.
- Interface names use prefix `I`.
- API response envelope uses `Success`, `Data`, and `Errors`.
- Strict Waterfall order is `write-prompt -> business-analyst -> team-lead -> system-design -> coding-be -> testing-be`.
- All inter-phase handoff artifacts must be stored under `ai-docs/`.
- `write-prompt` may write only to `ai-docs/00-prompt-generation/`.
