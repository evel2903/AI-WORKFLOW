# Prompt Generation Metadata

## Version
v1

## Generated At
2026-04-23

## Feature Name
google-oauth-role-bound-authentication

## Scope Summary
Prepare Waterfall prompts for a backend authentication feature where:
- only `Admin` and `Photographer` are authenticated roles
- `Admin` accounts are manual-only
- `Photographer` accounts are created only through Google OAuth
- Google first login creates a `Photographer` account if absent
- subsequent Google logins authenticate the existing account without role changes
- roles are immutable after account creation
- email/password authentication is out of scope
- customer users are anonymous in this phase
- server-side validation and disabled-account enforcement are mandatory

## Traceability Strategy
- Functional requirements should use `FR-*`
- Non-functional requirements should use `NFR-*`
- Acceptance criteria should use `AC-*`
- Team tasks should use `TL-*`
- System design items should use `SD-*`
- Coding change items should use `CD-*`
- Test cases should use `TC-*`
- Preferred chain is `FR -> AC -> TL -> SD -> CD -> TC`

## Assumptions
- Google OAuth is already an accepted identity provider for this product phase, and the downstream phases should refine implementation details rather than revisit provider selection.
- Manual Admin account creation means an internal or operational backend-managed path exists or will be designed, but no public self-registration or Google OAuth-based Admin onboarding is allowed.
- Disabled account handling applies to both Admin and Photographer accounts.
- Later-phase authorization integration means the authentication design should preserve role and account-status data in a form usable by guards, policies, or equivalent authorization mechanisms.
- If repository-specific naming differs, downstream agents should adapt artifacts to existing code conventions without changing the business rules above.

## Known Gaps
- The request does not define the exact manual Admin provisioning mechanism.
- The request does not define the exact token/session model after successful Google authentication.
- The request does not define the persistence schema, existing auth module location, or current user entity shape.
- The request does not define the disabled-account response contract or error codes.

## Notes
- Prompts are written to keep downstream agents inside strict Waterfall boundaries.
- Prompts instruct downstream phases to record ambiguities explicitly instead of silently resolving them.
