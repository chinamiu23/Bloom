# Bloom — Assignment prompt

Copy the prompt below into an AI coding assistant. The deliverable is a working public application, not only a written proposal. The app name, purpose and feature requirements differ from DailyBudget; the architecture and quality expectations are adapted from the supplied reference prompts.

---

## Project: Bloom — Public Study, Habit and Goal Planner

Act as a senior full-stack software developer, UI/UX designer, security engineer and educational application expert. Build a complete, functional, responsive web application called **Bloom — Your Little Growth Space**.

The app helps students and lifelong learners manage assignments, develop healthy daily habits, record study sessions and track personal learning goals. Users enter their own data. My goal is to publish the app on **GitHub Pages** so other people can create accounts and use their own private workspace.

Start by building the actual application. Do not provide only a tutorial, mockup, pseudocode, or a landing page with non-working buttons. Make reasonable assumptions and clearly identify anything that requires external account configuration or has not been tested.

### 1. Required deliverables and technology

Provide these complete source files:

- `index.html`: all frontend HTML, CSS and vanilla JavaScript in exactly one file.
- `supabase_setup.sql`: normalized PostgreSQL schema, constraints, indexes, timestamps, ownership policies, and necessary functions.
- `README.md`: beginner-friendly configuration, local preview, GitHub Pages deployment, security, backups, testing and maintenance instructions.
- `.gitignore`: exclude secrets and private data exports.

Do not require React, npm, Node.js, compilation, or a custom server to run the frontend. Use the Supabase JavaScript client from a reputable CDN with a pinned version. Charts may use an appropriate lightweight library or accessible inline charts. The SQL file is backend setup, not a frontend build step.

### 2. Visual design

Create an original Bloom flower logo and a welcoming, feminine but readable design with blush pink backgrounds, white cards, rose accents, and subtle sage success indicators. Use rounded corners, clear typography, comfortable spacing and restrained decoration. Include light and dark modes. Build for desktop, tablet and mobile, with desktop sidebar and mobile navigation. Do not rely on color alone to communicate status.

Use friendly language such as “Small steps. Beautiful growth.” Avoid judgmental messages or fabricated claims about the user's progress.

### 3. Public landing page and authentication

Create a polished landing page with logo, description, feature overview, signup/login buttons, privacy information and an explicitly labeled sample demo.

Implement email/password registration, login, logout, email verification when configured, password recovery, session persistence, expiration handling, profile name and preferences. After login show the user's private dashboard. Clear loaded records on logout and when the authenticated account changes. Use Supabase Auth; do not create your own password storage.

Include obvious configuration constants for the project URL and publishable/legacy anon key. Never put service-role/secret keys or database passwords into the public HTML. The default unconfigured state must explain that account setup is required rather than pretend registration works.

### 4. Dashboard

Put important information at the top:

- Tasks due today or overdue
- Habits completed out of today's scheduled habits
- Today's saved focus time
- Active learning goals
- Upcoming or overdue assignments
- Daily habit check-ins and streaks
- Weekly focus-time chart
- Learning goal progress

Derive every figure from the user's actual saved data. Display helpful empty states for new users.

### 5. Tasks and assignments

Support create, view, edit, delete, complete/reopen, search and filter. Each task includes unique ID, authenticated owner ID, title, subject, due date, priority (Low/Medium/High), status, completion date, notes, creation/update timestamps. Sort upcoming tasks by deadline, identify overdue tasks, and keep completed tasks available. Toggling complete must update the completion date correctly; reopening clears it.

### 6. Habit tracker

Allow creating, editing and deleting habits with a name, Daily or Weekdays schedule, start date and optional notes. Permit one check-in per habit per calendar date and allow undo. Support viewing and correcting past check-ins; reject future check-ins.

Explain streak calculations. Daily habits require consecutive calendar days; weekday habits skip weekends. A missed check-in today may preserve yesterday's streak until the next scheduled day. Do not count days before the habit's start date. Habit consistency uses scheduled opportunities, not every calendar day. Clearly state if changing a habit's schedule recalculates history. Confirm that deleting a habit also deletes its check-ins.

