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

### Configure Supabase

The app reads its Supabase settings from `config.js`, which must be next to `index.html` and `admin.html`. The current code expects the `CFG` object shown below:

```js
const CFG = {
  url: "https://your-project-ref.supabase.co",
  key: "your-anon-or-publishable-key"
};
```

Use the project URL and the **anon public** or publishable key from Supabase Project Settings → API. Never put a `service_role` or secret key in this file or in any browser-delivered file.

> `config.js` contains a public client key and is not a substitute for database security. Configure Supabase Row Level Security (RLS), storage policies, and organiser permissions before using the app with real users.

### Set up the database

This repository does not currently include a `schema.sql` or a documented database migration. Before running the app, create the Supabase tables, functions, storage bucket, and RLS policies required by the code in `index.html` and `admin.html`. In particular, the app uses:

- Supabase Auth for email sign-up, sign-in, confirmation, and password reset
- A `profiles` table with profile details and photo paths
- A private `photos` storage bucket for profile images
- Tables and policies for likes, passes, matches, messages, reports, blocks, Campus Buzz posts/reactions, events, RSVPs, and moderation logs
- The `unread_summary` database function for message badges
- Organiser access controls used by the admin dashboard

If you deploy the app to a different domain, also add that domain to Supabase Authentication → URL Configuration, including the password-reset redirect URL.

### Run locally

1. Clone the repository:

   ```bash
   git clone https://github.com/walterisalion/mukmatch.git
   cd mukmatch
   ```

2. Configure Supabase as described above.
3. Serve the directory with a local HTTP server. For example, with Python:

   ```bash
   python -m http.server 8000
   ```

4. Open `http://localhost:8000/` in your browser. The admin dashboard is available at `/admin.html`.

Opening `index.html` directly with a `file://` URL may cause authentication, module, or network restrictions in some browsers, so a local server is preferred.

## Deployment

The site is a static frontend and can be hosted on GitHub Pages or another static hosting provider:

1. Push the repository to GitHub.
2. Enable GitHub Pages for the repository branch and folder containing the site.
3. Add the deployed origin to Supabase Auth's allowed redirect URLs.
4. Confirm that the production Supabase RLS and storage policies prevent unauthorised reads and writes.
5. Test sign-up, email confirmation, password reset, photo upload, messaging, reporting, account deletion, and organiser access in production.

## How it was built

MukMatch was built by a team of four and tested with 15 real users. Feedback from testing led to two changes:

- Sign-up now works with **personal emails**, not just student emails
- The email confirmation code was shortened from **8 to 6 characters**

## Team

- **Walter** ([@walterisalion](https://github.com/walterisalion)): concept, name, and design direction; admin page and privacy page

## License

No license has been specified yet. Until a license is added, all rights are reserved by the copyright holder.
