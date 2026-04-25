# Team Lead Risk Log

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

## Risks

| Risk ID | Source | Risk | Impact | Mitigation | Owner Phase |
| --- | --- | --- | --- | --- | --- |
| `RISK-001` | `BAQ-001`, `FR-002`, `FR-003`, `FR-004`, `FR-012` | Manual Admin provisioning is undefined and could be accidentally implemented as a public or OAuth flow. | High | System Design must define explicit no-public-registration and no-Google-Admin-onboarding boundaries before Coding. | `system-design` |
| `RISK-002` | `BAQ-002`, `FR-016`, `NFR-005` | Missing post-auth token/session contract could block coding or produce data that is not authorization-ready. | Medium | System Design must define authentication output and authorization-ready account/role data without implementing broad authorization rules. | `system-design` |
| `RISK-003` | `BAQ-003`, `FR-006`, `FR-007`, `NFR-001`, `NFR-003` | Unclear Google account matching could create duplicate accounts or authenticate the wrong account. | High | System Design must define server-side account matching and uniqueness rules based on validated Google identity data. | `system-design` |
| `RISK-004` | `BAQ-004`, `FR-015`, `NFR-006` | Disabled-account denial may be inconsistent if response and check timing are not specified. | Medium | System Design must define denial timing and API-level error behavior; Coding must enforce before login completion; Testing must cover denial. | `system-design`, `coding-be`, `testing-be` |
| `RISK-005` | `BAQ-005`, `FR-003`, `FR-007`, `FR-008`, `FR-009`, `FR-010` | A Google login matching a non-Photographer account could conflict with role immutability and no-Google-Admin-creation rules. | High | System Design must decide permitted or denied behavior while preserving stored role and never creating or converting Admin through Google OAuth. | `system-design` |
| `RISK-006` | `NFR-002`, `NFR-003`, `NFR-004`, `AC-009`, `AC-010` | Client-provided role or identity data could be trusted by mistake, causing privilege escalation or identity spoofing. | High | System Design must define server-only trust boundaries; Coding must ignore client role input; Testing must include spoofing attempts. | `system-design`, `coding-be`, `testing-be` |
| `RISK-007` | `FR-009`, `FR-010`, `AC-006` | Role immutability could be bypassed through profile, admin, login, or account update paths if not treated as a global invariant. | High | System Design must define role as immutable after creation; Coding must avoid mutation paths; Testing must cover attempted role changes. | `system-design`, `coding-be`, `testing-be` |
| `RISK-008` | `FR-011`, `FR-012`, `FR-013`, `FR-014`, `NFR-007`, `AC-013` | Unsupported flows may partially exist in codebase and accidentally remain active. | Medium | System Design and Coding must inspect and constrain unsupported flows; Testing must verify unsupported flows fail safely or are absent. | `system-design`, `coding-be`, `testing-be` |
| `RISK-009` | Waterfall policy, data design rule | Coding may be blocked if `sd-data-design.md` is incomplete or placeholder-only. | High | System Design must produce implementation-usable data design covering account, role, identity mapping, disabled state, and immutability. | `system-design` |

## Carried Assumptions
- `BAQ-001`: Admin manual provisioning is internal and non-public; exact mechanism deferred.
- `BAQ-002`: Token/session output format is deferred to System Design.
- `BAQ-003`: Google identity matching mechanics are deferred to System Design.
- `BAQ-004`: Disabled-account error response details are deferred to System Design.
- `BAQ-005`: Existing non-Photographer account behavior on Google login is deferred to System Design, with role immutability and no Admin OAuth creation as hard constraints.

## Current Blockers
- None. No BA question entry is marked `Status: OPEN`.
