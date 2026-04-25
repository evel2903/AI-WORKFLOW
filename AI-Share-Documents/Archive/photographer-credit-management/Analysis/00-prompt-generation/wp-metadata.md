# Prompt Generation Metadata

## Version
v1

## Generated At
2026-04-23

## Feature Name
photographer-credit-management

## Scope Summary
Prepare Waterfall prompts for a backend feature where:
- `credit` is a dedicated attribute for users with role `Photographer`
- users with other roles must not have usable credit
- admins can manually update photographer credit
- first successful photographer login initializes `credit = 10`
- this phase stores credit and exposes manual admin update capability only
- credit spending, recharge, increment/decrement engines, and broader credit workflows are out of scope

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
- This feature extends the existing backend user/authentication flow rather than introducing a new actor type or a standalone credit ledger.
- Credit is a stored attribute associated only with the `Photographer` role and must not become a general-purpose field for other roles.
- Admin permission to update photographer credit means an authenticated admin-only backend-managed operation will exist, but the exact endpoint or use case shape is deferred to downstream phases.
- First-time photographer login refers to the first successful photographer onboarding/authentication event in the current system flow.
- The exact representation for non-photographer credit (`null`, hidden, omitted, or structurally absent in some contracts) may be refined downstream, but the business invariant remains that non-photographer roles do not have usable credit.

## Known Gaps
- The request does not define the exact API contract for reading or updating credit.
- The request does not define whether existing photographer records created before this feature need migration or backfill behavior for `credit`.
- The request does not define whether photographer credit should appear in every user response or only in role-specific/admin contexts.
- The request does not define whether invalid admin updates should fail with validation errors, authorization errors, or domain-rule errors when the target user is not a photographer.
- The request does not define the exact persistence type or numeric constraints for `credit` beyond initializing to `10`.

## Notes
- Prompts are written to keep downstream agents inside the strict Waterfall workflow and repository boundaries.
- Prompts instruct downstream phases to formalize the photographer-only credit invariant, first-login initialization, and admin-only update behavior without expanding scope into broader credit business logic.
- Prompts instruct downstream phases to document ambiguities explicitly instead of silently deciding them.
