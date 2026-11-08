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

Supabase email/password authentication, organization onboarding, workspace switching, and initial live reads/inserts for events, collectors, contributions and expenses are now implemented in `index.html`. Browser demo data is no longer used for the authenticated dashboard. This integration has been committed and code-verified, but has **not yet passed a complete signed-in browser end-to-end test**. Receipt numbers are currently random client-generated identifiers, not server-issued sequential receipts. Audit history, public receipt verification, robust offline synchronization, role-based UI, and production-grade financial controls remain pending. **Do not use this as an official financial ledger yet.**

Never put a Supabase service-role or secret key in frontend code. The frontend must use only the publishable key, with RLS providing the data boundary.

## Deployment

The current frontend is a static website. A static hosting service can serve the repository root; the entry point is `index.html`. No build command is required for the demo.

## Next phase

Run an end-to-end authenticated browser test, implement server-side transactional receipt numbering and audit history, and harden role-based actions before using it for real collections.
