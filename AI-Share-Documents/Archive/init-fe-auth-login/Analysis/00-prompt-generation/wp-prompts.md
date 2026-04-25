# Generated Waterfall Prompts

## 1. Business Analyst Prompt

Role running: `business-analyst`

Project name: `EvelS`

Feature name: `init-fe-auth-login`

Input files or folders:
- `AGENTS.md`
- `RULE.md`
- `SKILLS/Analysis/shared/WATERFALL_POLICY.md`
- `SKILLS/Analysis/shared/TRACEABILITY.md`
- `SKILLS/Analysis/business-analyst/`
- `AI-Share-Documents/Analysis/00-prompt-generation/wp-input.md`
- `AI-Share-Documents/Analysis/00-prompt-generation/wp-metadata.md`
- `AI-Share-Documents/Analysis/00-prompt-generation/wp-self-check.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/Analysis/03-system-design/sd-api-contract.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/Analysis/03-system-design/sd-data-design.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/BE/04-coding/coding-change-log.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/BE/05-testing/test-results.md`

Allowed output directories:
- `AI-Share-Documents/Analysis/01-business-analysis/`

Completion artifacts:
- `AI-Share-Documents/Analysis/01-business-analysis/ba-feature-spec.md`
- `AI-Share-Documents/Analysis/01-business-analysis/ba-acceptance-criteria.md`
- `AI-Share-Documents/Analysis/01-business-analysis/ba-open-questions.md`
- `AI-Share-Documents/Analysis/01-business-analysis/ba-self-check.md`

Mission:
Create Business Analysis artifacts for initializing the EvelS frontend and implementing role-specific login flows that reuse the already implemented backend authentication system.

Required scope framing:
- Treat expected implementation scope as `FE_ONLY`.
- State expected scope fields for downstream validation:
  - `Implementation Scope: FE_ONLY`
  - `Backend in scope: No`
  - `Frontend in scope: Yes`
  - `Backend handoff required for FE: No`
- Do not request backend implementation work unless archive/backend context contradicts the requested feature in a blocking way.
- Read the archived backend authentication documents before writing analysis artifacts.
- Use archived backend auth documents only as read-only contract context.

Business requirements to capture:
- Setup the initial frontend project under `FE/FE-EvelS`.
- Follow existing FE architecture where discoverable; if the frontend is uninitialized, document the need to establish a maintainable project structure.
- Admin users sign in using username and password only.
- Successful Admin login redirects to the Admin dashboard/homepage.
- Photographer users sign in using Google authentication only.
- Photographer Google login reuses the existing backend callback-based Google auth flow.
- Successful Photographer login redirects to the Photographer dashboard/homepage.
- Authentication state is persisted correctly in the browser according to frontend design.
- Protected routes prevent unauthenticated access.
- Role-based routing prevents users from accessing pages outside their assigned role.
- Clear user-facing error handling is shown for failed login attempts.

Archived backend facts to preserve:
- Admin password login exists through `/auth/login` and authenticates existing Admin accounts only.
- Google OAuth callback flow exists through `GET /auth/google` and `GET /auth/google/callback`.
- `GET /auth/google` returns `AuthorizationUrl` and `CallbackUrl`.
- Auth success includes application token and server-side account data, including role.
- Roles are `Admin` and `Photographer`.
- Client-controlled role data must not be trusted.
- Disabled accounts are denied by backend before token issuance.

Required outputs:
- `ba-feature-spec.md` must define actors, scope, role-specific flows, frontend initialization need, dependencies, non-goals, assumptions, and backend contract reuse.
- `ba-acceptance-criteria.md` must define verifiable criteria with stable IDs such as `AC-001`, mapped to `FR-*` and `NFR-*`.
- `ba-open-questions.md` must record ambiguities with explicit `Status`, `Blocking`, `Impact`, assumption, and rationale fields where applicable.
- `ba-self-check.md` must confirm completeness, traceability, scope declaration, archive context usage, and any open questions.

