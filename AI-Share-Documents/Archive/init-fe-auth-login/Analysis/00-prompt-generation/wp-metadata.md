# Write Prompt Metadata

## Role Running
`write-prompt`

## Project
- Project name: `EvelS`
- Feature name: `init-fe-auth-login`
- Backend code path: `BE/BE-EvelS`
- Frontend code path: `FE/FE-EvelS`
- Final archive path: `AI-Share-Documents/Archive/init-fe-auth-login`

## Scope Metadata
- Expected implementation scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`
- Generated backend coding prompt: `No`
- Generated backend testing prompt: `No`
- Generated frontend coding prompt: `Yes`
- Generated frontend testing prompt: `Yes`

## Rules Applied
- `AGENTS.md`
- `RULE.md`
- `SKILLS/Analysis/shared/WATERFALL_POLICY.md`
- `SKILLS/Analysis/shared/TRACEABILITY.md`
- `SKILLS/Analysis/write-prompt/SYSTEM_PROMPT.md`
- `SKILLS/Analysis/write-prompt/CHECKLIST.md`
- `SKILLS/Analysis/write-prompt/INPUTS_OUTPUTS.md`

## Archive Context Used
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/Analysis/03-system-design/sd-api-contract.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/Analysis/03-system-design/sd-data-design.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/BE/04-coding/coding-change-log.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/BE/05-testing/test-results.md`

## Auth Contract Summary For Downstream Use
- Admin login: existing backend Admin-only password login at `/auth/login`.
- Photographer login: existing Google OAuth callback flow.
- Google initiation: `GET /auth/google` returns `AuthorizationUrl` and `CallbackUrl`.
- Google callback: `GET /auth/google/callback` exchanges the callback code and completes authentication.
- Auth result: application token plus server-side `Account` data including role.
- Roles: `Admin`, `Photographer`.
- Trust boundary: frontend must not send or rely on client-controlled role data for authorization decisions.

## Assumptions
- `FE/FE-EvelS` is the target frontend project and is currently uninitialized or effectively empty.
- The exact frontend framework, package manager, and test framework should be selected by System Design and Coding FE based on repository conventions, existing FE architecture guidance, and project constraints.
- No backend code changes are expected for this feature.
- Existing archived backend documents are read-only reference material and do not override current Analysis/System Design artifacts.

## Generated Prompt Set
1. `business-analyst`
2. `team-lead`
3. `system-design`
4. `coding-fe`
5. `testing-fe`
6. `workflow-archiver`

## Omitted Prompt Set
- `coding-be`: omitted because expected scope is `FE_ONLY`.
- `testing-be`: omitted because expected scope is `FE_ONLY`.
- BE handoff/share-documents prompt: omitted because no backend changes are in scope and current backend handoff is not required.
