# System Design Self Check

## Upstream Validation
- [ ] Team Lead self-check reviewed.
- [ ] BA and Team Lead artifacts are sufficient for design.

## Implementation Scope
- Implementation Scope: BE_ONLY | FE_ONLY | FULL_STACK | ANALYSIS_ONLY
- Backend in scope: Yes | No
- Frontend in scope: Yes | No
- Backend handoff required for FE: Yes | No | N/A

## Design Coverage
- [ ] Solution overview is complete.
- [ ] Domain design is complete.
- [ ] API contract is complete.
- [ ] Data design is complete.
- [ ] `sd-data-design.md` is implementation-usable or explicitly documents reuse of existing structures.
- [ ] Implementation guidelines are complete.

## Backend Alignment
- [ ] Backend design matches NestJS module-based Clean Architecture when backend work is in scope.
- [ ] Backend layer responsibilities are clear.
- [ ] Backend module wiring updates are called out when needed.
- [ ] Backend migration/data source updates are called out when needed.

## Frontend Alignment
- [ ] Frontend routes, UI states, and integration behavior are clear when frontend work is in scope.
- [ ] Frontend state/data ownership is clear.
- [ ] Frontend route protection, redirects, and session behavior are clear when applicable.
- [ ] Frontend component boundaries and server/client component decisions are clear when applicable.
- [ ] Frontend loading, empty, success, error, validation, authorization, and disabled states are specified when applicable.
- [ ] Frontend accessibility and responsive behavior expectations are specified when applicable.
- [ ] FE design constraints and `DESIGN.md` dependencies are called out when needed.

## Shared Publishing
- [ ] Design artifacts are ready in `AI-Share-Documents/Analysis/03-system-design/`.
- [ ] BE and FE agents can consume the shared System Design artifacts directly.

## Handoff Readiness
- [ ] Design is implementation-ready for Backend Coding review when backend is in scope, or backend is explicitly not in scope.
- [ ] Design is implementation-ready for Frontend Coding review when frontend is in scope, with BE handoff requirements stated.

## Known Gaps
- None
