# Template Family Task — upgrade preparation (NOT a completed sync)

## Verified starting points
- Public destination: `Ioonooni/family-task-template` (`main`).
- Latest private source used for comparison: `Ioonooni/family-task-ioon` at `912e58cf5d09961e03e5c54763858234f1e91c7a`.
- Source includes binary artwork and fonts that the connected text-only GitHub file reader cannot transfer safely. This branch therefore does **not** assert the latest UI has been cloned.
- The distinct `Ioonooni/family-task` production repo is **out of scope**: do not overwrite its main branch or reuse its Supabase/Google/Vercel settings.

## Safe migration checklist
1. With authorized local Git access to both repositories, copy reviewed source files and needed image assets into a fresh destination branch. Do not copy `.env`, credentials, browser sessions, `node_modules`, build output, or private documentation. Check license/reuse permission for any third-party/demo fonts and artwork before publishing them.
2. Rename user-facing branding to **Template Family Task** in the page title, login heading, manifest name/short_name/description, push notifications, and app-specific metadata. Keep code-level domain keys stable; use general-purpose example category labels instead of personal names.
3. Require each deployer to configure `VITE_SUPABASE_URL`, `VITE_SUPABASE_PUBLISHABLE_KEY`, `VITE_VAPID_PUBLIC_KEY` and `VITE_GOOGLE_CLIENT_ID`. Never carry over source VAPID public-key fallback or concrete source project IDs, endpoints, scheduler secret names, OAuth IDs, storage buckets or hosted URLs. Keep all private values outside Git.
4. Audit SQL/Edge Function bootstrap: replace instance-specific `get_ioon_push_config()`, Vault secret names and Cron URLs with reusable names. Keep server-side secrets server-side, with restricted EXECUTE and row-level security. Do not apply SQL to any live project as part of a template source sync.
5. Confirm user-facing privacy policy statements match the reusable product. Preserve Task CRUD, attachments, Finance Google Drive/Sheets integration, reminders, badge, and Service Worker.
6. Run `npm ci && npm run check`; inspect image paths and fonts. Visual smoke-test iPhone widths and Login, Home, folder browse, task detail, Loading, Privacy, Finance. Confirm deployment cannot connect to either original private Supabase project.
7. Open a separate review PR, verify all new public assets and source references, and merge only after tests and authorization. Deploy to an isolated demo project, never to another person's existing production instance.

## Shared family tasks (future feature, NOT implemented by cloning)
Current RLS scopes `tasks.owner_id = auth.uid()`, so different email accounts cannot see each other's tasks. To implement true shared household access, design and migrate:
- `families` and `family_members` with invite/accept flow, roles, and membership-based RLS.
- `tasks.family_id`, `created_by`, and optional `assigned_to` (identity and provenance kept separately).
- Owner-only personal tasks vs family-visible tasks; keep existing task records private until a deliberate migration decision.
- Permissions for attachments, push subscriptions, deduplicated per-recipient reminder deliveries, and clear revocation when a member leaves.
- Notification routing: creator/assignee only by default; opt-in notify-all family mode, honoring each member's own subscription and permission. Separate data visibility from notification preference.
- Integration tests for owner/member/nonmember access and revocation, with no guessed secrets or production migrations.

This is a preparation document only; no live backend, shared permissions, or deployment has been changed.
