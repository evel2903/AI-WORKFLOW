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
Photographer Credit Management

## Business Goal
Introduce a dedicated `credit` attribute for `Photographer` users so the system can store a future-ready credit value, initialize it automatically for photographer onboarding, and allow administrators to manage it manually without introducing broader credit business logic in this phase.

## Problem Statement
The system needs a role-bound credit attribute that applies only to `Photographer` users, remains unavailable to other roles, and can be initialized and maintained in a controlled way. Without explicit business rules, later implementation could incorrectly expose credit to ineligible roles, reset values on login, or broaden scope into unsupported credit workflows.

## Actors
- `Admin`: Authenticated administrative user who is the only actor allowed to manually update photographer credit.
- `Photographer`: Authenticated user role that is the only role eligible to own a credit value.
- `Other Non-Photographer User`: Any user with a role other than `Photographer`; this actor is ineligible for credit ownership and must not have usable credit.
- `System`: Backend service responsible for enforcing role eligibility, initializing default photographer credit on first login, persisting credit values, and protecting manual credit update behavior with server-side authorization.

## In Scope
- Add a dedicated `credit` attribute for users with role `Photographer`.
- Enforce that only `Photographer` users can own a usable credit value.
- Ensure users with roles other than `Photographer` do not have usable credit.
- Initialize `credit = 10` automatically when a photographer logs in for the first time.
- Allow `Admin` users to manually update credit for `Photographer` users.
- Preserve stored credit for later future-feature use.
- Enforce role-based credit eligibility and update authorization with server-side rules.
- Define business expectations for legacy photographer records that do not yet have credit through explicit assumptions.

## Out Of Scope
- Credit increase workflows driven by business events.
- Credit decrease or spending workflows.
- Credit recharge, purchase, or top-up workflows.
- Credit expiration, rollover, or scheduled reset workflows.
- Transaction history, ledger, audit history, or reporting specific to credit.
- Photographer self-service credit updates.
- Manual credit updates by non-admin roles.
- Broad redesign of authentication, user lifecycle, or unrelated user profile features.
- Detailed technical implementation choices such as endpoint shapes, persistence schema, or DTO structures.

## Functional Requirements
- `FR-001`: The system must support a dedicated `credit` attribute for users with role `Photographer`.
- `FR-002`: The system must ensure users with roles other than `Photographer` do not have a usable credit value.
- `FR-003`: The system must allow only `Admin` users to manually update credit for users with role `Photographer`.
- `FR-004`: The system must not allow non-admin actors to manually update photographer credit.
- `FR-005`: When a photographer logs in for the first time, the system must automatically initialize `credit = 10`.
- `FR-006`: After photographer credit has been initialized or manually updated, subsequent logins must preserve the stored credit value and must not incorrectly reset it to the default.
- `FR-007`: The system must reject, deny, or otherwise prevent assigning or manually updating credit for a user whose role is not `Photographer`.
- `FR-008`: The system must store photographer credit in a way that keeps the value available for future feature use.
- `FR-009`: The system must not implement credit spending, credit recharge, automated credit increase, automated credit decrease, expiration, or other broader credit business workflows in this phase.
- `FR-010`: The system must preserve the photographer-only credit invariant across relevant create, login, read, and update behaviors in this phase.

## Non-Functional Requirements
- `NFR-001`: Credit eligibility must be enforced using server-side role data rather than client-provided claims or client-chosen field values.
- `NFR-002`: Authorization for manual credit updates must be enforced server-side so only authenticated `Admin` users can perform the operation.
- `NFR-003`: Credit initialization and update behavior must preserve data integrity and must not accidentally overwrite an existing stored credit value during later logins.
- `NFR-004`: Unsupported credit operations must fail safely without mutating stored credit or creating usable credit for ineligible roles.
- `NFR-005`: The feature must remain scoped to credit storage and admin-managed updates only, so downstream phases do not expand into a credit engine.
- `NFR-006`: The feature must remain compatible with future feature work that consumes stored photographer credit.

## Business Rules
- `BR-001`: Only users with role `Photographer` are eligible to own a usable credit value.
- `BR-002`: Users with roles other than `Photographer` must not have usable credit, even if the eventual technical representation is `null`, omitted, hidden, or otherwise non-usable.
- `BR-003`: `Admin` is the only actor allowed to manually update photographer credit.
- `BR-004`: Photographer credit must initialize to `10` on first successful photographer login.
- `BR-005`: Default credit initialization is a one-time onboarding behavior and is not a recurring reset rule for subsequent logins.
- `BR-006`: This phase stores credit and permits admin-managed updates only; it does not include broader credit lifecycle logic.
- `BR-007`: Server-side role and persisted user state are authoritative for credit eligibility and update decisions.

## Dependencies
- The backend has or will have a server-side user/account model that distinguishes `Photographer` from other roles.
- The backend has or will have an authenticated admin capability that can authorize manual updates to eligible users.
- The existing photographer login flow is available to host first-login credit initialization behavior.
- Later phases or future features may consume stored photographer credit, so this feature must preserve the value as a stable business attribute.

## Assumptions
- `ASM-001`: This feature extends the existing user and authentication flow rather than introducing a standalone credit subsystem.
- `ASM-002`: The exact representation of non-photographer credit in API responses is a downstream design decision, provided non-photographer roles do not have usable credit.
- `ASM-003`: For photographer records that predate this feature and do not yet have credit, downstream phases may treat first successful login without an initialized credit value as the event that assigns the default `10`.
- `ASM-004`: Admin manual credit updates are intended for a direct admin-controlled backend operation, but the exact API contract is deferred to downstream planning and design.
- `ASM-005`: Numeric validation details for admin-set credit values are not specified in the request and must be carried forward explicitly from `ba-open-questions.md`.

## Handoff Notes
- Team Lead must carry all `ASSUMED` items from `ba-open-questions.md` into planning, risks, and handoff artifacts.
- Downstream phases must preserve the photographer-only credit invariant and must not expand scope into transactions, spending, recharge, or other credit-engine behavior.
- System Design must make legacy photographer handling, response visibility, and numeric validation implementation-usable without violating the business rules above.
