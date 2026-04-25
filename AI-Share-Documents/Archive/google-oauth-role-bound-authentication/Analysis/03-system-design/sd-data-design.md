# System Design Data Design

## Role Running
`system-design`

## Input Files
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `ai-docs/00-project/project-context.md`
- `ai-docs/00-project/constraints.md`
- `ai-docs/00-project/glossary.md`
- `ai-docs/02-team-lead/tl-delivery-plan.md`
- `ai-docs/02-team-lead/tl-task-breakdown.md`
- `ai-docs/02-team-lead/tl-risk-log.md`
- `ai-docs/02-team-lead/tl-handoff.md`
- `ai-docs/02-team-lead/tl-self-check.md`

## Allowed Output Directories
- `ai-docs/03-system-design/`

## Completion Artifacts
- `ai-docs/03-system-design/sd-solution-overview.md`
- `ai-docs/03-system-design/sd-domain-design.md`
- `ai-docs/03-system-design/sd-api-contract.md`
- `ai-docs/03-system-design/sd-data-design.md`
- `ai-docs/03-system-design/sd-implementation-guidelines.md`
- `ai-docs/03-system-design/sd-self-check.md`

## Data Design Case
New or changed data design is required unless Coding confirms equivalent existing account and external identity structures already exist.

If equivalent structures exist, Coding must map this contract onto existing tables/entities and document the reuse in `coding-plan.md`. The implementation must still satisfy all constraints in this file.

## Persistence Model

### Entity Candidate: Account
Design ID: `SD-032`

Maps to: `TL-001`, `TL-002`, `TL-003`, `TL-004`, `TL-005`, `TL-008`, `TL-009`, `FR-001` through `FR-010`, `FR-015`, `AC-001` through `AC-006`, `AC-011`

Purpose:
- Stores authenticated account data.
- Owns role and disabled state.
- Serves as authorization-ready account source.

Suggested TypeORM entity:
- `AccountOrmEntity`

Suggested table:
- `accounts`

Required fields:
- `Id`: UUID primary key.
- `Role`: enum or constrained string, allowed values `Admin`, `Photographer`, required, immutable after insert.
- `Status`: enum or constrained string, allowed values `Active`, `Disabled`, required, default `Active`.
- `Email`: nullable or required depending on existing user model; for Google-created Photographer accounts use verified Google email when available.
- `DisplayName`: nullable string.
- `AvatarUrl`: nullable string.
- `CreatedAt`: timestamp.
- `UpdatedAt`: timestamp.
- `DisabledAt`: nullable timestamp.

Constraints:
- `Role` must allow only `Admin` and `Photographer`.
- `Status` must allow only `Active` and `Disabled`.
- Application code must not expose role update behavior.
- Migration should add database check constraints for role/status where supported.
- If using databases without enum/check support, enforce in domain and application layers.

Role immutability:
- Primary enforcement is domain/application: account role is assigned in factory methods only and has no update method.
- Persistence enforcement should be added where practical:
  - avoid repository methods that update `Role`
  - avoid DTOs accepting `Role` except trusted internal manual Admin creation if implemented
  - optional database trigger or restricted update path may be added if the project uses such patterns

### Entity Candidate: AccountExternalIdentity
Design ID: `SD-033`

Maps to: `TL-003`, `TL-004`, `TL-007`, `TL-009`, `FR-005`, `FR-006`, `FR-007`, `NFR-001`, `NFR-003`, `NFR-004`, `AC-004`, `AC-005`, `AC-009`, `BAQ-003`

Purpose:
- Stores provider identity mapping for Google login account lookup.

Suggested TypeORM entity:
- `AccountExternalIdentityOrmEntity`

Suggested table:
- `account_external_identities`

Required fields:
- `Id`: UUID primary key.
- `AccountId`: UUID foreign key to `accounts.Id`, required.
- `Provider`: enum or constrained string, required. For this feature allowed provider value is `Google`.
- `ProviderSubject`: string, required. For Google this is the verified `sub` claim.
- `ProviderEmail`: string, nullable.
- `ProviderEmailVerified`: boolean, required default `false`.
- `CreatedAt`: timestamp.
- `UpdatedAt`: timestamp.

