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
- [x] Photographer-only credit invariant defined.
- [x] Non-photographer non-usable credit strategy defined.
- [x] First-login initialization behavior defined.
- [x] Subsequent-login preservation behavior defined.
- [x] Admin-only update authorization defined.
- [x] Invalid non-photographer target update behavior defined.
- [x] Scope boundary against broader credit-engine behavior defined.

## Data Design Validation
- [x] `sd-data-design.md` is present.
- [x] `sd-data-design.md` is not placeholder-only.
- [x] Entity or aggregate candidates are defined.
- [x] Persistence mapping direction is defined.
- [x] Relationships and ownership boundaries are defined.
- [x] Key constraints are defined.
- [x] Numeric validation rules are defined.
- [x] Legacy initialization behavior is defined.
- [x] Migration notes are defined.
- [x] `TypeOrmDataSource` update notes are defined.

## Boundary Validation
- [x] Wrote only to `ai-docs/03-system-design/`.
- [x] Did not write production code.
- [x] Did not write tests.
- [x] Did not modify Team Lead artifacts.
- [x] Did not introduce a credit ledger or automated credit engine.

## Assumptions Carried Forward
- `BAQ-001`: Non-photographer credit may be omitted or hidden, but must remain non-usable.
- `BAQ-002`: Legacy photographers with `Credit = null` may initialize to `10` on first successful login after rollout.
- `BAQ-003`: Credit is treated as a non-negative whole-number value.
- `BAQ-004`: Credit visibility is limited to approved backend use cases and not required in every response contract.

## Known Gaps
- Existing source modules and persistence structures were not part of the allowed System Design inputs, so Coding must inspect `src/` during its phase and document exact reuse points in `coding-plan.md`.
- Exact existing auth response contract may differ from this design and can be aligned during Coding so long as photographer credit visibility rules and invariants are preserved.

## Ready For Coding Backend
- [x] Yes
