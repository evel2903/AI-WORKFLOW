# Coding Backend Plan

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

## Preflight Result
Status: PASSED

`ai-docs/03-system-design/sd-self-check.md` was read first and indicates readiness for Coding Backend. `ai-docs/03-system-design/sd-data-design.md` is present and implementation-usable for this feature.

## Target Modules
- `src/Modules/Users`
- `src/Modules/Authentication`
- `src/Shared/Database`

## Existing Structures Reused
- Existing persisted `users` table and `UserOrmEntity`
- Existing `UserEntity`, `UserRepository`, and `UserModule`
- Existing successful Google authentication flow in `AuthenticateWithGoogleUseCase`
- Existing admin-guarded `UserController`

## Files To Update
- `src/Modules/Users/Domain/Entities/UserEntity.ts`
- `src/Modules/Users/Domain/Interfaces/IUserRepository.ts`
- `src/Modules/Users/Infrastructure/Persistence/Entities/UserOrmEntity.ts`
- `src/Modules/Users/Infrastructure/Mappers/UserOrmMapper.ts`
- `src/Modules/Users/Infrastructure/Persistence/Repositories/UserRepository.ts`
- `src/Modules/Users/Application/DTOs/UserDto.ts`
- `src/Modules/Users/Application/Mappers/UserDtoMapper.ts`
- `src/Modules/Users/Application/UseCases/UpdateUserUseCase.ts`
- `src/Modules/Users/Presentation/Controllers/UserController.ts`
- `src/Modules/Authentication/Application/UseCases/AuthenticateWithGoogleUseCase.ts`
- `src/Modules/Authentication/Application/UseCases/LoginUseCase.ts`
- `src/Modules/Authentication/Application/DTOs/AuthResultDto.ts`
- `src/Modules/Users/UserModule.ts`
- `src/Shared/Database/TypeOrmDataSource.ts`

## Files To Create
- `src/Modules/Users/Application/DTOs/UpdateUserCreditDto.ts`
- `src/Modules/Users/Application/UseCases/UpdateUserCreditUseCase.ts`
- `src/Modules/Users/Presentation/Requests/UpdateUserCreditRequest.ts`
- `src/Shared/Database/Migrations/1761000000000-AddPhotographerCredit.ts`

## Implementation Order
1. Extend domain and persistence model with nullable `Credit`.
2. Add mapper, repository, and DTO updates so photographer credit can be persisted and projected while omitted for non-photographers.
3. Implement one-time `Credit = 10` initialization inside successful Google login.
4. Implement admin-only `PATCH /users/:id/credit` flow with target-role and numeric validation.
5. Extend auth result contract so photographer login can return `Credit` when account data is already present.
6. Register new use case and migration.
7. Run build and lint verification.

## Constraints
- Implement only the approved photographer-credit scope.
- Preserve existing role and authorization boundaries.
- Do not add any credit ledger, spending, recharge, expiration, or automatic adjustment logic.
- Keep `Credit` non-usable for non-photographer roles by storing `null` and omitting it from DTO projections.
- Do not write tests in this phase.

## Legacy Handling
- Follow approved design: existing photographers with `Credit = null` will initialize to `10` on first successful login after rollout.
- Do not bulk backfill `10` in the migration.

## Out Of Scope
- Any self-service credit update path
- Any non-admin credit management
- Any credit history or transaction model
- Any unrelated user-management redesign
