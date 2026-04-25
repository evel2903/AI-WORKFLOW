# System Design API Contract

## API Scope
This phase does not add a new public audit endpoint. It updates the behavior of the existing admin credit update endpoint only.

## Endpoint

### SD-026: Admin Credit Update Endpoint
Maps to: `TL-001`, `TL-004`, `TL-005`, `FR-002`, `FR-003`, `FR-008`, `FR-009`, `AC-002`, `AC-003`, `AC-008`, `AC-009`

- Method: `PATCH`
- Path: `/users/:id/credit`
- Audience: authenticated admins only
- Purpose: update photographer credit and, when the value changes, generate the required audit record internally

## Request Contract

### Headers
- Authenticated bearer token or existing auth mechanism already used by protected user-management endpoints

### Path Params
- `id: string`

### Request Body
```json
{
  "Credit": 25
}
```

### Validation Rules
### SD-027: Input Validation
Maps to: `TL-004`, `FR-002`, `FR-009`, `NFR-001`

- `Credit` must remain a non-negative whole number, consistent with the existing approved credit feature.
- Request must fail before use-case execution if payload shape is invalid.

## Success Responses

### SD-028: Changed Value Success
Maps to: `TL-002`, `TL-003`, `FR-003`, `FR-004`, `FR-005`, `FR-006`, `FR-007`, `AC-003`, `AC-004`, `AC-005`, `AC-006`, `AC-007`

When the admin changes the stored credit value:
```json
{
  "Success": true,
  "Data": {
    "Id": "photo-1",
    "FirstName": "Photo",
    "LastName": "User",
    "EmailAddress": "photo@example.com",
    "Role": "Photographer",
    "Credit": 25,
    "CreatedAt": "2026-04-23T10:00:00.000Z"
  },
  "Errors": []
}
```

Internal side effect:
- audit record is created

### SD-029: No-Op Success
Maps to: `TL-002`, `TL-005`, `FR-008`, `AC-008`

When requested credit equals current stored credit:
```json
{
  "Success": true,
  "Data": {
    "Id": "photo-1",
    "FirstName": "Photo",
    "LastName": "User",
    "EmailAddress": "photo@example.com",
    "Role": "Photographer",
    "Credit": 25,
    "CreatedAt": "2026-04-23T10:00:00.000Z"
  },
  "Errors": []
}
```

Internal side effect:
- no audit record is created

Rationale:
- This satisfies the BA assumption that no-op response semantics may remain a normal success outcome while suppressing audit creation.

## Error Cases

### SD-030: Unauthorized Actor
Maps to: `TL-004`, `FR-002`, `AC-002`

- HTTP: existing forbidden status used by the application
- Envelope:
```json
{
  "Success": false,
  "Data": null,
  "Errors": ["Forbidden"]
}
```
- No audit record created

### SD-031: Non-Photographer Target
Maps to: `TL-004`, `FR-009`, `AC-009`

- HTTP: existing business-rule or bad-request status already used by the current credit-update flow
- Envelope:
```json
{
  "Success": false,
  "Data": null,
  "Errors": ["Target user is not eligible for credit updates."]
}
```
- No audit record created

### SD-032: Missing Target User
Maps to: `TL-004`

- HTTP: existing not-found status
- No audit record created

### SD-033: Validation Failure
Maps to: `TL-004`, `NFR-001`

- HTTP: existing validation error status
- No audit record created

### SD-034: Persistence Failure
Maps to: `TL-003`, `NFR-004`

- HTTP: existing server error status or mapped application exception
- If transaction-based persistence is used, neither the credit update nor the audit write remains committed.

## Visibility Contract

### SD-035: No Audit Read Surface In This Phase
Maps to: `TL-005`, `FR-010`, `AC-010`

- No new API endpoint for reading audit logs is required.
- Audit records remain internal persistence artifacts unless a later upstream artifact changes scope.
