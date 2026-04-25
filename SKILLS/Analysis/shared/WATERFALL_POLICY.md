# Waterfall Policy

You are part of a strict cross-kit Waterfall workflow.

## Core Rules
- All agent-to-agent communication must happen through stored files, never unstored chat context.
- The Analysis kit owns prompt generation, business analysis, team planning, and system design.
- The BE and FE kits own only coding, testing, and final archive/reset behavior.
- Backend implementation and testing gate frontend implementation only when backend work is in scope and System Design marks backend handoff as required for frontend.
- If required upstream artifacts are missing, incomplete, contradictory, or empty, stop and report the dependency gap in your own output area.
- Do not overwrite upstream phase artifacts from a downstream role.
- Stay within your allowed read/write boundaries.
- Do not expand business scope unless a new upstream artifact explicitly changes it.
- Every phase must produce its required self-check artifact before it is considered complete.
- Every downstream phase must validate upstream self-check artifacts before starting work.
- Open questions must never be ignored or silently assumed away.
- Fail fast when a blocking precondition is not satisfied.

## Scope Model
Every feature must declare one implementation scope:
- `BE_ONLY`
- `FE_ONLY`
- `FULL_STACK`
- `ANALYSIS_ONLY`

Team Lead and System Design must record:
- `Implementation Scope`
- `Backend in scope`
- `Frontend in scope`
- `Backend handoff required for FE`

Missing BE or FE artifacts are blocking only when that surface is in scope. If an out-of-scope role is invoked, it must write `Status: NOT_IN_SCOPE` to its self-check artifact and stop.

## Workflow Order
Analysis is always:
`write-prompt -> business-analyst -> team-lead -> system-design`

Implementation is conditional:
- `BE_ONLY`: `coding-be -> testing-be -> workflow-archiver`
- `FE_ONLY`: `coding-fe -> testing-fe -> workflow-archiver`
- `FULL_STACK`: `coding-be -> testing-be -> share-documents -> coding-fe -> testing-fe -> workflow-archiver`
- `ANALYSIS_ONLY`: `workflow-archiver`

## Analysis Output Rule
Analysis roles write artifacts under `AI-Share-Documents/Analysis/00-prompt-generation/` through `AI-Share-Documents/Analysis/03-system-design/`.

Analysis artifacts must not be copied into BE or FE output folders. BE and FE agents read `AI-Share-Documents` directly.

## Gating Rules
- `business-analyst` requires prompt-generation input or a direct user request.
- `team-lead` requires:
  - `AI-Share-Documents/Analysis/01-business-analysis/ba-feature-spec.md`
  - `AI-Share-Documents/Analysis/01-business-analysis/ba-acceptance-criteria.md`
  - `AI-Share-Documents/Analysis/01-business-analysis/ba-open-questions.md`
  - `AI-Share-Documents/Analysis/01-business-analysis/ba-self-check.md`
- `system-design` requires:
  - `AI-Share-Documents/Analysis/02-team-lead/tl-delivery-plan.md`
  - `AI-Share-Documents/Analysis/02-team-lead/tl-task-breakdown.md`
  - `AI-Share-Documents/Analysis/02-team-lead/tl-handoff.md`
  - `AI-Share-Documents/Analysis/02-team-lead/tl-self-check.md`
- `coding-be` requires shared Analysis artifacts in `AI-Share-Documents/Analysis/03-system-design/` only when backend is in scope.
- `testing-be` requires completed BE coding artifacts in `AI-Share-Documents/BE/04-coding-be/` only when backend is in scope.
- `coding-fe` requires shared Analysis artifacts in `AI-Share-Documents/Analysis/03-system-design/` only when frontend is in scope.
- `coding-fe` requires completed BE testing artifacts and BE handoff documents only when `Backend handoff required for FE: Yes`.
- `coding-fe` may read matching archived BE documents under `AI-Share-Documents/Archive/**/BE/**` as optional context, especially for `FE_ONLY`; missing archived documents are not blocking.
- `testing-fe` requires completed FE coding artifacts in `AI-Share-Documents/FE/04-coding-fe/` only when frontend is in scope.
- `workflow-archiver` requires completed testing artifacts for each in-scope surface, or `Status: NOT_IN_SCOPE` self-checks for invoked out-of-scope surfaces.

## Open Questions Gating Rule
For Business Analysis handoff:
- `team-lead` must perform a preflight check on `AI-Share-Documents/Analysis/01-business-analysis/ba-open-questions.md` before any other Team Lead work.
- If any question is marked `Status: OPEN` and has `Blocking: Yes`, `Impact: HIGH`, `Impact: UNKNOWN`, or prevents selecting implementation scope, `team-lead` must stop immediately.
- When blocked, `team-lead` must not read additional BA artifacts and must not produce planning artifacts.
- When blocked, `team-lead` may write only a blocking note in `AI-Share-Documents/Analysis/02-team-lead/tl-self-check.md`.
- `team-lead` may proceed when all blocking questions are either `RESOLVED` or explicitly marked `ASSUMED` with a documented assumption and rationale. Non-blocking open questions must be carried into `tl-risk-log.md` and `tl-handoff.md`.
- `team-lead` must carry all assumptions forward into `tl-risk-log.md` and `tl-handoff.md`.

## System Design Data Contract Rule
- `system-design` must generate `sd-data-design.md` as a required artifact.
- The file must be implementation-usable for backend data modeling and frontend state/data-flow work when those surfaces are affected.
- Backend guidance must cover entities, aggregates or records, persistence mapping, relationships, key constraints, and migration notes when applicable.
- Frontend guidance must cover data sources, API response mapping, state ownership, cache or revalidation boundaries, and browser/session persistence notes when applicable.
- If no new data design is needed for one side, the file must explicitly say why and name the existing structures being reused.

## Sharing Rule
After `testing-be` completes, backend handoff documents must be copied to `AI-Share-Documents/BE/` only when frontend work depends on backend changes.
When required, include API contracts, implementation notes, test results, known gaps, and any FE integration constraints.

## Archived BE Reference Rule
Archived BE documents in `AI-Share-Documents/Archive/**/BE/**` are read-only historical references for frontend roles. They may help `FE_ONLY` work locate existing backend contracts for the same feature or integration, but they never replace current Analysis/System Design artifacts and are never required gates.

## Archive Rule
After the last in-scope testing phase completes, or after System Design for `ANALYSIS_ONLY`, `workflow-archiver` must:
1. Pack `AI-Share-Documents/Analysis/`, `AI-Share-Documents/BE/`, and `AI-Share-Documents/FE/` into `AI-Share-Documents/Archive/<Feature_name>/`.
2. Write `AI-Share-Documents/Archive/<Feature_name>/Feature_Report.md` summarizing the feature and completed work.
3. Copy `AI-Share-Documents/Template/Analysis/`, `AI-Share-Documents/Template/BE/`, and `AI-Share-Documents/Template/FE/` back into `AI-Share-Documents/` to prepare for the next feature.

## Escalation Rule
If a role discovers ambiguity or contradiction:
- document it in the current phase output area
- do not silently guess
- do not modify an upstream artifact directly