Relationships:
- Many external identity rows may belong to one account in future.
- This feature requires only Google identity support.
- `AccountExternalIdentity.AccountId` references `Account.Id`.

Constraints:
- Unique composite index on `(Provider, ProviderSubject)`.
- Optional non-unique index on `AccountId`.
- `ProviderSubject` must come from verified Google token claims only.
- Do not use client-submitted email as the primary account matching key.

### Optional Entity Candidate: AuthSession Or RefreshToken
Design ID: `SD-034`

Maps to: `TL-010`, `TL-011`, `FR-016`, `NFR-005`, `AC-012`, `BAQ-002`

Use only if the repository uses server-side sessions or refresh tokens.

If stateless JWT access tokens are already the project pattern, no table is required for access token issuance.

Required fields if implemented:
- `Id`: UUID primary key.
- `AccountId`: UUID foreign key to `accounts.Id`.
- `TokenHash` or `RefreshTokenHash`: string, required.
- `ExpiresAt`: timestamp, required.
- `RevokedAt`: nullable timestamp.
- `CreatedAt`: timestamp.

Constraints:
- Tokens must be derived from server-side account data.
- Tokens must not encode client-provided role.

## Account Creation Rules

### Photographer Creation Through Google
Design ID: `SD-035`

Maps to: `TL-003`, `FR-005`, `FR-006`, `AC-003`, `AC-004`

Transaction requirements:
1. Validate Google ID token.
2. Start transaction.
3. Lookup `AccountExternalIdentity` by `(Google, verifiedSubject)`.
4. If missing, create `Account` with `Role = Photographer`, `Status = Active`.
5. Create `AccountExternalIdentity` linked to the new account.
6. Commit transaction.

Concurrency:
- Unique `(Provider, ProviderSubject)` constraint prevents duplicate accounts under concurrent first logins.
- If unique constraint conflicts during create, reload the existing mapping and continue as subsequent login.

### Existing Account Login
Design ID: `SD-036`

Maps to: `TL-004`, `TL-005`, `FR-007`, `FR-008`, `FR-009`, `FR-010`, `AC-005`, `AC-006`, `BAQ-005`

Rules:
- Existing account role must be read from `Account.Role`.
- No login flow may update `Account.Role`.
- Disabled check happens after account load and before token/session issuance.
- Existing Admin with an internally linked Google identity may authenticate as Admin only because the persisted account is already Admin.

### Manual Admin Provisioning
Design ID: `SD-037`

Maps to: `TL-002`, `FR-002`, `FR-003`, `FR-004`, `FR-012`, `AC-002`, `BAQ-001`

Rules:
- No public Admin registration table flow or public endpoint is introduced.
- If internal Admin creation is implemented, it creates `Account.Role = Admin` from trusted operational context only.
- Google first-login flow must never set `Role = Admin`.
- If internal Admin accounts can be linked to Google identities, the linking must be internal/trusted and must not be client self-service in this phase.

## Migration Notes
Design ID: `SD-038`

Maps to: `TL-009`, `RISK-009`

Required migration direction if structures do not exist:
- Create `accounts` table.
- Create `account_external_identities` table.
- Add enum/check constraints for role and status.
- Add unique index on `account_external_identities(Provider, ProviderSubject)`.
- Add foreign key from `account_external_identities.AccountId` to `accounts.Id`.
- Add timestamps and nullable disabled timestamp.

If existing structures exist:
- Add missing columns or constraints required by this design.
- Add missing provider identity mapping or uniqueness rule.
- Add missing disabled-account representation.
- Add missing migration for role/status constraints if not already enforced.

## TypeOrmDataSource Notes
Design ID: `SD-039`

Maps to: `TL-009`

Coding must update `src/Shared/Database/TypeOrmDataSource.ts` when applicable so new or changed TypeORM entities and migrations are registered.

Coding must also update `src/App.module.ts` when applicable so the `Auth` module, entity repositories, or imported modules are wired into the Nest application.

## Data Integrity Requirements
- Do not persist any role value from Google token claims or client request body.
- Do not persist any account identity from unvalidated client input as a trusted identity.
- Persist Google subject only after successful backend token validation.
- Deny disabled accounts without issuing tokens.
- Preserve account role across all login attempts.
- Keep account role and account ID available for future authorization integration.
