# Task: Develop Frontend UI

## Objective
Build the user interface for managing invoices.

## Context / Requirements
- **Tech**: React, Tailwind CSS.
- **Structure**:
    - **Dashboard**: Table/Grid of invoices. Show Status badges.
    - **Create Invoice**: Form to upload PDF and enter details.
    - **Send Action**: Button to trigger email send.

## Implementation Steps
1. Create `Dashboard` component/page.
    - Fetch invoices via tRPC `invoice.getAll`.
2. Create `UploadForm` component.
    - File input -> Upload mutation -> Save Form details.
3. Add `Send` button on the dashboard item.
    - Triggers `invoice.send` mutation.
4. Use Tailwind for "Premium" aesthetics (Gradients, Glassmorphism, etc.).
