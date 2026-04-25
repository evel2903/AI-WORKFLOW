# Write Prompt Self Check

## Role Running
`write-prompt`

## Input Traceability
- [x] Raw natural language request recorded in `wp-input.md`
- [x] Important assumptions recorded in `wp-metadata.md`
- [x] Required policy and traceability files were used as inputs

## Prompt Coverage
- [x] Business Analyst prompt created
- [x] Team Lead prompt created
- [x] System Design prompt created
- [x] Coding Backend prompt created
- [x] Testing Backend prompt created
- [x] Final archive/reset prompt appended at the bottom

## Boundary Validation
- [x] Each prompt defines role running
- [x] Each prompt defines inputs
- [x] Each prompt defines allowed output directories
- [x] Each prompt defines completion artifacts
- [x] Each prompt preserves Waterfall order
- [x] No generated artifact writes outside `ai-docs/00-prompt-generation/`

## Traceability Validation
- [x] Prompts instruct downstream phases to use stable IDs
- [x] Prompts preserve the preferred chain `FR -> AC -> TL -> SD -> CD -> TC`

## Known Gaps
- Exact manual Admin provisioning workflow is not specified in the source request and is intentionally left for downstream clarification.
- Exact post-auth session/token contract is not specified in the source request and is intentionally left for downstream clarification and design.
- Existing auth module structure and persistence model were not provided in the source request and must be validated downstream against the codebase.

## Ready For Business Analysis
- [x] Yes
