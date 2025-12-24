# Invoice-Tracker Architecture & Implementation Plan

## Goal Description
Design and build a "simple but production-ready" invoice tracking application where users can:
1.  Sign in with Google.
2.  Upload invoice PDFs.
3.  Send these invoices via their own Gmail account (using Gmail API).
4.  Track if the email was opened.
5.  View a dashboard of sent invoices and their status.

## User Review Required
> [!IMPORTANT]
> **Gmail API Verification**: To send emails on behalf of the user, the Google OAuth app requires the `https://www.googleapis.com/auth/gmail.send` scope. For an unverified app, users will see a "Google hasn't verified this app" warning screen. This is expected for Dev/MVP but requires Google verification for production.

> [!WARNING]
> **Email Tracking Limitations**: Open tracking relies on a hidden 1x1 pixel image.
> 1.  Many modern email clients (like Gmail web, Apple Mail) proxy images or block them by default, potentially preventing "Open" detection.
> 2.  It only signals if/when images are loaded. "Delivered" status is generally not available via Gmail API without complex bounce handling (which is out of scope for "simple"). We will track "Sent" (API success) and "Opened" (pixel load).

## System Architecture

### High-Level Components
1.  **Frontend**: Next.js (App Router), React, Tailwind CSS.
    *   Authentication state managed by `NextAuth` SessionProvider.
    *   Data fetching and mutations via `tRPC` (React Query wrapper).
    *   File uploads directly to Cloudinary (signed by backend).
2.  **Backend**: Next.js Server Actions / API Routes (tRPC).
    *   **tRPC Router**: Handles business logic (CRUD Invoices, Send Email).
    *   **Prisma Client**: Database interactions.
    *   **Gmail Service**: Helper to authenticate and send emails using `googleapis`.
    *   **Tracking API**: A dedicated HTTP endpoint to serve the tracking pixel.
3.  **Database**: PostgreSQL (via Prisma).
4.  **External Services**:
    *   **Google Identity Platform**: OAuth2 and Gmail API.
    *   **Cloudinary**: PDF object storage.

### Folder Structure (T3 Best Practices)
```
src/
├── app/                  # App Router
│   ├── _components/      # Shared UI components
│   ├── api/
│   │   ├── auth/         # NextAuth endpoints
│   │   ├── trpc/         # tRPC endpoint
│   │   └── tracking/     # Tracking pixel endpoint
│   ├── dashboard/        # Dashboard page
│   └── page.tsx          # Landing page
├── server/
│   ├── api/
│   │   ├── routers/      # tRPC routers (invoice.ts, user.ts)
│   │   └── root.ts       # Root router
│   ├── auth.ts           # NextAuth configuration
│   └── db.ts             # Prisma client instance
├── lib/
│   ├── cloudinary.ts     # Cloudinary helper (signing)
│   └── gmail.ts          # Gmail API helper
└── styles/               # Global styles
```

## Database Schema

```prisma
// Users and Auth
model User {
    id            String    @id @default(cuid())
    name          String?
    email         String?   @unique
    emailVerified DateTime?
    image         String?
    accounts      Account[]
    sessions      Session[]
    invoices      Invoice[]
}

// NextAuth Account (Stores Google Tokens)
model Account {
    id                       String  @id @default(cuid())
    userId                   String
    type                     String
    provider                 String
    providerAccountId        String
    refresh_token            String? // @db.Text (Crucial for Gmail API)
    access_token             String? // @db.Text
    expires_at               Int?
    token_type               String?
    scope                    String?
    id_token                 String? // @db.Text
    session_state            String?
    user                     User    @relation(fields: [userId], references: [id], onDelete: Cascade)
    refresh_token_expires_in Int?

    @@unique([provider, providerAccountId])
}

// Invoice Data
model Invoice {
    id             String     @id @default(cuid())
    userId         String
    user           User       @relation(fields: [userId], references: [id])
    fileUrl        String     // Cloudinary URL
    fileName       String     // Original filename
    recipientEmail String
    subject        String?    // Email subject
    message        String?    // Email body text
    status         InvoiceStatus @default(DRAFT)
    sentAt         DateTime?
    createdAt      DateTime   @default(now())
    updatedAt      DateTime   @updatedAt
    emailLogs      EmailLog[]
}

// Tracking Logs
model EmailLog {
    id           String    @id @default(cuid())
    invoiceId    String
    invoice      Invoice   @relation(fields: [invoiceId], references: [id], onDelete: Cascade)
    gmailMessageId String? // ID returned by Gmail API
    openedAt     DateTime? // Set when pixel is loaded
}

enum InvoiceStatus {
    DRAFT
    SENT
}
```

