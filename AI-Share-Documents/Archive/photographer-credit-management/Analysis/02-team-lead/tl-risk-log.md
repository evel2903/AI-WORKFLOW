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
| `RISK-001` | `BAQ-001`, `FR-002`, `FR-007`, `FR-010` | Ambiguous non-photographer credit representation can produce inconsistent API behavior or accidental exposure of credit in contexts where it should be non-usable. | Medium | System Design must define a consistent representation rule and preserve the invariant that non-photographer roles never have usable credit. | `system-design` |
| `RISK-002` | `BAQ-002`, `FR-005`, `FR-006`, `NFR-003` | Legacy photographers without initialized credit may be skipped, initialized twice, or handled inconsistently if rollout behavior is not specified clearly. | High | System Design must define implementation-usable legacy handling and Coding must document and implement that behavior exactly once. | `system-design`, `coding-be` |
| `RISK-003` | `BAQ-003`, `FR-003`, `NFR-003`, `NFR-004` | Missing numeric validation could allow invalid credit values such as negatives or unsupported numeric formats. | High | System Design must define numeric constraints and validation direction; Coding must enforce them; Testing must cover invalid update attempts. | `system-design`, `coding-be`, `testing-be` |
| `RISK-004` | `FR-003`, `FR-004`, `NFR-002`, `AC-003`, `AC-004` | Admin-update authorization may check the caller role but fail to validate the target user role, allowing credit updates on ineligible users. | High | System Design must require both actor authorization and target eligibility checks. Testing must cover valid photographer targets and invalid non-photographer targets. | `system-design`, `testing-be` |
| `RISK-005` | `FR-005`, `FR-006`, `BR-005`, `AC-005`, `AC-006` | First-login initialization logic may incorrectly run again on subsequent logins and overwrite existing credit values. | High | System Design must define one-time initialization rules and Coding must preserve existing stored values on later logins. | `system-design`, `coding-be` |
| `RISK-006` | `FR-009`, `NFR-005`, `AC-009` | Scope creep can expand the feature into spending, recharge, adjustment, or ledger behavior that was not approved. | Medium | Team Lead and System Design must preserve explicit non-goals, and Coding must implement only storage plus admin-managed updates. | `system-design`, `coding-be` |
| `RISK-007` | `NFR-001`, `NFR-002`, `AC-010` | Client-provided role or credit-related input could influence eligibility or authorization if server-side checks are incomplete. | High | System Design must define server-authoritative role and state evaluation; Coding must ignore client attempts to override eligibility; Testing must exercise spoofing-adjacent cases. | `system-design`, `coding-be`, `testing-be` |
| `RISK-008` | Waterfall policy, data design rule | Coding may be blocked or may implement unsafe persistence if `sd-data-design.md` is incomplete, especially for nullability, constraints, and legacy handling. | High | System Design must produce implementation-usable data design with explicit constraints, migration notes, and reuse or extension guidance. | `system-design` |

## Carried Assumptions
- `BAQ-001`: Non-photographer credit representation may vary technically, but non-photographer roles must never have usable credit.
- `BAQ-002`: Legacy photographers with no initialized credit may receive `10` on first successful login after rollout.
- `BAQ-003`: Credit should be treated as a non-negative whole-number value unless later clarified upstream.
- `BAQ-004`: Credit visibility is limited to approved backend use cases and does not need to be universal across every response shape.

## Current Blockers
- None. No BA question entry is marked `Status: OPEN`.
