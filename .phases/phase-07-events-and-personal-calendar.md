# Phase 7: Events and Personal Calendar

## Plan reference

Source plan §§2–2.2 and F3; Events and personalized 1999 acceptance. Read `testing-and-delivery-contract.md`. Depends on phases 3–6.

## Tasks

- [ ] Write failing lifecycle/evidence tests for active, upcoming, expired and unknown windows, verified exclusive relevance, recurrence and event identity using audited public schemas and synthetic schedules.
- [ ] Project existing WFHelper World/vendor/Prime Resurgence models through their owning stores/services, without rereading raw source files or fetching their websites/endpoints; trace and reuse existing code access for required fields. If a gap remains, ask the user before extending any existing service/API; do not recreate the service as a workaround. Require separate explicit approval for a genuinely new supplemental source. Distinguish verified expiry from predictions and unknown availability.
- [ ] Build Events Active/Upcoming/History tabs with source/freshness, deadlines, interest/pins, personal participation objectives and notes using extension-owned records.
- [ ] Implement explicitly user-entered personal 1999 choices and calendar step completion without inferring account-specific rewards from global World data or duplicating a Weekly occurrence.
- [ ] Implement optional local reminders through existing notification conventions with opt-in/opt-out, no duplicate reminders after refresh/restart and no unsupported delivery guarantees while the app is closed.
- [ ] Connect event relevance/expiry to the shared engine and other views through stable references; completing a personal objective must never grant inventory ownership.
- [ ] Complete `events.spec.ts` for pin/objective/note edits, personal choices/calendar step, independent persisted-state checks, navigation/reload/full restart and unchanged choices after unrelated source refresh.
- [ ] Add before/at/after expiry and upcoming activation E2Es, unknown deadlines, source outage/staleness, failed saves and reminder deduplication/opt-out; retain history/notes while expired events cease active competition.
- [ ] Verify source-backed event-over-weekly ordering, native usability and privacy, then run focused Vitest/Electron tests and applicable gates with recorded evidence.

## Acceptance and verification

An interested event shows a supported deadline; its note and objective survive restart. At expiry it moves to History and stops competing as an active opportunity. An unknown expiration has no fabricated countdown. User-entered 1999 state survives global refresh, and source-derived calendar views reference the same underlying occurrence where applicable.

## Notes

Test notification behavior at the native notification boundary without replacing event storage/UI. Use invented event schedules with declared evidence provenance; do not ship fabricated game-event metadata as live facts. Preserve notes and known historical evidence when current sources disappear.
