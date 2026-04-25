# Team Lead Risk Log

## Role Running
`team-lead`

## Risks

### RISK-001: Name Snapshot Policy Is Still Assumed
- Type:
  Ambiguity / Data Design
- Maps to:
  `FR-004`, `FR-005`, `NFR-003`, `TL-003`
- Description:
  BA assumption `Q3` leaves open whether admin and target names are stored as immutable snapshots or derived from current user data later.
- Impact:
  HIGH
- Mitigation:
  System Design must make the policy explicit in `sd-data-design.md` and `sd-domain-design.md`, and Coding must implement exactly that approved approach.

### RISK-002: No-Op Behavior Could Produce Misleading Audit Outcomes
- Type:
  Delivery / API / Data Integrity
- Maps to:
  `FR-008`, `NFR-002`, `TL-002`, `TL-005`
- Description:
  BA assumption `Q1` does not define the exact API outcome for a no-op update. Without a clear design decision, downstream implementation may create inconsistent responses or accidentally emit audit records.
- Impact:
  HIGH
- Mitigation:
  System Design must define no-op response handling and explicitly prevent audit creation when the requested value matches the current stored value.

### RISK-003: Audit Visibility Scope Could Expand Accidentally
- Type:
  Scope / Ambiguity
- Maps to:
  `FR-010`, `NFR-005`, `NFR-006`, `TL-005`
- Description:
  BA assumption `Q2` keeps audit visibility narrow, but downstream work could accidentally introduce read APIs or reporting scope not approved by BA.
- Impact:
  MEDIUM
- Mitigation:
  Keep visibility requirements explicit in the handoff and treat any audit-log read surface as out of scope unless System Design receives explicit upstream approval.

### RISK-004: Credit Update And Audit Record Could Drift
- Type:
  Data Integrity
- Maps to:
  `FR-003`, `FR-006`, `FR-007`, `NFR-004`, `TL-003`, `TL-006`
- Description:
  If credit updates and audit-log creation are not coordinated correctly, the system could persist a credit change without a matching audit record or write an audit record that does not match the final stored value.
- Impact:
  HIGH
- Mitigation:
  System Design must define consistency or transaction expectations explicitly, including save order and failure handling boundaries.

### RISK-005: Invalid Or Unauthorized Requests Might Create False Audit Entries
- Type:
  Authorization / Data Integrity
- Maps to:
  `FR-002`, `FR-009`, `NFR-001`, `NFR-002`, `TL-004`
- Description:
  If audit creation is triggered too early in the flow, denied, invalid, or non-photographer-targeted requests could generate misleading records.
- Impact:
  HIGH
- Mitigation:
  Keep authorization and target validation ahead of audit creation, and require tests that prove no audit records are created for failed or invalid flows.

### RISK-006: Audit Persistence May Require Migration Or Registration Work
- Type:
  Delivery / Persistence
- Maps to:
  `FR-003`, `FR-004`, `FR-005`, `FR-006`, `FR-007`, `TL-003`, `TL-006`
- Description:
  If the approved design introduces a new audit store, Coding may need migration, ORM registration, and runtime wiring updates.
- Impact:
  MEDIUM
- Mitigation:
  System Design must state clearly whether new persistence structures are required and what migration/DataSource changes are needed.

### RISK-007: Historical Backfill Scope Could Be Reintroduced Unintentionally
- Type:
  Scope / Delivery
- Maps to:
  `FR-010`, `NFR-006`, `TL-003`
- Description:
  BA assumption `Q5` explicitly keeps historical backfill logging out of scope, but downstream design or coding could reintroduce that work implicitly through migration or replay ideas.
- Impact:
  MEDIUM
- Mitigation:
  Preserve historical backfill as a hard non-goal in handoff and design constraints unless a later upstream artifact changes scope.

### RISK-008: Sparse Project Context Can Hide Repo-Specific Constraints
- Type:
  Delivery
- Maps to:
  `NFR-005`, `TL-006`
- Description:
  The project context files are largely template-level, so repository-specific implementation constraints will only become clear during System Design and Coding inspection.
- Impact:
  MEDIUM
- Mitigation:
  System Design should explicitly inspect and document reuse points in the existing credit-update flow and persistence setup before Coding begins.
