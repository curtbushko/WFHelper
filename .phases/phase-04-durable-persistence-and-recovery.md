# Phase 4: Durable Persistence and Recovery

## Plan reference

Source plan §4.1, F1/F8 and persistence/privacy scenarios in §6.1. Read `testing-and-delivery-contract.md`. Depends on phase 3.

## Tasks

- [ ] Write failing schema, serialization, migration and round-trip tests for versioned goals/builds/events, bounded occurrence history, notes and preferences; reject invalid ranks, quantities, identities and unsupported versions without destructive reset.
- [ ] Implement extension-owned user-data storage with upstream atomic file helpers, serialized concurrent writes, visible save errors, last-good backup recovery and stable record IDs.
- [ ] Add the narrow typed renderer client and sender-authorized IPC/preload/main registration; test unauthorized senders, malformed/oversized payloads and out-of-order/concurrent writes without broadening API permissions.
- [ ] Implement idempotent migrations and corruption/unknown-version handling that preserves original files, extension records and untouched `world-dailies` state; define bounded history retention without silently deleting archives.
- [ ] Add native extension controls for validated extension-only export/import and recovery, with explicit conflict/idempotency rules; exclude raw inventory, credentials and arbitrary filesystem access.
- [ ] Provide a minimal real user goal/pin and note interaction through extension-owned UI so production persistence can be exercised before full feature views exist; retain user intent through source/profile refresh.
- [ ] Complete `recovery.spec.ts` happy paths using actual UI/save/read, independent disk inspection, navigation/reload and same-sandbox full restart; cover import/export and migration without reseeding edited state.
- [ ] Add deterministic failed/interrupted write, concurrent edit, corrupt document, invalid import and unauthorized IPC cases; verify visible errors, recoverable last-good records and absent producer-only secret sentinels in stores/exports/logs.
- [ ] Run focused Vitest/Electron recovery/privacy tests and applicable gates; document IPC/storage seams, on-disk schema, recovery workflow and migration evidence.

## Acceptance and verification

A real planner goal survives source refresh, renderer reload and full Electron restart, with no raw private source payload in extension files. Repeated import/migration is safe. A write failure cannot report success and lose an edit; UI distinguishes pending/unsaved from saved state. A corrupt or future-version document is never silently replaced with an empty state.

## Notes

Use `services/extensions/farming/`, `ipc/extensions/farming/` and shared renderer persistence modules. Inject faults at the filesystem/external boundary or prepare actual corrupt sandbox documents; never replace planner save/read with a mock to claim acceptance. Test backup contents as user-authored state only. Export/recovery controls must remain reachable as the section UIs evolve.
