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

`ai-docs/03-system-design/sd-self-check.md` was reviewed first. `ai-docs/03-system-design/sd-data-design.md` is present and implementation-usable for this feature.

## Target Modules
- `src/Modules/Users`
- `src/Shared/Database`

## Files To Create
- `src/Modules/Users/Domain/Entities/CreditAuditLogEntity.ts`
- `src/Modules/Users/Infrastructure/Persistence/Entities/CreditAuditLogOrmEntity.ts`
- `src/Modules/Users/Infrastructure/Mappers/CreditAuditLogOrmMapper.ts`
- `src/Shared/Database/Migrations/1762000000000-AddCreditAuditLogs.ts`

## Files To Update
- `src/Modules/Users/Application/DTOs/UpdateUserCreditDto.ts`
- `src/Modules/Users/Application/UseCases/UpdateUserCreditUseCase.ts`
- `src/Modules/Users/Domain/Interfaces/IUserRepository.ts`
- `src/Modules/Users/Infrastructure/Persistence/Repositories/UserRepository.ts`
- `src/Modules/Users/UserModule.ts`
- `src/Modules/Users/Presentation/Controllers/UserController.ts`
- `src/Shared/Database/TypeOrmDataSource.ts`

## Implementation Order
1. Add the audit-log domain and ORM models plus mapping.
2. Extend the user repository contract and implementation to persist credit updates and audit records in one transaction.
3. Update the credit update DTO/controller/use case to pass actor ID, load actor and target, short-circuit no-op updates, and create audit records only for changed values.
4. Register the new ORM entity in `UserModule`.
5. Add the migration and register it in `TypeOrmDataSource`.
6. Write coding artifacts and run build/lint verification.

## Constraints
- Implement only the approved audit-logging enhancement.
- Preserve the existing photographer-only credit rule.
- Preserve the existing admin-only credit update rule.
- Do not add any audit read API, general-purpose audit abstraction, or broader credit history behavior.
- Do not write tests in this phase.
- Keep production edits inside `src/` only.

## Transaction And Consistency Plan
- Preferred path from design will be used: save the user credit update and the new audit row in one TypeORM transaction.
- No-op updates will return current user state and will not touch persistence.

## Out Of Scope
- Credit spending, recharge, expiration, or automatic adjustment
- Non-admin credit management
- Historical audit backfill
- Audit browsing/listing/reporting APIs
- Generalized audit platform abstractions
