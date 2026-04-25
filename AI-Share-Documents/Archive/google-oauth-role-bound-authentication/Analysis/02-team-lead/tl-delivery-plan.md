# Team Lead Delivery Plan

## Role Running
`team-lead`

## Input Files
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `ai-docs/00-project/project-context.md`
- `ai-docs/00-project/constraints.md`
- `ai-docs/00-project/glossary.md`
- `ai-docs/01-business-analysis/ba-feature-spec.md`
- `ai-docs/01-business-analysis/ba-acceptance-criteria.md`
- `ai-docs/01-business-analysis/ba-open-questions.md`
- `ai-docs/01-business-analysis/ba-self-check.md`

## Allowed Output Directories
- `ai-docs/02-team-lead/`

## Completion Artifacts
- `ai-docs/02-team-lead/tl-delivery-plan.md`
- `ai-docs/02-team-lead/tl-task-breakdown.md`
- `ai-docs/02-team-lead/tl-risk-log.md`
- `ai-docs/02-team-lead/tl-handoff.md`
- `ai-docs/02-team-lead/tl-self-check.md`

## Preflight Result
Status: PASSED

`ai-docs/01-business-analysis/ba-open-questions.md` was read before other BA artifacts. No question entry is marked `Status: OPEN`. All unresolved details are explicitly marked `Status: ASSUMED` and are carried into `tl-risk-log.md` and `tl-handoff.md`.

## Delivery Goal
Prepare System Design, Coding, and Testing for a secure authentication feature where only `Admin` and `Photographer` can authenticate, Photographer onboarding happens through Google OAuth only, Admin onboarding remains manual-only, account roles are immutable, disabled accounts cannot log in, and authentication output is ready for later authorization integration.

## Scope Summary
In scope:
- Google OAuth authentication for Photographer users.
- First-login Photographer account creation when no matching account exists.
- Subsequent-login authentication of matching existing accounts.
- Immutable account role enforcement.
- Admin manual-only account boundary.
- Denial or absence of Admin public registration and Admin Google onboarding.
- Denial or absence of email/password authentication.
- Customer users remaining unauthenticated and accountless.
- Backend-side Google token validation and server-side trust boundary.
- Disabled-account login prevention.
- Authentication output suitable for later authorization rules.

Out of scope:
- Email/password authentication.
- Admin self-registration or public Admin registration.
- Google OAuth creation of Admin accounts.
- Customer account creation, registration, or login.
- Role upgrade and downgrade workflows.
- Full authorization policy implementation for later protected business actions.

## Milestones

### Milestone 1: System Design Readiness
Objective: Produce implementation-usable design artifacts from BA scope and Team Lead handoff.

Expected System Design outcomes:
- Define authentication flow boundaries.
- Define account lookup and first-login creation behavior.
- Define immutable role rules and failure behavior.
- Define disabled-account denial behavior.
- Define backend-only trust boundaries for Google identity and role data.
- Define authorization-readiness expectations.
- Produce mandatory implementation-usable `sd-data-design.md`.

Primary tasks: `TL-001` through `TL-008`, `TL-010`, `TL-011`

### Milestone 2: Backend Implementation Readiness
Objective: Enable Coding Backend to implement only after System Design has completed all required artifacts, especially usable data design.

Expected Coding outcomes:
- Create `coding-plan.md` before code changes.
- Implement the approved authentication flow in `src/`.
- Preserve explicit out-of-scope flows.
- Record code changes and self-check.

Primary tasks: `TL-009`, `TL-012`, `TL-013`

### Milestone 3: Backend Test Readiness
Objective: Enable Testing Backend to validate behavior against BA acceptance criteria and coding change scope.

Expected Testing outcomes:
- Validate `coding-plan.md` and `coding-self-check.md`.
- Add tests under `test/` for first login, repeated login, role immutability, disabled accounts, unsupported flows, and server-side trust boundaries.
- Record test plan, cases, results, gaps, and self-check.

Primary tasks: `TL-014`, `TL-015`

## Dependency Order
1. `team-lead` completes this planning package.
2. `system-design` consumes Team Lead artifacts and produces all required design artifacts.
3. `system-design` must produce a non-placeholder, implementation-usable `sd-data-design.md`.
4. `coding-be` preflights System Design artifacts and creates `coding-plan.md` before code changes.
5. `coding-be` implements production changes only under `src/`.
6. `testing-be` preflights Coding artifacts and writes tests only under `test/`.
7. `workflow-archiver` may run only after Testing Backend completes.

## Assumptions Carried Forward
- `BAQ-001`: Manual Admin provisioning is internal and non-public; exact mechanism deferred.
- `BAQ-002`: Post-auth token or session format is deferred to System Design.
- `BAQ-003`: Google account matching fields and uniqueness rules are deferred to System Design.
- `BAQ-004`: Disabled-account error response details are deferred to System Design.
- `BAQ-005`: Existing non-Photographer account with matching Google identity must not have its role mutated; exact permitted/denied login behavior is deferred to System Design.

## Key Risks
- Ambiguous Admin provisioning can cause accidental public Admin creation paths if not constrained in design.
- Ambiguous session/token contract can delay coding if System Design does not define enough API behavior.
- Ambiguous Google account matching can create duplicate accounts or wrong-account authentication if not resolved in data design.
- Existing non-Photographer account behavior with Google identity requires careful design to avoid conflict between role immutability and Google onboarding constraints.
- Client-trust mistakes could allow role spoofing or identity spoofing if implementation does not centralize server-side validation.

## Phase Boundaries
- Team Lead writes only planning artifacts under `ai-docs/02-team-lead/`.
- System Design writes only design artifacts under `ai-docs/03-system-design/`.
- Coding Backend writes production code only under `src/` and coding artifacts under `ai-docs/04-coding/`.
- Testing Backend writes tests only under `test/` and testing artifacts under `ai-docs/05-testing/`.
