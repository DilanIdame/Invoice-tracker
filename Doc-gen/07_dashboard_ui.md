# Task 7: Build Dashboard UI

### Requirements
- Display a list of all invoices and their status.
- Provide filters for date, status, and recipient.

### Proposed Changes
- Dashboard Table:
    - Columns: Date, File, Recipient, Status (Draft/Sent), Open Status (Opened/Unopened).
- Upload Modal:
    - Drag & Drop zone for PDF files.
    - Integration with Cloudinary upload and `invoice.create` tRPC procedure.
