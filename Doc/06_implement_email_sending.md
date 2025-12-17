# Task: Implement Email Sending Service

## Objective
Send the invoice via email using Gmail.

## Context / Requirements
- **Service**: Nodemailer with Gmail SMTP.
- **Auth**: `GMAIL_USER` and `GMAIL_APP_PASSWORD`.
- **Content**: HTML email with link to PDF and Tracking Pixel.

## Implementation Steps
1. Install `nodemailer`.
2. Create a mailer service utility `src/server/mail.ts`.
3. Function `sendInvoiceEmail({ to, subject, pdfUrl, trackingId })`.
4. Construct HTML body:
   - Greeting.
   - Link: `<a href="${pdfUrl}">View Invoice</a>`
   - Pixel: `<img src="${NEXTAUTH_URL}/api/track/${trackingId}" width="1" height="1" />`
