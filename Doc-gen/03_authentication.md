# Task 3: Implement Authentication (NextAuth + Google)

### Requirements
- Configure Google OAuth.
- Request necessary Gmail scopes.
- Persist tokens to the database.

### Proposed Changes
- [MODIFY] `src/server/auth.ts`
    - Configure `GoogleProvider`.
    - Add scopes: `openid email profile https://www.googleapis.com/auth/gmail.send`.
    - Use `access_type: "offline"` and `prompt: "consent"` to ensure refresh tokens are provided.
