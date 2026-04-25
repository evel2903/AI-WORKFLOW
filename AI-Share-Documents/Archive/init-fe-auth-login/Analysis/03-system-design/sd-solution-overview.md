# System Design Solution Overview

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Design Summary
This feature initializes the EvelS frontend under `FE/FE-EvelS` and implements role-specific authentication flows for Admin and Photographer users. The frontend consumes the existing backend authentication contracts and does not require backend source or backend test changes.

Because `FE/FE-EvelS` currently has no application files beyond Git metadata, the frontend should be initialized as a conservative Next.js + React + TypeScript application. This aligns with the System Design frontend guidance and gives Coding FE a clear baseline for routing, client-side auth state, API clients, components, and tests.

## Backend Reuse Decision
Design ID: `SD-001`

Maps to: `TL-002`, `TL-007`, `TL-008`, `FR-004`, `FR-008`, `FR-009`, `NFR-006`, `NFR-008`, `AC-003`, `AC-007`, `AC-008`, `AC-017`

The backend is reused as-is.

Authoritative current auth facts for this frontend feature:
- Admin login uses existing `/auth/login`.
- Photographer login uses existing callback-based Google auth.
- `GET /auth/google` starts Google authentication and returns `AuthorizationUrl` and `CallbackUrl`.
- `GET /auth/google/callback` handles callback code exchange and completes authentication.
- Auth success returns a token and server-side account data.

Archived contract reconciliation:
- Older archived system design mentions `POST /auth/google/login`.
- Later archived backend coding/testing and the current request identify the implemented flow as `GET /auth/google` plus `GET /auth/google/callback`.
- For this feature, Coding FE must integrate with the current callback-based flow and treat the older `POST /auth/google/login` contract as non-authoritative historical context.

## Frontend Architecture
Design ID: `SD-002`

Maps to: `TL-001`, `TL-006`, `FR-001`, `FR-002`, `NFR-002`, `NFR-007`, `AC-001`

Recommended baseline:
- Next.js App Router.
- React with TypeScript.
- Client-side auth provider for browser auth state.
- Small API client layer for backend auth calls.
- Route groups for public auth routes and protected role routes.
- Component boundaries around forms, auth buttons, route guards, page shells, and error display.
- Tests selected by Coding FE from project conventions; if none exist, use a standard setup that can cover API-client behavior, auth state, and route guard behavior.

Recommended top-level structure:
- `src/app/` for routes and layouts.
- `src/features/auth/` for auth provider, login UI, callback handling helpers, and auth state logic.
- `src/features/admin/` for Admin route shell/page content.
- `src/features/photographer/` for Photographer route shell/page content.
- `src/shared/api/` for backend API client and response envelope handling.
- `src/shared/config/` for runtime frontend configuration.
- `src/shared/ui/` for reusable form, button, alert, and loading components where useful.
- `src/shared/routing/` for role route constants and guard helpers.

## Route Map
Design ID: `SD-003`

Maps to: `TL-004`, `TL-010`, `FR-005`, `FR-010`, `FR-014`, `FR-015`, `FR-016`, `FR-017`, `FR-022`, `NFR-004`, `AC-004`, `AC-009`, `AC-011`, `AC-012`, `AC-013`, `AC-016`

Required routes:
- `/login/admin`: public Admin username/password login page.
- `/login/photographer`: public Photographer Google login page.
- `/auth/google/callback`: public callback landing/processing route.
- `/admin`: Admin protected dashboard/homepage shell.
- `/photographer`: Photographer protected dashboard/homepage shell.
- `/unauthorized`: public wrong-role or denied-access page.
- `/`: landing redirect route.
- `not-found`: fallback page.

