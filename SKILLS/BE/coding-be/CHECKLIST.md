# Coding Backend - Checklist

## Preflight Check
- [ ] Read shared Analysis `03-system-design/sd-self-check.md`.
- [ ] Confirm System Design is ready and not blocked.
- [ ] Confirm backend work is in scope, or write `Status: NOT_IN_SCOPE` and stop.
- [ ] Read shared Analysis `03-system-design/sd-data-design.md`.
- [ ] Confirm `sd-data-design.md` is present and implementation-usable for backend work.
- [ ] Stop immediately if System Design is incomplete or data design is insufficient.

## Planning Gate
- [ ] Read approved system design artifacts.
- [ ] Create `coding-plan.md` before any source code change.
- [ ] Confirm target backend project under `BE/<Project name>/`.
- [ ] Confirm target module.
- [ ] Confirm files to create.
- [ ] Confirm files to update.
- [ ] Confirm implementation order.
- [ ] Confirm risks, constraints, and out-of-scope items.

## Coding Work
- [ ] Implement only inside `BE/<Project name>/src/`.
- [ ] Preserve Clean Architecture boundaries.
- [ ] Update module wiring only when required by design.
- [ ] Record created and updated files in the change log.
- [ ] Complete `coding-self-check.md`.
- [ ] Do not write tests.