### 7. Study and break timer

Provide configurable focus and break durations, a subject field, a countdown, start, finish/save, and cancel controls. Store the start time so refresh or reopening can reconstruct the timer. Support at most one active session per user across devices, enforced in the database.

Finishing must record actual elapsed time up to the planned duration. Saving the same session twice must not duplicate recorded time. Use server time for persisted session durations. Keep completed session history and allow deleting records with confirmation. Exclude breaks from focus-time statistics.

Do not claim to verify whether someone was actually studying. Explain that background audio, automatic reminders or pause features are unsupported if they are not implemented.

### 8. Personal learning goals

Users can create, edit and delete goals such as completing a course, reading books or practicing a skill. Each goal has name, target number, manually recorded progress, unit (lessons/books/steps), target date and notes. Display remaining progress and percentage, and permit small +1/−1 adjustments. Do not imply progress is automatically linked to tasks or timers unless that integration is implemented.

### 9. Reports

Include daily focus-time charts, weekly/monthly focus totals, completed-task counts, habit check-in counts/consistency, goal progress, and custom date ranges. Provide accessible chart values in a table or details panel. Export study records to CSV with proper quoting and spreadsheet formula-injection protection. Only export the signed-in user's records.

Insights must be simple calculations from recorded data. Never invent study hours, habit streaks or achievements. Report date and denominator conventions.

### 10. Database and authorization

Create well-structured tables for profiles/preferences, tasks, habits, habit logs, study sessions, and goals. Use user ownership fields linked to Supabase Auth users. Include primary/foreign keys, constraints, timestamps, owner/date indexes, unique daily habit check-ins and a partial unique index for an active session.

Enable RLS on every table with SELECT, INSERT, UPDATE and DELETE policies. Enforce `auth.uid() = user_id` in USING and WITH CHECK. Prevent changing ownership to another account. A check-in must not be linked to another user's habit. Anonymous users must not query private tables. Do not treat hiding UI elements or JavaScript filters as authorization.

Prefer SECURITY INVOKER for functions. Any necessary privileged account-deletion function must accept no arbitrary target user ID, derive the caller securely, use a safe empty search_path, and have explicit execution grants excluding PUBLIC/anonymous callers.

### 11. Privacy, export and account deletion

Never send user notes, tasks or study records to an AI service. Do not log credentials, access tokens or private records. HTML-escape user-generated content. Use Supabase's secure session handling and PKCE where appropriate; do not put personal records or access tokens in URL parameters.

Provide a full JSON data export, CSV export and account deletion with confirmation and password recheck. Explain backup scope and whether restoration is implemented. Financial/bank access is not needed for this app. Demo records must be labeled and kept separate from real user accounts.

### 12. Usability and validation

Every navigation item, button, form and filter must work. Include required-field validation, reasonable length limits, valid dates, bounded whole-number targets/durations, safe error messages, success notifications, deletion confirmations, keyboard-accessible fields and modal focus handling. Reject invalid dates, negative progress and non-finite values. Handle offline/database failures honestly; do not report success before a save succeeds.

### 13. Deployment and verification

Explain how to create a GitHub repository and enable Pages from `main` / root. Explain Supabase project creation, SQL setup, public-key configuration, email provider/SMTP, verification and redirect URLs, and the exact account settings that must be completed before inviting other users. No local frontend build may be required.

Test task CRUD/search/filter/completion, habit check-ins/streaks, timer restoration/finish, goal progress, reports, CSV escaping, account isolation and invalid input. Clearly distinguish automated source/calculation checks from live browser, database and authentication tests. Do not claim deployment or production readiness without verifying it.

Return the complete files and the deployed URL if deployment was possible. If external setup blocks deployment, finish all independent implementation work, provide the files, and identify only the concrete remaining steps.
