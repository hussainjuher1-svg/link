# Reference analysis

**Provenance and limits.** This was written from general knowledge of the *public* positioning of Boostlingo /
Interpreter Intelligence, Interpreters' Help (BoothMate), KUDO Interpreter Assist and Remoto by ProZ. The pages were
**not re-fetched or screen-captured during this build**, so treat specifics as unverified and re-check them against the
live public pages before relying on them. Nothing was scraped from authenticated areas, no APIs were called, and no
code, copy, imagery, names or logos were copied. Structure and behaviour only; everything here is rebuilt from scratch.

## Public-site patterns adopted
| Pattern | VoxRelay implementation |
| --- | --- |
| Utility bar + nav, persistent Sign in and primary CTA | Nav with Sign in + Start free; mobile menu keeps Start free visible |
| Hero with two CTAs | "Start free" + "See how it works" |
| Honest trust section | "What we do — and what we won't"; no badges, ratings, logos or testimonials |
| FAQ accordion | Native `<details>` accordion |
| Audience-specific contact selector | Contact page radio group (interpreter / organization / LSP / other) |
| Link-grouped footer with legal bar | Product / Company / Account groups; no status page link, since none exists |

## App patterns adopted
Left sidebar (collapsible; bottom nav on mobile) · one calendar (list + month view) · offers with accept/decline ·
job record from offer to close with history · weekly availability + date overrides · audit trail filterable by area ·
glossaries with source, version history, CSV import/export · support threads with a status model.

## Status models
Assignment: Offered, Scheduled, Preparation required, Ready, In progress, Completed, Rescheduled, Cancelled.
Support: Open, Assigned, Awaiting response, Escalated, Resolved, Closed. Status is always icon + label.

## Empty / error / loading states
Every list screen's first-run state says what the screen is for and offers one real action. Errors say what happened
and how to fix it. Sign-in errors are identical for all failure causes. Skeleton component exists but screens are
synchronous (localStorage) so none currently need it.

## Deliberately skipped
Agency billing, call routing, call delivery, interpreter ratings, any recording or transcription.

## Problems the features answer (survey-ranked)
Sound/connectivity → device check · missing briefings → prep checklist and "Preparation required" · fatigue →
optional break reminder · admin load → calendar, availability, .ics import · scattered glossaries → Terminology.
The survey ranking comes from the brief; the underlying surveys were not independently checked.
