# System Design Implementation Guidelines

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Coding FE Preflight
Design ID: `SD-044`

Maps to: `TL-006`, `TL-007`, `TL-008`, `NFR-001`, `AC-017`

Before source changes, Coding FE must:
- read `sd-self-check.md` first;
- read `sd-data-design.md` before other design artifacts;
- create `AI-Share-Documents/FE/04-coding-fe/coding-plan.md`;
- confirm frontend is in scope and backend is not in scope;
- document selected framework, package manager, test tooling, and backend contract assumptions.

## Project Initialization Guidance
Design ID: `SD-045`

Maps to: `TL-001`, `TL-006`, `FR-001`, `FR-002`, `NFR-002`, `NFR-007`, `AC-001`

Initialize `FE/FE-EvelS` as a Next.js + React + TypeScript app unless Coding FE discovers a stronger existing workspace convention.

Recommended capabilities:
- App Router.
- TypeScript strict enough to catch auth model errors.
- ESLint and formatting defaults.
- Test tooling capable of auth state and route behavior coverage.
- Environment variable support for backend base URL.

Do not create a marketing landing page. The first useful surfaces are authentication and role route shells.

## Recommended Folder Structure
Design ID: `SD-046`

Maps to: `TL-001`, `TL-006`, `FR-002`, `AC-001`

Recommended structure:
```text
src/
  app/
    layout.tsx
    page.tsx
    login/
      admin/page.tsx
      photographer/page.tsx
    auth/
      google/callback/page.tsx
    admin/page.tsx
    photographer/page.tsx
    unauthorized/page.tsx
    not-found.tsx
  features/
    auth/
      components/
      hooks/
      auth-provider.tsx
      auth-storage.ts
      auth-types.ts
      auth-guards.tsx
    admin/
    photographer/
  shared/
    api/
      api-client.ts
      auth-api.ts
    config/
      env.ts
    routing/
      routes.ts
    ui/
```

Coding FE may adjust names to match generated framework conventions but should preserve these boundaries.

## Client And Server Component Boundaries
Design ID: `SD-047`

Maps to: `TL-001`, `TL-003`, `TL-004`, `TL-007`, `TL-008`, `TL-009`, `TL-010`, `AC-001`, `AC-010`, `AC-011`

Client components required:
- AuthProvider, because it reads browser storage and owns auth state.
- AdminLoginForm, because it owns form state and submission lifecycle.
- GoogleLoginButton, because it handles user click and browser navigation.
- GoogleCallbackPage or callback processor, because it uses query string and client auth state.
- RequireAuth and RequireRole, because they depend on client auth state.
- Logout control.

Server components may be used for static route shells, but protected content must not render until client guards confirm authorization.

## API Client Guidelines
Design ID: `SD-048`

Maps to: `TL-002`, `TL-007`, `TL-008`, `FR-004`, `FR-008`, `FR-009`, `NFR-006`, `AC-003`, `AC-007`, `AC-008`

Create a small auth API layer with functions equivalent to:
- `loginAdmin(credentials)`
- `startGoogleLogin()`
- `completeGoogleCallback(queryString)`

Guidelines:
- Centralize backend base URL handling.
- Centralize response envelope parsing.
- Normalize errors into frontend messages/codes.
- Keep Admin request field mapping isolated.
- Keep Google callback behavior isolated.
- Do not spread endpoint strings across components.

## Auth Provider Guidelines
Design ID: `SD-049`

Maps to: `TL-003`, `TL-009`, `FR-012`, `FR-013`, `FR-021`, `FR-022`, `AC-010`, `AC-015`, `AC-016`

AuthProvider should expose:
- current session;
- auth status;
- `applyAuthResult(result, expectedRole)`;
- `logout()`;
- `hasRole(role)`;
- optional `getAccessToken()`.

`applyAuthResult` must:
- validate token;
- validate account;
- validate expected role;
- persist session only after validation;
- clear state on validation failure.

## Route Guard Guidelines
Design ID: `SD-050`

Maps to: `TL-004`, `TL-010`, `FR-014`, `FR-015`, `FR-016`, `FR-017`, `FR-022`, `AC-011`, `AC-012`, `AC-013`, `AC-016`

Route guards must:
- show loading state during auth hydration;
- avoid rendering protected children while loading or unauthorized;
- redirect anonymous users to the correct login route;
- redirect wrong-role users to `/unauthorized`;
- fail closed for malformed session state.

Recommended guard usage:
- `/admin`: `RequireRole("Admin")`
- `/photographer`: `RequireRole("Photographer")`

## Error Handling Guidelines
Design ID: `SD-051`

Maps to: `TL-005`, `TL-007`, `TL-008`, `FR-018`, `FR-019`, `FR-020`, `NFR-005`, `AC-005`, `AC-008`, `AC-014`

Map backend and frontend failures into stable user-facing messages:
- Invalid Admin credentials: "Username or password is incorrect."
- Disabled account: "This account is disabled. Contact an administrator."
- Google initiation failed: "Google sign-in could not be started. Try again."
- Google callback failed: "Google sign-in could not be completed. Try again."
- Network failure: "Unable to connect. Check your connection and try again."
- Malformed auth response: "Sign in could not be completed. Try again."
- Wrong role: "This account cannot access that page."

Do not show raw backend stack traces, token values, callback codes, or provider internals.

## UI Guidance
Design ID: `SD-052`

Maps to: `TL-005`, `TL-006`, `TL-007`, `TL-008`, `FR-018`, `FR-019`, `FR-020`, `NFR-005`, `AC-005`, `AC-008`, `AC-014`

Design authentication screens as compact operational UI:
- clear page title;
- labeled fields;
- obvious primary action;
- visible error area;
- loading state on submit;
- no nested cards;
- no marketing hero;
- responsive layout that remains readable on mobile.

Use icons only where supported by the selected UI/icon package and where they clarify actions.

## Accessibility Guidelines
Design ID: `SD-053`

Maps to: `TL-005`, `TL-007`, `TL-008`, `NFR-005`, `NFR-007`, `AC-005`, `AC-008`, `AC-014`

Requirements:
- Form fields must have accessible labels.
- Error messages must be visible and programmatically associated where practical.
- Buttons must be keyboard reachable and show disabled/loading states.
- Callback processing must expose a semantic loading/status state.
- Focus should move to error summary after submit failure where practical.
- Color must not be the only error indicator.

## Testing Guidance
Design ID: `SD-054`

Maps to: `TL-011`, `NFR-007`, `AC-001` through `AC-017`

Testing FE should prioritize:
- auth API response mapping;
- Admin login form validation, success, and failure;
- Google login initiation behavior;
- Google callback success and failure;
- AuthProvider persistence, hydration, malformed-state clearing, and logout;
- route guard behavior for anonymous, Admin, Photographer, and malformed sessions;
- no backend files changed.

Run available checks such as build, typecheck, lint, and tests according to the initialized tooling.

## Boundaries
Design ID: `SD-055`

Maps to: `TL-012`, `NFR-008`, `AC-017`

Coding FE must not:
- modify `BE/BE-EvelS`;
- add backend endpoints;
- edit Analysis artifacts;
- write backend tests;
- trust client-supplied role values;
- implement dashboard business functionality beyond route shells unless upstream scope changes.
