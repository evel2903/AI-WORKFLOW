# Reusable Prompt Rules

Use these rules when creating a first prompt for the workflow.

## Required Context
Run the first prompt from the workspace root. The Write Prompt agent must read and follow:
- `AGENTS.md`
- `SKILLS/Analysis/shared/WATERFALL_POLICY.md`
- `SKILLS/Analysis/shared/TRACEABILITY.md`
- `SKILLS/Analysis/write-prompt/SYSTEM_PROMPT.md`
- `SKILLS/Analysis/write-prompt/CHECKLIST.md`
- `SKILLS/Analysis/write-prompt/INPUTS_OUTPUTS.md`

## Write Prompt Output Rules
- Write prompt-generation outputs to `AI-Share-Documents/Analysis/00-prompt-generation/`.
- Do not copy Analysis artifacts into BE or FE output folders.
- Do not execute downstream agents.
- Do not write source code.
- Do not write outputs only in chat.
- Create `wp-input.md`, `wp-prompts.md`, `wp-metadata.md`, and `wp-self-check.md`.

## Generated Prompt Coverage
Generate downstream prompts for:
- `business-analyst` using `SKILLS/Analysis/business-analyst/`
- `team-lead` using `SKILLS/Analysis/team-lead/`
- `system-design` using `SKILLS/Analysis/system-design/`
- `coding-be` using `SKILLS/BE/coding-be/` only when backend is in scope
- `testing-be` using `SKILLS/BE/testing-be/` only when backend is in scope
- BE document sharing to `AI-Share-Documents/BE/` only when frontend depends on backend changes
- `coding-fe` using `SKILLS/FE/coding-fe/` only when frontend is in scope
- `testing-fe` using `SKILLS/FE/testing-fe/` only when frontend is in scope
- `workflow-archiver`

## Shared Document Paths
- Downstream Analysis prompts write to `AI-Share-Documents/Analysis/01-business-analysis/`, `AI-Share-Documents/Analysis/02-team-lead/`, and `AI-Share-Documents/Analysis/03-system-design/`.
- BE prompts read Analysis inputs from `AI-Share-Documents/Analysis/`.
- BE prompts write coding and testing docs to `AI-Share-Documents/BE/04-coding-be/` and `AI-Share-Documents/BE/05-testing-be/`.
- FE prompts read Analysis inputs from `AI-Share-Documents/Analysis/`.
- FE prompts read current BE handoff inputs from `AI-Share-Documents/BE/` only when backend handoff is required.
- FE prompts may read optional matching archived BE docs from `AI-Share-Documents/Archive/**/BE/` when useful.
- FE prompts write coding and testing docs to `AI-Share-Documents/FE/04-coding-fe/` and `AI-Share-Documents/FE/05-testing-fe/`.

## Scope And Order
- Preserve the required Analysis order: `write-prompt -> business-analyst -> team-lead -> system-design`.
- After System Design, generate only the BE and/or FE implementation phases required by scope.
- If a generated prompt invokes an out-of-scope BE or FE role, that role must write `Status: NOT_IN_SCOPE` to its self-check and stop.

## Archive Rule
The `workflow-archiver` prompt must:
1. Pack `AI-Share-Documents/Analysis/`, `AI-Share-Documents/BE/`, and `AI-Share-Documents/FE/` into `AI-Share-Documents/Archive/<Feature_name>/`.
2. Write `AI-Share-Documents/Archive/<Feature_name>/Feature_Report.md`.
3. Copy `AI-Share-Documents/Template/Analysis/`, `AI-Share-Documents/Template/BE/`, and `AI-Share-Documents/Template/FE/` back into `AI-Share-Documents/`.
