# Coding Frontend - Inputs and Outputs

## Allowed Inputs
- `AGENTS.md`
- Project design guidance when present
- `SKILLS/FE/shared/*`
- `AI-Share-Documents/Analysis/**`
- `AI-Share-Documents/BE/**` only when backend handoff is required for frontend
- `AI-Share-Documents/Archive/**/BE/**` as optional read-only reference material for matching archived backend documents
- `FE/<Project name>/app/**`
- `FE/<Project name>/src/**`
- `FE/<Project name>/components/**`
- `FE/<Project name>/features/**`
- `FE/<Project name>/lib/**`
- `FE/<Project name>/hooks/**`
- `FE/<Project name>/providers/**`
- `FE/<Project name>/styles/**`
- `FE/<Project name>/public/**`
- `FE/<Project name>/middleware.ts`
- `FE/<Project name>/next.config.*`
- `FE/<Project name>/package.json`
- `FE/<Project name>/tsconfig.json`

## Allowed Outputs
Write only to:
- `FE/<Project name>/app/**`
- `FE/<Project name>/src/**`
- `FE/<Project name>/components/**`
- `FE/<Project name>/features/**`
- `FE/<Project name>/lib/**`
- `FE/<Project name>/hooks/**`
- `FE/<Project name>/providers/**`
- `FE/<Project name>/styles/**`
- `FE/<Project name>/public/**`
- `FE/<Project name>/middleware.ts`
- `FE/<Project name>/next.config.*`
- `FE/<Project name>/package.json`
- `FE/<Project name>/tsconfig.json`
- `AI-Share-Documents/FE/04-coding-fe/coding-plan.md`
- `AI-Share-Documents/FE/04-coding-fe/coding-change-log.md`
- `AI-Share-Documents/FE/04-coding-fe/coding-self-check.md`

## Precondition
System Design must mark frontend work as in scope. If frontend is out of scope, write `Status: NOT_IN_SCOPE` to `AI-Share-Documents/FE/04-coding-fe/coding-self-check.md` and stop.
When frontend is in scope, `sd-data-design.md` must exist and be implementation-usable for frontend state/data work before coding may start.
Backend handoff documents must exist under `AI-Share-Documents/BE/` only when System Design marks backend handoff as required for frontend.
Matching archived BE documents under `AI-Share-Documents/Archive/**/BE/**` may be read for context, but they are optional and cannot block FE-only work when missing.
Project design guidance must be read and followed before coding may start when present.

## Execution Rule
`coding-plan.md` must be created before any production code changes start.