Constraints:
- Follow strict Waterfall policy.
- Do not write planning, design, code, or test artifacts.
- Do not modify BE or FE source.
- Do not write outside `AI-Share-Documents/Analysis/01-business-analysis/`.
- Do not use `ai-docs/00-project/`, kit-local `docs/`, or kit-local `ai-docs/` for workflow artifacts.

## 2. Team Lead Prompt

Role running: `team-lead`

Project name: `EvelS`

Feature name: `init-fe-auth-login`

Input files or folders:
- `AGENTS.md`
- `RULE.md`
- `SKILLS/Analysis/shared/WATERFALL_POLICY.md`
- `SKILLS/Analysis/shared/TRACEABILITY.md`
- `SKILLS/Analysis/team-lead/`
- `AI-Share-Documents/Analysis/01-business-analysis/ba-feature-spec.md`
- `AI-Share-Documents/Analysis/01-business-analysis/ba-acceptance-criteria.md`
- `AI-Share-Documents/Analysis/01-business-analysis/ba-open-questions.md`
- `AI-Share-Documents/Analysis/01-business-analysis/ba-self-check.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/Analysis/03-system-design/sd-api-contract.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/BE/04-coding/coding-change-log.md`

Allowed output directories:
- `AI-Share-Documents/Analysis/02-team-lead/`

Completion artifacts:
- `AI-Share-Documents/Analysis/02-team-lead/tl-delivery-plan.md`
- `AI-Share-Documents/Analysis/02-team-lead/tl-task-breakdown.md`
- `AI-Share-Documents/Analysis/02-team-lead/tl-risk-log.md`
- `AI-Share-Documents/Analysis/02-team-lead/tl-handoff.md`
- `AI-Share-Documents/Analysis/02-team-lead/tl-self-check.md`

Mandatory preflight:
- Read `AI-Share-Documents/Analysis/01-business-analysis/ba-open-questions.md` first.
- Stop only for blocking open questions: `Blocking: Yes`, `Impact: HIGH`, `Impact: UNKNOWN`, or questions that prevent selecting implementation scope.
- If blocked, write only `AI-Share-Documents/Analysis/02-team-lead/tl-self-check.md` with the blocking note and do not read additional BA artifacts.
- Proceed when blocking questions are resolved or explicitly assumed with rationale.

Mission:
Create Team Lead planning artifacts for a `FE_ONLY` frontend initialization and authentication UI/routing feature that integrates with existing backend auth contracts.

Required planning scope:
- Record:
  - `Implementation Scope: FE_ONLY`
  - `Backend in scope: No`
  - `Frontend in scope: Yes`
  - `Backend handoff required for FE: No`
- Break work into traceable tasks using IDs such as `TL-001`.
- Plan frontend project initialization under `FE/FE-EvelS`.
- Plan authentication state ownership, persistence, bootstrapping, and logout behavior.
- Plan Admin username/password login against the existing backend Admin login contract.
- Plan Photographer Google login using the existing backend callback flow.
- Plan callback handling, token/account capture, and role-based post-login redirects.
- Plan protected route behavior for unauthenticated users.
- Plan role guards so Admin and Photographer users cannot access each other's pages.
- Plan dashboard/homepage placeholders or route shells for each role if needed to prove routing behavior.
- Plan clear error display for failed login attempts and failed callback completion.
- Carry forward any BA assumptions into risks and handoff.

Required outputs:
- `tl-delivery-plan.md` must describe scope, milestones, dependencies, sequencing, and current backend contract reuse.
- `tl-task-breakdown.md` must map `TL-*` tasks to upstream `FR-*`, `NFR-*`, and `AC-*`.
- `tl-risk-log.md` must capture risks around uninitialized FE structure, backend contract mismatch, auth persistence, callback handling, route guarding, and role leakage.
- `tl-handoff.md` must tell System Design exactly what to design for frontend architecture, data flow, routing, and backend integration.
- `tl-self-check.md` must confirm preflight status, scope declaration, artifact completeness, and traceability.

