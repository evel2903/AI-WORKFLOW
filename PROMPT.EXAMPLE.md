# First Prompt Example

Use this prompt from the workspace root.

```md
Run as the Write Prompt agent.

Follow:
- RULE.md

Task:
Generate full cross-role Waterfall prompts for this feature:

"Create Google authentication for the EvelS project.

Users must be able to sign in with Google OAuth.
The backend must verify Google tokens, create or update user accounts, assign the correct role, and return the standard API response envelope.
The frontend must provide Google login UI and route users after login based on their role.
Admin users should land on the admin area.
Photographer users should land on the photographer area.
Unauthenticated users must not access protected pages.
Authentication errors must be shown clearly.
The implementation must follow the existing BE and FE architecture."

Project:
- Project name: EvelS
- Feature name: google-auth
- Expected implementation scope: FULL_STACK
- Backend code path: BE/BE-EvelS
- Frontend code path: FE/FE-EvelS
- Active Analysis document path: AI-Share-Documents/Analysis
- Active Backend document path: AI-Share-Documents/BE
- Active Frontend document path: AI-Share-Documents/FE
- Final archive path: AI-Share-Documents/Archive/google-auth

Instructions:
- Apply all reusable workflow rules from RULE.md.
- Use `google-auth` wherever RULE.md references `<Feature_name>`.
```
