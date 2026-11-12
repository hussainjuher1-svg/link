# VoxRelay

A calm workspace for **human** remote interpreters: assignments → prep → session → close-out.
VoxRelay is not an automatic translator and never listens to, records or transcribes sessions.

> **Status: front-end-only early build.** There is no server. Accounts, data and the activity log live in this
> browser's `localStorage`. See [REPORT.md](REPORT.md) for what is and isn't built.

## Run

```bash
npm install
npm run dev         # development
npm run build       # production build → dist/index.html (single file, hash routing)
npm test            # Vitest: policy engine (T3, default-deny), RBAC matrix, auth validation, ICS/CSV
npm run e2e         # build first; Playwright + axe full flow (needs `npx playwright install chromium`)
```

`npm run e2e` serves `dist/` on port 4173 and walks: register → verify → onboard → add assignment → prep →
device check (fake microphone) → session → close-out → support request, plus axe (WCAG 2.2 AA) on key screens
and a mobile overflow check. Set `SHOTS=1` to save screenshots to `e2e/shots/`.

## Environment variables

None are read by this build. The planned server build needs:

| Variable | Purpose |
| --- | --- |
| `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET` | Google OAuth 2.0 / OIDC |
| `AUTH_SECRET`, `AUTH_URL` | Auth.js session signing and base URL |
| `DATABASE_URL` | Postgres (SQLite locally) |
| `SMTP_*` | Email delivery |

### Google sign-in (not available yet)
The login page always shows **Continue with Google** disabled with a plain explanation, because a real OAuth code
flow with PKCE needs a server-side callback. When the server build exists: create an OAuth client in Google Cloud
Console → APIs & Services → Credentials, add `AUTH_URL/api/auth/callback/google` as an authorized redirect URI, and
set the variables above. Only accounts with `email_verified` may link, and only by verified email.

## Where things are
- `src/site/` public site (Studio Warm look) · `src/auth/` login, verify, reset
- `src/app/` signed-in workspace (quiet editorial look) · `src/components/` UI kit and device check
- `src/lib/policy.ts` AssistancePolicy + engine · `src/lib/authz.ts` RBAC matrix · `src/lib/auth.ts` client auth
- Docs: [ARCHITECTURE.md](ARCHITECTURE.md), [REFERENCE_ANALYSIS.md](REFERENCE_ANALYSIS.md), [REPORT.md](REPORT.md)
