# Business Analysis Self Check

## Role Running
`business-analyst`

## Input Files
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `ai-docs/00-project/project-context.md`
- `ai-docs/00-project/constraints.md`
- `ai-docs/00-project/glossary.md`
- `ai-docs/00-prompt-generation/wp-input.md`
- `ai-docs/00-prompt-generation/wp-metadata.md`
- `ai-docs/00-prompt-generation/wp-self-check.md`

## Allowed Output Directories
- `ai-docs/01-business-analysis/`

## Completion Artifacts
- [x] `ai-docs/01-business-analysis/ba-feature-spec.md`
- [x] `ai-docs/01-business-analysis/ba-acceptance-criteria.md`
- [x] `ai-docs/01-business-analysis/ba-open-questions.md`
- [x] `ai-docs/01-business-analysis/ba-self-check.md`

## Upstream Validation
- [x] Prompt-generation input exists.
- [x] Prompt-generation metadata exists.
- [x] Prompt-generation self-check exists and indicates readiness for Business Analysis.
- [x] Required shared policy and traceability files were read.

## Checklist
- [x] Business goal and problem statement defined.
- [x] Actors identified.
- [x] In-scope items defined.
- [x] Out-of-scope items defined.
- [x] Functional requirements defined with stable IDs.
- [x] Non-functional requirements defined with stable IDs.
- [x] Acceptance criteria written in Given/When/Then format.
- [x] Acceptance criteria mapped to requirement IDs.
- [x] Ambiguities captured in `ba-open-questions.md`.
- [x] Assumptions explicitly marked `Status: ASSUMED` with rationale.
- [x] Known gaps documented.
- [x] No planning, design, code, or test artifacts were created.
- [x] Outputs written only to `ai-docs/01-business-analysis/`.

## Open Questions Status
- No items are marked `Status: OPEN`.
- Team Lead may proceed only after confirming this file and preflighting `ba-open-questions.md`.
- All `Status: ASSUMED` items must be carried forward into Team Lead planning and risk artifacts.

## Known Gaps
- Exact response visibility rules for photographer credit are not specified and are marked `Status: ASSUMED`.
- Exact legacy photographer initialization or backfill behavior is not specified and is marked `Status: ASSUMED`.
- Exact numeric validation constraints for admin-set credit values are not specified and are marked `Status: ASSUMED`.
- Exact API contract for admin-managed credit updates is not specified and remains downstream scope.

## Ready For Team Lead
- [x] Yes
