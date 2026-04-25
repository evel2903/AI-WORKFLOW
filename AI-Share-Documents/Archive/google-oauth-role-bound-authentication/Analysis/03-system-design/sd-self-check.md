# System Design Self Check

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
- [x] `ai-docs/03-system-design/sd-solution-overview.md`
- [x] `ai-docs/03-system-design/sd-domain-design.md`
- [x] `ai-docs/03-system-design/sd-api-contract.md`
- [x] `ai-docs/03-system-design/sd-data-design.md`
- [x] `ai-docs/03-system-design/sd-implementation-guidelines.md`
- [x] `ai-docs/03-system-design/sd-self-check.md`

## Preflight Validation
- [x] Read `ai-docs/02-team-lead/tl-self-check.md` before other Team Lead artifacts.
- [x] Confirmed Team Lead is not blocked.
- [x] Confirmed Team Lead is ready for System Design.
- [x] Proceeded only after preflight passed.

## Design Coverage
- [x] Solution overview created.
- [x] Domain design created.
- [x] API contract created.
- [x] Data design created.
- [x] Implementation guidelines created.
- [x] Stable `SD-*` IDs used.
- [x] `SD-*` IDs mapped to upstream `TL-*`, `FR-*`, `NFR-*`, and `AC-*` IDs where practical.
- [x] Manual-only Admin account boundary defined.
- [x] Google OAuth-only Photographer onboarding defined.
- [x] First-login Photographer account creation defined.
- [x] Subsequent-login existing account authentication without role mutation defined.
- [x] Role immutability defined as a domain and persistence invariant.
- [x] Disabled-account login denial defined.
- [x] Backend Google token validation defined.
- [x] Server-side-only identity and role trust boundary defined.
- [x] Authorization readiness defined without implementing full authorization policy.

## Data Design Validation
- [x] `sd-data-design.md` is present.
- [x] `sd-data-design.md` is not placeholder-only.
- [x] Entity/aggregate candidates are defined.
- [x] Persistence mapping direction is defined.
- [x] Relationships and ownership boundaries are defined.
- [x] Key constraints and uniqueness rules are defined.
- [x] Role immutability constraints are defined.
- [x] Disabled-account state handling is defined.
- [x] Migration notes are defined.
- [x] `TypeOrmDataSource` update notes are defined.

## Boundary Validation
- [x] Wrote only to `ai-docs/03-system-design/`.
- [x] Did not write production code.
- [x] Did not write tests.
- [x] Did not modify Team Lead artifacts.
- [x] Did not rely on client-provided role data anywhere in the design.

## Assumptions Carried Forward
- `BAQ-001`: Manual Admin provisioning remains internal and non-public.
- `BAQ-002`: Token/session contract is defined as an application auth result with server-derived account ID and role; Coding may align naming with existing repo conventions.
- `BAQ-003`: Google matching uses verified Google `sub` as primary provider identity key with unique `(Provider, ProviderSubject)`.
- `BAQ-004`: Disabled-account denial returns `AUTH_ACCOUNT_DISABLED` in the standard response envelope.
- `BAQ-005`: Existing manually linked Admin may authenticate through Google without role mutation; Google OAuth never creates or converts Admin.

## Known Gaps
- Existing source modules and persistence structures were not part of the allowed System Design inputs, so Coding must inspect `src/` during its phase and document reuse or adaptation in `coding-plan.md`.
- Exact token/session implementation may be aligned with existing codebase conventions during Coding, provided server-side account ID and role are authoritative.

## Ready For Coding Backend
- [x] Yes
