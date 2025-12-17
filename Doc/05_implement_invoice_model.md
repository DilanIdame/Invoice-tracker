# Task: Implement Invoice Data Model

## Objective
Define and migrate the database schema for Invoices.

## Context / Requirements
- **Prisma Schema**: `Invoice` model related to `User`.
- **Fields**:
    - `id`, `userId`, `recipientEmail`, `subject`.
    - `pdfUrl`, `pdfPublicId`.
    - `status` (DRAFT, SENT, OPENED).
    - `trackingId` (Unique string for tracking pixel).
    - `timestamps` (createdAt, updatedAt, sentAt, openedAt).

## Implementation Steps
1. Edit `prisma/schema.prisma`.
2. Add `Invoice` model.
3. Add relation in `User` model (`invoices Invoice[]`).
4. Run `npx prisma db push` or `migrate dev` to apply changes.
