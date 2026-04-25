# Waterfall Policy

You are part of a strict cross-kit Waterfall workflow.

## Core Rules
- The Analysis kit owns prompt generation, business analysis, team planning, and system design.
- This BE kit owns backend coding, backend testing, and backend handoff sharing.
- BE agents read upstream Analysis artifacts from `AI-Share-Documents/Analysis/`.
- Backend implementation and testing gate frontend implementation only when backend work is in scope and System Design marks backend handoff as required for frontend.
- All agent-to-agent communication must happen through files in `AI-Share-Documents/`.
- Never rely on unstored chat context as the source of truth.
- If required upstream artifacts are missing, incomplete, contradictory, or empty, stop and report the dependency gap in your own output area.
- Do not overwrite upstream Analysis artifacts.
- Stay within your allowed read/write boundaries.
- Do not expand business scope unless a new upstream artifact explicitly changes it.
- Every phase must produce its required self-check artifact before it is considered complete.

## Scope Rule
BE roles run only when System Design records `Backend in scope: Yes`.
If a BE role is invoked when backend is out of scope, it must write `Status: NOT_IN_SCOPE` to its self-check artifact and stop without modifying source or tests.

## Workflow Order
Backend implementation is conditional:
- `BE_ONLY`: `analysis -> coding-be -> testing-be -> workflow-archiver`
- `FULL_STACK`: `analysis -> coding-be -> testing-be -> share-documents -> coding-fe -> testing-fe -> workflow-archiver`

## Gating Rules
- `coding-be` requires:
  - `AI-Share-Documents/Analysis/03-system-design/sd-solution-overview.md`
  - `AI-Share-Documents/Analysis/03-system-design/sd-domain-design.md`
  - `AI-Share-Documents/Analysis/03-system-design/sd-api-contract.md`
  - `AI-Share-Documents/Analysis/03-system-design/sd-data-design.md`
  - `AI-Share-Documents/Analysis/03-system-design/sd-implementation-guidelines.md`
  - `AI-Share-Documents/Analysis/03-system-design/sd-self-check.md`
- `testing-be` requires:
  - updated production code in `BE/<Project name>/src/`
  - `AI-Share-Documents/BE/04-coding-be/coding-plan.md`
  - `AI-Share-Documents/BE/04-coding-be/coding-change-log.md`
  - `AI-Share-Documents/BE/04-coding-be/coding-self-check.md`
- `share-documents` requires:
  - `AI-Share-Documents/BE/05-testing-be/test-plan.md`
  - `AI-Share-Documents/BE/05-testing-be/test-results.md`
  - `AI-Share-Documents/BE/05-testing-be/test-gaps.md`
  - `AI-Share-Documents/BE/05-testing-be/test-self-check.md`
- `workflow-archiver` requires completed BE testing artifacts when backend is in scope and completed FE testing artifacts when frontend is in scope.

## System Design Data Contract Rule
- `sd-data-design.md` is a required artifact produced by the Analysis kit.
- Backend coding must preflight `AI-Share-Documents/Analysis/03-system-design/sd-data-design.md` before writing any code.
- If `sd-data-design.md` is missing, empty, placeholder-only, or not implementation-usable for backend work, `coding-be` must stop immediately.
- When blocked by missing or insufficient data design, `coding-be` must not write production code and may write only a blocking note in `AI-Share-Documents/BE/04-coding-be/coding-self-check.md`.

## Coding Plan Contract Rule
- `coding-be` must create `AI-Share-Documents/BE/04-coding-be/coding-plan.md` before any production code changes begin.
- `coding-plan.md` must identify target module, inputs read, files to create, files to update, implementation order, key constraints, and explicit out-of-scope items.
- If `coding-plan.md` is missing, placeholder-only, or created after code changes start, the coding phase is incomplete.

## Testing Preflight Rule
- `testing-be` must preflight `AI-Share-Documents/BE/04-coding-be/coding-self-check.md` before any testing work.
- `testing-be` must validate that `AI-Share-Documents/BE/04-coding-be/coding-plan.md` exists and is usable before writing tests.
- If `coding-plan.md` is missing, empty, placeholder-only, or clearly inconsistent with the implemented change scope, `testing-be` must stop immediately.
- When blocked by missing or insufficient coding plan, `testing-be` must not write tests and may write only a blocking note in `AI-Share-Documents/BE/05-testing-be/test-self-check.md`.

## Sharing Rule
After backend testing completes, copy backend handoff documents to `AI-Share-Documents/BE/` only when frontend work depends on backend changes.
When required, include API contracts, implementation notes, test results, known gaps, and FE integration constraints.

## Archive Rule
After the last in-scope testing phase completes, or after System Design for `ANALYSIS_ONLY`, `workflow-archiver` must pack `AI-Share-Documents/Analysis/`, `AI-Share-Documents/BE/`, and `AI-Share-Documents/FE/` into `AI-Share-Documents/Archive/<Feature_name>/`, write `Feature_Report.md` there, then copy `AI-Share-Documents/Template/Analysis/`, `AI-Share-Documents/Template/BE/`, and `AI-Share-Documents/Template/FE/` back into `AI-Share-Documents/`.

## Escalation Rule
If a role discovers ambiguity or contradiction:
- document it in the current phase output area
- do not silently guess
- do not modify an upstream artifact directly
