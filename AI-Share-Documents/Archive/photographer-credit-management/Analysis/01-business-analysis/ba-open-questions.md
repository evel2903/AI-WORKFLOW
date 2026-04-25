# Business Analysis Open Questions

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
- `ai-docs/01-business-analysis/ba-feature-spec.md`
- `ai-docs/01-business-analysis/ba-acceptance-criteria.md`
- `ai-docs/01-business-analysis/ba-open-questions.md`
- `ai-docs/01-business-analysis/ba-self-check.md`

## Questions And Assumptions

### BAQ-001: Non-Photographer Credit Representation
Status: ASSUMED
Impact: MEDIUM

Question: How should credit appear in response models for users whose role is not `Photographer`?

Assumption: The exact representation may be `null`, omitted, hidden, or otherwise non-usable, provided the business invariant is preserved that non-photographer roles do not have usable credit.

Rationale: The source request explicitly allows multiple representation approaches and cares primarily about preserving the eligibility rule rather than dictating response-shape details at BA stage.

Impacted Requirements: `FR-002`, `FR-007`, `FR-010`, `NFR-004`

### BAQ-002: Legacy Photographer Initialization Behavior
Status: ASSUMED
Impact: HIGH

Question: How should photographers created before this feature receive their initial credit if they do not already have one?

Assumption: If a photographer record does not yet have an initialized credit value when the feature becomes active, the system may treat the first successful photographer login after rollout as the event that initializes `credit = 10`.

Rationale: The request mandates first-login initialization but does not specify a separate migration-only or bulk backfill business rule for existing photographers. This assumption allows downstream planning without inventing a mandatory data migration at the BA stage.

Impacted Requirements: `FR-005`, `FR-006`, `FR-008`, `NFR-003`

### BAQ-003: Numeric Validation For Admin-Set Credit
Status: ASSUMED
Impact: HIGH

Question: What numeric values are valid when an admin manually updates photographer credit?

Assumption: Credit should be treated as a whole-number business value, and downstream phases should preserve a non-negative constraint unless a later upstream clarification changes that rule.

Rationale: The request defines credit as a storable value for future use but does not specify bounds or whether negative values are meaningful. A non-negative whole-number assumption is the most conservative business interpretation for this phase.

Impacted Requirements: `FR-003`, `FR-008`, `NFR-003`, `NFR-004`

### BAQ-004: Credit Visibility Scope
Status: ASSUMED
Impact: MEDIUM

Question: In which response contexts should photographer credit be visible?

Assumption: This feature requires credit to be stored and available to approved backend use cases, but it does not require BA to define that every user-facing or admin-facing response must expose the field. Downstream design may restrict visibility to contexts that already return relevant user data.

Rationale: The request focuses on ownership, initialization, storage, and admin update permission, not on universal response exposure.

Impacted Requirements: `FR-001`, `FR-002`, `FR-008`, `FR-010`

## Open Items Summary
- No items are marked `Status: OPEN`.
- All unresolved details are marked `Status: ASSUMED` with rationale and must be carried forward by Team Lead.
