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

## Endpoint: Admin Update Photographer Credit
Design ID: `SD-020`

Maps to: `TL-003`, `TL-004`, `TL-007`, `FR-003`, `FR-004`, `FR-007`, `FR-008`, `NFR-002`, `NFR-004`, `AC-003`, `AC-004`, `AC-007`, `AC-008`

Method and path:
- `PATCH /users/:Id/credit`

Authorization:
- Must require authenticated `Admin` access.
- Guarding should happen in Presentation and be revalidated in Application/business logic.

Request body:
```json
{
  "Credit": 25
}
```

Request rules:
- `Credit` is required.
- `Credit` must be a whole number.
- `Credit` must be `>= 0`.
- Target user is identified by route parameter `Id`.

Success response:
```json
{
  "Success": true,
  "Data": {
    "User": {
      "Id": "user-id",
      "Role": "Photographer",
      "Credit": 25
    }
  },
  "Errors": []
}
```

Notes:
- Success is valid only when the target user role is `Photographer`.
- The response may include additional existing user fields if already part of the repo contract, but it must not expose usable credit for non-photographer roles.

## Endpoint: Photographer Login Success Contract Extension
Design ID: `SD-021`

Maps to: `TL-002`, `TL-005`, `FR-005`, `FR-006`, `FR-008`, `AC-005`, `AC-006`, `AC-008`, `BAQ-004`

Behavior:
- Do not introduce a new login endpoint solely for credit.
- If the existing successful photographer authentication response already returns account or user data, include `Credit` for photographer users after initialization/preservation rules run.
- If the existing success contract does not return user/account data, do not create a brand-new read endpoint in this phase just to expose credit.

Photographer success payload when user data is already present:
```json
{
  "Success": true,
  "Data": {
    "Account": {
      "Id": "user-id",
      "Role": "Photographer",
      "Credit": 10
    }
  },
  "Errors": []
}
```

Rules:
- If the authenticated role is not `Photographer`, `Credit` must be omitted from the returned account or user payload.
- Existing stored photographer credit must be returned as persisted after initialization or preservation logic completes.

## Error Cases

### Forbidden Non-Admin Update
Design ID: `SD-022`

Maps to: `TL-003`, `FR-004`, `NFR-002`, `AC-004`

Condition:
- Caller is not authorized as `Admin`.

Response:
```json
{
  "Success": false,
  "Data": null,
  "Errors": [
    {
      "Code": "USER_CREDIT_UPDATE_FORBIDDEN",
      "Message": "Only admins can update photographer credit."
    }
  ]
}
```

Recommended status: `403 Forbidden`

### Non-Photographer Target
Design ID: `SD-023`

Maps to: `TL-004`, `FR-007`, `NFR-004`, `AC-007`

Condition:
- Target user role is not `Photographer`.

Response:
```json
{
  "Success": false,
  "Data": null,
  "Errors": [
    {
      "Code": "USER_CREDIT_NOT_ALLOWED_FOR_ROLE",
      "Message": "Credit is allowed only for photographer users."
    }
  ]
}
```

Recommended status: `400 Bad Request`

### Invalid Credit Value
Design ID: `SD-024`

Maps to: `TL-003`, `TL-006`, `FR-003`, `NFR-003`, `NFR-004`, `AC-003`, `BAQ-003`

Condition:
- Request value is missing, negative, non-integer, or otherwise invalid.

Response:
```json
{
  "Success": false,
  "Data": null,
  "Errors": [
    {
      "Code": "USER_CREDIT_INVALID_VALUE",
      "Message": "Credit must be a non-negative whole number."
    }
  ]
}
```

Recommended status: `400 Bad Request`

### Target User Not Found
Design ID: `SD-025`

Maps to: `TL-007`, `FR-003`, `AC-003`

Condition:
- Route target `Id` does not map to an existing user/account.

Response:
```json
{
  "Success": false,
  "Data": null,
  "Errors": [
    {
      "Code": "USER_NOT_FOUND",
      "Message": "Target user was not found."
    }
  ]
}
```

Recommended status: `404 Not Found`

## Validation Rules
- Presentation DTO must accept only `Credit` for the admin update body.
- Client input must not decide whether a target is eligible for credit; eligibility is derived from persisted role.
- Any user/account payload containing `Credit` must omit that field for non-photographer roles.
- Existing authentication success handling must run credit initialization before final payload construction for photographers when `Credit` is still `null`.
