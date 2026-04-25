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

### BAQ-001: Manual Admin Provisioning Mechanism
Status: ASSUMED

Question: What exact mechanism creates Admin accounts manually?

Assumption: Admin provisioning is an internal operational process outside public registration and outside Google OAuth onboarding. The exact operational or technical mechanism can be planned and designed downstream as long as it preserves that boundary.

Rationale: The request clearly states Admin accounts are created manually and must not be created through Google OAuth or public registration, but does not define the manual workflow.

Impacted Requirements: `FR-002`, `FR-003`, `FR-004`, `FR-012`

### BAQ-002: Post-Authentication Token Or Session Contract
Status: ASSUMED

Question: What token, session, or response contract should the backend issue after successful authentication?

Assumption: The exact post-authentication token/session format is a downstream design decision. Business acceptance requires only that the result is based on server-side validated account data and remains ready for later authorization integration.

Rationale: The request defines authentication and security rules but does not specify session mechanics.

Impacted Requirements: `FR-016`, `NFR-004`, `NFR-005`

### BAQ-003: Account Matching Rule For Google Login
Status: ASSUMED

Question: Which Google identity fields determine whether an account already exists?

Assumption: Account matching uses server-side persisted account identity linked to validated Google identity data. The precise matching fields and uniqueness rules are deferred to system design.

Rationale: The request says to create an account if it does not exist and authenticate the existing account if it does, but does not specify identity matching rules.

Impacted Requirements: `FR-006`, `FR-007`, `NFR-001`, `NFR-003`, `NFR-004`

### BAQ-004: Disabled Account Response Behavior
Status: ASSUMED

Question: What exact user-facing error response should be returned when a disabled account attempts login?

Assumption: Disabled accounts must be denied login. Exact error code, message, and response envelope details are downstream design decisions.

Rationale: The request specifies the business/security outcome but not the response details.

Impacted Requirements: `FR-015`, `NFR-006`

### BAQ-005: Existing Non-Photographer Account With Matching Google Identity
Status: ASSUMED

Question: If a Google login matches an existing account whose role is not `Photographer`, what should happen?

Assumption: The system authenticates the existing account only if the account is permitted to authenticate through the attempted flow under downstream design rules, but it must never mutate the stored role. Google OAuth must not create or convert an account to `Admin`.

Rationale: The request says subsequent Google logins authenticate existing accounts and preserve role, while also saying Admin accounts must not be created through Google OAuth. The safe business invariant is that role never changes and Google never creates Admin.

Impacted Requirements: `FR-003`, `FR-007`, `FR-008`, `FR-009`, `FR-010`

## Open Items Summary
- No items are marked `Status: OPEN`.
- All unresolved details are marked `Status: ASSUMED` with rationale and must be carried forward by Team Lead.
