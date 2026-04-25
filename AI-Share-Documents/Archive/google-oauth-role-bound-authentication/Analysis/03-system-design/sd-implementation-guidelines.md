# System Design Implementation Guidelines

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

## Coding Preflight Requirements
Before writing production code, Coding Backend must:
- Read all six System Design artifacts.
- Validate that `sd-data-design.md` is implementation-usable.
- Create `ai-docs/04-coding/coding-plan.md` before modifying `src/`.
- Record any reuse of existing account/auth structures in `coding-plan.md`.

## Module Structure Guidance
Design ID: `SD-040`

Maps to: `TL-012`, `TL-013`

Preferred module if no equivalent exists:
- `src/Modules/Auth`

Preferred layer placement:
- `Presentation`: controller, request DTOs, response DTOs.
- `Application`: `AuthenticateWithGoogleUseCase`, auth result assembly, transaction orchestration.
- `Domain`: `Account`, `AccountRole`, `AccountStatus`, `VerifiedGoogleIdentity`, domain errors, repository interfaces.
- `Infrastructure`: TypeORM entities/repositories, Google token verifier adapter, token/session issuer adapter.

Naming conventions:
- PascalCase classes, methods, and DTO properties.
- Interface names prefixed with `I`.
- Response envelope uses `Success`, `Data`, `Errors`.

## Implementation Sequence
Design ID: `SD-041`

Maps to: `TL-012`, `TL-013`, `SD-001` through `SD-039`

Recommended order:
1. Inspect existing auth/account/user modules and decide reuse versus new `Auth` module.
2. Create or adapt domain role/status/account models.
3. Create or adapt persistence entities and migrations for accounts and external identities.
4. Register entities and migrations in `TypeOrmDataSource` when applicable.
5. Implement Google token verifier adapter.
6. Implement account lookup/create transaction.
7. Implement immutable role enforcement by avoiding role update methods and rejecting role mutation inputs.
8. Implement disabled-account denial before auth token/session issuance.
9. Implement auth result issuance from server-side account data.
10. Implement Google login endpoint and unsupported-flow behavior.
11. Update `App.module.ts` module wiring when applicable.
12. Run build/lint/unit checks available to Coding Backend and document results.

## Security Guardrails
Design ID: `SD-042`

Maps to: `TL-007`, `NFR-001`, `NFR-002`, `NFR-003`, `NFR-004`, `AC-009`, `AC-010`

Required:
- Validate Google token audience, issuer, expiry, and signature using a trusted backend verifier.
- Use verified Google `sub` as the primary provider identity key.
- Do not accept `Role` in public Google login request DTO.
- Do not accept `AccountId` or provider subject from client as trusted identity data.
- Do not derive role from Google token claims.
- Do not issue tokens for disabled accounts.
- Do not log raw Google ID tokens.
- Do not expose internal disabled or account-existence details beyond approved API errors.

## Role Immutability Guardrails
Design ID: `SD-043`

Maps to: `TL-005`, `FR-009`, `FR-010`, `AC-006`

Required:
- Account role must be set only by creation factory methods.
- `CreatePhotographerFromGoogle` always sets role `Photographer`.
- Internal manual Admin creation, if implemented, is the only allowed creation path for role `Admin`.
- No public DTO accepts role mutation.
- No repository method updates role after insert.
- Existing-account login must never call role update logic.

## Disabled Account Guardrails
Design ID: `SD-044`

Maps to: `TL-008`, `FR-015`, `NFR-006`, `AC-011`

Required:
- Load account after validated provider identity lookup.
- Check account status before token/session issuance.
- Return `AUTH_ACCOUNT_DISABLED` with `Success: false`.
- Do not create a replacement account for a disabled account with the same provider identity.

## Unsupported Flow Guardrails
Design ID: `SD-045`

Maps to: `TL-006`, `FR-011`, `FR-012`, `FR-013`, `FR-014`, `NFR-007`, `AC-007`, `AC-008`, `AC-013`

Required:
- Do not add email/password login.
- Do not add public registration.
- Do not add Customer authentication.
- Do not add role upgrade or downgrade endpoints.
- If existing unsupported routes are present, Coding must disable or reject them within this feature scope and document the decision.

## Authorization Readiness
Design ID: `SD-046`

Maps to: `TL-010`, `FR-016`, `NFR-005`, `AC-012`

Authentication success must expose:
- account ID from persistence
- role from persistence
- provider-authenticated identity linkage
- active status already verified

Later authorization can use these fields in guards or policies. This feature must not implement unrelated authorization rules.

## Configuration Guidance
Design ID: `SD-047`

Maps to: `TL-007`, `NFR-001`, `AC-009`

Expected environment/config values:
- Google OAuth client ID or accepted audience.
- Google issuer configuration if verifier requires it.
- Application auth token secret/private key or existing token issuer configuration if JWT is used.
- Token expiry value if issuing JWT/access tokens.

Configuration must be read server-side only. Clients must not choose token audience, issuer, role, account ID, or token claims.

## Explicit Out Of Scope
- Writing production code during System Design.
- Writing tests during System Design.
- Email/password authentication.
- Public Admin registration.
- Google OAuth creation of Admin accounts.
- Customer accounts or Customer authentication.
- Role upgrade/downgrade.
- Full authorization policy rules beyond preserving server-side auth data.
