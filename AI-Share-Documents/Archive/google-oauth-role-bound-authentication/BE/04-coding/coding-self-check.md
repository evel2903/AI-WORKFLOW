# Coding Backend Self Check

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
- [x] `ai-docs/04-coding/coding-plan.md`
- [x] `ai-docs/04-coding/coding-change-log.md`
- [x] `ai-docs/04-coding/coding-self-check.md`
- [x] production code changes under `src/`

## Preflight Validation
- [x] Read `ai-docs/03-system-design/sd-self-check.md` before implementation work.
- [x] Confirmed System Design is ready and not blocked.
- [x] Read `ai-docs/03-system-design/sd-data-design.md` before other design artifacts.
- [x] Confirmed `sd-data-design.md` is present and implementation-usable.
- [x] Created `coding-plan.md` before production code changes.

## Implementation Validation
- [x] Reused existing `Authentication` module.
- [x] Reused existing `Users` persistence model as account store.
- [x] Implemented Google OAuth redirect and callback endpoints.
- [x] Updated Google OAuth start endpoint to return authorization and callback URLs as JSON.
- [x] Implemented server-side callback code exchange.
- [x] Implemented signed OAuth `state` validation.
- [x] Implemented server-side Google token validation adapter.
- [x] Implemented first-login `Photographer` creation.
- [x] Implemented existing Google-linked account authentication without role mutation.
- [x] Implemented disabled-account login denial before token issuance.
- [x] Removed `User` from authenticated role enum and retained only `Admin` and `Photographer`.
- [x] Supports email/password login for existing Admin accounts only.
- [x] Keeps Photographer authentication Google OAuth-only.
- [x] Disabled public registration behavior.
- [x] Protected user management behind Admin role guards for manual Admin provisioning boundary.
- [x] Added migration and TypeORM entity fields for account status and Google identity.
- [x] Did not add Customer account creation or Customer login.
- [x] Did not add role upgrade or downgrade flows.

## Boundary Validation
- [x] Wrote production code only under `src/`.
- [x] Wrote coding artifacts only under `ai-docs/04-coding/`.
- [x] Did not write tests.
- [x] Did not modify upstream design, planning, or BA artifacts during Coding.
- [x] Did not expand scope beyond approved System Design.

## Verification Results
- [x] `npm.cmd run build` passed.
- [x] `npm.cmd run lint` passed.
- [ ] `npm.cmd test -- --runInBand` passed.

Test result note:
- Existing tests failed because they still target the previous behavior: email/password login, public registration, public user routes, `Role.User`, and old controller dependencies. This is expected to be handled in the Testing Backend phase, which may write under `test/`.

## Known Gaps
- `.env.example` was not updated with `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, or `GOOGLE_REDIRECT_URI` because Coding Backend output boundaries allow only `src/` and `ai-docs/04-coding/`.
- Google callback flow requires server configuration `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, and `GOOGLE_REDIRECT_URI`; without them, Google login fails configuration validation.
- Existing tests require Testing Backend updates to match the approved feature scope.

## Scope Alignment Note
- `coding-plan.md` was updated to match the implemented scope: existing Admin accounts may authenticate with email/password.
- Photographer authentication remains Google OAuth-only.
- Email/password authentication does not create accounts.
- Public registration and Customer authentication remain unsupported.

## Ready For Testing Backend
- [x] Yes
