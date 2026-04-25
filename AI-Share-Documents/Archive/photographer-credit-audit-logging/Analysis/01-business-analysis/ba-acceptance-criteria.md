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

### AC-001: Credit Remains Photographer-Only
Maps to: `FR-001`, `FR-009`, `BR-001`, `BR-006`

Given the system is evaluating who can own usable credit  
When the user's role is `Photographer`  
Then the system must continue to treat that user as eligible for credit  
And users with any other role must not gain usable credit through this enhancement

### AC-002: Only Admins Can Update Photographer Credit
Maps to: `FR-002`, `NFR-001`, `BR-002`

Given a request to update photographer credit  
When the acting user is not an authorized `Admin`  
Then the system must deny the update  
And no successful credit-change audit record must be created

### AC-003: Real Admin Credit Change Creates Audit Log
Maps to: `FR-003`, `NFR-002`, `NFR-004`, `BR-003`

Given an authorized `Admin` updates the credit of a valid `Photographer` target  
And the requested credit value changes the stored credit value  
When the update succeeds  
Then the system must create an audit log record for that credit change

### AC-004: Audit Log Captures Acting Admin Identity
Maps to: `FR-004`, `NFR-003`

Given a successful admin-driven photographer credit change requires an audit log  
When the audit log record is created  
Then the record must contain the acting admin's user ID  
And the record must contain the acting admin's name

### AC-005: Audit Log Captures Target Photographer Identity
Maps to: `FR-005`, `NFR-003`

Given a successful admin-driven photographer credit change requires an audit log  
When the audit log record is created  
Then the record must contain the target photographer's user ID  
And the record must contain the target photographer's name

### AC-006: Audit Log Captures Before-And-After Credit Values
Maps to: `FR-006`, `NFR-003`

Given a successful admin-driven photographer credit change requires an audit log  
When the audit log record is created  
Then the record must contain the previous credit value  
And the record must contain the new credit value

### AC-007: Audit Log Captures Action Time
Maps to: `FR-007`, `NFR-003`

Given a successful admin-driven photographer credit change requires an audit log  
When the audit log record is created  
Then the record must contain the timestamp for the action

### AC-008: No Audit Log For No-Op Update
Maps to: `FR-008`, `NFR-002`, `BR-004`

Given an authorized `Admin` submits a credit update for a valid `Photographer` target  
And the requested credit value matches the current stored credit value  
When the system processes the request  
Then the system must not create an audit log record for that request

### AC-009: Non-Photographer Targets Cannot Produce Successful Credit-Update Audit Events
Maps to: `FR-009`, `NFR-001`, `NFR-002`, `BR-006`

Given a credit update request targets a user whose role is not `Photographer`  
When the system evaluates the request  
Then the system must prevent a successful photographer credit update outcome  
And the system must not create a successful credit-change audit log for that target

### AC-010: Traceability Scope Stays Narrow
Maps to: `FR-010`, `NFR-005`, `NFR-006`, `BR-005`

Given this enhancement is implemented  
When the feature is reviewed  
Then it must provide traceability and accountability for admin photographer-credit changes  
And it must not introduce broader credit workflows or a general-purpose audit-history platform in this phase
