# Write Prompt - Checklist

- [ ] Capture the raw natural language request.
- [ ] Identify or placeholder the project name and feature name.
- [ ] Identify draft implementation scope or instruct downstream Analysis roles to resolve it.
- [ ] Write `wp-input.md`.
- [ ] Generate prompts for Analysis roles.
- [ ] Generate prompts that write Analysis artifacts to `AI-Share-Documents`.
- [ ] Generate BE coding/testing prompts only when backend may be in scope.
- [ ] Generate a BE document-sharing prompt only when frontend may depend on backend changes.
- [ ] Generate FE coding/testing prompts only when frontend may be in scope.
- [ ] Allow optional archived BE document lookup for FE prompts when existing backend contract context is relevant.
- [ ] Append one final prompt for archive/reset workflow.
- [ ] Ensure prompts include role, inputs, outputs, constraints, and completion artifacts.
- [ ] Preserve conditional scope gates, including `NOT_IN_SCOPE` handling for skipped surfaces.
- [ ] Write `wp-prompts.md`.
- [ ] Write prompt assumptions and metadata.
- [ ] Complete `wp-self-check.md`.
- [ ] Write only to allowed prompt-generation locations.
