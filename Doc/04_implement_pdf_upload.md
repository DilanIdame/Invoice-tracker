# Task: Implement PDF Upload (Cloudinary)

## Objective
Allow users to upload PDF invoices which are then stored in Cloudinary.

## Context / Requirements
- **Storage Service**: Cloudinary.
- **Method**: Server-side upload or Client-side signed upload.
- **Performance**: Client-side is better for large files, but Server-side often easier to secure initially in tRPC.
- **Decision**: Use a tRPC mutation or Next.js API route that accepts the file (as Base64 or Form Data), then uploads to Cloudinary using `cloudinary` Node SDK.

## Implementation Steps
1. Install `cloudinary` SDK.
2. Add `CLOUDINARY_CLOUD_NAME`, `API_KEY`, `API_SECRET` to env.
3. Create a backend utility/service to handle upload.
4. Create a tRPC mutation (e.g., `invoice.upload`) that takes file string/buffer.
5. Return the `secure_url` and `public_id` to the client.
