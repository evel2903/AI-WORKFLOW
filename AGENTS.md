# Agent Manager

This file is the root-level control document for the AI workflow workspace.
Use it to route work to the correct role skill, enforce phase order, and keep shared documents in one place.

## Workspace Layout
- Reusable first-prompt rules: `RULE.md`
- Shared skills: `SKILLS/Analysis/`, `SKILLS/BE/`, `SKILLS/FE/`
- Active Analysis documents: `AI-Share-Documents/Analysis/`
- Active Backend documents: `AI-Share-Documents/BE/`
- Active Frontend documents: `AI-Share-Documents/FE/`
- Document templates: `AI-Share-Documents/Template/`
- Feature archive: `AI-Share-Documents/Archive/<Feature_name>/`
- Backend source projects: `BE/<Project name>/`
- Frontend source projects: `FE/<Project name>/`

## Source Of Truth
- Analysis outputs are written to `AI-Share-Documents/Analysis/`.
- BE coding, testing, and handoff outputs are written to `AI-Share-Documents/BE/`.
- FE coding, testing, and handoff outputs are written to `AI-Share-Documents/FE/`.
- Templates live under `AI-Share-Documents/Template/`; do not write feature artifacts there during normal phase work.
- BE and FE agents read Analysis documents directly from `AI-Share-Documents/Analysis/`.
- `AI-Share-Documents/Archive/` is the final historical record for completed features.
- FE agents may search `AI-Share-Documents/Archive/**/BE/**` for matching archived backend documents as optional read-only reference material. Archived BE documents never override current Analysis/System Design artifacts and never block `FE_ONLY` work when missing.

## Scope Model
Every feature must declare one implementation scope in Analysis artifacts:
- `BE_ONLY`: backend code and backend tests only.
- `FE_ONLY`: frontend code and frontend tests only.
- `FULL_STACK`: backend and frontend work; backend testing and BE handoff gate frontend coding only when frontend depends on backend changes.
- `ANALYSIS_ONLY`: documentation, design, or planning only; no production code or tests.

Team Lead and System Design must record:
- `Implementation Scope: BE_ONLY | FE_ONLY | FULL_STACK | ANALYSIS_ONLY`
- `Backend in scope: Yes | No`
- `Frontend in scope: Yes | No`
- `Backend handoff required for FE: Yes | No | N/A`

Missing BE or FE artifacts are blocking only when that surface is in scope. If an out-of-scope role is invoked, it must write `Status: NOT_IN_SCOPE` to its self-check artifact and stop without modifying source or tests.

## Active Folder Contract
Each active feature uses this shared layout:

```text
AI-Share-Documents/
  Analysis/
    00-prompt-generation/
    01-business-analysis/
    02-team-lead/
    03-system-design/
  BE/
    04-coding-be/
    05-testing-be/
  FE/
    04-coding-fe/
    05-testing-fe/
  Archive/
    <Feature_name>/
      Analysis/
      BE/
      FE/
      Feature_Report.md
  Template/
    Analysis/
    BE/
    FE/
```

## Agent Routing
- `write-prompt`: run from the workspace root; use `SKILLS/Analysis/write-prompt/`.
- `business-analyst`: run from the workspace root; use `SKILLS/Analysis/business-analyst/`.
- `team-lead`: run from the workspace root; use `SKILLS/Analysis/team-lead/`.
- `system-design`: run from the workspace root; use `SKILLS/Analysis/system-design/`.
- `coding-be`: run from the workspace root; modify `BE/<Project name>/src/` and `AI-Share-Documents/BE/04-coding-be/`.
- `testing-be`: run from the workspace root; modify `BE/<Project name>/test/`, `AI-Share-Documents/BE/05-testing-be/`, and BE handoff docs.
- `coding-fe`: run from the workspace root; modify approved frontend files under `FE/<Project name>/` and `AI-Share-Documents/FE/04-coding-fe/`.
- `testing-fe`: run from the workspace root; modify approved frontend test files under `FE/<Project name>/`, `AI-Share-Documents/FE/05-testing-fe/`, and optional FE handoff docs.
- `workflow-archiver`: run only after the last in-scope testing phase is complete, or after System Design for `ANALYSIS_ONLY`.

