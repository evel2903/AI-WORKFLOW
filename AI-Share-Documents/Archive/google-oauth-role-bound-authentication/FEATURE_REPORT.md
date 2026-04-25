# Feature Report

## Role Running
`workflow-archiver`

## Input Files
- `ai-docs/archive/google-oauth-role-bound-authentication/00-prompt-generation/`
- `ai-docs/archive/google-oauth-role-bound-authentication/01-business-analysis/`
- `ai-docs/archive/google-oauth-role-bound-authentication/02-team-lead/`
- `ai-docs/archive/google-oauth-role-bound-authentication/03-system-design/`
- `ai-docs/archive/google-oauth-role-bound-authentication/04-coding/`
- `ai-docs/archive/google-oauth-role-bound-authentication/05-testing/`
- `ai-docs/templates/`

## Allowed Output Directories
- `ai-docs/archive/`
- `ai-docs/`

## Completion Artifacts
- `ai-docs/archive/google-oauth-role-bound-authentication/`
- `ai-docs/archive/google-oauth-role-bound-authentication/FEATURE_REPORT.md`
- restored working folders under `ai-docs/`

## Feature Name
`google-oauth-role-bound-authentication`

## Archive Date
2026-04-23

## Workflow Status
Archived after Testing Backend completion. Readiness is confirmed in [05-testing/test-self-check.md](./05-testing/test-self-check.md), including completed testing artifacts, passing `npm.cmd test -- --runInBand`, passing `npm.cmd run lint`, and explicit readiness for the next workflow step.

## Feature Summary
The archived workflow delivered a backend authentication feature where only `Admin` and `Photographer` are authenticated roles, `Photographer` onboarding happens through Google OAuth, Admin onboarding remains manual-only, account roles are immutable after creation, disabled accounts are denied login, and authentication decisions rely on validated provider identity plus server-side account data. The feature scope originates in [00-prompt-generation/wp-metadata.md](./00-prompt-generation/wp-metadata.md) and is defined at the business level in [01-business-analysis/ba-feature-spec.md](./01-business-analysis/ba-feature-spec.md).

Planning in [02-team-lead/tl-delivery-plan.md](./02-team-lead/tl-delivery-plan.md) and [02-team-lead/tl-risk-log.md](./02-team-lead/tl-risk-log.md) carried forward the key constraints: no public Admin onboarding, no Google-created Admin accounts, no role mutation, disabled-account enforcement, and no trust in client-supplied identity or role fields.

System design translated those rules into implementation-ready module, API, and data decisions in [03-system-design/sd-solution-overview.md](./03-system-design/sd-solution-overview.md) and [03-system-design/sd-data-design.md](./03-system-design/sd-data-design.md). The design resolved Google identity matching around verified provider subject data, preserved existing linked Admin accounts without role mutation, and required server-side trust boundaries for all authentication decisions.

Coding implemented the approved scope in [04-coding/coding-change-log.md](./04-coding/coding-change-log.md). The main delivered changes were:
- authenticated role set restricted to `Admin` and `Photographer`
- account status and auth error handling added
- user persistence extended with Google identity and disabled-account fields
- Google OAuth start and callback flow implemented with signed `state`, code exchange, and server-side token validation
- first Google login creates a `Photographer`; later logins authenticate the existing linked account without role changes
- public registration remains rejected
- existing Admin accounts may authenticate with password, while Photographer password login remains unsupported
- user management is guarded behind Admin access for the manual Admin provisioning boundary

Coding readiness and boundaries are recorded in [04-coding/coding-self-check.md](./04-coding/coding-self-check.md), including passing `npm.cmd run build` and `npm.cmd run lint`.

Testing validated the implementation against the approved coding scope in [05-testing/test-plan.md](./05-testing/test-plan.md) and [05-testing/test-results.md](./05-testing/test-results.md). The archived test run passed 25 suites and 56 tests, covering first Google login creation, repeated login role preservation, Admin non-creation through Google OAuth, disabled-account denial, server-side token validation, ignored client callback identity extras, rejected public registration, Admin-only password login, and callback URL contract behavior.

## Known Gaps At Archive Time
Residual gaps are documented in [05-testing/test-gaps.md](./05-testing/test-gaps.md):
- live Google OAuth flow was not executed against real provider credentials
- real database migration execution was not exercised in the automated test suite
- full JWT and role-guard integration remains only partially covered
- the final implemented scope includes existing Admin password login, which is documented as a scope update during Coding and Testing

## Archived Artifact Index
- Prompt generation: [00-prompt-generation/wp-input.md](./00-prompt-generation/wp-input.md), [00-prompt-generation/wp-metadata.md](./00-prompt-generation/wp-metadata.md), [00-prompt-generation/wp-prompts.md](./00-prompt-generation/wp-prompts.md), [00-prompt-generation/wp-self-check.md](./00-prompt-generation/wp-self-check.md)
- Business analysis: [01-business-analysis/ba-feature-spec.md](./01-business-analysis/ba-feature-spec.md), [01-business-analysis/ba-acceptance-criteria.md](./01-business-analysis/ba-acceptance-criteria.md), [01-business-analysis/ba-open-questions.md](./01-business-analysis/ba-open-questions.md), [01-business-analysis/ba-self-check.md](./01-business-analysis/ba-self-check.md)
- Team lead: [02-team-lead/tl-delivery-plan.md](./02-team-lead/tl-delivery-plan.md), [02-team-lead/tl-task-breakdown.md](./02-team-lead/tl-task-breakdown.md), [02-team-lead/tl-risk-log.md](./02-team-lead/tl-risk-log.md), [02-team-lead/tl-handoff.md](./02-team-lead/tl-handoff.md), [02-team-lead/tl-self-check.md](./02-team-lead/tl-self-check.md)
- System design: [03-system-design/sd-solution-overview.md](./03-system-design/sd-solution-overview.md), [03-system-design/sd-domain-design.md](./03-system-design/sd-domain-design.md), [03-system-design/sd-api-contract.md](./03-system-design/sd-api-contract.md), [03-system-design/sd-data-design.md](./03-system-design/sd-data-design.md), [03-system-design/sd-implementation-guidelines.md](./03-system-design/sd-implementation-guidelines.md), [03-system-design/sd-self-check.md](./03-system-design/sd-self-check.md)
- Coding: [04-coding/coding-plan.md](./04-coding/coding-plan.md), [04-coding/coding-change-log.md](./04-coding/coding-change-log.md), [04-coding/coding-self-check.md](./04-coding/coding-self-check.md)
- Testing: [05-testing/test-plan.md](./05-testing/test-plan.md), [05-testing/test-cases.md](./05-testing/test-cases.md), [05-testing/test-results.md](./05-testing/test-results.md), [05-testing/test-gaps.md](./05-testing/test-gaps.md), [05-testing/test-self-check.md](./05-testing/test-self-check.md)

## Reset Status
Fresh working folders were restored from `ai-docs/templates/` into `ai-docs/00-prompt-generation/` through `ai-docs/05-testing/` to prepare the next feature workflow.
