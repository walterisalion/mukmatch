# MukMatch 💘

**Find your person on the Hill.**
MukMatch is a web platform that helps university students connect, interact, and find dates or potential romantic partners.

🔗 **Live demo:** https://walterisalion.github.io/mukmatch/

## Features

- **Discover:** swipe through student profiles, pass or like, and view multiple photos per profile
- **Matches and chat:** message people you've matched with
- **Campus Buzz:** a space for anonymous campus posts
- **Date ideas and events:** suggestions for dates and campus events
- **Profiles:** first name, age, year, college, where you stay, interests, and 1 to 4 photos
- **Email confirmation:** sign up with any email address and confirm with a 6-character code
- **Admin dashboard:** administrators review and moderate reported accounts (`admin.html`)

## Safety and privacy

- Strictly **18+**: users confirm their age and agree to the Terms and Privacy Policy at sign-up
- **Report** and **Block** are available at any time
- Reported accounts are reviewed by moderators through the admin page
- Users can **delete their account and data** whenever they want
- Safe-dating reminders are shown in the app: meet in public places and tell a friend

See [Terms](terms.html) and [Privacy Policy](privacy.html).

## Built with

- HTML, CSS, and JavaScript
- [Supabase](https://supabase.com) for authentication and database
- GitHub Pages for hosting

## Getting started

1. Clone the repository:
```bash
   git clone https://github.com/walterisalion/mukmatch.git
   cd mukmatch
```
2. Create a Supabase project at supabase.com.
3. Create a file named `config.js` next to `index.html` with your Supabase details:
```js
   window.CONFIG = {
     SUPABASE_URL: "your-project-url",
     SUPABASE_ANON_KEY: "your-anon-key"
   };
```
   *(Adjust the variable names to match your code.)*
4. [Add your database setup here, e.g. "Run `schema.sql` in the Supabase SQL editor."]
5. Open `index.html` in a browser, or serve the folder with a local server.

## How it was built

MukMatch was built by a team of four and tested with 15 real users. Feedback from testing led to two changes:
- Sign-up now works with **personal emails**, not just student emails
- The email confirmation code was shortened from **8 to 6 characters**

## Team

- **Walter** ([@walterisalion](https://github.com/walterisalion)): concept, name, and design direction; admin page and privacy page
  

## License

[Add a license, e.g. MIT, or remove this section]
