# Business Analysis Acceptance Criteria

## Scope Declaration
- Implementation Scope: `FE_ONLY`
- Backend in scope: `No`
- Frontend in scope: `Yes`
- Backend handoff required for FE: `No`

## Acceptance Criteria

### AC-001: Frontend Project Initialization
Maps to: `FR-001`, `FR-002`, `NFR-002`, `NFR-007`

Given `FE/FE-EvelS` is the target frontend project location,  
When the feature is implemented,  
Then a runnable frontend project exists there with a maintainable structure for auth, routing, backend integration, role pages, and tests.

### AC-002: Admin Login Method
Maps to: `FR-003`, `FR-006`

Given an Admin user is on the Admin login experience,  
When the user reviews the available sign-in options,  
Then username and password are the only Admin sign-in method shown.

### AC-003: Admin Login Backend Reuse
Maps to: `FR-004`, `NFR-006`, `NFR-008`

Given an Admin user submits username and password,  
When the frontend attempts authentication,  
Then it uses the existing backend Admin password login contract and does not require backend changes.

### AC-004: Admin Login Success Redirect
Maps to: `FR-005`, `FR-017`

Given the backend authenticates an Admin user successfully and returns Admin account data,  
When the frontend processes the successful auth response,  
Then the user is redirected to the Admin dashboard/homepage.

### AC-005: Admin Login Failure Error
Maps to: `FR-018`, `NFR-005`

Given an Admin login attempt fails,  
When the backend returns an auth error or the request cannot complete,  
Then the frontend displays a clear user-facing failure message and does not grant protected access.

### AC-006: Photographer Login Method
Maps to: `FR-007`, `FR-011`

Given a Photographer user is on the Photographer login experience,  
When the user reviews the available sign-in options,  
Then Google authentication is the only Photographer sign-in method shown.

### AC-007: Photographer Google Auth Initiation
Maps to: `FR-008`, `NFR-006`

Given a Photographer chooses Google sign-in,  
When the frontend starts authentication,  
Then it reuses the existing backend Google initiation flow that provides Google authorization and callback information.

### AC-008: Photographer Google Callback Handling
Maps to: `FR-009`, `FR-019`, `NFR-005`

Given Google redirects back through the backend-supported callback flow,  
When the frontend receives or processes the callback outcome,  
Then it completes the login journey on success or displays a clear failure message on error.

### AC-009: Photographer Login Success Redirect
Maps to: `FR-010`, `FR-017`

Given the backend authenticates a Photographer successfully and returns Photographer account data,  
When the frontend processes the successful auth response,  
Then the user is redirected to the Photographer dashboard/homepage.

### AC-010: Auth State Persistence
Maps to: `FR-012`, `FR-013`, `NFR-003`, `NFR-004`

Given a user has authenticated successfully,  
When the app is refreshed or reopened within the supported persistence behavior,  
Then the frontend restores or validates the auth state and routes the user according to that state.

### AC-011: Unauthenticated Protected Route Access
Maps to: `FR-014`, `NFR-004`

Given a user has no valid authenticated state,  
When the user tries to access an Admin or Photographer protected route,  
Then the frontend blocks access and sends the user to the appropriate login experience or unauthorized flow.

### AC-012: Admin Cannot Access Photographer Pages
Maps to: `FR-015`, `FR-017`, `FR-022`, `NFR-004`

Given an authenticated user has the server-returned role `Admin`,  
When the user tries to access a Photographer-only page,  
Then the frontend blocks access and does not render Photographer-only content.

### AC-013: Photographer Cannot Access Admin Pages
Maps to: `FR-016`, `FR-017`, `FR-022`, `NFR-004`

Given an authenticated user has the server-returned role `Photographer`,  
When the user tries to access an Admin-only page,  
Then the frontend blocks access and does not render Admin-only content.

### AC-014: Disabled Account Failure
Maps to: `FR-020`, `NFR-005`

Given the backend denies login because the account is disabled,  
When the frontend receives that failure,  
Then it displays a clear user-facing error and does not persist authenticated state.

### AC-015: Logout Clears Access
Maps to: `FR-021`, `NFR-003`, `NFR-004`

Given a user is authenticated,  
When the user logs out,  
Then the frontend clears local authentication state and protected pages are no longer accessible without logging in again.

### AC-016: Client Role Data Is Not Trusted
Maps to: `FR-017`, `FR-022`, `NFR-004`

Given route access depends on user role,  
When the frontend determines where a user may navigate,  
Then it bases that decision on backend-returned account data and fails closed for missing or malformed role data.

### AC-017: No Backend Implementation Required
Maps to: `NFR-001`, `NFR-008`

Given this feature is scoped as frontend-only,  
When downstream implementation work begins,  
Then backend code and backend tests remain unchanged unless a later Analysis/System Design artifact identifies a blocking contradiction.
