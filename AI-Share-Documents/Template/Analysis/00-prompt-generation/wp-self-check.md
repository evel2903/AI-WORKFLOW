# Write Prompt Self Check

## Input Traceability
- [ ] Raw natural language request recorded in `wp-input.md`.
- [ ] Project name and feature name recorded or explicitly marked as placeholders.
- [ ] Important assumptions recorded in `wp-metadata.md`.

## Prompt Coverage
- [ ] Business Analyst prompt created for the Analysis kit.
- [ ] Team Lead prompt created for the Analysis kit.
- [ ] System Design prompt created for the Analysis kit.
- [ ] Analysis artifact publishing prompt created.
- [ ] Coding Backend prompt created.
- [ ] Testing Backend prompt created.
- [ ] BE document-sharing prompt created.
- [ ] Coding Frontend prompt created.
- [ ] Testing Frontend prompt created.
- [ ] Workflow Archiver prompt created.

## Boundary Validation
- [ ] Each prompt defines inputs.
- [ ] Each prompt defines outputs.
- [ ] Each prompt preserves conditional scope gates and BE-before-FE order only when backend handoff is required for frontend.
- [ ] Each prompt states allowed write locations.
- [ ] Archive/reset prompt packs active Analysis/BE/FE folders into `AI-Share-Documents/Archive/<Feature_name>/` and restores active folders from `AI-Share-Documents/Template/`.

## Known Gaps
- None

## Ready For Business Analysis
- [ ] Yes
