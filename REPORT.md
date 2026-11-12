# Report

## Built and working (verified by `npm test` and `npm run e2e`)
- Public site: Home (Studio Warm), How it works, For interpreters, For organizations, Privacy, About, Contact.
- `/login` split layout: Sign in / Create account / Forgot password; show-hide password; correct autocomplete;
  error summary that takes focus; Google button disabled with a plain note; request-organization-access link.
- Email auth stand-in: verification before sign-in, single-use tokens, generic errors, back-off, session list + revoke.
- Onboarding (language pairs, time zone, hours, modality) → Overview with empty first-run state, "Next up",
  "Needs attention", "Recent changes", support entry. No seeded or invented data anywhere.
- Assignments (list, calendar, filters, manual add, .ics import reading times only, conflict + availability
  warnings), detail with computed readiness, real device check (mic level via getUserMedia, headset confirmation,
  online status, latency to own server, browser support), session focus mode (dark theme, "/" term search, mobile
  bottom sheet, optional break reminder), close-out (technical issue → support request).
- Availability, Terminology (versions, CSV import/export, Unverified by default on import), Resources, Support
  (threads), Activity log (filter by area, append-only in-app), Settings (profile, consent records, retention,
  sessions, JSON export, deletion with typed confirmation).
- Policy engine (T3 hard stop, default-deny, adapter never reached on denial), RBAC matrix, ICS/CSV parsers: 18 unit tests.
- Playwright + axe: no violations on 10 screens; no console errors; no horizontal overflow at 1440 and 390 px.

## Not built / unfinished
- **No backend.** No Next.js, Prisma, Auth.js, Postgres, SMTP, Argon2id, httpOnly cookies, CSRF, CSP headers or rate
  limiting that survives clearing browser storage. The client-side auth is a stand-in, not security.
- **Google sign-in** is disabled (needs a server OAuth callback). No fake flow.
- **Organization side**: invitations, directory, assignment offers from coordinators, support queue, approved
  resources, policy configuration, org audit view, cross-org denial E2E. Only the RBAC matrix and its unit tests exist.
- No human support team is connected; support requests are stored locally and the UI says so.
- Self-hosted fonts are bundled (Instrument Sans/Serif, IBM Plex Mono). Command palette is simple (no arrow-key list).
- "Assigned" and "Awaiting response" support statuses are only reachable from a support-side console that doesn't exist.
- Playwright is run as a script (`e2e/flow.mjs`), not the `@playwright/test` runner.
- Playwright was not run in WebKit/Firefox, and no screen reader testing was done beyond axe.

## Assumptions
- A front-end-only deliverable was the right scope for this environment; the Next.js stack in the brief was not available.
- Public site follows the picked "Studio Warm" design; the app follows the quiet editorial design system. They share
  ink and forest tones but intentionally differ.
- Orange was darkened to `#B43712` for text and white-on-orange contrast (the sketch's `#E5582E` fails AA).

## Risks
- Data lives in `localStorage`: clearing site data loses everything; anyone with device access can read it.
- Passwords are hashed client-side; this is demo-grade only and must not hold real credentials.
- `REFERENCE_ANALYSIS.md` was not verified against live reference pages.
- Not production-ready, and no compliance (HIPAA, GDPR, SOC 2) or certification is claimed.
