# Task 2: Implement Database Schema (Prisma)

### Requirements
- Update `schema.prisma` with `Invoice` and `EmailLog` models.
- Ensure `Account` model captures `refresh_token` for Gmail API access.

### Proposed Changes
- [MODIFY] `prisma/schema.prisma`
    - Add `Invoice` model with fields for file URL, recipient, and status.
    - Add `EmailLog` model for tracking open events.
    - Add `InvoiceStatus` enum (DRAFT, SENT).
