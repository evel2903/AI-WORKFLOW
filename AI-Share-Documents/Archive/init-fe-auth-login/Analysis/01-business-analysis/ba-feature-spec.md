# Business Analysis Feature Spec

## Task Start Checklist
1. Role running: `business-analyst`
2. Project name: `EvelS`
3. Feature name: `init-fe-auth-login`
4. Input files or folders:
   - `AGENTS.md`
   - `RULE.md`
   - `SKILLS/Analysis/shared/WATERFALL_POLICY.md`
   - `SKILLS/Analysis/shared/TRACEABILITY.md`
   - `SKILLS/Analysis/business-analyst/`
   - `AI-Share-Documents/Analysis/00-prompt-generation/`
   - `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/`
5. Allowed output directories:
   - `AI-Share-Documents/Analysis/01-business-analysis/`
6. Completion artifacts:
   - `AI-Share-Documents/Analysis/01-business-analysis/ba-feature-spec.md`
   - `AI-Share-Documents/Analysis/01-business-analysis/ba-acceptance-criteria.md`
   - `AI-Share-Documents/Analysis/01-business-analysis/ba-open-questions.md`
   - `AI-Share-Documents/Analysis/01-business-analysis/ba-self-check.md`

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Business Goal
Initialize the EvelS frontend and provide clear, role-specific login journeys for Admin and Photographer users while reusing the already implemented backend authentication system.

## Problem Statement
The backend can already authenticate Admin users with password login and Photographer users with Google callback authentication. EvelS needs an initial frontend application that exposes those login journeys, persists authenticated state, redirects users to the correct role home area, and prevents unauthenticated or wrong-role access.

## Actors
- Admin user: an existing Admin account holder who signs in with username and password.
- Photographer user: a Photographer account holder or first-time Google-authenticated Photographer user who signs in with Google.
- Unauthenticated visitor: a user who has not completed login and should be limited to public login routes.
- Backend authentication system: existing read-only dependency that validates credentials, performs Google callback authentication, returns auth state, and denies disabled accounts.

## In Scope
- Initialize the frontend project under `FE/FE-EvelS`.
- Establish a maintainable initial frontend structure because the current frontend project has no app files beyond repository metadata.
- Provide an Admin login entry point using username and password.
- Provide a Photographer login entry point using Google authentication only.
- Reuse the existing backend Admin password login flow.
- Reuse the existing backend Google callback authentication flow.
- Persist frontend authentication state after successful login.
- Restore or validate frontend authentication state when the app starts.
- Redirect Admin users to an Admin dashboard/homepage after login.
- Redirect Photographer users to a Photographer dashboard/homepage after login.
- Protect authenticated routes from unauthenticated access.
- Prevent users from accessing pages outside their assigned role.
- Display clear user-facing errors for failed login attempts and failed Google callback outcomes.
- Keep frontend decisions based on backend-returned account data, especially role and account status.

## Out Of Scope
- Backend source changes.
- Backend tests.
- New backend authentication endpoints.
- Public Admin registration.
- Public Photographer password login.
- Customer login or customer account creation.
- Role mutation or role management UI.
- Full business content for Admin or Photographer dashboards beyond what is needed to prove correct routing.
- Production deployment, hosting, or CI/CD setup.

## Archived Backend Contract Reuse
The archived `google-oauth-role-bound-authentication` feature is read-only context for this feature.

Backend facts preserved for downstream phases:
- `/auth/login` authenticates existing Admin accounts only.
- Photographer accounts must use Google OAuth and cannot use password login.
- `GET /auth/google` starts Google authentication and returns `AuthorizationUrl` and `CallbackUrl`.
- `GET /auth/google/callback` handles the callback code exchange and completes authentication.
- Auth success returns an application token and server-side account data such as account ID, role, status, email, and display name.
- Valid authenticated roles are `Admin` and `Photographer`.
- Disabled accounts are denied before token issuance.
- Client-provided role or identity data must not be trusted.
- Backend tests for the existing auth behavior passed in the archived testing phase.

