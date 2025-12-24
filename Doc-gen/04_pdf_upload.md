# Task 4: Implement PDF Upload (Cloudinary)

### Requirements
- Securely upload PDF invoices to Cloudinary.
- Store Cloudinary URLs in the database.

### Proposed Changes
- [NEW] `src/server/api/routers/upload.ts`
    - Procedure `getUploadSignature`: Returns a timestamp and signature using Cloudinary API Secret.
- Frontend implementation to handle direct upload to Cloudinary using the signature.
