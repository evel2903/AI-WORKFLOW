# System Design Implementation Guidelines

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

## Coding Preflight Requirements
Before writing production code, Coding Backend must:
- Read all six System Design artifacts.
- Validate that `sd-data-design.md` is implementation-usable.
- Create `ai-docs/04-coding/coding-plan.md` before modifying `src/`.
- Record exactly which existing user/account and authentication structures are being reused.

## Module Structure Guidance
Design ID: `SD-032`

Maps to: `TL-009`, `TL-010`

Preferred module ownership:
- `src/Modules/Users`
  - ORM/entity change for `Credit`
  - repository and mapper updates
  - admin update controller/use case if user-management endpoints already live here
- `src/Modules/Authentication`
  - first-login initialization hook in successful photographer auth flow
  - auth-response projection update if current auth success response already returns user/account data

If the repository uses equivalent module names, Coding should adapt those names while keeping the responsibilities above.

## Implementation Sequence
Design ID: `SD-033`

Maps to: `TL-009`, `TL-010`, `SD-001` through `SD-031`

Recommended order:
1. Inspect the existing `Users` and `Authentication` modules and decide exact reuse points.
2. Extend the user/account domain and persistence model with nullable `Credit`.
3. Add migration and `TypeOrmDataSource` registration.
4. Implement non-negative integer validation and photographer-only eligibility checks.
5. Implement first-login initialization inside the successful photographer authentication flow.
6. Implement admin-only manual credit update use case and endpoint.
7. Update response projections so non-photographer payloads omit `Credit`.
8. Update module wiring in `src/App.module.ts` only if new providers/controllers need registration.
9. Run build and lint checks available to Coding Backend and document the results.

## Layer Responsibilities
- `Presentation`
  - validate `PATCH /users/:Id/credit` request shape
  - enforce admin guard
  - shape success and error envelopes
- `Application`
  - orchestrate initialization and admin update use cases
  - coordinate repository load and save behavior
- `Domain`
  - centralize eligibility, initialization, and numeric invariants
- `Infrastructure`
  - persist nullable `Credit`
  - apply migration and ORM updates

## Coding Constraints
Design ID: `SD-034`

Maps to: `TL-010`, `FR-002`, `FR-007`, `FR-009`, `NFR-001`, `NFR-002`, `NFR-004`, `NFR-005`

Required:
- Do not create a standalone credit ledger or transaction entity.
- Do not add spending, recharge, expiration, or automatic adjustment behavior.
- Do not allow non-admin code paths to mutate `Credit`.
- Do not let any non-photographer target end up with usable `Credit`.
- Do not trust client input to determine user role eligibility.

## Edge Cases
Design ID: `SD-035`

Maps to: `TL-002`, `TL-003`, `TL-004`, `TL-005`, `RISK-001` through `RISK-007`

Coding must handle:
- existing photographer with `Credit = null`
- existing photographer with already initialized credit
- admin update targeting a non-photographer
- admin update with negative or decimal values
- auth success responses where user/account payload is absent and therefore no new visibility surface should be invented
- non-photographer DTO shaping so `Credit` is omitted consistently

## Security Guardrails
Design ID: `SD-036`

Maps to: `TL-003`, `TL-004`, `FR-003`, `FR-004`, `NFR-001`, `NFR-002`, `AC-003`, `AC-004`, `AC-010`

Required:
- Use persisted server-side role as the source of truth for credit eligibility.
- Keep admin authorization checks server-side.
- Avoid public DTOs that accept role overrides for credit behavior.
- Keep target-role validation inside the update use case even if controller guards exist.

## Explicit Out Of Scope
- Writing production code during System Design.
- Writing tests during System Design.
- Credit spending, recharge, expiration, or automated adjustment.
- Credit transaction history or ledger.
- Non-admin credit management.