## Proposed Changes

### 1. Database Implementation
#### [NEW] [schema.prisma](file:///e:/Company/Demo-Project/email-tracker/prisma/schema.prisma)
- Update schema with `Invoice`, `EmailLog` models.
- Ensure `Account` model captures `refresh_token`.

### 2. Authentication Configuration
#### [MODIFY] [server/auth.ts](file:///e:/Company/Demo-Project/email-tracker/src/server/auth.ts)
- Configure `GoogleProvider`.
- Add scopes: `openid email profile https://www.googleapis.com/auth/gmail.send`.
- **Crucial**: Ensure `access_token` and `refresh_token` are persisted to the database via the Prisma Adapter.

### 3. File Upload (Cloudinary)
#### [NEW] [server/api/routers/upload.ts](file:///e:/Company/Demo-Project/email-tracker/src/server/api/routers/upload.ts)
- Procedure `getUploadSignature`: Returns a timestamp and signature using Cloudinary API Secret.
- Frontend uses this to upload to Cloudinary client-side.

### 4. Logic & API (tRPC)
#### [NEW] [server/api/routers/invoice.ts](file:///e:/Company/Demo-Project/email-tracker/src/server/api/routers/invoice.ts)
- `create`: Save invoice metadata from Cloudinary result.
- `getAll`: Fetch user's invoices with pagination/filters.
- `send`:
    1.  Check if User has linked Google Account.
    2.  Refresh Access Token if needed (using `google-auth-library` or manual refresh flow).
    3.  Create an `EmailLog` entry to generate an ID.
    4.  Construct Email MIME message:
        -   To: `recipientEmail`
        -   Subject: `subject`
        -   Body: `message` + HTML `<img src="${Host}/api/tracking/${emailLogId}/pixel.png" />`.
        -   Attachment: Fetch PDF from `fileUrl` and attach as Base64.
    5.  Send via `gmail.users.messages.send`.
    6.  Update `Invoice` status to `SENT`.

### 5. Tracking Endpoint
#### [NEW] [app/api/tracking/[id]/pixel.png/route.ts](file:///e:/Company/Demo-Project/email-tracker/src/app/api/tracking/[id]/pixel.png/route.ts)
- GET request handler.
- DB Update: Find `EmailLog` by `id`, set `openedAt = now()`.
- Return: `Cache-Control: no-store`, Content-Type: `image/png`, Body: 1x1 transparent pixel.

### 6. Dashboard UI
- **Dashboard**: Table of invoices.
    - Columns: Date, File, Recipient, Status (Draft/Sent), Open Status (Opened/Unopened).
    - Actions: "Send" (if draft).
- **Upload Modal**: Drag & Drop zone -> Upload to Cloudinary -> Create Invoice.

## Verification Plan

### Automated Tests
- We will rely on manual verification for the MVP as integration tests with real Gmail API are complex.
- **Unit Tests**:
    - Test tracking pixel route updates DB.

### Manual Verification
1.  **Auth**: Sign in with Google. Check DB `Account` table for valid `refresh_token`.
2.  **Upload**: Upload a PDF. Check Cloudinary dashboard (or verify URL opens).
3.  **Send**: Send invoice to `your-own-alias@gmail.com`.
    - Check if email arrives with PDF.
    - Check if Status updates to SENT in dashboard.
4.  **Tracking**:
    - Open the received email (ensure images are enabled).
    - Refresh dashboard. Status should show "Opened" timestamp.
