# Team Lead Delivery Plan

## Task Start Checklist
1. Role running: `team-lead`
2. Project name: `EvelS`
3. Feature name: `init-fe-auth-login`
4. Input files or folders:
   - `AGENTS.md`
   - `RULE.md`
   - `SKILLS/Analysis/shared/WATERFALL_POLICY.md`
   - `SKILLS/Analysis/shared/TRACEABILITY.md`
   - `SKILLS/Analysis/team-lead/`
   - `AI-Share-Documents/Analysis/01-business-analysis/`
   - `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/`
5. Allowed output directories:
   - `AI-Share-Documents/Analysis/02-team-lead/`
6. Completion artifacts:
   - `AI-Share-Documents/Analysis/02-team-lead/tl-delivery-plan.md`
   - `AI-Share-Documents/Analysis/02-team-lead/tl-task-breakdown.md`
   - `AI-Share-Documents/Analysis/02-team-lead/tl-risk-log.md`
   - `AI-Share-Documents/Analysis/02-team-lead/tl-handoff.md`
   - `AI-Share-Documents/Analysis/02-team-lead/tl-self-check.md`

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Preflight Result
`ba-open-questions.md` was read before other BA artifacts. No blocking open questions were found. All BA questions are marked `ASSUMED`, `Blocking: No`, and do not prevent scope selection.

## Delivery Objective
Deliver an initial EvelS frontend that supports role-specific authentication and routing for Admin and Photographer users by reusing the existing backend authentication contracts. The implementation path is frontend-only: Analysis proceeds to System Design, then FE coding, FE testing, and workflow archiving.

## Backend Contract Reuse
Current backend auth behavior is treated as an existing dependency:
- Admin login is available through `/auth/login` and authenticates existing Admin accounts only.
- Photographer login uses the existing Google callback flow.
- Archived BE coding says `GET /auth/google` starts auth and returns `AuthorizationUrl` and `CallbackUrl`.
- Archived BE coding says `GET /auth/google/callback` exchanges the callback code and completes authentication.
- Auth success returns a token plus server-side account data including role.
- Valid roles are `Admin` and `Photographer`.
- Disabled accounts are denied before token issuance.
- Client-provided role or identity values are not trusted.

System Design must reconcile the older archived `sd-api-contract.md` reference to `POST /auth/google/login` with the later archived BE coding/testing evidence for the callback flow. The requested feature explicitly requires reusing the current callback-based flow.

## Milestones

### M1: System Design Completion
System Design produces frontend-focused design artifacts for app structure, route model, auth state, persistence, backend API integration, error handling, and testability.

Exit criteria:
- `sd-self-check.md` confirms `FE_ONLY`.
- `sd-data-design.md` covers frontend auth state, response mapping, persistence, and route guard dependencies.
- `sd-api-contract.md` documents the frontend-consumed Admin login and Google callback contracts, including any assumptions.

### M2: Frontend Project Initialization And Auth Implementation
Coding FE initializes `FE/FE-EvelS` and implements the approved frontend structure, auth integration, persistence, route protection, role redirects, dashboard route shells, and user-facing errors.

Exit criteria:
- `AI-Share-Documents/FE/04-coding-fe/coding-plan.md` exists before source changes.
- `coding-change-log.md` maps frontend changes to `SD-*`, `TL-*`, `FR-*`, and `AC-*`.
- `coding-self-check.md` documents completed work and verification.

### M3: Frontend Testing
Testing FE creates and runs tests for login flows, callback handling, auth persistence, route guards, role separation, logout, and error states.

Exit criteria:
- FE test artifacts exist under `AI-Share-Documents/FE/05-testing-fe/`.
- Test results record commands and outcomes.
- Test gaps document any untested behavior.

### M4: Archive And Reset
Workflow Archiver packs completed Analysis, BE, and FE shared documents into `AI-Share-Documents/Archive/init-fe-auth-login/` and restores active folders from templates.

## Sequencing
1. `system-design` designs the frontend-only solution from BA and Team Lead artifacts.
2. `coding-fe` reads System Design artifacts, creates `coding-plan.md`, then initializes and implements the frontend.
3. `testing-fe` reads FE coding artifacts, creates tests, executes checks, and records outcomes.
4. `workflow-archiver` archives after FE testing completes.

## Dependencies
- Current BA artifacts under `AI-Share-Documents/Analysis/01-business-analysis/`.
- Archived backend auth docs under `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/`.
- Target frontend path `FE/FE-EvelS`.
- No current backend handoff is required because no backend change is planned.

## Planning Assumptions Carried Forward
- `ASM-001` / `BAQ-002`: `FE/FE-EvelS` is ready for initial frontend setup and tooling selection is a design/coding decision.
- `ASM-003` / `BAQ-001`: Admin UI labels are username/password, while exact backend request field names must be confirmed later.
- `ASM-004` / `BAQ-003`: dashboard/homepage content can be minimal route shells for this feature.
- `ASM-005` / `BAQ-004` / `BAQ-006`: persistence and runtime configuration choices are design/coding decisions.
- `BAQ-005`: frontend callback route shape must be designed against the existing backend callback flow.

## Non-Goals
- Backend code or backend tests.
- Backend auth redesign.
- Admin registration.
- Photographer password login.
- Customer authentication.
- Full dashboard business workflows.
- Deployment or production hosting setup.
