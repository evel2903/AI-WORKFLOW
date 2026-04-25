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

`sd-self-check.md` confirms System Design is ready. `sd-data-design.md` is present and implementation-usable because it defines account data, external identity data, constraints, migration direction, and TypeORM registration notes.

## Target Module
Reuse existing module: `src/Modules/Authentication`

Reuse existing account store: `src/Modules/Users`

Reason: the codebase already has `AuthenticationModule`, `UserModule`, `UserOrmEntity`, JWT token issuance, and role guard wiring. Creating a parallel `Auth` module or `Account` table would duplicate account concepts.

## Files To Create
- `src/Modules/Authentication/Application/DTOs/GoogleLoginDto.ts`
- `src/Modules/Authentication/Application/DTOs/VerifiedGoogleIdentityDto.ts`
- `src/Modules/Authentication/Application/UseCases/AuthenticateWithGoogleUseCase.ts`
- `src/Modules/Authentication/Domain/Interfaces/IGoogleOAuthClient.ts`
- `src/Modules/Authentication/Domain/Interfaces/IGoogleTokenVerifier.ts`
- `src/Modules/Authentication/Infrastructure/OAuth/GoogleOAuthClient.ts`
- `src/Modules/Authentication/Infrastructure/OAuth/GoogleTokenVerifier.ts`
- `src/Modules/Authentication/Presentation/Requests/GoogleCallbackQuery.ts`
- `src/Shared/Database/Migrations/1760000000000-AddGoogleOAuthAccountFields.ts`
- `src/Common/Constants/AccountStatus.ts`

## Files To Update
- `src/Common/Constants/Role.ts`
- `src/Common/Constants/ErrorCode.ts`
- `src/Common/Exceptions/AppException.ts`
- `src/Modules/Users/Domain/Entities/UserEntity.ts`
- `src/Modules/Users/Infrastructure/Persistence/Entities/UserOrmEntity.ts`
- `src/Modules/Users/Infrastructure/Mappers/UserOrmMapper.ts`
- `src/Modules/Users/Domain/Interfaces/IUserRepository.ts`
- `src/Modules/Users/Infrastructure/Persistence/Repositories/UserRepository.ts`
- `src/Modules/Users/Application/DTOs/UserDto.ts`
- `src/Modules/Users/Application/Mappers/UserDtoMapper.ts`
- `src/Modules/Users/Application/UseCases/CreateUserUseCase.ts`
- `src/Modules/Users/Application/UseCases/UpdateUserUseCase.ts`
- `src/Modules/Users/Presentation/Controllers/UserController.ts`
- `src/Modules/Authentication/Application/DTOs/AuthResultDto.ts`
- `src/Modules/Authentication/Application/UseCases/LoginUseCase.ts`
- `src/Modules/Authentication/Application/UseCases/RegisterUseCase.ts`
- `src/Modules/Authentication/AuthenticationModule.ts`
- `src/Modules/Authentication/Presentation/Controllers/AuthController.ts`
- `src/Modules/Authentication/Infrastructure/Jwt/JwtStrategy.ts`
- `src/Shared/Config/Env/Env.ts`
- `src/Shared/Config/AppConfig.ts`
- `src/Shared/Database/TypeOrmDataSource.ts`
- `src/Shared/Database/Seed/Seed.ts`
- `.env.example` is out of bounds for Coding Backend, so it will not be changed.

## Implementation Order
1. Update role constants to only `Admin` and `Photographer`.
2. Extend `UserEntity` and `UserOrmEntity` to serve as the approved account model with status and Google identity fields.
3. Update user mapper, DTO, repository, and use cases to preserve immutable roles and support Google identity lookup/creation.
4. Add Google OAuth callback DTOs, OAuth client interface, verifier interface, OAuth client adapter, verifier adapter, and callback query DTO.
5. Add `AuthenticateWithGoogleUseCase` implementing callback ID-token authentication, first-login Photographer creation, existing login without role mutation, disabled-account denial, and server-side token issuance.
6. Update `AuthenticationModule` wiring for Google callback login and Admin-only password login.
7. Update `AuthController` to expose `GET /auth/google` and `GET /auth/google/callback`, keep `GET /auth/me`, and reject legacy `/auth/login` and `/auth/register` without authenticating or creating accounts.
8. Protect `UserController` with JWT + Admin role guards so manual Admin account provisioning is not public.
9. Update JWT strategy and seed to align with Admin/Photographer roles and active account status.
10. Add a TypeORM migration for new account fields and role/status constraints.
11. Run build/lint where feasible.
12. Write `coding-change-log.md` and `coding-self-check.md`.

## Key Constraints
- Google callback login must never create Admin accounts.
- First Google login creates only `Photographer`.
- Existing login must never mutate stored role.
- Disabled accounts must not receive tokens.
- Client role or identity input must not be trusted.
- Email/password auth may authenticate existing `Admin` accounts only.
- Email/password auth must not authenticate `Photographer` accounts.
- Email/password auth must not create accounts.
- Customer login/account creation is out of scope.
- Public Admin registration is out of scope.

## Out Of Scope
- Tests under `test/`.
- New Customer account flows.
- Email/password authentication for non-Admin accounts.
- Email/password account creation.
- Role upgrade or downgrade.
- Full authorization policy implementation beyond server-side role/account token claims.
