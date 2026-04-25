# Coding Frontend Plan

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

## Preflight Results
- `sd-self-check.md` was read first.
- `sd-data-design.md` was read before other design artifacts.
- System Design status: `PASS`
- Frontend in scope: `Yes`
- Backend handoff required: `No`
- `sd-data-design.md` is implementation-usable for frontend state, data flow, persistence, and route guards.
- No project-specific frontend design guidance file exists in `FE/FE-EvelS`.

## Selected Framework And Tooling
- Framework: Next.js App Router
- UI runtime: React with TypeScript
- Styling: global CSS modules via `src/app/globals.css`
- Package manager: npm
- Validation commands planned:
  - `npm install`
  - `npm run lint`
  - `npm run typecheck`
  - `npm run build`

## State Persistence Approach
- Store auth state in browser `localStorage` key `evels.auth.session`.
- Persist only token, account, and authentication timestamp.
- Hydrate session on app startup.
- Fail closed and clear storage when persisted state is missing, malformed, disabled, or has an unknown role.

## Backend Contract Assumptions
- Admin login endpoint: `POST /auth/login`.
- Admin UI labels are `Email address` and `Password`.
- Admin request body uses `EmailAddress` and `Password`, isolated in the auth API client.
- Google initiation endpoint: `GET /auth/google`, expecting `AuthorizationUrl`.
- Google callback completion endpoint: `GET /auth/google/callback`, called by forwarding the frontend callback query string.
- Backend base URL comes from `NEXT_PUBLIC_API_BASE_URL`, defaulting to `http://localhost:3000`.
- Response envelope uses `Success`, `Data`, and `Errors`.
- Auth token field prefers `AccessToken`, with aliases handled in the API mapper.

## Target Files To Create
- `package.json`
- `next.config.mjs`
- `tsconfig.json`
- `next-env.d.ts`
- `src/app/layout.tsx`
- `src/app/page.tsx`
- `src/app/globals.css`
- `src/app/login/admin/page.tsx`
- `src/app/login/photographer/page.tsx`
- `src/app/auth/google/callback/page.tsx`
- `src/app/admin/page.tsx`
- `src/app/photographer/page.tsx`
- `src/app/unauthorized/page.tsx`
- `src/app/not-found.tsx`
- `src/features/auth/auth-provider.tsx`
- `src/features/auth/auth-storage.ts`
- `src/features/auth/auth-types.ts`
- `src/features/auth/auth-guards.tsx`
- `src/features/auth/components/admin-login-form.tsx`
- `src/features/auth/components/google-login-button.tsx`
- `src/features/auth/components/logout-button.tsx`
- `src/features/auth/components/google-callback-handler.tsx`
- `src/features/admin/admin-home.tsx`
- `src/features/photographer/photographer-home.tsx`
- `src/shared/api/api-client.ts`
- `src/shared/api/auth-api.ts`
- `src/shared/config/env.ts`
- `src/shared/routing/routes.ts`
- `src/shared/ui/auth-page.tsx`
- `src/shared/ui/alert.tsx`

## Implementation Order
1. Create Next.js project configuration.
2. Create shared config, routing, and API client modules.
3. Create auth types, storage validation, provider, and guards.
4. Create Admin login, Photographer Google login, callback handler, and logout components.
5. Create route pages and protected dashboard shells.
6. Create global CSS for accessible operational UI.
7. Run validation commands and record results.
8. Write `coding-change-log.md` and `coding-self-check.md`.

## Out Of Scope
- Backend source changes.
- Backend tests.
- Frontend tests in this phase.
- Full dashboard business functionality beyond route shells.
- New backend endpoints.
