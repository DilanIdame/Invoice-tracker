# Task: Implement Email Open Tracking

## Objective
Detect when the recipient opens the email using a tracking pixel.

## Context / Requirements
- **Endpoint**: A specialized Next.js API route (e.g., `/api/track/[id]`).
- **Behavior**: When requested, it updates the database and returns a 1x1 image.

## Implementation Steps
1. Create API Route `src/app/api/track/[id]/route.ts`.
2. In `GET` handler:
   - Extract `id` (trackingId).
   - Find Invoice in DB.
   - Update `status` = 'OPENED', `openedAt` = Now (only if not already opened).
   - Return transparent 1x1 PNG/GIF with correct headers (`Content-Type: image/png`, `Cache-Control: no-cache`).
