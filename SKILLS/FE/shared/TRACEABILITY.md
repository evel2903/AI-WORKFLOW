# Traceability Rules

Use stable IDs whenever practical:
- Requirement: `FR-001`, `NFR-001`
- Acceptance Criteria: `AC-001`
- Team Task: `TL-001`
- Design Item: `SD-001`
- Code Change: `CD-001`
- Test Case: `TC-001`

Preferred trace chain:
`FR -> AC -> TL -> SD -> CD -> TC`

Each downstream phase should reference upstream IDs where possible.
