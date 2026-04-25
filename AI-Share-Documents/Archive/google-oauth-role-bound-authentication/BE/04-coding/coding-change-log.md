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

### CD-001: Authenticated Role Set Restricted
Maps to: `SD-001`, `SD-002`, `SD-013`, `TL-001`, `FR-001`, `AC-001`

Changed:
- `src/Common/Constants/Role.ts`

Summary:
- Replaced legacy `User` role with `Photographer`.
- Authenticated role enum now contains only `Admin` and `Photographer`.

### CD-002: Account Status And Auth Error Support
Maps to: `SD-007`, `SD-014`, `SD-025`, `SD-044`, `TL-008`, `FR-015`, `NFR-006`, `AC-011`

Changed:
- `src/Common/Constants/AccountStatus.ts`
- `src/Common/Constants/ErrorCode.ts`
- `src/Common/Exceptions/AppException.ts`

Summary:
- Added `Active` and `Disabled` account status values.
- Added auth-specific exceptions and error codes for invalid Google token, disabled account, unsupported auth flow, and untrusted client auth data.

### CD-003: Existing User Store Adapted As Account Store
Maps to: `SD-011`, `SD-012`, `SD-032`, `SD-033`, `SD-035`, `SD-036`, `TL-009`

Changed:
- `src/Modules/Users/Domain/Entities/UserEntity.ts`
- `src/Modules/Users/Domain/Interfaces/IUserRepository.ts`
- `src/Modules/Users/Infrastructure/Persistence/Entities/UserOrmEntity.ts`
- `src/Modules/Users/Infrastructure/Persistence/Repositories/UserRepository.ts`
- `src/Modules/Users/Infrastructure/Mappers/UserOrmMapper.ts`
- `src/Modules/Users/Application/DTOs/UserDto.ts`
- `src/Modules/Users/Application/Mappers/UserDtoMapper.ts`

Summary:
- Added account status, Google subject, Google email verification, avatar URL, and disabled timestamp.
- Added unique Google subject lookup support.
- Preserved role as constructor-assigned and non-publicly mutable in domain usage.
- Added TypeORM check constraints for role and status.

### CD-004: Manual Admin Provisioning Boundary
Maps to: `SD-003`, `SD-017`, `SD-030`, `SD-037`, `TL-002`, `FR-002`, `FR-003`, `FR-004`, `FR-012`, `AC-002`, `AC-003`

Changed:
- `src/Modules/Users/Application/UseCases/CreateUserUseCase.ts`
- `src/Modules/Users/Presentation/Controllers/UserController.ts`

Summary:
- Admin-created users are assigned `Admin`.
- `UserController` is protected by JWT auth and Admin role guards so user creation is no longer public.
- No public Admin registration path was added.

### CD-005: Google OAuth Login Implementation
Maps to: `SD-004`, `SD-005`, `SD-008`, `SD-016`, `SD-021`, `SD-023`, `TL-003`, `TL-004`, `TL-007`, `FR-005`, `FR-006`, `FR-007`, `FR-008`, `NFR-001`, `NFR-003`, `NFR-004`, `AC-004`, `AC-005`, `AC-009`

Changed:
- `src/Modules/Authentication/Application/DTOs/GoogleLoginDto.ts`
- `src/Modules/Authentication/Application/DTOs/VerifiedGoogleIdentityDto.ts`
- `src/Modules/Authentication/Application/UseCases/AuthenticateWithGoogleUseCase.ts`
- `src/Modules/Authentication/Domain/Interfaces/IGoogleOAuthClient.ts`
- `src/Modules/Authentication/Domain/Interfaces/IGoogleTokenVerifier.ts`
- `src/Modules/Authentication/Infrastructure/OAuth/GoogleOAuthClient.ts`
- `src/Modules/Authentication/Infrastructure/OAuth/GoogleTokenVerifier.ts`
- `src/Modules/Authentication/Presentation/Requests/GoogleCallbackQuery.ts`
- `src/Modules/Authentication/AuthenticationModule.ts`
- `src/Modules/Authentication/Presentation/Controllers/AuthController.ts`

Summary:
- Added `GET /auth/google` redirect initiation and `GET /auth/google/callback`.
- Updated `GET /auth/google` to return `AuthorizationUrl` and `CallbackUrl` instead of issuing a 302 redirect.
- Backend signs and validates OAuth `state`.
- Backend exchanges callback `code` for a Google ID token.
- Backend validates Google ID tokens through a server-side verifier after callback exchange.
- First verified Google login creates a `Photographer` account.
- Subsequent Google login authenticates the existing Google-linked account without role mutation.
- Email collisions with different or missing Google subject are rejected instead of trusted.
- Disabled accounts are denied before token issuance.

### CD-006: Admin Password Login And Public Registration Boundary
Maps to: `SD-009`, `SD-018`, `SD-027`, `SD-028`, `SD-045`, `TL-006`, `FR-011`, `FR-012`, `FR-013`, `FR-014`, `NFR-007`, `AC-007`, `AC-008`, `AC-013`

Changed:
- `src/Modules/Authentication/Application/UseCases/LoginUseCase.ts`
- `src/Modules/Authentication/Application/UseCases/RegisterUseCase.ts`
- `src/Modules/Authentication/Presentation/Controllers/AuthController.ts`

Summary:
- `/auth/login` authenticates existing Admin accounts only.
- Photographer accounts remain Google OAuth-only and cannot use password login.
- Disabled Admin accounts are denied before token issuance.
- Legacy `/auth/register` remains a rejecting endpoint and does not create accounts.
- No public registration flow is supported.

### CD-007: Authorization-Ready Auth Result
Maps to: `SD-010`, `SD-022`, `SD-031`, `SD-046`, `TL-010`, `FR-016`, `NFR-005`, `AC-012`

Changed:
- `src/Modules/Authentication/Application/DTOs/AuthResultDto.ts`
- `src/Modules/Authentication/Infrastructure/Jwt/JwtTokenService.ts`
- `src/Modules/Authentication/Infrastructure/Jwt/JwtStrategy.ts`
- `src/Modules/Authentication/Presentation/Controllers/AuthController.ts`

Summary:
- Auth result now returns server-side `Account` data with account ID, role, status, email, and display name.
- JWT token claims continue to be issued from persisted server-side role/account data.

### CD-008: Persistence And Configuration Wiring
Maps to: `SD-038`, `SD-039`, `SD-047`, `TL-009`

Changed:
- `src/Shared/Config/Env/Env.ts`
- `src/Shared/Config/AppConfig.ts`
- `src/App.module.ts`
- `src/Shared/Database/TypeOrmDataSource.ts`
- `src/Shared/Database/Seed/Seed.ts`
- `src/Shared/Database/Migrations/1760000000000-AddGoogleOAuthAccountFields.ts`

Summary:
- Added optional server-side Google client ID, client secret, and redirect URI config.
- Registered Google config in `AppModule`.
- Added migration for role default, account status, Google subject, Google email verification, avatar URL, disabled timestamp, unique Google subject index, and role/status check constraints.
- Updated seed Admin account to use `Admin` role and `Active` status.

## Verification
- `npm.cmd run build`: passed.
- `npm.cmd run lint`: passed.
- `npm.cmd test -- --runInBand`: failed because existing tests still assert old email/password login, public registration, public users controller behavior, and old `Role.User` expectations. These are now intentionally out of scope per the approved feature and must be updated by `testing-be`.
