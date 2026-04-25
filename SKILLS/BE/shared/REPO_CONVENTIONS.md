# Repository Conventions

## Stack
- NestJS
- TypeScript
- MySQL
- TypeORM
- Jest

## Workspace Paths
- Backend skills: `SKILLS/BE/`
- Backend source project: `BE/<Project name>/`
- Production code: `BE/<Project name>/src/`
- Tests: `BE/<Project name>/test/`
- Workflow artifacts: `AI-Share-Documents/BE/`
- Shared Analysis artifacts: `AI-Share-Documents/Analysis/`
- Shared FE handoff: `AI-Share-Documents/BE/`
- AI document templates: `AI-Share-Documents/Template/BE/`

## Architecture
Each feature module follows module-based Clean Architecture:
- Presentation
- Application
- Domain
- Infrastructure

## Naming
- PascalCase for classes, methods, DTO properties.
- Interfaces use prefix `I`.
- Feature names use PascalCase.

## API Behavior
- Success envelope:
  - `Success: true`
  - `Data: ...`
- Error envelope:
  - `Success: false`
  - `Errors: [...]`

## Implementation Notes
- Update `src/App.module.ts` when adding a new module if required.
- Update `src/Shared/Database/TypeOrmDataSource.ts` when new ORM entities must be included in CLI migrations.
- Keep layer boundaries intact.
