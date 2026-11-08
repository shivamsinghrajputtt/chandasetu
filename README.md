# ChandaSetu

**Community accounts, made clear.** A lightweight prototype for local committees, puja organizers and community groups to record contributions and expenses.

## Run locally
Open `index.html` in a modern browser. No build step or package installation is required.

## MVP features
- Multiple organization workspaces (demo switcher)
- Event-wise contribution tracking
- Manual cash and UPI entry (no payment gateway)
- Auto-numbered contribution receipts with print / Save as PDF
- Collector register and collector-wise totals
- Expense recording
- Income, expense and balance reports
- CSV export
- Browser-local persistence with localStorage
- Responsive layout for mobile and desktop

## Current prototype limitations
This is a **frontend-only prototype**. Data is stored in the current browser only; it is not synchronized across users or devices. Authentication, secure organization membership, server database, role permissions, audit log, receipt QR verification, reliable offline sync and payment gateway integrations are not implemented. Do not use this version as the official financial ledger for live collections.

## Deployment
This is a static website. A static hosting service can serve the repository root; the entry point is `index.html`. No build command or environment variables are required for this prototype.

## Planned next phase
Move persistence to Supabase, add authentication and organization-level row security, server-side receipt numbering, immutable audit history and public receipt verification.