## Functional Requirements
- FR-001: The frontend project must be initialized under `FE/FE-EvelS`.
- FR-002: The frontend must provide a maintainable starting structure for authentication, routing, API integration, role pages, and tests.
- FR-003: The frontend must provide an Admin login flow that accepts username and password only.
- FR-004: The frontend must submit Admin login attempts to the existing backend Admin password login contract.
- FR-005: The frontend must redirect successfully authenticated Admin users to the Admin dashboard/homepage.
- FR-006: The frontend must not offer Google login as the Admin login method.
- FR-007: The frontend must provide a Photographer login flow that uses Google authentication only.
- FR-008: The frontend must start Photographer Google login using the existing backend Google authentication initiation flow.
- FR-009: The frontend must handle the existing backend Google callback authentication flow.
- FR-010: The frontend must redirect successfully authenticated Photographer users to the Photographer dashboard/homepage.
- FR-011: The frontend must not offer username/password login as the Photographer login method.
- FR-012: The frontend must persist successful authentication state in the browser according to the design selected later.
- FR-013: The frontend must restore or validate persisted authentication state when the app starts.
- FR-014: The frontend must protect authenticated routes from unauthenticated access.
- FR-015: The frontend must prevent Admin users from accessing Photographer-only pages.
- FR-016: The frontend must prevent Photographer users from accessing Admin-only pages.
- FR-017: The frontend must route users after login according to the server-returned account role.
- FR-018: The frontend must expose clear user-facing errors for failed Admin login.
- FR-019: The frontend must expose clear user-facing errors for failed Photographer Google login or callback completion.
- FR-020: The frontend must handle backend-denied disabled account responses as failed login attempts with a clear error.
- FR-021: The frontend must provide a logout path that clears local authentication state and prevents continued protected access.
- FR-022: The frontend must not rely on client-supplied role values to grant access.

## Non-Functional Requirements
- NFR-001: The implementation must follow the Waterfall workflow and produce downstream artifacts through shared files only.
- NFR-002: The frontend must follow existing FE architecture if discoverable; if none exists, it must establish a simple, maintainable initial architecture.
- NFR-003: Authentication state persistence must balance user continuity with reasonable protection against stale or malformed state.
- NFR-004: Route protection and role checks must fail closed when authentication state is missing, malformed, expired, or not role-compatible.
- NFR-005: User-facing auth errors must be understandable without exposing sensitive backend or security details.
- NFR-006: The frontend must keep backend auth contracts isolated enough that contract adjustments can be made without spreading changes across the app.
- NFR-007: The frontend must remain testable for login, callback handling, persistence, protected routes, and role-based routing.
- NFR-008: The feature must not introduce backend implementation or testing work.

## Dependencies
- Existing backend project: `BE/BE-EvelS`.
- Existing frontend project location: `FE/FE-EvelS`.
- Archived backend authentication context:
  - `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/Analysis/03-system-design/sd-api-contract.md`
  - `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/Analysis/03-system-design/sd-data-design.md`
  - `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/BE/04-coding/coding-change-log.md`
  - `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/BE/05-testing/test-results.md`

## Assumptions
- ASM-001: `FE/FE-EvelS` is intentionally ready for initial project setup because it currently contains no application files.
- ASM-002: The exact frontend framework and package/test tooling will be selected during System Design and Coding FE based on repository conventions and project needs.
- ASM-003: "Username" is the business label for Admin credential entry; the exact backend request field names will be confirmed by System Design/Coding FE from the backend contract or source.
- ASM-004: Admin and Photographer dashboard/homepage content can be minimal route shells for this feature unless a later artifact adds content requirements.
- ASM-005: Backend base URL, environment variable names, and token storage details are implementation concerns for System Design/Coding FE.

## Business Handoff Notes
- Team Lead should proceed with `FE_ONLY` planning unless a blocking backend contract contradiction is discovered.
- Non-blocking assumptions should be carried into `tl-risk-log.md` and `tl-handoff.md`.
- System Design must define frontend data/state behavior in `sd-data-design.md`, including auth response mapping and browser persistence.