Constraints:
- Do not write system design, production code, or tests.
- Do not modify BE or FE source.
- Do not create backend implementation tasks unless the current BA artifacts prove backend work is in scope.
- Do not write outside `AI-Share-Documents/Analysis/02-team-lead/`.

## 3. System Design Prompt

Role running: `system-design`

Project name: `EvelS`

Feature name: `init-fe-auth-login`

Input files or folders:
- `AGENTS.md`
- `RULE.md`
- `SKILLS/Analysis/shared/WATERFALL_POLICY.md`
- `SKILLS/Analysis/shared/TRACEABILITY.md`
- `SKILLS/Analysis/system-design/`
- `AI-Share-Documents/Analysis/02-team-lead/tl-delivery-plan.md`
- `AI-Share-Documents/Analysis/02-team-lead/tl-task-breakdown.md`
- `AI-Share-Documents/Analysis/02-team-lead/tl-risk-log.md`
- `AI-Share-Documents/Analysis/02-team-lead/tl-handoff.md`
- `AI-Share-Documents/Analysis/02-team-lead/tl-self-check.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/Analysis/03-system-design/sd-api-contract.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/Analysis/03-system-design/sd-data-design.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/BE/04-coding/coding-change-log.md`
- `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/BE/05-testing/test-results.md`
- Optional read-only context: `FE/FE-EvelS`

Allowed output directories:
- `AI-Share-Documents/Analysis/03-system-design/`

Completion artifacts:
- `AI-Share-Documents/Analysis/03-system-design/sd-solution-overview.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-domain-design.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-api-contract.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-data-design.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-implementation-guidelines.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-self-check.md`

Mission:
Design the frontend solution for the initial EvelS frontend project and role-based authentication login flows, reusing the existing backend authentication implementation.

Mandatory scope declaration:
- `Implementation Scope: FE_ONLY`
- `Backend in scope: No`
- `Frontend in scope: Yes`
- `Backend handoff required for FE: No`

Required design scope:
- Use stable design IDs such as `SD-001`, mapped to `TL-*`, `FR-*`, and `AC-*`.
- Define frontend architecture for an initial project under `FE/FE-EvelS`.
- Select frontend structure based on existing repository conventions where available; if none exist, define a conservative structure that supports routing, auth state, API clients, UI components, and tests.
- Define route map for public login pages, Google callback route, Admin protected routes, Photographer protected routes, and fallback/unauthorized behavior.
- Define auth state model and persistence strategy.
- Define how the frontend consumes the backend auth result and stores token/account data.
- Define Admin username/password login API interaction with existing backend `/auth/login`.
- Define Photographer Google flow:
  - start from frontend Photographer login action
  - call existing backend Google initiation endpoint or navigate according to backend contract
  - handle callback route
  - exchange/complete callback through existing backend flow
  - persist returned auth state
  - redirect to Photographer dashboard/homepage
- Define role-based redirect rules after login and after app bootstrap.
- Define guards for unauthenticated and wrong-role access.
- Define logout behavior.
- Define error handling for invalid credentials, failed Google callback, disabled account, network failure, malformed backend response, and unauthorized route access.

Required API contract output:
- `sd-api-contract.md` must restate the frontend-consumed backend contracts from archive without requesting backend changes:
  - Admin login endpoint and expected response/error envelope.
  - Google initiation endpoint `GET /auth/google` returning `AuthorizationUrl` and `CallbackUrl`.
  - Google callback endpoint `GET /auth/google/callback`.
  - Auth result fields needed by frontend: token plus account role/status/identity.
- If the exact Admin login request field names are not proven by current artifacts, mark them as an integration assumption or open design risk instead of inventing certainty.

