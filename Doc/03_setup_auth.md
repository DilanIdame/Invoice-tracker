# Task: Setup Authentication

## Objective
Implement User Authentication using NextAuth.js with Google Provider.

## Context / Requirements
- **Provider**: Google (requires Client ID and Secret).
- **Persistence**: Store users in the database (Prisma Adapter).

## Implementation Steps
1. Obtain Google OAuth credentials (User will need to provide these or we use mocks for dev).
2. Configure `src/server/auth.ts` (or equivalent T3 file).
3. Add Google Provider to the `NextAuth` configuration.
4. Ensure `PrismaAdapter` is correctly wired up.
5. Create a basic Sign-in page or use the default one to verify login.
