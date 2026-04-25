# Frontend Testing Plan

## Role Running
`testing-fe`

## Project
- Project name: `EvelS`
- Feature name: `init-fe-auth-login`

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Preflight
- `AI-Share-Documents/FE/04-coding-fe/coding-self-check.md` was read first.
- `AI-Share-Documents/FE/04-coding-fe/coding-plan.md` was read before writing tests.
- FE coding status: `PASS`
- Frontend in scope: `Yes`
- Coding plan usable: `Yes`

## Strategy
Use frontend unit and component tests to validate auth integration behavior, local auth persistence, protected-route guards, Admin login UI behavior, and Photographer Google initiation. Run framework validation commands to catch type, lint, and production build issues.

## Test Levels
- Unit tests:
  - auth API mapper and endpoint usage
  - auth storage validation
- Component tests:
  - Admin login form
  - Google login button
  - route guards with `AuthProvider`
- Build/static validation:
  - TypeScript typecheck
  - ESLint
  - Next production build

## Tools
- Vitest
- jsdom
- React Testing Library
- Testing Library user-event
- Testing Library jest-dom matchers
- Next.js build/type tooling

## Commands
- `npm.cmd run test`
- `npm.cmd run typecheck`
- `npm.cmd run lint`
- `npm.cmd run build`

## Test Files
- `FE/FE-EvelS/src/shared/api/__tests__/auth-api.test.ts`
- `FE/FE-EvelS/src/features/auth/__tests__/auth-storage.test.ts`
- `FE/FE-EvelS/src/features/auth/__tests__/auth-guards.test.tsx`
- `FE/FE-EvelS/src/features/auth/components/__tests__/admin-login-form.test.tsx`
- `FE/FE-EvelS/src/features/auth/components/__tests__/google-login-button.test.tsx`

## Config Files Added Or Updated
- `FE/FE-EvelS/package.json`
- `FE/FE-EvelS/package-lock.json`
- `FE/FE-EvelS/vitest.config.ts`
- `FE/FE-EvelS/src/test/setup.ts`

## Exclusions
- No backend source or backend test changes.
- No live backend integration test was run.
- No browser E2E suite was added in this phase.
- No production frontend code was modified during Testing FE.
