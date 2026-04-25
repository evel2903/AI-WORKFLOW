# Coding Backend Change Log

## Role Running
`coding-be`

## Input Files
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `ai-docs/03-system-design/sd-solution-overview.md`
- `ai-docs/03-system-design/sd-domain-design.md`
- `ai-docs/03-system-design/sd-api-contract.md`
- `ai-docs/03-system-design/sd-data-design.md`
- `ai-docs/03-system-design/sd-implementation-guidelines.md`
- `ai-docs/03-system-design/sd-self-check.md`
- existing code under `src/`

## Allowed Output Directories
- `src/`
- `ai-docs/04-coding/`

## Completion Artifacts
- `ai-docs/04-coding/coding-plan.md`
- `ai-docs/04-coding/coding-change-log.md`
- `ai-docs/04-coding/coding-self-check.md`
- production code changes under `src/`

## Code Changes

### CD-001: Photographer Credit Domain And Persistence Support
Maps to: `SD-001`, `SD-002`, `SD-003`, `SD-008`, `SD-026`, `SD-027`, `TL-001`, `TL-004`, `TL-006`, `FR-001`, `FR-002`, `FR-007`, `FR-008`, `FR-010`, `AC-001`, `AC-002`, `AC-007`, `AC-008`

Changed:
- `src/Modules/Users/Domain/Entities/UserEntity.ts`
- `src/Modules/Users/Infrastructure/Persistence/Entities/UserOrmEntity.ts`
- `src/Modules/Users/Infrastructure/Mappers/UserOrmMapper.ts`
- `src/Modules/Users/Application/DTOs/UserDto.ts`
- `src/Modules/Users/Application/Mappers/UserDtoMapper.ts`

Summary:
- Added nullable `Credit` to the existing user/account model.
- Enforced photographer-only credit ownership and non-negative integer validation in the domain entity.
- Added ORM-level nullable `Credit` column and check constraints for non-negative values and non-photographer nullability.
- Updated DTO mapping so photographer users can expose `Credit` while non-photographer payloads omit it.

### CD-002: First-Login Photographer Credit Initialization
Maps to: `SD-004`, `SD-005`, `SD-014`, `SD-021`, `SD-028`, `TL-002`, `FR-005`, `FR-006`, `NFR-003`, `AC-005`, `AC-006`

Changed:
- `src/Modules/Authentication/Application/UseCases/AuthenticateWithGoogleUseCase.ts`
- `src/Modules/Authentication/Application/DTOs/AuthResultDto.ts`
- `src/Modules/Authentication/Application/UseCases/LoginUseCase.ts`

Summary:
- New photographer accounts created through Google login now persist with `Credit = 10`.
- Existing photographers with `Credit = null` are initialized to `10` on successful login.
- Later logins preserve any existing stored credit value.
- Auth result payloads now support optional `Credit` when account data is already returned.

### CD-003: Admin-Only Photographer Credit Update Path
Maps to: `SD-006`, `SD-007`, `SD-015`, `SD-020`, `SD-022`, `SD-023`, `SD-024`, `SD-025`, `TL-003`, `TL-004`, `TL-007`, `FR-003`, `FR-004`, `FR-007`, `NFR-002`, `NFR-004`, `AC-003`, `AC-004`, `AC-007`

Created:
- `src/Modules/Users/Application/DTOs/UpdateUserCreditDto.ts`
- `src/Modules/Users/Application/UseCases/UpdateUserCreditUseCase.ts`
- `src/Modules/Users/Presentation/Requests/UpdateUserCreditRequest.ts`

Changed:
- `src/Modules/Users/Presentation/Controllers/UserController.ts`
- `src/Modules/Users/UserModule.ts`

Summary:
- Added admin-only `PATCH /users/:id/credit`.
- Request validation now enforces a non-negative whole-number `Credit`.
- Application use case revalidates caller role and blocks non-photographer targets.
- User module wiring was extended to register the new use case without changing unrelated user-management behavior.

### CD-004: Migration And Runtime Wiring For Photographer Credit
Maps to: `SD-026`, `SD-027`, `SD-028`, `SD-030`, `SD-031`, `SD-032`, `SD-033`, `TL-006`, `TL-009`, `TL-010`, `FR-008`, `NFR-006`, `AC-008`

Created:
- `src/Shared/Database/Migrations/1761000000000-AddPhotographerCredit.ts`

Changed:
- `src/Shared/Database/TypeOrmDataSource.ts`
- `src/Shared/Database/Seed/Seed.ts`

Summary:
- Added migration to extend the existing `users` table with nullable `Credit`.
- Registered the migration in `TypeOrmDataSource`.
- Preserved the approved legacy behavior by not bulk backfilling `10` in the migration.
- Kept seeded admin users explicitly credit-less with `Credit = null`.

## Verification
- `npm.cmd run build`: passed.
- `npm.cmd run lint`: passed.
- `npm.cmd test -- --runInBand`: not run in this phase.