Required data design output:
- `sd-data-design.md` is mandatory and must be implementation-usable for frontend work.
- Cover frontend data sources, API response mapping, auth state shape, account model, token storage, browser/session persistence, bootstrap hydration, cache or revalidation boundaries, and route guard data dependencies.
- Explicitly state that no backend schema or migration is in scope.

Required outputs:
- `sd-solution-overview.md` must explain the frontend architecture, auth flows, scope, and backend reuse.
- `sd-domain-design.md` must define frontend auth/domain concepts, role rules, route access invariants, and state transitions.
- `sd-api-contract.md` must define frontend integration contracts and error mapping.
- `sd-data-design.md` must define frontend state/data-flow and persistence.
- `sd-implementation-guidelines.md` must provide concrete coding guidance, folder/module recommendations, constraints, and testability notes.
- `sd-self-check.md` must confirm scope, archive context usage, data-design completeness, and traceability.

Constraints:
- Do not write production code or tests.
- Do not modify BE or FE source.
- Do not write outside `AI-Share-Documents/Analysis/03-system-design/`.
- Do not require current BE handoff documents because backend is not in scope.
- Do not trust client-provided role data beyond server-returned account data.

## 4. Coding Frontend Prompt

Role running: `coding-fe`

Project name: `EvelS`

Feature name: `init-fe-auth-login`

Input files or folders:
- `AGENTS.md`
- `RULE.md`
- `SKILLS/Analysis/shared/WATERFALL_POLICY.md`
- `SKILLS/Analysis/shared/TRACEABILITY.md`
- `SKILLS/FE/coding-fe/`
- `AI-Share-Documents/Analysis/03-system-design/sd-self-check.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-data-design.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-solution-overview.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-domain-design.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-api-contract.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-implementation-guidelines.md`
- Optional read-only archived BE references:
  - `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/Analysis/03-system-design/sd-api-contract.md`
  - `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/BE/04-coding/coding-change-log.md`
  - `AI-Share-Documents/Archive/google-oauth-role-bound-authentication/BE/05-testing/test-results.md`
- Frontend source project: `FE/FE-EvelS`

Allowed output directories:
- `FE/FE-EvelS/`
- `AI-Share-Documents/FE/04-coding-fe/`

Completion artifacts:
- `AI-Share-Documents/FE/04-coding-fe/coding-plan.md`
- `AI-Share-Documents/FE/04-coding-fe/coding-change-log.md`
- `AI-Share-Documents/FE/04-coding-fe/coding-self-check.md`
- Production frontend code changes under `FE/FE-EvelS/`

Mandatory preflight:
- Read `AI-Share-Documents/Analysis/03-system-design/sd-self-check.md` first.
- Read `AI-Share-Documents/Analysis/03-system-design/sd-data-design.md` before any other design artifact.
- Confirm `Frontend in scope: Yes`.
- If frontend is out of scope, write `Status: NOT_IN_SCOPE` to `AI-Share-Documents/FE/04-coding-fe/coding-self-check.md` and stop without source changes.
- If required System Design artifacts are missing, incomplete, contradictory, or not implementation-usable, stop and write the dependency gap in `coding-self-check.md`.
- Create `AI-Share-Documents/FE/04-coding-fe/coding-plan.md` before any frontend source change.

Mission:
Initialize the EvelS frontend project and implement role-specific authentication flows for Admin and Photographer using the current backend auth contracts.

Required implementation scope:
- Work only in `FE/FE-EvelS/` and `AI-Share-Documents/FE/04-coding-fe/`.
- Initialize the frontend project using the architecture and toolchain approved by System Design.
- Build Admin login with username/password only.
- Send Admin credentials to the existing backend Admin login contract.
- Redirect successful Admin login to the Admin dashboard/homepage.
- Build Photographer login with Google authentication only.
- Reuse existing backend Google callback flow.
- Implement callback handling and Photographer redirect after successful Google login.
- Persist authentication state according to System Design.
- Hydrate auth state on app startup.
- Protect routes from unauthenticated access.
- Prevent Admin users from accessing Photographer-only pages and Photographer users from accessing Admin-only pages.
- Implement role-based routing after login and on direct navigation.
- Show clear errors for failed Admin login, failed Google login/callback, disabled account responses, network errors, and unexpected auth responses.
- Provide dashboard/homepage route shells where needed to complete the flow.
- Do not modify backend source.
- Do not create backend tests.