## Workflow Order
Analysis order is always strict:

```text
write-prompt -> business-analyst -> team-lead -> system-design
```

Implementation order is conditional:
- `BE_ONLY`: `coding-be -> testing-be -> workflow-archiver`
- `FE_ONLY`: `coding-fe -> testing-fe -> workflow-archiver`
- `FULL_STACK`: `coding-be -> testing-be -> share-documents -> coding-fe -> testing-fe -> workflow-archiver`
- `ANALYSIS_ONLY`: `workflow-archiver`

Frontend coding must not start until Analysis artifacts exist under `AI-Share-Documents/Analysis/`.
Backend testing and BE handoff gate frontend coding only when `Backend handoff required for FE: Yes`.
For `FE_ONLY`, FE agents may read matching archived BE documents under `AI-Share-Documents/Archive/**/BE/**` to understand existing backend contracts, but this is not a required gate.

## Hard Boundaries
- Analysis agents must not write production code or tests.
- Analysis agents may write `AI-Share-Documents/Analysis/00-prompt-generation/` through `AI-Share-Documents/Analysis/03-system-design/`.
- BE agents must not modify FE source files.
- FE agents must not modify BE source files.
- BE and FE agents must not edit Analysis artifacts.
- Agents must not edit `AI-Share-Documents/Template/` during normal phase work.
- Do not use `ai-docs/00-project/`.
- Do not use kit-local `docs/` or `ai-docs/` for workflow artifacts.

## Required Gates
- Team Lead must inspect `AI-Share-Documents/Analysis/01-business-analysis/ba-open-questions.md` first and stop only for blocking open questions: `Blocking: Yes`, `Impact: HIGH`, `Impact: UNKNOWN`, or questions that prevent selecting implementation scope.
- System Design must record implementation scope and whether backend handoff is required for frontend.
- Coding BE runs only when `Backend in scope: Yes`; otherwise it writes `Status: NOT_IN_SCOPE` to `AI-Share-Documents/BE/04-coding-be/coding-self-check.md`.
- Coding FE runs only when `Frontend in scope: Yes`; otherwise it writes `Status: NOT_IN_SCOPE` to `AI-Share-Documents/FE/04-coding-fe/coding-self-check.md`.
- Coding BE and Coding FE must read `AI-Share-Documents/Analysis/03-system-design/sd-self-check.md` first when their surface is in scope.
- Coding BE and Coding FE must read `AI-Share-Documents/Analysis/03-system-design/sd-data-design.md` before any other design artifact when their surface is in scope.
- Coding BE must create `AI-Share-Documents/BE/04-coding-be/coding-plan.md` before any backend source change when backend is in scope.
- Coding FE must create `AI-Share-Documents/FE/04-coding-fe/coding-plan.md` before any frontend source change when frontend is in scope.
- Testing BE and Testing FE must read their surface's `coding-self-check.md` and `coding-plan.md` before writing tests when that surface is in scope.

## Archive Rule
After the last in-scope testing phase completes, or after System Design for `ANALYSIS_ONLY`, `workflow-archiver` must:

1. Pack `AI-Share-Documents/Analysis/`, `AI-Share-Documents/BE/`, and `AI-Share-Documents/FE/` into `AI-Share-Documents/Archive/<Feature_name>/`.
2. Write `AI-Share-Documents/Archive/<Feature_name>/Feature_Report.md` summarizing the feature and completed work.
3. Copy `AI-Share-Documents/Template/Analysis/`, `AI-Share-Documents/Template/BE/`, and `AI-Share-Documents/Template/FE/` back into `AI-Share-Documents/` to prepare for the next feature.

## Task Start Checklist
Every task must state:
1. Role running.
2. Project name.
3. Feature name.
4. Input files or folders.
5. Allowed output directories.
6. Completion artifacts.
