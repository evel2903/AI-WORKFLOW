# Coding Frontend Change Log

## Role Running
`coding-fe`

## Project
- Project name: `EvelS`
- Feature name: `init-fe-auth-login`

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Code Changes

### CD-001: Initialized Next.js Frontend Project
Maps to: `SD-002`, `SD-044`, `SD-045`, `SD-046`, `TL-001`, `TL-006`, `FR-001`, `FR-002`, `AC-001`

Created:
- `FE/FE-EvelS/package.json`
- `FE/FE-EvelS/package-lock.json`
- `FE/FE-EvelS/next.config.mjs`
- `FE/FE-EvelS/eslint.config.mjs`
- `FE/FE-EvelS/tsconfig.json`
- `FE/FE-EvelS/next-env.d.ts`
- `FE/FE-EvelS/.gitignore`
- `FE/FE-EvelS/src/app/layout.tsx`
- `FE/FE-EvelS/src/app/page.tsx`
- `FE/FE-EvelS/src/app/globals.css`

Summary:
- Initialized the empty frontend folder as a Next.js App Router, React, and TypeScript application.
- Added npm scripts for dev, build, lint, start, and typecheck.
- Added global operational UI styling.

### CD-002: Added Auth Data Model, Persistence, And Provider
Maps to: `SD-008`, `SD-009`, `SD-010`, `SD-030` through `SD-040`, `SD-049`, `TL-003`, `TL-009`, `FR-012`, `FR-013`, `FR-021`, `FR-022`, `AC-010`, `AC-015`, `AC-016`

Created:
- `FE/FE-EvelS/src/features/auth/auth-types.ts`
- `FE/FE-EvelS/src/features/auth/auth-storage.ts`
- `FE/FE-EvelS/src/features/auth/auth-provider.tsx`

Summary:
- Added `Admin` and `Photographer` role validation.
- Added auth session and account mapping types.
- Persisted validated session data in `localStorage` key `evels.auth.session`.
- Hydrates auth state on app startup and fails closed for malformed or disabled state.
- Added logout clearing behavior.

### CD-003: Added Backend Auth API Client Layer
Maps to: `SD-001`, `SD-022` through `SD-029`, `SD-048`, `TL-002`, `TL-007`, `TL-008`, `FR-004`, `FR-008`, `FR-009`, `NFR-006`, `AC-003`, `AC-007`, `AC-008`

Created:
- `FE/FE-EvelS/src/shared/config/env.ts`
- `FE/FE-EvelS/src/shared/api/api-client.ts`
- `FE/FE-EvelS/src/shared/api/auth-api.ts`

Summary:
- Added isolated backend base URL handling through `NEXT_PUBLIC_API_BASE_URL`, defaulting to `http://localhost:3000`.
- Added response envelope parsing for `Success`, `Data`, and `Errors`.
- Added Admin login integration for `POST /auth/login` using isolated `EmailAddress` and `Password` request mapping.
- Added Photographer Google initiation through `GET /auth/google`.
- Added Google callback completion through `GET /auth/google/callback`.
- Added token/account mapping with accepted token field aliases.

### CD-004: Implemented Admin And Photographer Login UI
Maps to: `SD-005`, `SD-006`, `SD-016`, `SD-017`, `SD-018`, `SD-019`, `SD-051`, `SD-052`, `SD-053`, `TL-005`, `TL-007`, `TL-008`, `FR-003`, `FR-005`, `FR-006`, `FR-007`, `FR-010`, `FR-011`, `FR-018`, `FR-019`, `FR-020`, `AC-002`, `AC-004`, `AC-005`, `AC-006`, `AC-008`, `AC-009`, `AC-014`

Created:
- `FE/FE-EvelS/src/app/login/admin/page.tsx`
- `FE/FE-EvelS/src/app/login/photographer/page.tsx`
- `FE/FE-EvelS/src/app/auth/google/callback/page.tsx`
- `FE/FE-EvelS/src/features/auth/components/admin-login-form.tsx`
- `FE/FE-EvelS/src/features/auth/components/google-login-button.tsx`
- `FE/FE-EvelS/src/features/auth/components/google-callback-handler.tsx`
- `FE/FE-EvelS/src/shared/ui/auth-page.tsx`
- `FE/FE-EvelS/src/shared/ui/alert.tsx`

Summary:
- Added Admin email-address/password-only login page.
- Added Photographer Google-only login page.
- Added callback processing route with loading and error states.
- Added clear user-facing errors for invalid login, disabled account, network failure, malformed response, Google initiation failure, and callback failure.

### CD-005: Implemented Role-Based Routing And Route Shells
Maps to: `SD-003`, `SD-012` through `SD-015`, `SD-020`, `SD-042`, `SD-050`, `TL-004`, `TL-010`, `FR-005`, `FR-010`, `FR-014`, `FR-015`, `FR-016`, `FR-017`, `FR-021`, `FR-022`, `AC-004`, `AC-009`, `AC-011`, `AC-012`, `AC-013`, `AC-015`, `AC-016`

Created:
- `FE/FE-EvelS/src/shared/routing/routes.ts`
- `FE/FE-EvelS/src/features/auth/auth-guards.tsx`
- `FE/FE-EvelS/src/features/auth/components/logout-button.tsx`
- `FE/FE-EvelS/src/app/admin/page.tsx`
- `FE/FE-EvelS/src/app/photographer/page.tsx`
- `FE/FE-EvelS/src/app/unauthorized/page.tsx`
- `FE/FE-EvelS/src/app/not-found.tsx`
- `FE/FE-EvelS/src/features/admin/admin-home.tsx`
- `FE/FE-EvelS/src/features/photographer/photographer-home.tsx`

Summary:
- Added `/admin` protected route for Admin users.
- Added `/photographer` protected route for Photographer users.
- Added fail-closed route guard behavior for loading, anonymous, wrong-role, and malformed state.
- Added minimal dashboard/homepage shells and logout action.
- Added `/unauthorized`, root redirect, and not-found routes.

## Verification
- `npm.cmd install`: passed; installed dependencies and generated `package-lock.json`.
- `npm.cmd run lint`: passed.
- `npm.cmd run typecheck`: passed.
- `npm.cmd run build`: passed.

## Notes
- `npm install` reported 2 moderate dependency vulnerabilities through npm audit. No automatic audit fix was applied because `npm audit fix --force` may introduce breaking dependency changes.
- Backend source and backend tests were not modified.