Required documentation outputs:
- `coding-plan.md` must list selected framework/tooling, target files, implementation order, state persistence approach, backend contract assumptions, and out-of-scope items.
- `coding-change-log.md` must record concrete code changes using `CD-*` IDs mapped to `SD-*`, `TL-*`, and `AC-*` where practical.
- `coding-self-check.md` must record preflight results, completed changes, verification performed, known gaps, and whether FE scope is complete.

Constraints:
- Follow existing project conventions where present.
- Do not edit Analysis artifacts.
- Do not edit Backend source.
- Do not edit `AI-Share-Documents/Template/`.
- Keep backend archive documents read-only.
- If backend contract details are uncertain, implement through a small isolated API client layer and document the assumption in `coding-plan.md` and `coding-self-check.md`.

## 5. Testing Frontend Prompt

Role running: `testing-fe`

Project name: `EvelS`

Feature name: `init-fe-auth-login`

Input files or folders:
- `AGENTS.md`
- `RULE.md`
- `SKILLS/Analysis/shared/WATERFALL_POLICY.md`
- `SKILLS/Analysis/shared/TRACEABILITY.md`
- `SKILLS/FE/testing-fe/`
- `AI-Share-Documents/FE/04-coding-fe/coding-plan.md`
- `AI-Share-Documents/FE/04-coding-fe/coding-change-log.md`
- `AI-Share-Documents/FE/04-coding-fe/coding-self-check.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-data-design.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-api-contract.md`
- Frontend source project: `FE/FE-EvelS`

Allowed output directories:
- `FE/FE-EvelS/`
- `AI-Share-Documents/FE/05-testing-fe/`

Completion artifacts:
- `AI-Share-Documents/FE/05-testing-fe/test-plan.md`
- `AI-Share-Documents/FE/05-testing-fe/test-cases.md`
- `AI-Share-Documents/FE/05-testing-fe/test-results.md`
- `AI-Share-Documents/FE/05-testing-fe/test-gaps.md`
- `AI-Share-Documents/FE/05-testing-fe/test-self-check.md`
- Frontend test code changes under `FE/FE-EvelS/`

Mandatory preflight:
- Read `AI-Share-Documents/FE/04-coding-fe/coding-self-check.md` and `AI-Share-Documents/FE/04-coding-fe/coding-plan.md` before writing tests.
- Confirm FE coding completed successfully and frontend is in scope.
- If FE coding is marked `Status: NOT_IN_SCOPE`, write `Status: NOT_IN_SCOPE` to `AI-Share-Documents/FE/05-testing-fe/test-self-check.md` and stop.
- If required FE coding artifacts are missing, incomplete, or contradictory, stop and write the dependency gap in `test-self-check.md`.

Mission:
Create and run frontend tests for the initialized EvelS frontend authentication flows and role-based routing behavior.

Required testing scope:
- Use stable test IDs such as `TC-001`, mapped to `CD-*`, `SD-*`, `AC-*`, and `FR-*` where practical.
- Test Admin username/password login success and redirect to Admin dashboard/homepage.
- Test Admin login error display for failed credentials or backend error response.
- Test Photographer Google login initiation using the backend callback flow.
- Test Google callback success handling, auth persistence, and redirect to Photographer dashboard/homepage.
- Test Google callback failure/error display.
- Test auth state hydration from persisted storage.
- Test unauthenticated users are blocked from protected routes.
- Test wrong-role users are blocked from pages outside their assigned role.
- Test logout clears auth state and prevents protected access.
- Test malformed or missing auth response handling where practical.
- Run available frontend validation commands such as typecheck, lint, unit tests, component tests, or build according to the project tooling.

