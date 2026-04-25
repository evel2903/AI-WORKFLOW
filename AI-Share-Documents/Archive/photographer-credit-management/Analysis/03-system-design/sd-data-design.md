# System Design Data Design

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

## Decision
- [x] New or changed data design is required
- [ ] No new data design is required; existing structures are reused

## Data Design Case
This feature should extend the existing persisted user/account structure rather than create a separate credit table or ledger.

Reason:
- `Credit` is a direct role-bound attribute of `Photographer`.
- This phase requires storage and admin-managed updates only.
- Separate ledger or transaction tables would expand scope beyond approved business rules.

## Entity Or Aggregate Candidate: UserAccount
Design ID: `SD-026`

Maps to: `TL-001`, `TL-002`, `TL-003`, `TL-004`, `TL-006`, `FR-001` through `FR-010`, `AC-001` through `AC-010`

Responsibility:
- Owns persisted role and `Credit`.
- Serves as source of truth for eligibility and update behavior.

Persistence mapping direction:
- Extend the existing user/account ORM entity used by the repository.

Required field change:
- Add `Credit`

Suggested field shape:
- `Credit`: integer, nullable

Field semantics:
- `Credit = null` for all non-photographer roles.
- `Credit = null` may temporarily exist for legacy photographer records before first successful post-rollout login.
- `Credit >= 0` when present.

## Relationships And Ownership
- Relationship: `UserAccount` owns `Credit` directly.
- Ownership boundary: credit is not extracted to a separate aggregate, subsystem, or history record in this phase.

## Persistence Mapping
- Domain model to persistence model mapping:
  - `UserAccount.Credit` -> nullable integer column on the existing user/account persistence model
  - `UserAccount.Role` remains the source of truth for credit eligibility
- Table or collection names:
  - reuse the existing user/account table used by the repository
  - Coding must document the concrete table/entity name in `coding-plan.md` after inspecting `src/`

## Constraints

### Required fields
- `Role` remains required according to the existing account model.
- `Credit` is nullable because:
  - non-photographer roles must not have usable credit
  - legacy photographers may exist before initialization

### Uniqueness
- No uniqueness rule is required for `Credit`.

### Validation Or Integrity Constraints
Design ID: `SD-027`

Maps to: `TL-001`, `TL-002`, `TL-003`, `TL-004`, `TL-006`, `FR-002`, `FR-005`, `FR-006`, `FR-007`, `FR-010`, `NFR-003`, `NFR-004`, `AC-002`, `AC-005`, `AC-006`, `AC-007`

Required integrity rules:
- If `Role != Photographer`, `Credit` must be `null`.
- If `Credit` is not `null`, it must be an integer `>= 0`.
- First successful photographer login initializes `Credit = 10` only when current `Credit` is `null`.
- Later logins must not overwrite non-null `Credit`.

Preferred enforcement:
1. Domain/Application layer is mandatory:
   - login initialization logic
   - admin update validation
   - target role eligibility checks
2. Database constraint is recommended where supported:
   - check constraint such as:
     - `(Role = 'Photographer' AND Credit IS NULL OR Credit >= 0)`
     - `(Role <> 'Photographer' AND Credit IS NULL) OR (Role = 'Photographer' AND Credit IS NULL OR Credit >= 0)`
   - Coding may adapt exact SQL syntax to the database engine in use

## First-Login And Legacy Handling
Design ID: `SD-028`

Maps to: `TL-002`, `TL-006`, `FR-005`, `FR-006`, `FR-008`, `NFR-003`, `AC-005`, `AC-006`, `BAQ-002`

Behavior:
- New photographer created through the existing onboarding or login flow should persist with `Credit = 10` immediately.
- Existing photographer with `Credit = null` should be initialized to `10` during first successful login after rollout.
- Existing photographer with non-null `Credit` must keep the stored value.
- No mandatory bulk backfill migration is required by this design.

Reason:
- This follows the carried BA assumption and avoids introducing a separate migration-only business workflow.

## API Visibility Data Rule
Design ID: `SD-029`

Maps to: `TL-005`, `FR-002`, `FR-008`, `FR-010`, `AC-002`, `AC-008`, `BAQ-001`, `BAQ-004`

Persistence rule:
- `Credit` may physically exist as a nullable field on the user/account record.

Projection rule:
- DTOs for non-photographer users must omit `Credit` from response payloads.
- DTOs for photographer users may include `Credit` when the response context already returns user/account data or the admin update succeeds.

## Migration Notes
Design ID: `SD-030`

Maps to: `TL-006`, `RISK-002`, `RISK-003`, `RISK-008`

Migration required:
- Add nullable integer `Credit` column to the existing user/account table.
- Add a database check constraint for non-negative values where supported.
- Add a role-credit consistency constraint where supported and practical.

Migration behavior:
- Do not bulk assign `10` in the migration.
- Keep existing non-photographer rows with `Credit = null`.
- Keep existing photographer rows with `Credit = null` until first successful login if they have not yet been initialized.

## TypeOrmDataSource Updates
Design ID: `SD-031`

Maps to: `TL-006`, `RISK-008`

`TypeOrmDataSource` updates required:
- Register the new migration that adds `Credit` and any supporting constraints.
- Register any updated ORM entity metadata if the current entity class changes.

`App.module.ts` updates required:
- Only if coding introduces or wires a new admin credit update controller or provider in module registration.
- If existing `Users` and `Authentication` modules already exist, Coding should extend their wiring rather than create duplicate module structures.

## Reuse Notes
- Existing structures reused:
  - the current persisted user/account model
  - the current authentication success flow
- Why reuse is sufficient:
  - `Credit` is a direct attribute of the user/account, not a standalone business subsystem
  - initialization depends on existing login flow
  - admin updates align with existing user-management ownership boundaries
