# System Design - Inputs and Outputs

## Allowed Inputs
- `AGENTS.md`
- `SKILLS/Analysis/shared/*`
- `AI-Share-Documents/Analysis/01-business-analysis/*`
- `AI-Share-Documents/Analysis/02-team-lead/*`
- Optional backend code context from `BE/<Project name>/`
- Optional frontend code context from `FE/<Project name>/`
- Optional frontend design guidance from `FE/<Project name>/DESIGN.md` or approved design artifacts when present
- Optional shared context from `AI-Share-Documents/`
- Optional archived BE reference documents from `AI-Share-Documents/Archive/**/BE/**` when `FE_ONLY` work needs existing backend contract context

## Allowed Outputs
Write primary outputs only to:
- `AI-Share-Documents/Analysis/03-system-design/sd-solution-overview.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-domain-design.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-api-contract.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-data-design.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-implementation-guidelines.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-self-check.md`

## Output Requirement
`sd-data-design.md` is a required completion artifact.
The phase must be treated as incomplete if this file is missing, empty, or placeholder-only.
`sd-self-check.md` must state `Implementation Scope`, `Backend in scope`, `Frontend in scope`, and `Backend handoff required for FE`.
When frontend work is in scope, the System Design outputs must include route/layout design, component boundaries, UI states, state ownership, integration behavior, and project design guidance implications when applicable.
