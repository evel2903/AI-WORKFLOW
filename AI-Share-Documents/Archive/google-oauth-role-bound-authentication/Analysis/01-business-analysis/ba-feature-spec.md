# Business Analysis Feature Spec

## Role Running
`business-analyst`

## Input Files
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `ai-docs/00-project/project-context.md`
- `ai-docs/00-project/constraints.md`
- `ai-docs/00-project/glossary.md`
- `ai-docs/00-prompt-generation/wp-input.md`
- `ai-docs/00-prompt-generation/wp-metadata.md`
- `ai-docs/00-prompt-generation/wp-self-check.md`

## Allowed Output Directories
- `ai-docs/01-business-analysis/`

## Completion Artifacts
- `ai-docs/01-business-analysis/ba-feature-spec.md`
- `ai-docs/01-business-analysis/ba-acceptance-criteria.md`
- `ai-docs/01-business-analysis/ba-open-questions.md`
- `ai-docs/01-business-analysis/ba-self-check.md`

## Feature Name
Google OAuth Role-Bound Authentication

## Business Goal
Enable secure authenticated access for the only two supported authenticated roles in this phase, `Admin` and `Photographer`, while preventing public Admin onboarding, preventing role changes after account creation, and ensuring Google OAuth can onboard and authenticate Photographer accounts safely.

## Problem Statement
The system needs a controlled authentication model that prevents role escalation, avoids unsupported authentication methods, and establishes a server-side trust boundary for future authorization rules.

## Actors
- `Admin`: Authenticated internal user whose account is created manually outside any public registration or Google OAuth onboarding flow.
- `Photographer`: Authenticated user whose account is created and authenticated through Google OAuth only.
- `Customer`: Unauthenticated user in this phase; does not have an account and does not log in or register.
- `System`: Backend service responsible for validating Google OAuth tokens, managing account lookup or creation, enforcing account status, and using server-side account data for decisions.

## In Scope
- Support exactly two authenticated roles: `Admin` and `Photographer`.
- Authenticate Photographer users through Google OAuth.
- Create a Photographer account on first successful Google login when no matching account exists.
- Authenticate an existing account on subsequent Google logins without changing its stored role.
- Enforce role immutability after account creation.
- Prevent Admin account creation through Google OAuth.
- Prevent Admin account creation through public registration.
- Reject or omit email/password authentication.
- Treat Customer users as unauthenticated in this phase.
- Validate Google OAuth tokens in the backend.
- Prevent disabled accounts from logging in.
- Keep authentication results usable by future authorization rules.

## Out Of Scope
- Email/password login.
- Public registration for any Admin account.
- Google OAuth onboarding for Admin accounts.
- Customer account creation, customer login, or customer registration.
- Role upgrade or downgrade workflows.
- Detailed authorization rules for protected business actions beyond producing server-side role/account data ready for later authorization integration.
- Detailed technical implementation choices such as database schema, framework classes, token/session format, or endpoint shape.

## Functional Requirements
- `FR-001`: The system must support only `Admin` and `Photographer` as authenticated roles in this phase.
- `FR-002`: The system must allow Admin accounts to exist only through manual account creation.
- `FR-003`: The system must not create Admin accounts through Google OAuth.
- `FR-004`: The system must not create Admin accounts through any public registration flow.
- `FR-005`: The system must create Photographer accounts only through Google OAuth.
- `FR-006`: On first successful Google login, if no matching account exists, the system must create a new account with default role `Photographer`.
- `FR-007`: On subsequent successful Google login, if a matching account already exists, the system must authenticate that existing account.
- `FR-008`: On subsequent Google login, the system must preserve the existing account role unchanged.
- `FR-009`: Once an account is created, the account role must be immutable.
- `FR-010`: The system must not support role upgrade or role downgrade flows.
- `FR-011`: The system must not support email/password authentication.
- `FR-012`: The system must not support Admin self-registration.
- `FR-013`: Customer users must not have authenticated accounts in this phase.
- `FR-014`: Customer users must not go through login or registration flows in this phase.
- `FR-015`: Disabled accounts must not be allowed to log in.
- `FR-016`: Authentication output must expose enough server-side account and role information for later authorization-rule integration.

## Non-Functional Requirements
- `NFR-001`: The backend must validate Google OAuth tokens before using Google identity data for authentication or account creation.
- `NFR-002`: The backend must never trust client-provided role data for authentication, account creation, or authorization decisions.
- `NFR-003`: The backend must never trust client-provided identity data unless it is validated through the Google OAuth token validation process.
- `NFR-004`: Authentication decisions must rely on server-side persisted account data and validated provider identity only.
- `NFR-005`: Authorization-readiness data must be server-side controlled and stable enough for later guards, policies, or equivalent authorization mechanisms.
- `NFR-006`: Account-disabled checks must happen before the system completes a login.
- `NFR-007`: Unsupported authentication flows must fail safely without creating accounts or mutating roles.

## Business Rules
- `BR-001`: Only `Admin` and `Photographer` are authenticated roles in this phase.
- `BR-002`: `Admin` is manual-provisioning only.
- `BR-003`: Google OAuth can create only `Photographer` accounts, never `Admin` accounts.
- `BR-004`: Public registration must not create `Admin` accounts.
- `BR-005`: First successful Google login creates a new `Photographer` account only when no matching account exists.
- `BR-006`: Subsequent successful Google logins authenticate the matching existing account.
- `BR-007`: Stored role is authoritative and must not be changed during login.
- `BR-008`: Role upgrade and downgrade are prohibited after account creation.
- `BR-009`: Disabled accounts cannot complete login.
- `BR-010`: Customer users remain unauthenticated and accountless in this phase.
- `BR-011`: Client input cannot decide role, identity, account status, or authorization-relevant identity.

## Dependencies
- Google OAuth is the accepted external identity provider for Photographer authentication.
- The backend has or will have a server-side account store that can represent role, provider identity, and disabled-account state.
- Later authorization phases will consume server-side authentication/account data from this feature.

## Assumptions
- `ASM-001`: Manual Admin account creation is an internal operational process and is not part of any public user-facing registration flow.
- `ASM-002`: The exact manual Admin provisioning workflow is not required to be specified at the business-analysis level for this authentication scope, provided downstream design preserves the no-public-registration and no-Google-Admin-onboarding rules.
- `ASM-003`: The exact post-login token or session format is a downstream design decision, provided it uses server-side validated account and role data.
- `ASM-004`: Disabled-account enforcement applies to both Admin and Photographer accounts.
- `ASM-005`: A matching account on Google login is determined by a server-side mapping to validated Google identity data, with exact matching mechanics deferred to system design.

## Handoff Notes
- Team Lead must carry all `ASSUMED` items from `ba-open-questions.md` into planning and risk artifacts.
- Team Lead may proceed because no ambiguity is marked `Status: OPEN` at BA completion.
- Downstream phases must not broaden scope into email/password authentication, Customer authentication, role mutation, Admin Google onboarding, or Admin self-registration.
