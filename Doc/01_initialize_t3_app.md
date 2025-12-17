# Task: Initialize T3 App

## Objective
Initialize the base project using the T3 Stack (Next.js, tRPC, Prisma, Tailwind CSS).

## Context / Requirements
- Use `create-t3-app` to scaffold the project.
- Select: **Next.js**, **Solidity**, **Prisma**, **NextAuth**, **Tailwind CSS**, **tRPC**. (Wait, "Solidity" explains T3 usually doesn't have it, I should stick to standard T3).
- Standard T3 stack choice: 
    - JavaScript/TypeScript: **TypeScript**
    - Tailwind: **Yes**
    - tRPC: **Yes**
    - Authentication: **NextAuth.js**
    - Database: **Prisma** (w/ SQLite or Postgres, we'll configure later)
    - App Router: **Yes** (Recommended for modern Next.js, though tRPC integration might still lean partially on Pages router or server actions depending on version. Use idiomatic T3 setup).

## Implementation Steps
1. Run `npm create t3-app@latest ./` (or similar command in the root).
2. Configure project options as listed above.
3. Ensure the app runs locally (`npm run dev`).
