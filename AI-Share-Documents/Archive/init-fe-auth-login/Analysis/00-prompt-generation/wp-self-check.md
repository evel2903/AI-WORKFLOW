# Write Prompt Self-Check

## Role Running
`write-prompt`

## Checklist
- [x] Captured the raw natural language request in `wp-input.md`.
- [x] Identified project name: `EvelS`.
- [x] Identified feature name: `init-fe-auth-login`.
- [x] Identified expected implementation scope: `FE_ONLY`.
- [x] Read and applied `RULE.md`.
- [x] Read and applied Write Prompt instructions.
- [x] Read shared Waterfall and traceability rules.
- [x] Read archived authentication context before generating downstream prompts.
- [x] Generated Analysis prompts for Business Analyst, Team Lead, and System Design.
- [x] Generated FE coding and FE testing prompts.
- [x] Omitted BE coding and BE testing prompts because backend is out of scope.
- [x] Included optional archived BE document lookup for FE prompts.
- [x] Appended workflow archiver prompt.
- [x] Ensured each generated prompt states role, project, feature, inputs, allowed outputs, and completion artifacts.
- [x] Preserved conditional scope gates and `NOT_IN_SCOPE` handling guidance.
- [x] Wrote only to `AI-Share-Documents/Analysis/00-prompt-generation/`.

## Validation Result
Status: `PASS`

## Notes
- The generated workflow expects downstream Analysis to confirm `Implementation Scope: FE_ONLY`, `Backend in scope: No`, `Frontend in scope: Yes`, and `Backend handoff required for FE: No`.
- Backend authentication details were taken from the archived `google-oauth-role-bound-authentication` feature and must be treated as read-only context for this frontend feature.
