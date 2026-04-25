# System Design API Contract

## Role Running
`system-design`

## Input Files
- `AGENTS.md`
- `.codex/skills/shared/WATERFALL_POLICY.md`
- `.codex/skills/shared/TRACEABILITY.md`
- `ai-docs/00-project/project-context.md`
- `ai-docs/00-project/constraints.md`
- `ai-docs/00-project/glossary.md`
- `ai-docs/02-team-lead/tl-delivery-plan.md`
- `ai-docs/02-team-lead/tl-task-breakdown.md`
- `ai-docs/02-team-lead/tl-risk-log.md`
- `ai-docs/02-team-lead/tl-handoff.md`
- `ai-docs/02-team-lead/tl-self-check.md`

## Allowed Output Directories
- `ai-docs/03-system-design/`

## Completion Artifacts
- `ai-docs/03-system-design/sd-solution-overview.md`
- `ai-docs/03-system-design/sd-domain-design.md`
- `ai-docs/03-system-design/sd-api-contract.md`
- `ai-docs/03-system-design/sd-data-design.md`
- `ai-docs/03-system-design/sd-implementation-guidelines.md`
- `ai-docs/03-system-design/sd-self-check.md`

## Response Envelope
All API responses must use the project envelope:
- `Success`
- `Data`
- `Errors`

## Endpoint: Google Login
Design ID: `SD-023`

Maps to: `TL-003`, `TL-004`, `TL-007`, `TL-008`, `TL-010`, `TL-011`, `FR-005`, `FR-006`, `FR-007`, `FR-008`, `FR-015`, `FR-016`, `NFR-001`, `NFR-002`, `NFR-003`, `NFR-004`, `NFR-005`, `NFR-006`, `AC-004`, `AC-005`, `AC-009`, `AC-010`, `AC-011`, `AC-012`

Method and path:
- `POST /auth/google/login`

Request body:
```json
{
  "IdToken": "google-id-token"
}
```

Request rules:
- `IdToken` is required.
- No `Role`, `AccountId`, `Email`, `ProviderSubject`, `IsAdmin`, or equivalent identity/role fields are accepted for decision-making.
- If extra client fields are present, Coding may ignore them or reject them according to existing validation policy. In either case, they must not affect role, identity, account lookup, or authorization-ready output.

Success response:
```json
{
  "Success": true,
  "Data": {
    "AccessToken": "application-auth-token",
    "Account": {
      "Id": "account-id",
      "Role": "Photographer",
      "Status": "Active",
      "Email": "verified-email@example.com",
      "DisplayName": "Verified Name"
    }
  },
  "Errors": []
}
```

Notes:
- `AccessToken` may be replaced with the repository's existing session/token response name if one exists.
- `Account.Role` must come from server-side account data only.
- For an existing manually provisioned Admin account with an internally linked Google identity, the role may be `Admin` in the success response, but only because the existing account role is already `Admin`; the Google login flow must not create or mutate it.

## Error Cases

### Invalid Google Token
Design ID: `SD-024`

Maps to: `TL-007`, `TL-011`, `NFR-001`, `NFR-003`, `AC-009`

Condition:
- Google token is missing, malformed, expired, signed for the wrong audience, issued by an invalid issuer, or otherwise fails backend validation.

Response:
```json
{
  "Success": false,
  "Data": null,
  "Errors": [
    {
      "Code": "AUTH_INVALID_GOOGLE_TOKEN",
      "Message": "Google authentication token is invalid."
    }
  ]
}
```

Recommended status: `401 Unauthorized`

### Disabled Account
Design ID: `SD-025`

Maps to: `TL-008`, `TL-011`, `FR-015`, `NFR-006`, `AC-011`, `BAQ-004`

Condition:
- A verified Google identity matches an account whose status is `Disabled`.

Response:
```json
{
  "Success": false,
  "Data": null,
  "Errors": [
    {
      "Code": "AUTH_ACCOUNT_DISABLED",
      "Message": "Account is disabled."
    }
  ]
}
```

Recommended status: `403 Forbidden`

### Client Role Spoofing
Design ID: `SD-026`

Maps to: `TL-007`, `TL-011`, `NFR-002`, `NFR-004`, `AC-010`

Condition:
- Client submits `Role`, `IsAdmin`, `AccountId`, or any role/identity field alongside Google login.

Behavior:
- Preferred: reject request as invalid input.
- Acceptable if consistent with existing validation policy: ignore extra fields.
- Required invariant: these fields must not control account creation role, account lookup, auth result role, or authorization-ready data.

Recommended error if rejected:
```json
{
  "Success": false,
  "Data": null,
  "Errors": [
    {
      "Code": "AUTH_UNTRUSTED_CLIENT_AUTH_DATA",
      "Message": "Client-provided authentication role or identity data is not accepted."
    }
  ]
}
```

Recommended status: `400 Bad Request`

### Unsupported Email Password Authentication
Design ID: `SD-027`

Maps to: `TL-006`, `TL-011`, `FR-011`, `NFR-007`, `AC-007`, `AC-013`

Condition:
- Client attempts email/password authentication.

Behavior:
- No endpoint should be introduced.
- If a route already exists, Coding must disable, remove, or reject it for this phase.

Recommended status if request reaches backend: `404 Not Found` if route absent, or `400 Bad Request`/`501 Not Implemented` if explicitly rejected by an existing route.

### Unsupported Admin Public Registration
Design ID: `SD-028`

Maps to: `TL-002`, `TL-006`, `TL-011`, `FR-003`, `FR-004`, `FR-012`, `AC-002`, `AC-003`, `AC-013`

Condition:
- Client attempts public Admin registration or attempts to pass role `Admin` during Google login.

Behavior:
- Must not create an Admin account.
- Must not mutate any account role to `Admin`.
- Must not authenticate through a public registration flow.

Recommended error code:
- `AUTH_ADMIN_REGISTRATION_UNSUPPORTED`

### Unsupported Customer Authentication
Design ID: `SD-029`

Maps to: `TL-001`, `TL-006`, `FR-013`, `FR-014`, `AC-008`, `AC-013`

Condition:
- Client attempts Customer login, Customer registration, or Customer account creation.

Behavior:
- Must not create a Customer account.
- Must not authenticate a Customer.

Recommended error code:
- `AUTH_CUSTOMER_AUTH_UNSUPPORTED`

## Internal Admin Provisioning Boundary
Design ID: `SD-030`

Maps to: `TL-002`, `FR-002`, `FR-003`, `FR-004`, `FR-012`, `AC-002`, `BAQ-001`

Public API:
- No public Admin registration endpoint is part of this feature.

Internal behavior:
- If Coding discovers an existing internal Admin provisioning path, it must preserve manual-only behavior.
- Any internal endpoint or command must require trusted operational access outside this public authentication flow.

## Auth Result Contract
Design ID: `SD-031`

Maps to: `TL-010`, `FR-016`, `NFR-005`, `AC-012`, `BAQ-002`

Required fields for downstream authorization readiness:
- server-side `Account.Id`
- server-side `Account.Role`
- active/disabled eligibility already evaluated before success
- token/session claims derived from persisted account data, not client input

Recommended token claims if JWT is used:
- `sub`: account ID
- `role`: account role from persisted account
- `typ`: application auth token

Do not include or trust client-provided role claims.
