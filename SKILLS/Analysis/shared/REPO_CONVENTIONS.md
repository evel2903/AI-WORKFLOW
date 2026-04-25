# Repository Conventions

## Workspace Paths
- Backend skills: `SKILLS/BE/`
- Frontend skills: `SKILLS/FE/`
- Backend source projects: `BE/<Project name>/`
- Frontend source projects: `FE/<Project name>/`
- Shared handoff documents: `AI-Share-Documents/`
- Final project documents: `AI-Share-Documents/Archive/<Feature_name>/`
- Archived backend reference documents: `AI-Share-Documents/Archive/**/BE/**`

## Backend Assumptions
- NestJS
- TypeScript
- Module-based Clean Architecture
- Feature modules usually live in `src/Modules/<FeatureName>`
- Layers: `Presentation`, `Application`, `Domain`, `Infrastructure`
- API responses may use the `Success`, `Data`, `Errors` envelope

## Frontend Assumptions
- Next.js
- React
- TypeScript
- Routes commonly live in `app/` or `src/app/`
- Shared UI commonly lives in `components/`
- Feature logic may live in `features/`, `lib/`, `hooks/`, or `providers/`
- Visual implementation must follow project design guidance when present.

## Documentation Rules
- Do not use `ai-docs/00-project/`.
- Analysis artifacts live in `AI-Share-Documents/Analysis/00-prompt-generation/` through `AI-Share-Documents/Analysis/03-system-design/`.
- BE and FE coding/testing artifacts live in `AI-Share-Documents/BE/04-coding-be/`, `BE/05-testing-be/`, `FE/04-coding-fe/`, and `FE/05-testing-fe/`.
- BE and FE agents read upstream Analysis artifacts from `AI-Share-Documents/Analysis`.
- FE agents may read matching archived BE documents from `AI-Share-Documents/Archive/**/BE/**` as optional read-only reference material.
- Final archival goes to `AI-Share-Documents/Archive/<Feature_name>/`, not kit-local `ai-docs/archive/`.
- Templates live in `AI-Share-Documents/Template/` and must not be edited during feature work.
