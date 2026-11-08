# ChandaSetu

**Community accounts, made clear.** A lightweight application for local committees, puja organizers and community groups to record contributions and expenses.

## Current frontend prototype

Open `index.html` in a modern browser. No build step or package installation is required.

- Multiple organization workspaces (currently browser demo data)
- Event-wise contribution tracking
- Manual cash and UPI entry (no payment gateway)
- Printable contribution receipts
- Collector register and collector-wise totals
- Expense recording
- Income, expense and balance reports
- CSV export
- Browser-local persistence with localStorage
- Responsive layout for mobile and desktop

## Supabase backend foundation

A Supabase project has been provisioned in the Mumbai region. The database currently contains these tables:

- `profiles` — user profile details
- `organizations` — committee workspaces
- `organization_members` — organization membership and roles
- `events` — puja and community events
- `collectors` — collection team
- `contributions` — donor records, amounts, payment modes and receipt numbers
- `expenses` — event and organization expenses

Row Level Security (RLS) is enabled on all seven public tables. Policies scope reads and writes by organization membership and role. Foreign-key indexes have been added where the database advisor identified missing coverage, and the duplicate collector SELECT policy has been split to avoid overlapping permissive policies.

## Important implementation status

The frontend is **not yet connected** to Supabase. It still uses demo data in the browser's localStorage. Authentication screens, live database reads/writes, server-generated receipt numbering, audit history, public receipt verification and offline synchronization are still pending. Do not use the browser demo as the official financial ledger for live collections.

Never put a Supabase service-role or secret key in frontend code. The frontend must use only the publishable key, with RLS providing the data boundary.

## Deployment

The current frontend is a static website. A static hosting service can serve the repository root; the entry point is `index.html`. No build command is required for the demo.

## Next phase

Connect Supabase Auth and the existing RLS-protected tables to the application, then add transactional receipt numbering and audit history before using it for real collections.
