# Coding Backend Change Log

## Role Running
`coding-be`

## Code Changes

### CD-001: Added Dedicated Credit Audit Log Domain And Persistence Model
Maps to: `SD-003`, `SD-012`, `SD-037`, `SD-045`, `TL-003`, `FR-003`, `FR-004`, `FR-005`, `FR-006`, `FR-007`, `AC-003`, `AC-004`, `AC-005`, `AC-006`, `AC-007`

Created:
- `src/Modules/Users/Domain/Entities/CreditAuditLogEntity.ts`
- `src/Modules/Users/Infrastructure/Persistence/Entities/CreditAuditLogOrmEntity.ts`
- `src/Modules/Users/Infrastructure/Mappers/CreditAuditLogOrmMapper.ts`

Summary:
- Added an append-only domain entity for admin photographer-credit audit records.
- Added a dedicated ORM entity and mapper for the new `credit_audit_logs` persistence structure.
- Enforced required actor/target identities, non-negative credit values, and changed-value-only audit semantics.

### CD-002: Coordinated Credit Update And Audit Write In Repository Layer
Maps to: `SD-020`, `SD-047`, `TL-003`, `TL-006`, `NFR-004`, `AC-003`

Changed:
- `src/Modules/Users/Domain/Interfaces/IUserRepository.ts`
- `src/Modules/Users/Infrastructure/Persistence/Repositories/UserRepository.ts`

Summary:
- Extended the user repository contract with `UpdateWithCreditAuditLog`.
- Implemented a shared TypeORM transaction that saves the updated user and the audit log row together.

### CD-003: Extended Admin Credit Update Flow For Audit Logging And No-Op Suppression
Maps to: `SD-002`, `SD-013`, `SD-014`, `SD-016`, `SD-024`, `TL-002`, `TL-004`, `FR-002`, `FR-003`, `FR-008`, `FR-009`, `AC-002`, `AC-003`, `AC-008`, `AC-009`

Changed:
- `src/Modules/Users/Application/DTOs/UpdateUserCreditDto.ts`
- `src/Modules/Users/Application/UseCases/UpdateUserCreditUseCase.ts`
- `src/Modules/Users/Presentation/Controllers/UserController.ts`

Summary:
- Added `ActorUserId` to the credit update DTO and controller flow.
- Revalidated the acting admin from persisted user state, not just JWT role.
- Suppressed persistence and audit creation for no-op updates.
- Created audit records only for successful changed-value photographer credit updates.
- Preserved rejection for unauthorized actors and invalid targets.

### CD-004: Registered Audit Entity In Users Module
Maps to: `SD-001`, `SD-049`, `TL-006`

Changed:
- `src/Modules/Users/UserModule.ts`

Summary:
- Registered the new audit ORM entity in the Users module TypeORM feature wiring.

### CD-005: Added Audit Log Migration And DataSource Registration
Maps to: `SD-048`, `SD-049`, `TL-003`, `TL-006`, `FR-010`, `AC-010`

Created:
- `src/Shared/Database/Migrations/1762000000000-AddCreditAuditLogs.ts`

Changed:
- `src/Shared/Database/TypeOrmDataSource.ts`

Summary:
- Added a migration to create `credit_audit_logs` with required fields, indexes, and change-only check constraints.
- Registered the new entity and migration with the shared TypeORM data source.
