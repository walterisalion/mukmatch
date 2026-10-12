# MukMatch 💘

**Find your person on the Hill.**

MukMatch is a web platform that helps university students connect, interact, and find dates or potential romantic partners.

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

> `config.js` is now listed in `.gitignore` for safer local usage. The repository still keeps the existing tracked `config.js` for current deployment compatibility; replace values locally for your own environment.

### 2) Set up Supabase database + storage

Run the SQL in [`migrations/001_supabase_schema.sql`](migrations/001_supabase_schema.sql) in Supabase SQL Editor.

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
   In Supabase Auth → URL Configuration, add:
   - Site URL (e.g. your GitHub Pages URL)
   - Redirect URL for password reset/email links (same origin/path used by the app)

3. **SMTP/email provider setup**  
   Configure Supabase Auth email delivery for sign-up verification and password reset.

4. **Realtime**  
   The migration attempts to add `public.messages` to `supabase_realtime`. If your project differs, enable Realtime for `messages` manually.

### 3) Create organiser/admin access

Add organiser users (after they exist in `auth.users`) by inserting their UUID into `public.admins`:

```sql
insert into public.admins (user_id) values ('<auth-user-uuid>');
```

The admin dashboard (`admin.html`) relies on this table and RLS policies for access gating.

### 4) Quick RLS/storage verification

After migration:

1. Confirm RLS is enabled on all created tables.
2. Validate access with two different test users:
   - user A cannot edit user B profile/messages/reads
   - non-admin cannot read reports or moderation log
   - admin can read reports/mod log and manage events
3. Validate storage:
   - each user can upload/delete photos only in their own `<uid>/...` folder
   - authenticated users can resolve signed photo URLs needed by discover/match screens

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

The site is static HTML/CSS/JS and works on GitHub Pages or any static host:

1. Push the repository to GitHub.
2. Enable GitHub Pages for the repository branch and folder containing the site.
3. Deploy with your own `config.js` (URL + anon/publishable key only).
4. Add your deployed origin to Supabase Auth redirect URLs.
5. Confirm production RLS/storage policy behavior before inviting users.

## Manual verification checklist

- [ ] Sign-up with email + password
- [ ] Email confirmation code and resend flow
- [ ] Sign-in and password reset flow
- [ ] Profile creation/edit (name, age, college, interests, prompt, photos)
- [ ] Discover like/pass interactions
- [ ] Mutual-like match appears
- [ ] Chat send/receive and unread badge updates
- [ ] Campus Buzz posting and reactions
- [ ] Event list and RSVP toggling
- [ ] Report + Block from discover/chat flows
- [ ] Account deletion removes profile data and signs user out
- [ ] Organiser dashboard access for admins only
- [ ] Report dismissal and remove/ban flow in admin dashboard

## Screenshots / demo media (placeholder)

Add screenshots or GIFs here when available (do not use fabricated media):

- Discovery screen
- Matches/chat
- Campus Buzz
- Events
- Admin dashboard

## How it was built

MukMatch was built by a team of four and tested with 15 real users. Feedback from testing led to two changes:

- Sign-up now works with **personal emails**, not just student emails
- The email confirmation code was shortened from **8 to 6 characters**

## Team

- **Walter** ([@walterisalion](https://github.com/walterisalion)): concept, name, and design direction; admin page and privacy page

## License

No license has been specified yet. Until a license is added, all rights are reserved by the copyright holder.
