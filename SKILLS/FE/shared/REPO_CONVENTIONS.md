# Repository Conventions

## Stack
- Next.js
- React
- TypeScript
- Repository-configured frontend test stack (Jest, Vitest, Playwright, or equivalent)

## Workspace Paths
- Frontend skills: `SKILLS/FE/`
- Frontend source project: `FE/<Project name>/`
- Workflow artifacts: `AI-Share-Documents/FE/`
- Shared Analysis artifacts: `AI-Share-Documents/Analysis/`
- Backend handoff documents: `AI-Share-Documents/BE/`
- Archived backend reference documents: `AI-Share-Documents/Archive/**/BE/**`
- AI document templates: `AI-Share-Documents/Template/FE/`
- Visual design guidance: project `DESIGN.md` or approved design artifacts when present

## Architecture
Frontend code should keep route, component, state, and integration responsibilities explicit.
Frontend visual implementation must adhere to project design guidance when present.

## Paths
- Frontend routes: `FE/<Project name>/app/` or `FE/<Project name>/src/app/`
- Shared UI: `FE/<Project name>/components/`
- Feature logic: `FE/<Project name>/features/`, `FE/<Project name>/lib/`, `FE/<Project name>/hooks/`, `FE/<Project name>/providers/`
- Public assets: `FE/<Project name>/public/`
- Tests: `FE/<Project name>/test/`, `FE/<Project name>/tests/`, `FE/<Project name>/e2e/`, or colocated `__tests__/`

## Naming
- PascalCase for React components and major UI modules.
- Hooks use the `use` prefix.
- Utilities and helpers use camelCase.
- Feature names use PascalCase unless the existing repo already uses another convention consistently.

## API Behavior
- When backend APIs use the standard envelope, expect:
  - `Success: true`
  - `Data: ...`
- Error envelope:
  - `Success: false`
  - `Errors: [...]`

## Implementation Notes
- Keep server and client component boundaries explicit.
- Add `'use client'` only when required.
- Preserve route and layout hierarchy.
- Treat project design guidance as the source of truth for visual styling, component look-and-feel, color, type, spacing, and interaction presentation when present.
- Update `middleware.ts`, `next.config.*`, and tooling config only when required by the approved design.
