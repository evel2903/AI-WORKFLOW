# Write Prompt - Inputs and Outputs

## Allowed Inputs
- Current user request
- `AGENTS.md`
- `SKILLS/Analysis/shared/*`
- Existing `AI-Share-Documents/Analysis/00-prompt-generation/*` if refining prompts
- Optional existing code context from `BE/<Project name>/` and `FE/<Project name>/`
- Optional handoff context from `AI-Share-Documents/`

## Allowed Outputs
Write primary outputs only to:
- `AI-Share-Documents/Analysis/00-prompt-generation/wp-input.md`
- `AI-Share-Documents/Analysis/00-prompt-generation/wp-prompts.md`
- `AI-Share-Documents/Analysis/00-prompt-generation/wp-metadata.md`
- `AI-Share-Documents/Analysis/00-prompt-generation/wp-self-check.md`
