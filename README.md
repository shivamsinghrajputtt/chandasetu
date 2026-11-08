# ChandaSetu

**Community accounts, made clear.** A lightweight application for local committees, puja organizers and community groups to record contributions and expenses.

## Current frontend

Open `index.html` in a modern browser. No build step or package installation is required.

- Multiple organization workspaces with Supabase membership and role handling
- Event-wise contribution tracking
- Manual cash and UPI entry (no payment gateway)
- Printable contribution receipts
- Collector register and collector-wise totals
- Expense recording
- Income, expense and balance reports
- CSV export
- Live organization-scoped Supabase reads and inserts
- Responsive layout for mobile and desktop
- GitHub Actions CI checks for inline JavaScript syntax and key backend integrations

## Supabase backend foundation

A Supabase project has been provisioned in the Mumbai region. The database currently contains these tables:

- `profiles` — user profile details
- `organizations` — committee workspaces
- `organization_members` — organization membership and roles
- `events` — puja and community events
- `collectors` — collection team
- `contributions` — donor records, amounts, payment modes and receipt numbers
- `expenses` — event and organization expenses

Row Level Security (RLS) is enabled on all public application tables, including the audit log. The private receipt counter is stored in a non-exposed `private` schema. Policies scope reads and writes by organization membership and role. Foreign-key indexes have been added where the database advisor identified missing coverage, and the duplicate collector SELECT policy has been split to avoid overlapping permissive policies.

## Implementation status

Implemented in `index.html`:
- Supabase email/password authentication and profile upsert
- Organization creation, membership onboarding, workspace switching
- Live organization-scoped reads and inserts for events, collectors, contributions and expenses
- Role-aware UI: owners/admins manage events, collectors and expenses; collectors can record contributions
- Atomic, server-generated receipt numbers scoped by organization and year (`CS-YYYY-000001`)
- Contribution reversal workflow: only owners/admins can reverse a confirmed entry with a reason; original receipt and record remain in history, and reversal is audited
- Offline contribution outbox using IndexedDB; queued items are scoped to the signed-in user, marked as provisional (not official receipts), retried on reconnect/login, and receive official server receipts only after sync
- Financial reconciliation dashboard separates confirmed collection, expenses, reversed amount and offline-pending entries; CSV export includes status and reversal reason
- Read-only audit history for event, collector, contribution and expense changes; visible only to owners/admins
- Audit CSV export
- Public receipt verification using organization verification code + receipt number; public response excludes donor name and phone

The receipt allocator is an internal `SECURITY DEFINER` function in the non-exposed `private` schema, called through an authenticated `SECURITY INVOKER` RPC wrapper. The public verification function returns only receipt status, organization, amount, payment method, event and date. Supabase security advisor returned **no findings** after these changes.

A rollback-only database test exercised authenticated contribution creation, sequential receipt allocation, audit trigger creation and owner audit visibility. A second rollback-only test exercised anonymous receipt verification. A further rollback-only test exercised contribution reversal, reversal metadata and the resulting audit entry. Supabase security and health advisors returned no findings after these changes. These are database-level tests, **not** a complete signed-in browser end-to-end test.

Still pending:
- Complete browser testing with a real account (signup, email verification, login, workspace creation, data entry, refresh, sign-out and public receipt verification)
- Offline queue currently covers contribution entry only; event, collector and expense edits still require connectivity
- Browser-level offline/reconnect testing on real Android and desktop devices
- Offline support for event, collector and expense writes (currently contribution entry only)
- Audit-log retention/export controls and full user-name attribution
- Expense receipt attachments, approval workflow, immutable cash closing and production financial controls
- Hosting/deployment and domain configuration

**Do not use this as an official financial ledger yet.** Treat the app as a test-stage prototype until the remaining browser and financial-control checks are complete.

Never put a Supabase service-role or secret key in frontend code. The frontend must use only the publishable key, with RLS providing the data boundary.

## Deployment

The current frontend is a static website. A static hosting service can serve the repository root; the entry point is `index.html`. No build command is required. Hosting and Supabase Auth redirect URLs have not yet been configured or verified.

## Next phase

Complete signed-in browser end-to-end testing, build robust offline synchronization and conflict handling, add contribution reversal/reconciliation workflows, and complete production financial controls before using it for real collections.
