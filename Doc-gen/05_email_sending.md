# Task 5: Implement Email Sending (Gmail API)

### Requirements
- Construct and send emails with PDF attachments using the user's Gmail account.
- Embed a tracking pixel in the email body.

### Proposed Changes
- [NEW] `src/server/api/routers/invoice.ts`
    - `send` procedure:
        1.  Verify Google Account linkage.
        2.  Refresh tokens if necessary.
        3.  Construct MIME message with Base64 PDF attachment.
        4.  Inject tracking pixel `<img>` tag.
        5.  Call Gmail API `messages.send`.
        6.  Update record status.
