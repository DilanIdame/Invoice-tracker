# Task: Configure Environment & Database

## Objective
Set up the database connection and environment variables.

## Context / Requirements
- **Database**: PostgreSQL is preferred for production, but SQLite is fine for dev. Since we are using Prisma, switching is easy. Let's aim for a setup that supports potential PostgreSQL deployment (e.g., via Supabase or Neon later) or just local Postgres.
- **Environment Variables**:
    - `DATABASE_URL`: Connection string.
    - `NEXTAUTH_SECRET`: For authentication.
    - `NEXTAUTH_URL`: Localhost URL.

## Implementation Steps
1. Define validations in `src/env.js` (or `mjs`).
2. Update `prisma/schema.prisma` to use the chosen provider (e.g., `postgresql`).
3. Spin up a local Postgres instance if chosen, or use SQLite file.
4. Verify `npx prisma db push` works against the DB.
