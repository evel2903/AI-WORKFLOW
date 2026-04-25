# System Design Domain Design

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

## Domain Model

### Aggregate: Account
Design ID: `SD-011`

Maps to: `TL-001`, `TL-002`, `TL-003`, `TL-004`, `TL-005`, `TL-008`, `FR-001` through `FR-010`, `FR-015`, `AC-001` through `AC-006`, `AC-011`

Purpose:
- Represents an authenticated user account.
- Owns immutable role and account status.
- Is the server-side source of truth for authentication and later authorization.

Candidate fields:
- `Id`
- `Role`
- `Status`
- `Email`
- `DisplayName`
- `AvatarUrl`
- `CreatedAt`
- `UpdatedAt`
- `DisabledAt`

Invariants:
- `Role` must be one of `Admin` or `Photographer`.
- `Role` is assigned only at creation.
- `Role` must not be updated after account creation.
- `Status: Disabled` prevents login.
- Customer users are not represented as accounts in this phase.

### Entity: AccountExternalIdentity
Design ID: `SD-012`

Maps to: `TL-003`, `TL-004`, `TL-007`, `TL-009`, `FR-005`, `FR-006`, `FR-007`, `NFR-001`, `NFR-003`, `NFR-004`, `AC-004`, `AC-005`, `AC-009`

Purpose:
- Links an account to a verified external identity provider.
- Stores the provider identity used for account matching.

Candidate fields:
- `Id`
- `AccountId`
- `Provider`
- `ProviderSubject`
- `ProviderEmail`
- `ProviderEmailVerified`
- `CreatedAt`
- `UpdatedAt`

Invariants:
- `Provider` must support `Google` for this feature.
- `(Provider, ProviderSubject)` must be unique.
- Google account lookup must use validated provider identity, not client-provided identity.
- A provider identity belongs to exactly one account.

### Value Object: AccountRole
Design ID: `SD-013`

Maps to: `TL-001`, `TL-005`, `FR-001`, `FR-009`, `FR-010`, `AC-001`, `AC-006`

Allowed values:
- `Admin`
- `Photographer`

Rules:
- No other authenticated role is allowed in this phase.
- No role upgrade or downgrade operation exists.
- The domain should not expose a public setter or update method for role.

### Value Object: AccountStatus
Design ID: `SD-014`

Maps to: `TL-008`, `FR-015`, `NFR-006`, `AC-011`

Allowed values:
- `Active`
- `Disabled`

Rules:
- Only `Active` accounts can complete login.
- `Disabled` accounts must be denied before token/session issuance.

### Value Object: VerifiedGoogleIdentity
Design ID: `SD-015`

Maps to: `TL-003`, `TL-004`, `TL-007`, `FR-005`, `FR-006`, `FR-007`, `NFR-001`, `NFR-003`, `NFR-004`, `AC-004`, `AC-005`, `AC-009`

Purpose:
- Represents identity data after backend token validation.

Candidate properties:
- `Subject`
- `Email`
- `EmailVerified`
- `Name`
- `PictureUrl`

Rules:
- Must be produced only by backend Google token validation.
- Must not be constructed directly from arbitrary client input.
- `Subject` is the primary provider identity key.

## Use Cases

### Use Case: AuthenticateWithGoogle
Design ID: `SD-016`

Maps to: `TL-003`, `TL-004`, `TL-007`, `TL-008`, `TL-010`, `TL-011`, `FR-005`, `FR-006`, `FR-007`, `FR-008`, `FR-015`, `FR-016`, `NFR-001`, `NFR-004`, `NFR-006`, `AC-004`, `AC-005`, `AC-009`, `AC-011`, `AC-012`

Flow:
1. Accept Google ID token from Presentation.
2. Validate token through `IGoogleTokenVerifier`.
3. Build `VerifiedGoogleIdentity`.
4. Look up `AccountExternalIdentity` by provider `Google` and verified subject.
5. If no identity exists, create `Account` with role `Photographer`, status `Active`, and linked Google identity.
6. If identity exists, load existing `Account` and preserve role unchanged.
7. If account status is `Disabled`, reject login.
8. Return an authorization-ready authentication result.

### Use Case: ManuallyProvisionAdminAccount
Design ID: `SD-017`

Maps to: `TL-002`, `FR-002`, `FR-003`, `FR-004`, `FR-012`, `AC-002`, `AC-003`, `BAQ-001`

Flow boundary:
- This feature does not expose a public Admin registration endpoint.
- If an internal admin provisioning use case is implemented now or already exists, it must be internal-only and must create role `Admin` explicitly from trusted server-side operational input.
- It must not be reachable through Google OAuth onboarding or public self-registration.

### Use Case: RejectUnsupportedAuthFlow
Design ID: `SD-018`

Maps to: `TL-006`, `FR-011`, `FR-012`, `FR-013`, `FR-014`, `NFR-007`, `AC-007`, `AC-008`, `AC-013`

Rules:
- Email/password authentication is not supported.
- Customer login and registration are not supported.
- Public Admin registration is not supported.
- Unsupported flows must not create accounts, authenticate users, or mutate roles.

## Repository Interfaces

### IAccountRepository
Design ID: `SD-019`

Expected operations:
- `FindById(Id)`
- `FindByExternalIdentity(Provider, ProviderSubject)`
- `CreatePhotographerFromGoogle(VerifiedGoogleIdentity)`
- `CreateAdminManually(...)` only if internal provisioning is implemented in this scope
- `Save(Account)` excluding role mutation after creation

### IAccountExternalIdentityRepository
Design ID: `SD-020`

Expected operations:
- `FindByProviderSubject(Provider, ProviderSubject)`
- `CreateForAccount(AccountId, Provider, ProviderSubject, ProviderEmail, ProviderEmailVerified)`

### IGoogleTokenVerifier
Design ID: `SD-021`

Expected operation:
- `VerifyIdToken(GoogleIdToken): VerifiedGoogleIdentity`

### IAuthTokenIssuer
Design ID: `SD-022`

Expected operation:
- `IssueForAccount(Account): AuthResult`

The exact token/session format is deferred from `BAQ-002`, but Coding needs a concrete implementation from this design: issue an application auth token or equivalent existing session result that contains server-derived account ID and role claims only.

## Domain Errors
- `InvalidGoogleToken`
- `AccountDisabled`
- `UnsupportedAuthenticationFlow`
- `UnsupportedRegistrationFlow`
- `RoleMutationNotAllowed`
- `ExternalIdentityConflict`

## Assumptions Carried Forward
- `BAQ-001`: Manual Admin provisioning is internal and non-public.
- `BAQ-002`: Application auth token/session details may align with existing repo conventions; auth output must include server-derived account ID and role.
- `BAQ-003`: Google matching uses verified Google `Subject` as primary key.
- `BAQ-004`: Disabled-account responses use the standard API error envelope.
- `BAQ-005`: Existing Admin with manually linked Google identity may authenticate without role mutation; Google OAuth must not create or convert Admin.
