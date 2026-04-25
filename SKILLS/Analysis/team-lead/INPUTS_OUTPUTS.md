# Team Lead - Inputs and Outputs

## Allowed Inputs
- `AGENTS.md`
- `SKILLS/Analysis/shared/*`
- `AI-Share-Documents/Analysis/01-business-analysis/*`

## Fail-Fast Preflight Rule
Before doing any other work, inspect `AI-Share-Documents/Analysis/01-business-analysis/ba-open-questions.md` first.

If any item is still `Status: OPEN` and has `Blocking: Yes`, `Impact: HIGH`, `Impact: UNKNOWN`, or prevents selecting implementation scope, you must:
- stop immediately
- avoid reading other BA artifacts
- avoid generating planning artifacts
- write only a blocking note to `AI-Share-Documents/Analysis/02-team-lead/tl-self-check.md`

## Allowed Outputs
When blocked, write only:
- `AI-Share-Documents/Analysis/02-team-lead/tl-self-check.md`

When preflight passes, write primary outputs only to:
- `AI-Share-Documents/Analysis/02-team-lead/tl-delivery-plan.md`
- `AI-Share-Documents/Analysis/02-team-lead/tl-task-breakdown.md`
- `AI-Share-Documents/Analysis/02-team-lead/tl-risk-log.md`
- `AI-Share-Documents/Analysis/02-team-lead/tl-handoff.md`
- `AI-Share-Documents/Analysis/02-team-lead/tl-self-check.md`

## Scope Output Requirement
`tl-self-check.md` and `tl-handoff.md` must state:
- `Implementation Scope: BE_ONLY | FE_ONLY | FULL_STACK | ANALYSIS_ONLY`
- `Backend in scope: Yes | No`
- `Frontend in scope: Yes | No`
- `Backend handoff required for FE: Yes | No | N/A`
