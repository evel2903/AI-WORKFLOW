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

### Aggregate: UserAccount
Design ID: `SD-011`

Maps to: `TL-001`, `TL-002`, `TL-003`, `TL-004`, `TL-006`, `FR-001` through `FR-010`, `AC-001` through `AC-010`

Purpose:
- Represents the persisted authenticated user/account state used by the current system.
- Owns `Role` and `Credit`.
- Enforces photographer-only credit eligibility and one-time default initialization behavior.

Candidate fields:
- `Id`
- `Role`
- `Email`
- `DisplayName`
- `Status` if already present in the existing user/account model
- `Credit`
- `CreatedAt`
- `UpdatedAt`

Invariants:
- `Credit` is eligible only when `Role = Photographer`.
- If `Role != Photographer`, `Credit` must be `null`.
- `Credit` must be a non-negative whole number when present.
- Default credit initialization occurs only when `Role = Photographer` and current `Credit` is `null`.
- Later logins must not overwrite an existing non-null `Credit`.

### Value Object: PhotographerCredit
Design ID: `SD-012`

Maps to: `TL-001`, `TL-002`, `TL-003`, `TL-006`, `FR-001`, `FR-005`, `FR-006`, `FR-008`, `NFR-003`, `AC-005`, `AC-006`, `AC-008`, `BAQ-003`

Purpose:
- Encapsulates valid stored credit semantics.

Rules:
- Allowed values are whole numbers `>= 0`.
- Default initialization value is `10`.
- This value object does not model spending, recharge, expiration, or arithmetic business workflows in this phase.

### Value Object: UserRole
Design ID: `SD-013`

Maps to: `TL-001`, `TL-004`, `FR-001`, `FR-002`, `FR-007`, `FR-010`, `AC-001`, `AC-002`, `AC-007`

Relevant rules for this feature:
- `Photographer` is the only role eligible for credit ownership.
- Any other role is ineligible for usable credit.
- Eligibility checks must rely on persisted server-side role data.

## Use Cases

### Use Case: EnsurePhotographerCreditInitializedOnLogin
Design ID: `SD-014`

Maps to: `TL-002`, `FR-005`, `FR-006`, `NFR-003`, `AC-005`, `AC-006`, `BAQ-002`

Purpose:
- Runs inside the successful photographer authentication flow.

Flow:
1. Receive an already authenticated user/account.
2. Check persisted `Role`.
3. If role is not `Photographer`, do nothing and preserve non-usable credit invariant.
4. If role is `Photographer` and `Credit` is `null`, assign default `10`.
5. If role is `Photographer` and `Credit` already exists, preserve the stored value unchanged.

Rules:
- Must be idempotent for already initialized users.
- Must not run as a recurring reset rule.
- Must support legacy photographers whose `Credit` is still `null`.

### Use Case: UpdatePhotographerCreditAsAdmin
Design ID: `SD-015`

Maps to: `TL-003`, `TL-004`, `FR-003`, `FR-004`, `FR-007`, `NFR-002`, `NFR-004`, `AC-003`, `AC-004`, `AC-007`

Purpose:
- Allows an admin to set a new credit value for a photographer user.

Flow:
1. Receive authenticated caller context and target user identifier.
2. Verify caller is authorized as `Admin`.
3. Load target user/account from persistence.
4. Reject if target role is not `Photographer`.
5. Validate requested credit value as non-negative whole number.
6. Persist the new value.
7. Return updated photographer credit result.

Rules:
- Caller authorization and target eligibility are separate checks and both are required.
- No non-admin actor may perform this use case.
- No target with role other than `Photographer` may complete this use case.

### Use Case: ProjectUserResponseWithCreditVisibility
Design ID: `SD-016`

Maps to: `TL-005`, `FR-002`, `FR-008`, `FR-010`, `AC-002`, `AC-008`, `BAQ-001`, `BAQ-004`

Purpose:
- Defines how credit appears in approved response contexts.

Rules:
- In photographer-specific or admin update success contexts, `Credit` may be returned.
- In non-photographer contexts, `Credit` must be omitted from the response payload rather than exposed as usable data.
- No new read-only endpoint is required solely to display credit if an existing appropriate response surface already exists.

## Repository Interfaces

### IUserRepository
Design ID: `SD-017`

Maps to: `TL-001`, `TL-002`, `TL-003`, `TL-006`, `FR-001` through `FR-008`, `AC-003` through `AC-008`

Expected operations:
- `FindById(UserId)`
- `Save(UserAccount)`
- `UpdateCredit(UserId, Credit)` if repository pattern uses explicit targeted updates

Rules:
- Repository contract must not allow setting usable credit on non-photographer users without application/domain validation.
- Repository contract must not include any broader credit-ledger behavior.

### IAuthenticationCreditInitializer
Design ID: `SD-018`

Maps to: `TL-002`, `FR-005`, `FR-006`, `AC-005`, `AC-006`

Expected responsibility:
- Invoked by the successful photographer authentication flow to ensure one-time default initialization semantics.

### IAdminCreditUpdateAuthorizer
Design ID: `SD-019`

Maps to: `TL-003`, `FR-003`, `FR-004`, `NFR-002`, `AC-003`, `AC-004`

Expected responsibility:
- Confirms caller is an authorized admin before update use case continues.

## Domain Errors
- `PhotographerCreditNotAllowedForRole`
- `PhotographerCreditValueInvalid`
- `PhotographerCreditUpdateForbidden`
- `PhotographerCreditTargetNotFound`

## Assumptions Carried Forward
- `BAQ-001`: API responses may omit non-photographer credit rather than expose a `null` field everywhere.
- `BAQ-002`: Legacy photographers may be initialized on first successful login after rollout if current `Credit` is still `null`.
- `BAQ-003`: Credit is modeled as a non-negative whole-number value.
- `BAQ-004`: Credit visibility is constrained to approved response contexts and not required universally.
