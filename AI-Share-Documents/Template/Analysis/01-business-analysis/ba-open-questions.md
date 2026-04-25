# Open Questions

Use this file whenever Business Analysis cannot determine a requirement with confidence.
Do not delete questions after answering them. Update their fields so the decision trail stays traceable.

## Status Rules
- `OPEN`: unanswered, unresolved, or still ambiguous
- `RESOLVED`: answered and accepted as the source of truth
- `ASSUMED`: not answered by the requester, but allowed to proceed with an explicit assumption and rationale

## Impact Rules
- `HIGH`: blocks planning or can materially change design, security, access, scope, or data behavior
- `MEDIUM`: important, but not a hard blocker if explicitly assumed
- `LOW`: useful clarification with limited downstream impact
- `UNKNOWN`: impact not yet understood; treat as blocking until clarified

## Blocking Rules
- `Blocking: Yes` means Team Lead must stop until the question is resolved or explicitly assumed.
- `Blocking: No` means the question can remain open only if it does not affect implementation scope, security, access, data behavior, or an in-scope surface.
- Any `Impact: HIGH` or `Impact: UNKNOWN` question is blocking.

## Question Template
### Q1: <Short Title>
- Question:
  <What needs clarification?>
- Impact:
  HIGH | MEDIUM | LOW | UNKNOWN
- Affected Surface:
  BE | FE | BOTH | NONE | UNKNOWN
- Blocking:
  Yes | No
- Answer:
  <Leave empty until answered>
- Assumption:
  <Fill only when Status is ASSUMED>
- Rationale:
  <Why this assumption is acceptable>
- Status:
  OPEN | RESOLVED | ASSUMED

## Current Questions
- None yet.
