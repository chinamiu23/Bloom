# Bloom — Your Little Growth Space 🌷

Bloom is an original study, habits and goals app adapted from the DailyBudget reference prompts. It has a blush-pink design, desktop sidebar, mobile navigation, light/dark modes, task management, habit check-ins/streaks, a focus/break timer, learning goals, reports and exports.

**Assignment deliverables:** `PROMPT.md` contains the full English generation prompt; `index.html` is the generated application; `supabase_setup.sql` is its backend setup. No npm, Node.js, React or frontend compilation is required.

## Try it

Open `index.html` in a browser and choose **Explore demo**. Demo forms, editing, checking habits, timers, goal progress and exports work in memory. It is clearly labeled and disappears on leaving/reloading. Real cloud accounts require the setup below. Safari's Files preview may not execute HTML; upload the file to GitHub Pages for a normal iPhone browser experience.

## Publish from iPhone

1. Open https://github.com/new . Create a **Public** repository named `bloom`, with a README. Upload the individual files into its root (not only the ZIP).
2. Open https://supabase.com/dashboard . Create a dedicated Bloom project in your organization. Check its plan/fees and keep its database password private.
3. Supabase → SQL Editor: paste all of `supabase_setup.sql` and run it. Tables use `bloom_` names so they do not reuse DailyBudget tables. Use a dedicated project for account deletion: deleting the Auth account cascades its records in that project.
4. Find the project URL and **publishable** key in Supabase's Connect/API settings. Edit these two constants in `index.html`:
   ```js
   const SUPABASE_URL='https://your-project.supabase.co';
   const SUPABASE_PUBLIC_KEY='sb_publishable_...';
   ```
   A legacy anon key is supported. Never use a service_role/secret key, database password or management token. The client rejects privileged keys.
5. GitHub repository → Settings → Pages → Deploy from a branch → `main` → `/(root)` → Save. No application build is required. The expected URL is `https://YOUR_USERNAME.github.io/bloom/`; do not call it live before GitHub reports deployment success.
6. Supabase Authentication: enable email/password and email confirmation. Set Site URL and allowed Redirect URL to the exact Pages URL with trailing slash. Add `/index.html` as well if you use that explicit URL. Configure production SMTP for signups/reset emails outside your project team; the built-in email service can have restrictions.
7. Signup verification and recovery use PKCE. Open email links in the same browser/device used to request them. Passwords must have at least 12 characters. Test signup/login/recovery with two real test accounts before inviting classmates.

The connected accounts previously had no accessible repository or Supabase project. **Bloom has not been published or connected to a database yet.** Source generation is complete; setup and live verification remain.

## Use Bloom

- **Dashboard:** today/overdue tasks, scheduled habit completions, saved focus time, goals, and seven-day focus chart.
- **Tasks:** add/edit/delete, search, status/priority filters, due-date ordering, complete/reopen. Reports count completion dates of tasks still marked done, not every historical completion event.
- **Habits:** Daily/Weekdays schedules, one check-in per habit/day, past corrections and scheduled-day streaks. No future check-ins. Weekends are skipped for weekday habits. Missing today preserves yesterday's streak until another scheduled day passes. Changing a habit's schedule/start date changes historical consistency calculations. Deleting a habit cascades its check-ins after confirmation.
- **Study timer:** focus/break, subject and planned minutes, start, finish/save and cancel. Cloud start time reconstructs the countdown after refresh; a unique database index limits each user to one active timer. Finish uses server elapsed time capped at the plan duration and locks the session to prevent duplicate finish. No pause, background sound or automatic finishing; tap Finish to save. Cancel deletes the pending session. Sessions are assigned to their local start date even if they finish after midnight. Focus time excludes breaks and unsaved active timers.
- **Goals:** manually entered target/progress/unit/date with +1/−1 and edit controls. Progress is not automatically linked to tasks or timers. Percentages can exceed 100% and the visual bar caps at 100%.
- **Reports:** range focus totals, completed tasks, habit consistency, daily bars (up to 31 days), accessible numbers, weekly and calendar-month totals, session history and CSV.
- **Settings:** name, focus/break defaults, theme, JSON export, CSV, refresh, signout and deletion.

## Data and security

Real accounts use Supabase Auth and PostgreSQL. Every `bloom_` table has RLS, authenticated ownership checks, an immutable ownership trigger and an Auth user foreign key. Habit logs reference `(habit_id,user_id)` to prevent cross-user links. Anonymous table privileges are revoked. Update policies include SELECT plus USING/WITH CHECK. Record edits use `updated_at` for stale-edit protection.

Sessions use SECURITY INVOKER finishing logic so RLS remains enforced. The account-deletion RPC is the sole SECURITY DEFINER operation; it derives `auth.uid()` itself, has an empty search_path, accepts no target ID and grants execution only to authenticated users. The UI rechecks the password. Deleting an auth user does not immediately expire every issued JWT, but its rows are removed and ownership foreign keys prevent reinserting rows for the deleted identity. Use a dedicated project; deleting that account removes all of its project data.

Supabase manages session token storage/refresh. App records are held in memory and fetched from the private database, never used as localStorage authorization. UI state clears on logout/account changes. User content is HTML-escaped. CSV fields are quoted and formula-prefix escaped, including leading whitespace. No AI, analytics, bank credentials or private keys are included. The pinned Supabase SDK is loaded from jsDelivr; review future updates and CDN supply-chain risk. Display-name metadata is for display only, never authorization.

Data exports contain private records; do not upload them to GitHub. JSON export contains the currently loaded snapshot, including all paginated rows. Refresh before exporting. Automatic restore is not implemented; retain the JSON for administrator-assisted restoration and use Supabase backup facilities appropriate to the project plan. Accounts and profile data are exportable; passwords/tokens are not. Account deletion is permanent.

## Verification and maintenance

JavaScript syntax and automated in-memory checks cover rendering, task CRUD/completion, habit streaks/check-ins, timer caps/restoration, goal updates, report totals and safe CSV/HTML handling. **Live SQL migration, RLS isolation, auth email flows and browser click-through/mobile rendering have not been tested**, because no live project/site exists and browser automation was unavailable. The SQL schema is source-reviewed, not a demonstrated live security audit.

Before public launch, test two accounts: A must not read/edit/delete B's rows or insert B's user_id; anonymous requests must not read private tables. Test concurrent active timers/finish, refresh, password recovery, export and account deletion. Run Supabase security advisors. Review email limits, project quotas/availability, abuse protection and privacy/contact information. Schema changes should be new tracked migrations; the initial SQL is re-runnable but not a general upgrade engine.

## Submission

Submit `PROMPT.md`, the source files, and your GitHub repository/Pages URLs after deployment. Explain that Bloom adapts DailyBudget's dashboard/CRUD/reports/security architecture to student productivity. Do not submit the old DailyBudget source as the Bloom project.

References:
- https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
- https://supabase.com/docs/guides/database/postgres/row-level-security
- https://supabase.com/docs/guides/auth/passwords
- https://supabase.com/docs/guides/auth/redirect-urls
