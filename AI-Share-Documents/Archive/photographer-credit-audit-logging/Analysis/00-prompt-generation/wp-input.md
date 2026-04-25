# Raw Request

## Role Running
`write-prompt`

## Description
Generate full Waterfall prompts for a backend change that updates the existing credit feature to include audit logging whenever an admin updates a user's credit.

The requested behavior is:
- Whenever an admin updates the credit of a user with the `photographer` role, the system must create a log record.
- The log should capture at minimum:
  - Admin user ID / name who performed the action
  - Target user ID / name whose credit was updated
  - Previous credit value
  - New credit value
  - Action timestamp
- Logging only applies when the credit value is actually changed.
- If the update request does not change the credit value, no log is required.
- Only admins can perform credit updates, following the existing permission rules.
- Credit still only applies to users with the photographer role.

In short, every admin credit update must be traceable through an audit log for tracking and accountability.

## Context
- This request updates an existing backend feature rather than introducing credit from scratch.
- The existing feature already establishes photographer-only credit ownership and admin-managed credit updates.
- The new scope adds auditability to successful admin credit changes without expanding into broader credit-engine behavior.

## Required Outputs
- `ai-docs/00-prompt-generation/wp-input.md`
- `ai-docs/00-prompt-generation/wp-prompts.md`
- `ai-docs/00-prompt-generation/wp-metadata.md`
- `ai-docs/00-prompt-generation/wp-self-check.md`

## Constraints
- Write all outputs only to `ai-docs/00-prompt-generation/`
- Do not execute downstream agents
- Do not write outputs only in chat
- Ensure generated prompts are ready for:
  - `business-analyst`
  - `team-lead`
  - `system-design`
  - `coding-be`
  - `testing-be`
