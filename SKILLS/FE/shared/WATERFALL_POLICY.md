# Waterfall Policy

You are part of a strict cross-kit Waterfall workflow.

## Core Rules
- The Analysis kit owns prompt generation, business analysis, team planning, and system design.
- The BE kit owns backend coding, backend testing, and backend handoff sharing.
- This FE kit owns frontend coding, frontend testing, and final archive/reset behavior.
- FE agents read upstream Analysis artifacts from `AI-Share-Documents/Analysis/`.
- Backend implementation and testing gate frontend implementation only when backend work is in scope and System Design marks backend handoff as required for frontend.
- All agent-to-agent communication must happen through files in `AI-Share-Documents/`.
- Never rely on unstored chat context as the source of truth.
- If required upstream artifacts are missing, incomplete, contradictory, or empty, stop and report the dependency gap in your own output area.
- Do not overwrite upstream Analysis artifacts.
- Stay within your allowed read/write boundaries.
- Do not expand business scope unless a new upstream artifact explicitly changes it.
- Every phase must produce its required self-check artifact before it is considered complete.

## Scope Rule
FE roles run only when System Design records `Frontend in scope: Yes`.
If an FE role is invoked when frontend is out of scope, it must write `Status: NOT_IN_SCOPE` to its self-check artifact and stop without modifying source or tests.

## Workflow Order
Frontend implementation is conditional:
- `FE_ONLY`: `analysis -> coding-fe -> testing-fe -> workflow-archiver`
- `FULL_STACK`: `analysis -> coding-be -> testing-be -> share-documents -> coding-fe -> testing-fe -> workflow-archiver`

## Gating Rules
- `coding-fe` requires:
  - shared Analysis artifacts under `AI-Share-Documents/Analysis/`
  - completed backend testing and handoff docs under `AI-Share-Documents/BE/` only when `Backend handoff required for FE: Yes`
- `testing-fe` requires:
  - updated production code in approved frontend source locations under `FE/<Project name>/`
  - `AI-Share-Documents/FE/04-coding-fe/coding-plan.md`
  - `AI-Share-Documents/FE/04-coding-fe/coding-change-log.md`
  - `AI-Share-Documents/FE/04-coding-fe/coding-self-check.md`
- `workflow-archiver` requires:
  - completed BE testing artifacts in `AI-Share-Documents/BE/05-testing-be/` when backend is in scope
  - completed FE testing artifacts in `AI-Share-Documents/FE/05-testing-fe/` when frontend is in scope

## Archived BE Reference Rule
- Archived BE documents under `AI-Share-Documents/Archive/**/BE/**` are optional read-only references for FE roles and never a blocking gate.
- FE roles may use archived BE documents to understand existing backend contracts for a matching feature or integration, but current Analysis/System Design remains authoritative.

## System Design Data Contract Rule
- `sd-data-design.md` is a required artifact produced by the Analysis kit.
- Frontend coding must preflight `AI-Share-Documents/Analysis/03-system-design/sd-data-design.md` before writing any code.
- The file must contain enough detail for frontend state, data flow, and integration work.
- If `sd-data-design.md` is missing, empty, placeholder-only, or not implementation-usable, `coding-fe` must stop immediately.
- When blocked by missing or insufficient data design, `coding-fe` must not write production code and may write only a blocking note in `AI-Share-Documents/FE/04-coding-fe/coding-self-check.md`.

## Coding Preflight Rule
- `coding-fe` must read project design guidance before creating the coding plan or writing frontend code when such guidance exists.
- `coding-fe` must verify backend handoff documents exist in `AI-Share-Documents/BE/` only when System Design marks backend handoff as required for frontend.
- For `FE_ONLY`, `coding-fe` may search `AI-Share-Documents/Archive/**/BE/**` for matching backend contract documents. Missing archived documents must not block frontend work.

## Coding Plan Contract Rule
- `coding-fe` must create `AI-Share-Documents/FE/04-coding-fe/coding-plan.md` before any production code changes begin.
- `coding-plan.md` must identify target route or feature, inputs read including any project design guidance, files to create, files to update, implementation order, key constraints, backend handoff inputs, and explicit out-of-scope items.
- If `coding-plan.md` is missing, placeholder-only, or created after code changes start, the coding phase is incomplete.

## Testing Preflight Rule
- `testing-fe` must preflight `AI-Share-Documents/FE/04-coding-fe/coding-self-check.md` before any testing work.
- `testing-fe` must validate that `AI-Share-Documents/FE/04-coding-fe/coding-plan.md` exists and is usable before writing tests.
- If `coding-plan.md` is missing, empty, placeholder-only, or clearly inconsistent with the implemented change scope, `testing-fe` must stop immediately.
- When blocked by missing or insufficient coding plan, `testing-fe` must not write tests and may write only a blocking note in `AI-Share-Documents/FE/05-testing-fe/test-self-check.md`.

## Archive Rule
After the last in-scope testing phase completes, or after System Design for `ANALYSIS_ONLY`, `workflow-archiver` must pack `AI-Share-Documents/Analysis/`, `AI-Share-Documents/BE/`, and `AI-Share-Documents/FE/` into `AI-Share-Documents/Archive/<Feature_name>/`, write `Feature_Report.md` there, then copy `AI-Share-Documents/Template/Analysis/`, `AI-Share-Documents/Template/BE/`, and `AI-Share-Documents/Template/FE/` back into `AI-Share-Documents/`.

## Escalation Rule
If a role discovers ambiguity or contradiction:
- document it in the current phase output area
- do not silently guess
- do not modify an upstream artifact directly
