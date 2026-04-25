# Team Lead Handoff To System Design

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## System Design Mission
Design the frontend-only solution for initializing `FE/FE-EvelS` and implementing role-based authentication flows that consume the existing backend Admin password login and Google callback authentication contracts.

## Required Design Decisions

### Frontend Architecture
Design IDs should cover:
- Initial app structure and tooling for an empty frontend project.
- Auth feature boundaries.
- API client boundary.
- Route definitions.
- Shared layout or page shell strategy.
- Testability strategy.

Related tasks: `TL-001`, `TL-006`

### Backend Contract Consumption
System Design must document the frontend-consumed API contracts without requesting backend changes:
- Admin password login through `/auth/login`.
- Google callback auth using the current flow evidenced by archived BE coding/testing:
  - `GET /auth/google`
  - `GET /auth/google/callback`
- Auth success envelope/token/account mapping.
- Disabled account and auth failure mapping.
- Runtime backend base URL configuration.

Important: archived `sd-api-contract.md` includes an older `POST /auth/google/login` contract, but the current request and archived BE coding/testing point to the callback-based flow. Prefer callback flow and document this reconciliation.

Related tasks: `TL-002`, `TL-007`, `TL-008`

### Frontend Data Design
`sd-data-design.md` must be implementation-usable for frontend work and cover:
- Auth state shape.
- Account model fields needed by frontend.
- Token field mapping.
- API response envelope mapping.
- Browser/session persistence behavior.
- App bootstrap hydration and validation.
- Logout clearing.
- Cache or revalidation boundaries.
- Route guard data dependencies.
- Explicit note that no backend schema/migration is in scope.

Related tasks: `TL-003`, `TL-009`

### Routing And Role Access
Design must cover:
- Public login routes.
- Admin login entry point.
- Photographer Google login entry point.
- Google callback landing/processing route.
- Admin dashboard/homepage route shell.
- Photographer dashboard/homepage route shell.
- Unauthenticated protected-route behavior.
- Wrong-role behavior for Admin accessing Photographer pages.
- Wrong-role behavior for Photographer accessing Admin pages.
- Default post-login redirects based on server-returned role.

Related tasks: `TL-004`, `TL-010`

### Error Handling
Design must define user-facing behavior for:
- Invalid Admin credentials.
- Disabled account.
- Failed Google initiation.
- Failed Google callback.
- Network failure.
- Malformed or missing auth response.
- Missing or malformed persisted auth state.
- Unauthenticated protected-route access.
- Wrong-role route access.

Related task: `TL-005`

## Assumptions To Carry Forward
- `BAQ-001`: Admin credential UI uses username/password labels; exact backend field mapping must be confirmed by System Design or isolated as an implementation assumption.
- `BAQ-002`: Frontend framework, package manager, and test tooling are design/coding decisions because no frontend app exists yet.
- `BAQ-003`: Dashboard/homepage content can be minimal route shells for this feature.
- `BAQ-004`: Auth persistence mechanism must be designed with security and usability tradeoffs documented.
- `BAQ-005`: Callback frontend route shape must be designed around the existing backend callback flow.
- `BAQ-006`: Runtime backend base URL and auth configuration must be defined for frontend implementation.

## Required Traceability
System Design should map `SD-*` items to:
- `TL-001` through `TL-012`
- `FR-001` through `FR-022`
- `NFR-001` through `NFR-008`
- `AC-001` through `AC-017`

## Boundaries
- Do not design backend source changes.
- Do not require BE coding or BE testing.
- Do not require BE handoff documents for FE because backend is not changing.
- Do not trust client-provided role data for access decisions.
- Do not expand dashboard business content beyond route shells unless an upstream artifact is changed.

## Expected System Design Outputs
- `AI-Share-Documents/Analysis/03-system-design/sd-solution-overview.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-domain-design.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-api-contract.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-data-design.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-implementation-guidelines.md`
- `AI-Share-Documents/Analysis/03-system-design/sd-self-check.md`
