# Architecture

## Current build vs. target
**Built:** Vite + React + Tailwind SPA, hash routing, all state in `localStorage` (`vr.*` keys; per-user data under
`vr.data.<accountId>`). **Target:** Next.js + TypeScript, Postgres/Prisma, Auth.js, Zod. The pure modules
(`policy`, `authz`, `ics`, `csv`, `validate`) have no browser dependencies and port unchanged.

## Roles
Owner, Admin, Coordinator, Interpreter. New accounts are Interpreters; org roles only by invitation (not built).

## Workflow
Assignment (Offered → Scheduled) → Prep (checklist + terms + resources + device check; status shows *Preparation
required* until readiness is 100%, then *Ready*) → Session (In progress) → Close-out (Completed, exceptions by
category; "Technical issue" creates a support request). Side states: Rescheduled, Cancelled.
Readiness is computed (`readiness()` in `lib/store.tsx`), never stored.

## Screen map
Public: Home, How it works, For interpreters, For organizations, Privacy & responsible technology, About, Contact.
Auth: Login (sign in / create / forgot / check email), Verify, Reset. App: Onboarding, Overview, Assignments
(list + calendar, add, .ics import), Assignment detail, Session (focus mode), Close-out, Availability, Device check,
Terminology, Resources, Support, Activity log, Settings.

## Data model (`src/lib/types.ts`)
Profile, Assignment (internal ID, no client identity), ChecklistItem, DeviceResult, Term (+versions), Resource,
SupportRequest (+updates), AuditEvent (actor, org, area, type, objectRef, result, policyVersion), ConsentRecord
(type, subject, state, timestamp, policyVersion), availability (weekly + overrides), prefs.

## Safety architecture
- `AssistancePolicy` is versioned (`assistance-policy/1.0.0`), default-deny, AI disabled. T0–T2 ship with **no**
  permitted actions; they are to be defined by the deploying organization, not invented here.
- **T3 is human only.** `evaluateAssist` checks `tier === "T3"` first, before reading the policy, flags or adapter, and
  returns `Human support required`, `route: "human-support"`, and a metadata-only audit record. `runAssist` never calls
  the adapter on a denied path. Covered by tests including permissive policies and extra flags.
- Model output (if ever enabled) is truncated to the tier limit and returned as `Unverified`, `requiresConfirmation`.
- The session workspace's "I need human judgment" button calls the engine with T3, creates an *Escalated* support
  request, and logs a `Safety` event. No conversation content is accepted anywhere; free text is capped and warned.

## Permission matrix (`src/lib/authz.ts`, default deny, cross-org always denied)
| Permission | Owner | Admin | Coordinator | Interpreter |
| --- | :-: | :-: | :-: | :-: |
| org.settings.write | ✓ | | | |
| org.invitations.write, org.policy.write, org.audit.read | ✓ | ✓ | | |
| org.members.read, assignment.offer.write, support.queue.read | ✓ | ✓ | ✓ | |
| resource.approved.read | ✓ | ✓ | ✓ | ✓ |
| resource.approved.write | ✓ | ✓ | | |
| glossary.personal.read, notes.personal.read, assignment.own.read | owner-of-record only, any role | | | |

Not yet enforced by a backend.

## Tier policy
T0–T2: undefined, deny all. T3: human only, immutable. AI master switch: off.

## Auth (client-side stand-in)
PBKDF2-SHA256 (310k iterations, per-user salt), 12+ char passwords, verification link required before sign-in,
single-use hashed tokens (24 h verify / 30 min reset), exponential back-off after 5 failures, generic errors,
session list with revoke, reset signs out all sessions. **Not** Argon2id, httpOnly cookies, CSRF or real email;
those require the server build. The dev mailer is labelled and renders messages in-page.