Required outputs:
- `test-plan.md` must describe testing strategy, levels, tools, commands, scope, and exclusions.
- `test-cases.md` must enumerate traceable test cases.
- `test-results.md` must record executed commands and outcomes.
- `test-gaps.md` must record any untested or partially tested behavior with reasons.
- `test-self-check.md` must confirm preflight status, artifact completeness, test execution, and residual risks.

Constraints:
- Write tests only in approved frontend test locations under `FE/FE-EvelS/`.
- Write testing artifacts only under `AI-Share-Documents/FE/05-testing-fe/`.
- Do not modify backend source or backend tests.
- Do not edit Analysis artifacts.
- Do not edit `AI-Share-Documents/Template/`.
- If a production code defect blocks tests, document it clearly before making any minimal FE-only fix, and record it in test results and gaps.

## 6. Workflow Archiver Prompt

Role running: `workflow-archiver`

Project name: `EvelS`

Feature name: `init-fe-auth-login`

Input files or folders:
- `AI-Share-Documents/Analysis/`
- `AI-Share-Documents/BE/`
- `AI-Share-Documents/FE/`
- `AI-Share-Documents/Template/Analysis/`
- `AI-Share-Documents/Template/BE/`
- `AI-Share-Documents/Template/FE/`

Allowed output directories:
- `AI-Share-Documents/Archive/init-fe-auth-login/`
- `AI-Share-Documents/Analysis/`
- `AI-Share-Documents/BE/`
- `AI-Share-Documents/FE/`

Completion artifacts:
- `AI-Share-Documents/Archive/init-fe-auth-login/Analysis/`
- `AI-Share-Documents/Archive/init-fe-auth-login/BE/`
- `AI-Share-Documents/Archive/init-fe-auth-login/FE/`
- `AI-Share-Documents/Archive/init-fe-auth-login/Feature_Report.md`
- Fresh active workflow folders restored from:
  - `AI-Share-Documents/Template/Analysis/`
  - `AI-Share-Documents/Template/BE/`
  - `AI-Share-Documents/Template/FE/`

Mandatory preflight:
- Run only after the last in-scope testing phase is complete.
- For this expected `FE_ONLY` workflow, require completed FE testing artifacts under `AI-Share-Documents/FE/05-testing-fe/`.
- Confirm Analysis artifacts exist through System Design.
- Confirm any out-of-scope BE folders either contain no required work or contain appropriate `NOT_IN_SCOPE` self-checks if BE roles were invoked.

Mission:
Archive the completed `init-fe-auth-login` feature workflow and reset active shared document folders for the next feature.

Required archive and reset behavior:
1. Pack `AI-Share-Documents/Analysis/`, `AI-Share-Documents/BE/`, and `AI-Share-Documents/FE/` into `AI-Share-Documents/Archive/init-fe-auth-login/`.
2. Write `AI-Share-Documents/Archive/init-fe-auth-login/Feature_Report.md` summarizing:
   - project and feature
   - implementation scope
   - completed Analysis artifacts
   - completed FE coding/testing artifacts
   - backend out-of-scope status
   - key verification results
   - known gaps and follow-ups
3. Copy `AI-Share-Documents/Template/Analysis/`, `AI-Share-Documents/Template/BE/`, and `AI-Share-Documents/Template/FE/` back into `AI-Share-Documents/` to prepare for the next feature.

Constraints:
- Do not modify source code in `BE/` or `FE/`.
- Do not edit archived historical documents except the new `AI-Share-Documents/Archive/init-fe-auth-login/` package.
- Do not use kit-local `docs/` or `ai-docs/` for workflow artifacts.
- Preserve the active folder contract from `AGENTS.md`.
