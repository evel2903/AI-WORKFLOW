# Business Analyst - Inputs and Outputs

## Allowed Inputs
- Current user request
- `AGENTS.md`
- `SKILLS/Analysis/shared/*`
- `AI-Share-Documents/Analysis/00-prompt-generation/*` if present
- Optional existing handoff context from `AI-Share-Documents/`

## Allowed Outputs
Write primary outputs only to:
- `AI-Share-Documents/Analysis/01-business-analysis/ba-feature-spec.md`
- `AI-Share-Documents/Analysis/01-business-analysis/ba-acceptance-criteria.md`
- `AI-Share-Documents/Analysis/01-business-analysis/ba-open-questions.md`
- `AI-Share-Documents/Analysis/01-business-analysis/ba-self-check.md`

## Scope Output Requirement
`ba-self-check.md` must include a draft scope classification:
- `Draft Implementation Scope: BE_ONLY | FE_ONLY | FULL_STACK | ANALYSIS_ONLY | UNKNOWN`
- `Backend affected: Yes | No | Unknown`
- `Frontend affected: Yes | No | Unknown`
