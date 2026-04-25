# Team Lead Task Breakdown

## Role Running
`team-lead`

## Input Files
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `ai-docs/00-project/project-context.md`
- `ai-docs/00-project/constraints.md`
- `ai-docs/00-project/glossary.md`
- `ai-docs/01-business-analysis/ba-feature-spec.md`
- `ai-docs/01-business-analysis/ba-acceptance-criteria.md`
- `ai-docs/01-business-analysis/ba-open-questions.md`
- `ai-docs/01-business-analysis/ba-self-check.md`

## Allowed Output Directories
- `ai-docs/02-team-lead/`

## Completion Artifacts
- `ai-docs/02-team-lead/tl-delivery-plan.md`
- `ai-docs/02-team-lead/tl-task-breakdown.md`
- `ai-docs/02-team-lead/tl-risk-log.md`
- `ai-docs/02-team-lead/tl-handoff.md`
- `ai-docs/02-team-lead/tl-self-check.md`

## Tasks

| Task ID | Task | Upstream Mapping | Dependencies | Expected Downstream Owner |
| --- | --- | --- | --- | --- |
| `TL-001` | Define authenticated role scope for exactly `Admin` and `Photographer`, including explicit exclusion of Customer authentication. | `FR-001`, `FR-013`, `FR-014`, `AC-001`, `AC-008` | BA artifacts complete | `system-design` |
| `TL-002` | Plan manual-only Admin account boundary and ensure no public or Google OAuth Admin creation path enters the feature scope. | `FR-002`, `FR-003`, `FR-004`, `FR-012`, `AC-002`, `AC-003`, `BAQ-001` | `TL-001` | `system-design` |
| `TL-003` | Plan Google OAuth-only Photographer onboarding, including first-login account creation with default role `Photographer`. | `FR-005`, `FR-006`, `AC-003`, `AC-004`, `NFR-001`, `NFR-003` | `TL-001` | `system-design` |
| `TL-004` | Plan existing-account Google login behavior, including account lookup and authentication without role mutation. | `FR-007`, `FR-008`, `FR-009`, `AC-005`, `BAQ-003`, `BAQ-005` | `TL-003` | `system-design` |
| `TL-005` | Plan immutable role enforcement and absence of role upgrade or downgrade flows. | `FR-009`, `FR-010`, `AC-006`, `AC-013` | `TL-002`, `TL-004` | `system-design` |
| `TL-006` | Plan unsupported authentication flow handling for email/password and unsupported registration attempts. | `FR-011`, `FR-012`, `FR-013`, `FR-014`, `NFR-007`, `AC-007`, `AC-008`, `AC-013` | `TL-001`, `TL-002` | `system-design` |
| `TL-007` | Plan server-side trust boundary for Google token validation and rejection of client-provided role or unvalidated identity data. | `NFR-001`, `NFR-002`, `NFR-003`, `NFR-004`, `AC-009`, `AC-010` | `TL-003`, `TL-004` | `system-design` |
| `TL-008` | Plan disabled-account login denial for all authenticated account roles. | `FR-015`, `NFR-006`, `AC-011`, `BAQ-004` | `TL-004`, `TL-007` | `system-design` |
| `TL-009` | Require System Design to produce implementation-usable data design for accounts, roles, provider identity mapping, disabled state, and immutability constraints. | `FR-001` through `FR-016`, `NFR-001` through `NFR-007`, `AC-001` through `AC-013`, `BAQ-003` | `TL-001` through `TL-008` | `system-design` |
| `TL-010` | Plan authorization-readiness without implementing full authorization policy rules. | `FR-016`, `NFR-005`, `AC-012` | `TL-001`, `TL-007` | `system-design` |
| `TL-011` | Require API-level behavior definition for successful login, denied disabled login, invalid token, unsupported flows, and role spoofing attempts. | `AC-004`, `AC-005`, `AC-007`, `AC-009`, `AC-010`, `AC-011`, `AC-013`, `BAQ-002`, `BAQ-004` | `TL-003` through `TL-008` | `system-design` |
| `TL-012` | Require Coding Backend to create a coding plan before source changes and implement only approved System Design scope. | `AC-001` through `AC-013` | Completed System Design artifacts | `coding-be` |
| `TL-013` | Require Coding Backend to preserve phase boundaries: production code in `src/`, coding artifacts in `ai-docs/04-coding/`, no tests. | Repository constraints, Waterfall policy | `TL-012` | `coding-be` |
| `TL-014` | Require Testing Backend to validate coding plan and implement tests covering traceable acceptance criteria. | `AC-001` through `AC-013` | Completed Coding artifacts | `testing-be` |
| `TL-015` | Require Testing Backend to document test results, gaps, and residual risks, especially unsupported flows and security boundaries. | `NFR-001` through `NFR-007`, `AC-009`, `AC-010`, `AC-011`, `AC-013` | `TL-014` | `testing-be` |

## Critical Path
1. Resolve design-level treatment of `BAQ-003` and `BAQ-005` without changing BA business rules.
2. Produce `sd-data-design.md` with account identity, role, disabled state, and immutability constraints.
3. Define API/authentication responses enough for coding and testing.
4. Implement authentication behavior only after coding plan is created.
5. Test all supported and explicitly unsupported authentication paths.

## Explicit Non-Goals For All Downstream Phases
- Do not add email/password authentication.
- Do not add Admin self-registration.
- Do not add Google OAuth Admin onboarding.
- Do not add Customer accounts or Customer authentication.
- Do not add role upgrade or downgrade workflows.
- Do not implement broad authorization rules beyond preserving server-side auth data for future authorization.
