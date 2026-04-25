# Write Prompt Input

## Task Start Checklist
1. Role running: `write-prompt`
2. Project name: `EvelS`
3. Feature name: `init-fe-auth-login`
4. Input files or folders:
   - `AGENTS.md`
   - `RULE.md`
   - `SKILLS/Analysis/shared/WATERFALL_POLICY.md`
   - `SKILLS/Analysis/shared/TRACEABILITY.md`
   - `SKILLS/Analysis/write-prompt/SYSTEM_PROMPT.md`
   - `SKILLS/Analysis/write-prompt/CHECKLIST.md`
   - `SKILLS/Analysis/write-prompt/INPUTS_OUTPUTS.md`
   - `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/**`
   - `BE/BE-EvelS`
   - `FE/FE-EvelS`
5. Allowed output directories:
   - `AI-Share-Documents/Analysis/00-prompt-generation/`
6. Completion artifacts:
   - `AI-Share-Documents/Analysis/00-prompt-generation/wp-input.md`
   - `AI-Share-Documents/Analysis/00-prompt-generation/wp-prompts.md`
   - `AI-Share-Documents/Analysis/00-prompt-generation/wp-metadata.md`
   - `AI-Share-Documents/Analysis/00-prompt-generation/wp-self-check.md`

## Raw Request

Run as the Write Prompt agent.

Follow:
- `RULE.md`

Task:
Generate full cross-role Waterfall prompts for this feature:

> Setup initial frontend project for the EvelS project.
>
> The backend authentication system already exists and was developed previously. Read and reuse the existing authentication implementation from the Archive documents before generating prompts.
>
> Implement login flows for two roles: Admin and Photographer.
>
> Admin login requirements:
> - Admin users must sign in using username and password only.
> - After successful login, Admin users must be redirected to the Admin dashboard/homepage.
>
> Photographer login requirements:
> - Photographer users must sign in using Google authentication only.
> - Google authentication already exists in the backend and currently works using a callback-based flow.
> - Reuse the existing Google auth backend flow.
> - After successful login, Photographer users must be redirected to the Photographer dashboard/homepage.
>
> General requirements:
> - Frontend must be initialized and structured following the existing FE architecture.
> - Authentication state must be persisted correctly.
> - Protected routes must prevent unauthenticated access.
> - Users must not access pages outside their assigned role.
> - Clear error handling must be shown for failed login attempts.
> - Role-based routing must be implemented after login.
> - The implementation must follow the existing BE and FE architecture.

Project:
- Project name: `EvelS`
- Feature name: `init-fe-auth-login`
- Expected implementation scope: `FE_ONLY`
- Backend code path: `BE/BE-EvelS`
- Frontend code path: `FE/FE-EvelS`
- Active Analysis document path: `AI-Share-Documents/Analysis`
- Active Backend document path: `AI-Share-Documents/BE`
- Active Frontend document path: `AI-Share-Documents/FE`
- Final archive path: `AI-Share-Documents/Archive/init-fe-auth-login`

Instructions:
- Apply all reusable workflow rules from `RULE.md`.
- Read existing authentication flows from Archive first.
- Reuse current Google callback authentication flow.
- Use `init-fe-auth-login` wherever `RULE.md` references `<Feature_name>`.

## Derived Scope

- Expected implementation scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`
- Backend source path is read-only context for this feature: `BE/BE-EvelS`
- Frontend source path for implementation: `FE/FE-EvelS`
- Analysis artifacts must still declare and validate the scope.

## Archived Authentication Context Read First

Relevant archive feature:
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/`

Important archived backend facts to carry into downstream prompts:
- Existing backend supports Admin password login through `/auth/login`.
- Archived implementation notes state `/auth/login` authenticates existing Admin accounts only.
- Photographer accounts must use Google OAuth only.
- Existing Google OAuth implementation uses callback flow:
  - `GET /auth/google` initiates Google authentication and returns `AuthorizationUrl` and `CallbackUrl`.
  - `GET /auth/google/callback` handles the callback code exchange.
- Backend validates Google tokens server-side and issues auth results from persisted account data.
- Auth result includes an application access token and account data such as account ID, role, status, email, and display name.
- Valid authenticated roles are `Admin` and `Photographer`.
- Disabled accounts are denied before token issuance.
- Client-provided role or identity data must not be trusted.
- The frontend must not request backend changes in this feature unless Analysis/System Design discovers a blocking contradiction.

Primary archive references:
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/Analysis/03-system-design/sd-api-contract.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/Analysis/03-system-design/sd-data-design.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/BE/04-coding/coding-change-log.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/BE/05-testing/test-results.md`
