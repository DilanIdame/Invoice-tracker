# Task 0: Design Architecture & Database Schema

## System Architecture

### High-Level Components
1.  **Frontend**: Next.js (App Router), React, Tailwind CSS.
2.  **Backend**: Next.js Server Actions / API Routes (tRPC).
3.  **Database**: PostgreSQL (via Prisma).
4.  **External Services**: Google Identity Platform (Gmail API), Cloudinary.

### Folder Structure
```
src/
├── app/                  # App Router
├── server/
│   ├── api/              # tRPC routers
│   ├── auth.ts           # NextAuth configuration
│   └── db.ts             # Prisma client
├── lib/                  # Service helpers (Cloudinary, Gmail)
└── styles/               # Global styles
```

## Database Schema

```prisma
model User {
    id            String    @id @default(cuid())
    invoices      Invoice[]
}

model Account {
    refresh_token            String?
    access_token             String?
}

model Invoice {
    id             String     @id @default(cuid())
    fileUrl        String
    recipientEmail String
    status         InvoiceStatus
}

model EmailLog {
    id           String    @id @default(cuid())
    openedAt     DateTime?
}
```
