# MukMatch 💘

[![Live demo](https://img.shields.io/badge/demo-live-ff4f8b)](https://walterisalion.github.io/mukmatch/)
[![Hosted with GitHub Pages](https://img.shields.io/badge/hosted%20with-GitHub%20Pages-222222?logo=github)](https://pages.github.com/)

**Find your person on the Hill.**

MukMatch is a static web platform that helps university students connect, interact, and find dates or potential romantic partners.

## Project status

MukMatch is a prototype/starter app for student discovery, matching, and campus community interactions. It runs as a static frontend backed by Supabase for authentication, storage, and database functionality.

The live demo is intended for demonstration and testing. Before using the app with real users, review the security, moderation, privacy, and deployment steps below.

🔗 **Live demo:** https://walterisalion.github.io/mukmatch/

## Features

- **Discover:** browse student profiles, pass or like, and view multiple photos per profile
- **Matches and chat:** message people you've matched with
- **Campus Buzz:** share anonymous campus posts
- **Date ideas and events:** discover date suggestions and campus events
- **Profiles:** first name, age, year, college, residence, interests, an ideal-date prompt, and 1–4 photos
- **Email authentication:** create an account with any email address, confirm it by email, and reset your password
- **Admin dashboard:** organisers review reports, manage events, and moderate reported accounts through `admin.html`
- **Theme support:** switch between light and dark mode

## Why MukMatch exists

MukMatch is designed around campus-specific discovery: helping students meet people connected to the same university community while providing basic tools for consent, reporting, blocking, and safer meetups.

## Safety and privacy

- Strictly **18+**: users confirm their age and agree to the Terms and Privacy Policy at sign-up
- **Report** and **Block** are available at any time
- Reported accounts are reviewed by organisers through the admin dashboard
- Organisers cannot read private messages through the app
- Users can edit their profile and delete their account and associated data
- Safe-dating reminders encourage users to meet in public places and tell a friend

See [Terms](terms.html) and [Privacy Policy](privacy.html).

## Built with

- HTML, CSS, and JavaScript
- [Supabase](https://supabase.com) for authentication, database, storage, and email
- GitHub Pages for hosting

## Getting started

### Prerequisites

- A modern web browser
- A Supabase project
- A local static web server for development (recommended)

### 1) Configure local `config.js` safely

The app reads runtime config from `config.js` (next to `index.html` and `admin.html`).

1. Copy the template:

   ```bash
   cp config.example.js config.js
   ```

2. Edit `config.js` with your project values:

   ```js
   const CFG = {
     url: "https://your-project-ref.supabase.co",
     key: "your-anon-or-publishable-key"
   };
   ```

Use only the **anon/publishable** key from Supabase. Never use `service_role` or any secret key in browser code.

> `config.js` is listed in `.gitignore` for safer local usage. This keeps project-specific credentials out of version control while preserving a local override for your environment.

### 2) Set up Supabase database + storage

Run the SQL in [`migrations/001_supabase_schema.sql`](migrations/001_supabase_schema.sql) in the Supabase SQL Editor.

This migration creates the database objects inferred from `index.html` and `admin.html`, including:

- Tables: `profiles`, `likes`, `matches`, `messages`, `reads`, `reports`, `blocks`, `posts`, `reactions`, `events`, `rsvps`, `admins`, `mod_log`
- Views: `buzz_feed`, `event_counts`
- Functions/RPCs: `unread_summary`, `delete_my_account`, `admin_remove_profile`
- Trigger: auto-create matches after mutual likes
- Private storage bucket: `photos`
- RLS and storage policies for student and organiser/admin flows

#### Manual security steps you still need

Some behavior cannot be fully and safely inferred from frontend code alone:

1. **Ban durability after account removal**  
   `admin_remove_profile` deletes the target auth user and app data. If you need permanent email/device-level bans, add a separate Supabase auth hook or external moderation workflow.

2. **Auth URL configuration**  
   In Supabase Auth → URL Configuration, add the site URL and the redirect URL used by password reset and email links.

3. **SMTP/email provider setup**  
   Configure Supabase Auth email delivery for sign-up verification and password reset.

4. **Realtime**  
   The migration attempts to add `public.messages` to `supabase_realtime`. If your project differs, enable Realtime for `messages` manually.

### 3) Create organiser/admin access

Add organiser users after they exist in `auth.users` by inserting their UUID into `public.admins`:

```sql
insert into public.admins (user_id) values ('<auth-user-uuid>');
```

The admin dashboard (`admin.html`) relies on this table and its RLS policies for access gating.

### 4) Verify RLS and storage

After migration:

1. Confirm RLS is enabled on all created tables.
2. Test with two separate accounts:
   - User A cannot edit User B's profile, messages, or reads.
   - A non-admin cannot read reports or the moderation log.
   - An admin can read reports and manage events.
3. Confirm that each user can upload and delete photos only in their own `<uid>/...` folder.
4. Confirm authenticated users can resolve the signed photo URLs used by discovery and matching screens.

### Run locally

1. Clone the repository:

   ```bash
   git clone https://github.com/walterisalion/mukmatch.git
   cd mukmatch
   ```

2. Configure Supabase and `config.js` as described above.
3. Serve the directory with a local HTTP server. For example, with Python:

   ```bash
   python -m http.server 8000
   ```

4. Open `http://localhost:8000/` in your browser. The admin dashboard is available at `/admin.html`.

Opening `index.html` directly with a `file://` URL may cause authentication, module, or network restrictions in some browsers, so a local server is preferred.

## Deployment

The site is static HTML/CSS/JS and works on GitHub Pages or another static host:

1. Push the repository to GitHub.
2. Enable GitHub Pages for the repository branch and folder containing the site.
3. Deploy with your own `config.js` containing only the project URL and anon/publishable key.
4. Add the deployed origin to Supabase Auth redirect URLs.
5. Confirm production RLS and storage policy behavior before inviting users.
6. Run the manual verification checklist below against the deployed site.

## Manual verification checklist

- [ ] Sign up with email and password
- [ ] Complete email confirmation and resend flow
- [ ] Sign in and reset a password
- [ ] Create and edit a profile, including photos
- [ ] Like and pass on profiles
- [ ] Confirm a mutual-like match appears
- [ ] Send and receive chat messages; verify unread badges
- [ ] Create Campus Buzz posts and reactions
- [ ] View events and toggle RSVPs
- [ ] Report and block from relevant user flows
- [ ] Delete an account and verify sign-out/data removal
- [ ] Confirm only admins can access the organiser dashboard
- [ ] Test report dismissal and remove/ban flows in the admin dashboard

## Screenshots / demo media

Screenshots and GIFs can be added here when available. Do not use fabricated media or include real user data.

Suggested captures:

- Discovery screen
- Matches and chat
- Campus Buzz
- Events
- Admin dashboard

## Contributing

Contributions are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

## Security

Please review [SECURITY.md](SECURITY.md) before reporting a vulnerability or security issue.

## How it was built

MukMatch was built by a team of four and tested with 15 real users. Feedback from testing led to two changes:

- Sign-up now works with **personal emails**, not just student emails
- The email confirmation code was shortened from **8 to 6 characters**

## Team

- **Walter** ([@walterisalion](https://github.com/walterisalion)): concept, name, and design direction; admin page and privacy page

## License

This project is licensed under the [MIT License](LICENSE).

Copyright (c) 2026 Walter Isalion
