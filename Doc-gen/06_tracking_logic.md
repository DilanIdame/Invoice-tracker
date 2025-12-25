# Task 6: Implement Tracking Logic (Pixels/Gmail)

### Requirements
- Detect when an email is opened via the tracking pixel.
- Update the `EmailLog` in the database.

### Proposed Changes
- [NEW] `src/app/api/tracking/[id]/pixel.png/route.ts`
    - GET handler that:
        1.  Extracts the `EmailLog` ID from the URL.
        2.  Updates `openedAt` for that log entry.
        3.  Returns a 1x1 transparent PNG.
        4.  Sets `Cache-Control: no-store` to prevent caching.
