# Team Lead Task Breakdown

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Tasks

### TL-001: Design Initial Frontend Architecture
Maps to: `FR-001`, `FR-002`, `NFR-002`, `NFR-007`, `AC-001`, `BAQ-002`

Define the frontend project structure, tooling selection criteria, routing boundary, auth feature boundary, API integration boundary, and test approach for an initially empty `FE/FE-EvelS` project.

Owner phase: `system-design`

### TL-002: Design Backend Auth Contract Consumption
Maps to: `FR-004`, `FR-008`, `FR-009`, `NFR-006`, `AC-003`, `AC-007`, `AC-008`, `BAQ-001`, `BAQ-005`, `BAQ-006`

Define how the frontend will consume existing backend auth endpoints for Admin password login and Photographer Google callback auth. Confirm or isolate assumptions about request fields, response envelope, token field names, callback URL behavior, and runtime backend URL configuration.

Owner phase: `system-design`

### TL-003: Design Auth State And Persistence
Maps to: `FR-012`, `FR-013`, `FR-021`, `NFR-003`, `NFR-004`, `AC-010`, `AC-015`, `BAQ-004`

Define frontend auth state ownership, stored account/token shape, bootstrap hydration behavior, malformed-state handling, logout behavior, and failure-closed route behavior.

Owner phase: `system-design`

### TL-004: Design Role-Based Routing And Guards
Maps to: `FR-005`, `FR-010`, `FR-014`, `FR-015`, `FR-016`, `FR-017`, `FR-022`, `NFR-004`, `AC-004`, `AC-009`, `AC-011`, `AC-012`, `AC-013`, `AC-016`, `BAQ-003`

Define public login routes, callback route behavior, Admin protected route shell, Photographer protected route shell, default redirects, unauthorized behavior, and wrong-role blocking rules.

Owner phase: `system-design`

### TL-005: Design Error Handling Requirements
Maps to: `FR-018`, `FR-019`, `FR-020`, `NFR-005`, `AC-005`, `AC-008`, `AC-014`

Define user-facing error states for invalid Admin credentials, disabled accounts, failed Google initiation, failed callback completion, network failures, malformed responses, and unauthorized access.

Owner phase: `system-design`

### TL-006: Initialize Frontend Project
Maps to: `FR-001`, `FR-002`, `NFR-002`, `NFR-007`, `AC-001`

Create the initial frontend app in `FE/FE-EvelS` using the architecture and tooling approved by System Design.

Owner phase: `coding-fe`

### TL-007: Implement Admin Login Flow
Maps to: `FR-003`, `FR-004`, `FR-005`, `FR-006`, `FR-017`, `FR-018`, `NFR-005`, `NFR-006`, `AC-002`, `AC-003`, `AC-004`, `AC-005`

Implement the Admin username/password login experience, backend call, success state capture, Admin redirect, and failure messaging.

Owner phase: `coding-fe`

### TL-008: Implement Photographer Google Login Flow
Maps to: `FR-007`, `FR-008`, `FR-009`, `FR-010`, `FR-011`, `FR-017`, `FR-019`, `NFR-005`, `NFR-006`, `AC-006`, `AC-007`, `AC-008`, `AC-009`

Implement the Photographer Google-only login entry point, backend initiation integration, callback handling, success state capture, Photographer redirect, and callback error handling.

Owner phase: `coding-fe`

### TL-009: Implement Auth Persistence, Bootstrap, And Logout
Maps to: `FR-012`, `FR-013`, `FR-021`, `FR-022`, `NFR-003`, `NFR-004`, `AC-010`, `AC-015`, `AC-016`

Implement persistence, app startup auth restoration, malformed-state handling, logout clearing, and role data validation based on backend-returned account data.

Owner phase: `coding-fe`

### TL-010: Implement Protected Routes And Role Shells
Maps to: `FR-014`, `FR-015`, `FR-016`, `FR-017`, `FR-022`, `NFR-004`, `AC-011`, `AC-012`, `AC-013`, `AC-016`

Implement route guards, Admin route shell, Photographer route shell, unauthenticated redirects, and wrong-role denial behavior.

Owner phase: `coding-fe`

### TL-011: Test Frontend Auth Flows
Maps to: `AC-001` through `AC-017`, `NFR-007`

Create and execute tests or checks for app initialization, Admin login success/failure, Photographer Google initiation/callback handling, auth persistence, protected routes, role guards, disabled-account failure handling, logout, and no-backend-change scope.

Owner phase: `testing-fe`

### TL-012: Archive Completed Workflow
Maps to: `NFR-001`, `AC-017`

Archive Analysis, BE, and FE shared workflow documents into `AI-Share-Documents/Archive/init-fe-auth-login/` after FE testing completes, then restore active folders from templates.

Owner phase: `workflow-archiver`

## Out-Of-Scope Task Handling
- No `coding-be` task is planned.
- No `testing-be` task is planned.
- No BE document-sharing task is planned because backend handoff is not required for FE.
