# System Design Solution Overview

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

## Preflight Result
Status: PASSED

`ai-docs/02-team-lead/tl-self-check.md` was read first and confirms Team Lead readiness for System Design.

## Design Goal
Design a NestJS Clean Architecture authentication module that supports Google OAuth authentication for Photographer accounts, preserves manual-only Admin account boundaries, prevents role mutation after account creation, denies disabled-account login, validates Google identity server-side, and emits server-side account/role data that later authorization rules can consume.

## Proposed Module Boundary
Target module: `src/Modules/Auth`

Layer alignment:
- `Presentation`: receives authentication requests and returns response envelopes.
- `Application`: orchestrates Google login, account lookup/creation, disabled checks, and token/session issuance.
- `Domain`: owns account role, account status, provider identity, and immutable-role invariants.
- `Infrastructure`: validates Google OAuth tokens, persists accounts and provider identities, and issues application auth tokens if applicable.

If the codebase already contains an authentication or user/account module, Coding must reuse or extend the existing module while preserving this boundary. Do not create duplicate account concepts if equivalent structures already exist.

## Key Design Decisions
- `SD-001`: Use server-side account records as the only source of truth for role, account status, and authorization-ready identity data. Maps to `TL-001`, `TL-007`, `FR-001`, `FR-016`, `NFR-002`, `NFR-004`, `AC-001`, `AC-010`, `AC-012`.
- `SD-002`: Model roles as a closed set containing only `Admin` and `Photographer` for authenticated users in this phase. Maps to `TL-001`, `FR-001`, `AC-001`.
- `SD-003`: Treat Admin provisioning as an internal/manual-only account creation path outside public registration and outside Google OAuth onboarding. Maps to `TL-002`, `FR-002`, `FR-003`, `FR-004`, `FR-012`, `AC-002`, `AC-003`, `BAQ-001`.
- `SD-004`: Allow Google OAuth account creation only for new Photographer accounts with default role `Photographer`. Maps to `TL-003`, `FR-005`, `FR-006`, `NFR-001`, `AC-003`, `AC-004`.
- `SD-005`: Authenticate existing Google-linked accounts without changing their stored role. Maps to `TL-004`, `FR-007`, `FR-008`, `FR-009`, `AC-005`, `BAQ-003`.
- `SD-006`: Enforce role immutability as a domain invariant after account creation. Maps to `TL-005`, `FR-009`, `FR-010`, `AC-006`, `AC-013`.
- `SD-007`: Deny login for disabled accounts before issuing any success response or application auth token. Maps to `TL-008`, `FR-015`, `NFR-006`, `AC-011`, `BAQ-004`.
- `SD-008`: Validate Google OAuth tokens in the backend and derive identity only from verified token claims. Maps to `TL-007`, `NFR-001`, `NFR-003`, `NFR-004`, `AC-009`.
- `SD-009`: Reject or omit unsupported authentication and registration flows, including email/password auth, Admin public registration, Admin Google onboarding, and Customer auth. Maps to `TL-006`, `FR-011`, `FR-012`, `FR-013`, `FR-014`, `NFR-007`, `AC-007`, `AC-008`, `AC-013`.
- `SD-010`: Return authentication output that includes authorization-ready server-side account identity, role, and status-derived eligibility, without implementing full authorization policy rules. Maps to `TL-010`, `FR-016`, `NFR-005`, `AC-012`, `BAQ-002`.

## Main Google Login Flow
1. Client sends a Google ID token to the backend Google login endpoint.
2. Backend validates the token with the configured Google token verifier.
3. Backend extracts verified Google subject, email, email verification state, display name, and avatar URL if available.
4. Backend looks up an account provider identity using provider `Google` and verified Google subject.
5. If no provider identity exists, backend creates a new account with role `Photographer`, active status, and linked Google identity.
6. If provider identity exists, backend loads the existing account and does not modify the account role.
7. Backend denies login if the account is disabled.
8. Backend issues the approved application auth result using only server-side account data.

## Existing Non-Photographer Google Match Resolution
The handoff carries `BAQ-005`. This design resolves it as follows: if a verified Google identity is already linked to an existing account whose role is `Admin`, the login may authenticate the existing Admin only if that Admin account was manually provisioned and explicitly linked to that Google identity by an internal process. The Google login flow must not create an Admin account and must not convert any role. If no internal link exists, Google login must not create or infer Admin access.

This preserves the rules that Admin accounts are manual-only, Google OAuth never creates Admin accounts, and roles are immutable.

## Out Of Scope
- Email/password authentication.
- Public registration for Admin accounts.
- Google OAuth creation of Admin accounts.
- Customer account creation, login, or registration.
- Role upgrade or downgrade workflows.
- Full authorization policy implementation for protected business actions.
