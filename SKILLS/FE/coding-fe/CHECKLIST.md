# Coding Frontend - Checklist

## Preflight Check
- [ ] Read shared Analysis `03-system-design/sd-self-check.md`.
- [ ] Confirm System Design is ready and not blocked.
- [ ] Confirm frontend work is in scope, or write `Status: NOT_IN_SCOPE` and stop.
- [ ] Read shared Analysis `03-system-design/sd-data-design.md`.
- [ ] Confirm backend handoff documents exist in `AI-Share-Documents/BE/` only when required by System Design.
- [ ] Optionally search `AI-Share-Documents/Archive/**/BE/**` for matching archived backend documents when useful for `FE_ONLY` contract context.
- [ ] Read project design guidance when present.
- [ ] Confirm `sd-data-design.md` is present and implementation-usable for frontend work.
- [ ] Stop immediately if System Design, required backend handoff, or data design is insufficient.

## Planning Gate
- [ ] Read approved system design artifacts.
- [ ] Read backend handoff documents only when required by System Design.
- [ ] Record any archived BE documents used as optional reference inputs.
- [ ] Create `coding-plan.md` before any source code change.
- [ ] Confirm target frontend project under `FE/<Project name>/`.
- [ ] Confirm target route or feature.
- [ ] Confirm design constraints relevant to the target UI.
- [ ] Confirm files to create.
- [ ] Confirm files to update.
- [ ] Confirm implementation order.
- [ ] Confirm data fetching, state, and auth or session behavior.
- [ ] Confirm risks, constraints, and out-of-scope items.

## Coding Work
- [ ] Implement only inside approved frontend locations under `FE/<Project name>/`.
- [ ] Preserve route, component, and data-flow boundaries.
- [ ] Keep server and client component boundaries intentional.
- [ ] Adhere to project design guidance for frontend visuals when present.
- [ ] Record created and updated files in the change log.
- [ ] Complete `coding-self-check.md`.
- [ ] Do not write tests.