Redirect rules:
- `/` redirects authenticated Admin users to `/admin`.
- `/` redirects authenticated Photographer users to `/photographer`.
- `/` redirects unauthenticated users to `/login/admin` or a neutral login chooser if Coding FE creates one; no chooser is required by this feature.
- Public login pages redirect already-authenticated users to the dashboard for their server-returned role.
- Protected routes redirect unauthenticated users to the matching login page when route intent is known.
- Protected routes redirect wrong-role users to `/unauthorized`.

## Auth Flow Summary
Design ID: `SD-004`

Maps to: `TL-002`, `TL-003`, `TL-007`, `TL-008`, `TL-009`, `FR-003` through `FR-013`, `FR-017`, `FR-021`, `FR-022`, `AC-002` through `AC-010`, `AC-015`, `AC-016`

Admin flow:
1. Admin visits `/login/admin`.
2. Admin enters username and password.
3. Frontend submits credentials to `/auth/login`.
4. On success, frontend maps the response envelope into auth state.
5. If returned role is `Admin`, frontend persists auth state and redirects to `/admin`.
6. If returned role is not `Admin`, frontend clears any transient state and shows an authorization error.

Photographer flow:
1. Photographer visits `/login/photographer`.
2. Photographer selects Google sign-in.
3. Frontend calls `GET /auth/google`.
4. Frontend navigates browser to returned `AuthorizationUrl`.
5. Google/backend callback completes through `GET /auth/google/callback`.
6. Frontend callback route processes the resulting auth payload or callback failure.
7. If returned role is `Photographer`, frontend persists auth state and redirects to `/photographer`.
8. If returned role is not `Photographer`, frontend clears any transient state and shows an authorization error.

Logout flow:
1. Authenticated user chooses logout.
2. Frontend clears stored auth state.
3. Frontend redirects to the appropriate login route or a public route.
4. Protected routes must become inaccessible without logging in again.

## Component Boundary Summary
Design ID: `SD-005`

Maps to: `TL-001`, `TL-004`, `TL-005`, `TL-006`, `FR-002`, `FR-018`, `FR-019`, `FR-020`, `NFR-005`, `NFR-007`, `AC-001`, `AC-005`, `AC-008`, `AC-014`

Core frontend boundaries:
- `AuthProvider`: client component owning auth state hydration, persistence, login result application, and logout.
- `AdminLoginForm`: client component owning Admin credential form state and submission lifecycle.
- `GoogleLoginButton`: client component owning Google initiation action and disabled/loading state.
- `GoogleCallbackPage`: client route component for callback processing state and errors.
- `RequireAuth`: client guard for authenticated routes.
- `RequireRole`: client guard for role-specific routes.
- `AuthErrorAlert`: reusable visible error component.
- `RoleHomeShell`: simple dashboard/homepage layout shell for Admin and Photographer pages.

## UI And Accessibility Direction
Design ID: `SD-006`

Maps to: `TL-005`, `TL-006`, `TL-007`, `TL-008`, `FR-018`, `FR-019`, `FR-020`, `NFR-005`, `NFR-007`, `AC-005`, `AC-008`, `AC-014`

The UI should be work-focused and direct. This is an authentication surface for an operational product, so use restrained spacing, clear labels, obvious primary actions, and compact route shells. Avoid marketing-style landing content.

Accessibility requirements:
- Login fields must have semantic labels.
- Submit buttons must expose loading and disabled states.
- Errors must be visible text and associated with the form region.
- Google sign-in must be keyboard reachable.
- Callback loading and error states must be announced using semantic status/alert regions.
- Focus should move to the error summary on failed login when practical.

## Test Design Focus
Design ID: `SD-007`

Maps to: `TL-011`, `AC-001` through `AC-017`, `NFR-007`

Testing FE should cover:
- project boot/build/typecheck baseline;
- Admin login success and failure;
- Photographer Google initiation;
- callback success and failure;
- auth persistence and hydration;
- logout clearing;
- unauthenticated protected route blocking;
- wrong-role route blocking;
- disabled-account error handling;
- malformed auth response fail-closed behavior.
