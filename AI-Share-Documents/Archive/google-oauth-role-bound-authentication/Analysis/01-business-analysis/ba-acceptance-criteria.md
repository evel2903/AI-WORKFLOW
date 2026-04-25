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

### AC-001: Supported Authenticated Roles
Maps to: `FR-001`

Given the system is evaluating authenticated roles in this phase  
When an account is authenticated  
Then the authenticated role must be either `Admin` or `Photographer`

### AC-002: Admin Manual Creation Only
Maps to: `FR-002`, `FR-004`, `FR-012`

Given an Admin account is needed  
When account creation is considered  
Then the Admin account must be created only through a manual non-public process  
And no public self-registration flow may create an Admin account

### AC-003: Google OAuth Must Not Create Admin
Maps to: `FR-003`, `FR-005`, `NFR-002`, `NFR-004`

Given a user completes a Google OAuth login  
When the backend creates an account from that login  
Then the created account role must be `Photographer`  
And the backend must not create an `Admin` account from Google OAuth

### AC-004: First Google Login Creates Photographer
Maps to: `FR-005`, `FR-006`, `NFR-001`, `NFR-003`

Given a valid Google OAuth login succeeds  
And no matching account exists  
When the backend completes authentication  
Then the backend must create a new account  
And the new account must have role `Photographer`

### AC-005: Existing Google Account Authenticates Without Role Change
Maps to: `FR-007`, `FR-008`, `FR-009`

Given a valid Google OAuth login succeeds  
And a matching account already exists  
When the backend completes authentication  
Then the backend must authenticate the existing account  
And the existing account role must remain unchanged

### AC-006: Role Is Immutable
Maps to: `FR-009`, `FR-010`

Given an account has already been created with a role  
When any login, registration, profile, account, or client-driven operation occurs  
Then the operation must not upgrade, downgrade, or otherwise change the account role

### AC-007: Email Password Authentication Is Unsupported
Maps to: `FR-011`, `NFR-007`

Given a user attempts email/password authentication  
When the request reaches the backend  
Then the system must not authenticate through email/password  
And the system must not create an account through email/password

### AC-008: Customer Users Do Not Authenticate
Maps to: `FR-013`, `FR-014`

Given a Customer user is using the system in this phase  
When authentication or registration is considered  
Then the Customer must not have an account  
And the Customer must not go through login or registration

### AC-009: Backend Validates Google Token
Maps to: `NFR-001`, `NFR-003`, `NFR-004`

Given a Google OAuth login request is submitted  
When the backend receives token or identity material  
Then the backend must validate the Google OAuth token before trusting identity data  
And authentication must not succeed using unvalidated client-provided identity data

### AC-010: Client Role Input Is Not Trusted
Maps to: `NFR-002`, `NFR-004`, `BR-011`

Given a client submits role data or authorization-relevant identity data  
When the backend processes authentication or account creation  
Then the backend must ignore client-provided role data for role decisions  
And must rely only on server-side account data and validated provider identity

### AC-011: Disabled Accounts Cannot Log In
Maps to: `FR-015`, `NFR-006`

Given an account is disabled  
When a login attempt is made for that account  
Then the backend must deny login  
And the backend must not complete authentication for the disabled account

### AC-012: Authentication Is Authorization-Ready
Maps to: `FR-016`, `NFR-005`

Given a user has successfully authenticated  
When later authorization rules need to evaluate access  
Then the authentication outcome must provide server-side account and role data suitable for later authorization integration

### AC-013: Unsupported Flows Fail Safely
Maps to: `FR-003`, `FR-010`, `FR-011`, `FR-012`, `FR-013`, `FR-014`, `NFR-007`

Given an unsupported authentication, registration, or role-change flow is attempted  
When the backend processes the attempt  
Then the system must not create unsupported accounts  
And must not authenticate through unsupported methods  
And must not change any existing account role
