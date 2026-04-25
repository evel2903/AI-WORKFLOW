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
| `TL-001` | Define the photographer-only credit ownership invariant and explicit ineligibility for every non-photographer role. | `FR-001`, `FR-002`, `FR-010`, `NFR-001`, `AC-001`, `AC-002`, `AC-010`, `BAQ-001` | BA artifacts complete | `system-design` |
| `TL-002` | Plan first-login default initialization behavior for photographers, including preservation of credit on later logins. | `FR-005`, `FR-006`, `NFR-003`, `AC-005`, `AC-006`, `BAQ-002` | `TL-001` | `system-design` |
| `TL-003` | Plan admin-only manual update behavior for photographer credit, including denial of non-admin update attempts. | `FR-003`, `FR-004`, `NFR-002`, `NFR-004`, `AC-003`, `AC-004`, `BAQ-003` | `TL-001` | `system-design` |
| `TL-004` | Plan rejection or structural prevention of credit assignment or update for non-photographer targets across relevant flows. | `FR-002`, `FR-007`, `FR-010`, `NFR-001`, `NFR-004`, `AC-002`, `AC-007`, `AC-010`, `BAQ-001` | `TL-001`, `TL-003` | `system-design` |
| `TL-005` | Require System Design to define response visibility boundaries for photographer credit without violating the non-photographer invariant. | `FR-001`, `FR-002`, `FR-008`, `FR-010`, `AC-002`, `AC-008`, `BAQ-001`, `BAQ-004` | `TL-001` | `system-design` |
| `TL-006` | Require System Design to produce implementation-usable data design covering storage type, nullability behavior, constraints, and legacy initialization or migration notes. | `FR-005`, `FR-006`, `FR-007`, `FR-008`, `NFR-003`, `NFR-006`, `AC-005`, `AC-006`, `AC-007`, `AC-008`, `BAQ-002`, `BAQ-003` | `TL-001` through `TL-005` | `system-design` |
| `TL-007` | Require System Design to define API and authorization behavior for approved admin credit updates and any necessary read surfaces. | `FR-003`, `FR-004`, `FR-008`, `NFR-002`, `NFR-004`, `AC-003`, `AC-004`, `AC-008`, `BAQ-004` | `TL-003`, `TL-005`, `TL-006` | `system-design` |
| `TL-008` | Preserve explicit non-goals so System Design does not introduce spending, recharge, automatic adjustment, or ledger behavior. | `FR-009`, `NFR-005`, `AC-009` | `TL-001` through `TL-007` | `system-design` |
| `TL-009` | Require Coding Backend to create `coding-plan.md` before source changes and implement only the approved photographer-credit scope in `src/`. | `FR-001` through `FR-010`, `NFR-001` through `NFR-006`, `AC-001` through `AC-010` | Completed System Design artifacts | `coding-be` |
| `TL-010` | Require Coding Backend to preserve repo boundaries, enforce server-side eligibility and authorization rules, and document any legacy-data handling chosen by System Design. | Repository constraints, Waterfall policy, `NFR-001`, `NFR-002`, `NFR-003`, `BAQ-002`, `BAQ-003` | `TL-009` | `coding-be` |
| `TL-011` | Require Testing Backend to validate coding scope and cover first-login initialization, later-login preservation, admin-only update behavior, and non-photographer rejection behavior. | `AC-003` through `AC-010` | Completed Coding artifacts | `testing-be` |
| `TL-012` | Require Testing Backend to document results, gaps, and residual risks for visibility rules, legacy handling, and numeric validation constraints. | `NFR-003`, `NFR-004`, `NFR-006`, `BAQ-001`, `BAQ-002`, `BAQ-003`, `BAQ-004` | `TL-011` | `testing-be` |

## Critical Path
1. Define the photographer-only credit invariant and non-photographer ineligibility.
2. Make first-login initialization and later-login preservation implementation-usable.
3. Define admin-only update authorization and target eligibility checks.
4. Produce `sd-data-design.md` with concrete persistence constraints and legacy-handling direction.
5. Implement only after Coding creates `coding-plan.md`.
6. Test initialization, preservation, authorization, and rejection behavior.

## Explicit Non-Goals For All Downstream Phases
- Do not implement credit spending behavior.
- Do not implement credit increase or decrease workflows driven by business events.
- Do not implement recharge, top-up, expiration, or automatic adjustment behavior.
- Do not implement a transaction ledger or credit history subsystem.
- Do not implement non-admin credit management.
