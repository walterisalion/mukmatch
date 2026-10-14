# Contributing

Thanks for your interest in improving MukMatch.

## Development workflow

1. Fork the repository and create a feature branch.
2. Keep changes focused and aligned with the static frontend + Supabase architecture.
3. Run local smoke checks before opening a pull request.
4. Open a pull request with a clear description and testing notes.

## Local setup

```bash
git clone https://github.com/walterisalion/mukmatch.git
cd mukmatch
cp config.example.js config.js
python -m http.server 8000
```

Then open `http://localhost:8000/` in your browser.

## Supabase setup

- Copy the project URL and anon/publishable key into `config.js`.
- Run the SQL in `migrations/001_supabase_schema.sql` in the Supabase SQL Editor.
- Enable email auth, Auth redirect URLs, and the `photos` storage bucket before using the app with real users.
- Never commit real credentials, secrets, or production access tokens.

## Security expectations

- Do not expose `service_role` keys or secret tokens in browser-delivered code.
- Keep RLS and storage rules validated before shipping changes.
- Report security issues privately via the process in [SECURITY.md](SECURITY.md).

## Pull request checklist

- [ ] The change is scoped and easy to review
- [ ] README and config instructions are updated if needed
- [ ] No secrets or real user data are included
- [ ] Manual verification is described in the PR
