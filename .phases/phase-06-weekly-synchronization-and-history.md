# Phase 6: Weekly Synchronization and History

## Plan reference

Source plan §§2.1, 3 Weekly interoperability, 4.1 and F2; weekly count/reset/auto-evidence scenarios. Read `testing-and-delivery-contract.md`. Depends on phases 3–5.

## Tasks

- [ ] Write failing tests around upstream task IDs, target counts, period functions and auto-completion floors, including manual partial progress and before/at/after daily/weekly/source-driven resets.
- [ ] Trace current upstream save/load/subscription behavior and existing consumers to reuse available notifications. If a shared-save hook or other existing-service extension still appears necessary, ask the user and obtain explicit approval before implementing it; document the inspected paths and smallest proposal without recreating the service or editing `DailiesTracker.svelte`/original World UI.
- [ ] Implement Weekly Due/Completed/History with daily-repeatable subgroups, runs remaining, native reset countdowns and truthful unknown expiry/source states using existing task definitions.
- [ ] Implement two-way current-period reads/writes through the authoritative World progress path; test stale renderer snapshots and interleaved World/Weekly edits so newer counts are never clobbered.
- [ ] Persist one bounded `(taskId, periodKey)` history record with partial/completed counts, manual/auto provenance, timestamps and user notes; current World progress is not duplicated as an independent checklist.
- [ ] Reuse verified auto-evidence semantics and protect auto floors; reject stale/previous-profile/previous-period evidence and keep unknown expiry from clearing valid progress.
- [ ] Complete `weekly.spec.ts`: click Netracells from 0 to 2/5, compare real World UI and actual authoritative storage, navigate/reload/full-restart and retain 2/5.
- [ ] Add exact-reset Electron tests for fresh current occurrence and retained previous partial/completed history; verified auto completion must not falsely complete a new period or claim item ownership.
- [ ] Exercise World-origin edits while Weekly is closed, notification/history capture policy, concurrent changes, invalid counts and failed history writes; document and explicitly test any unavoidable capture limitation instead of promising invisible intermediate history.
- [ ] Verify priority invalidation, native keyboard/theme/locale/small-window behavior, untouched World regression specs and applicable full gates; record red/green and synchronization evidence.

## Acceptance and verification

Weekly and World agree on 2/5 across navigation, reload and full process restart. Crossing the real upstream period boundary produces a new occurrence while retaining the old 2/5 history. Editing World must be reflected on next Weekly open without overwriting a newer edit. Completed weekly tasks cannot return as outstanding until a new valid period begins.

## Notes

If perfect history capture appears to require a new shared-save notification, first ask the user before changing the existing service; only implement/test an explicitly approved hook. Do not alter the original screen or recreate its service to bypass approval. Do not synthesize missed historical actions while the app was not observing them; state capture guarantees in the UI/help or documentation. Test real `world-dailies` state independently of extension history.
