# AI-WORKFLOW

AI-WORKFLOW is a file-based workflow workspace for coordinating Analysis, Backend, and Frontend AI roles. The root control document is `AGENTS.md`; reusable first-prompt rules live in `RULE.md`; role instructions live under `SKILLS/`; active work artifacts live under `AI-Share-Documents/`.

## Workspace Layout

```text
AI-WORKFLOW/
  AGENTS.md
  RULE.md
  PROMPT.EXAMPLE.md
  SKILLS/
    Analysis/
    BE/
    FE/
  AI-Share-Documents/
    Analysis/
    BE/
    FE/
    Archive/
    Template/
  BE/
    <Project name>/
  FE/
    <Project name>/
```

## Source Of Truth

- Analysis artifacts are written to `AI-Share-Documents/Analysis/`.
- Backend coding, testing, and handoff artifacts are written to `AI-Share-Documents/BE/`.
- Frontend coding, testing, and handoff artifacts are written to `AI-Share-Documents/FE/`.
- Templates are kept in `AI-Share-Documents/Template/`.
- Completed feature archives go to `AI-Share-Documents/Archive/<Feature_name>/`.
- FE roles may search `AI-Share-Documents/Archive/**/BE/**` for matching archived backend documents as optional read-only reference material.

Do not use kit-local `ai-docs/` or `docs/` folders for workflow artifacts.

## Feature Scope

Each feature must declare one implementation scope:

- `BE_ONLY`: backend code and backend tests only.
- `FE_ONLY`: frontend code and frontend tests only.
- `FULL_STACK`: backend and frontend work.
- `ANALYSIS_ONLY`: planning/design/docs only; no production code or tests.

Backend artifacts block frontend work only when System Design says:

```text
Backend handoff required for FE: Yes
```

If a BE or FE role is invoked when that surface is out of scope, it writes `Status: NOT_IN_SCOPE` to its self-check artifact and stops.

For `FE_ONLY`, archived BE documents can be used to understand existing backend contracts for the same feature or integration. They are not a required gate and must not override the current Analysis/System Design outputs.

## Workflow Order

Analysis is always strict:

```text
write-prompt -> business-analyst -> team-lead -> system-design
```

Implementation depends on scope:

```text
BE_ONLY       -> coding-be -> testing-be -> workflow-archiver
FE_ONLY       -> coding-fe -> testing-fe -> workflow-archiver
FULL_STACK    -> coding-be -> testing-be -> share-documents -> coding-fe -> testing-fe -> workflow-archiver
ANALYSIS_ONLY -> workflow-archiver
```

## Role Skills

- `write-prompt`: `SKILLS/Analysis/write-prompt/`
- `business-analyst`: `SKILLS/Analysis/business-analyst/`
- `team-lead`: `SKILLS/Analysis/team-lead/`
- `system-design`: `SKILLS/Analysis/system-design/`
- `coding-be`: `SKILLS/BE/coding-be/`
- `testing-be`: `SKILLS/BE/testing-be/`
- `coding-fe`: `SKILLS/FE/coding-fe/`
- `testing-fe`: `SKILLS/FE/testing-fe/`

Each role has its own `SYSTEM_PROMPT.md`, `INPUTS_OUTPUTS.md`, and `CHECKLIST.md`.

## Required Gates

- Team Lead reads `AI-Share-Documents/Analysis/01-business-analysis/ba-open-questions.md` first.
- Blocking open questions stop Team Lead planning.
- System Design records implementation scope and backend handoff requirements.
- Coding roles read `sd-self-check.md` and `sd-data-design.md` before implementation when their surface is in scope.
- Coding roles create `coding-plan.md` before source changes.
- Testing roles read `coding-self-check.md` and `coding-plan.md` before writing tests.

## Archive And Reset

After the last in-scope testing phase completes, or after System Design for `ANALYSIS_ONLY`, `workflow-archiver` must:

1. Pack `AI-Share-Documents/Analysis/`, `AI-Share-Documents/BE/`, and `AI-Share-Documents/FE/` into `AI-Share-Documents/Archive/<Feature_name>/`.
2. Write `AI-Share-Documents/Archive/<Feature_name>/Feature_Report.md`.
3. Copy `AI-Share-Documents/Template/Analysis/`, `AI-Share-Documents/Template/BE/`, and `AI-Share-Documents/Template/FE/` back into `AI-Share-Documents/`.

## Quick Start

1. Open `PROMPT.EXAMPLE.md`.
2. Adapt the project name, feature name, implementation scope, and source paths.
3. Keep `RULE.md` in the prompt's `Follow` list.
4. Run the generated workflow from the workspace root.
5. Keep all phase outputs in `AI-Share-Documents/`.

For detailed routing and boundary rules, read `AGENTS.md`.
