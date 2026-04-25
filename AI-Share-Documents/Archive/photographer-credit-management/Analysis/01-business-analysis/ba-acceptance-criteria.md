# Business Analysis Acceptance Criteria

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

## Acceptance Criteria

### AC-001: Photographer Credit Eligibility
Maps to: `FR-001`, `FR-010`

Given the system is evaluating whether a user can own credit  
When the user role is `Photographer`  
Then the system must treat the user as eligible for a dedicated credit value

### AC-002: Non-Photographer Credit Ineligibility
Maps to: `FR-002`, `FR-007`, `FR-010`, `BR-002`

Given the system is evaluating a user whose role is not `Photographer`  
When credit ownership, display, or update behavior is processed  
Then the user must not have usable credit  
And the system must preserve that invariant regardless of whether the downstream representation is `null`, omitted, hidden, or otherwise non-usable

### AC-003: Admin Can Update Photographer Credit
Maps to: `FR-003`, `NFR-002`

Given an authenticated `Admin` user targets a user whose role is `Photographer`  
When the admin performs a manual credit update through the approved backend operation  
Then the system must allow the update  
And the stored photographer credit must reflect the approved new value

### AC-004: Non-Admin Cannot Update Photographer Credit
Maps to: `FR-004`, `NFR-002`, `NFR-004`

Given a non-admin actor attempts to manually update photographer credit  
When the request reaches the backend  
Then the system must deny the operation  
And the stored credit value must remain unchanged

### AC-005: First Photographer Login Initializes Default Credit
Maps to: `FR-005`, `NFR-003`

Given a user with role `Photographer` successfully logs in for the first time  
And no credit has yet been initialized for that photographer under the approved business rules  
When the login completes  
Then the system must automatically initialize the photographer credit to `10`

### AC-006: Subsequent Logins Preserve Existing Credit
Maps to: `FR-006`, `NFR-003`, `BR-005`

Given a photographer already has a stored credit value  
When the photographer logs in again  
Then the system must preserve the existing stored credit  
And must not incorrectly reset the credit to the default `10`

### AC-007: Non-Photographer Credit Assignment Is Prevented
Maps to: `FR-007`, `NFR-001`, `NFR-004`

Given a target user role is not `Photographer`  
When a create, login, or manual update path would otherwise assign or modify credit  
Then the system must reject, deny, or otherwise prevent that credit assignment or update  
And the target must not end up with usable credit

### AC-008: Credit Storage Is Future-Ready
Maps to: `FR-008`, `NFR-006`

Given photographer credit has been initialized or manually updated  
When the system persists the user state  
Then the photographer credit must remain stored as a stable business attribute for future feature use

### AC-009: Broader Credit Engine Behavior Remains Out Of Scope
Maps to: `FR-009`, `NFR-005`

Given this feature is implemented  
When credit-related behavior is reviewed  
Then the feature must be limited to storing photographer credit and allowing admin-managed updates  
And it must not introduce spending, recharge, expiration, automatic increase, automatic decrease, or broader credit-engine workflows

### AC-010: Server-Side Role Data Controls Credit Rules
Maps to: `FR-010`, `NFR-001`, `NFR-002`

Given a client submits role-related or credit-related input  
When the backend evaluates credit eligibility or update authorization  
Then the backend must rely on server-side role and user state as the source of truth  
And client input must not override role eligibility or admin authorization rules
