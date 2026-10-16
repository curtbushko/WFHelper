# Source-plan and acceptance coverage

Source: `../wfhelper-native-fork-plan.md`. This is a planning traceability matrix, not evidence of passing tests. Every phase also obeys `testing-and-delivery-contract.md`.

## Feature scope

| Source | Owning phases | Required outcome |
|---|---|---|
| F0, §§1–4 | 1, 2, 12 | Pinned fork baseline, native five-view shell, narrow touchpoints, safe identity/update behavior |
| F1, §4.1 | 3, 4 | Read-only upstream adapters, stable shared goals, freshness/profile semantics, durable validated storage |
| F2 | 6 | One authoritative current Weekly/World state, per-occurrence history, reset/auto safeguards |
| F3 | 7 | Events, source-backed availability, interest, notes, personal 1999 details, reminders |
| F4 | 8 | Active/planned/archived builds and truthful shared requirements |
| F5 | 9 | Independent targets, derived suggestions, source-backed acquisition alternatives |
| F6 | 10 | Catalog, Mastery, Prime, Arcanes, collection progress and classification |
| F7 | 5, 11 | Early shared deterministic policy, reactive explanations and final cross-view coherence |
| F8, §7 | 4, 12 | Backup/recovery, packaged native app, updater safety, upstream sync playbook |
| §6 mandatory policy | All | Test-first slices and actual UI/storage/restart acceptance for each feature |
| §8 definition of done | 11, 12 | Full integrated acceptance, original screen compatibility and maintainable release |

## Mandatory E2E scenarios from §6.1

Spec paths below are planned under `e2e/extensions/farming/`, not existing files or selectors. Shared setup/restart/privacy protocols are defined in the delivery contract.

| Scenario | Phases | Planned acceptance specs and checks |
|---|---|---|
| Five-route smoke | 2, 11 | `navigation.spec.ts`: all five new routes, original routes, sidebar preferences, locale/theme and restart |
| Weekly counts | 6 | `weekly.spec.ts`: Weekly 0→2/5, World agreement, World edit observed by Weekly, storage and restart |
| Weekly reset/history | 6 | `weekly.spec.ts`: before/at/after reset, partial and completed history, one occurrence per period |
| Weekly auto-evidence | 6 | `weekly.spec.ts`: verified current auto floor, stale/missing evidence, no new-period false completion |
| Events | 7 | `events.spec.ts`: interest/note, upcoming→active→expired, known/unknown deadline, history after restart |
| Personalized 1999 | 7 | `events.spec.ts`: manual choices and calendar step survive restart/global refresh without becoming global facts |
| Builds | 8 | `builds.spec.ts`: all lifecycle states, Arcane/mod/shard/Helminth/weapon/companion requirements, invalid edits |
| Multi-build inventory | 8 | `builds.spec.ts`: shared copies vs consumable allocation, changed owned quantities, no duplication |
| Short Term | 9 | `short-term.spec.ts`: standalone pin, copies, override, archive and intact originating build |
| Activity recommendation | 5, 9 | `short-term.spec.ts`: supported alternatives/overlap, changed target changes reasons/order, unknown rates nonnumeric |
| Long Term catalog | 10 | `long-term.spec.ts`: search/filter, native detail, explicit goal, reclassification with stable identity |
| Mastery/Prime/Arcane | 3, 8, 10 | `long-term.spec.ts`: distinct variants, mastered/crafted/unknown, Arcane copy/rank mapping and refresh |
| Global priority policy | 5, 11 | `priorities.spec.ts`: exclusive event vs weekly/build/pin/long-term, settings/restart, occurrence completion |
| Persistence recovery | 4, 11 | `recovery.spec.ts`: interrupted/failed writes, corrupt/unsupported documents, backup/import/migration, no silent reset |
| Privacy | 1, 3, 4, 11 | `privacy.spec.ts`: synthetic producer-only sentinels absent from saved/exported state and captured logs/artifacts |
| Upstream regression | 2, 6, 11, 12 | `upstream-regression.spec.ts` plus unchanged upstream suite: original World, Inventory, Relics, Foundry, Mastery, Dashboard |
| Packaging | 12 | Existing packaged/Linux smoke extended narrowly: packaged five routes plus original app, offline synthetic sources and updater guard |

## Additional failure and usability coverage

- Reuse of existing WFHelper types/models and existing code access, no duplicate services/raw-source readers/managed website requests, extension-owned persistence permitted: phases 1, 3, 7 and 11. Any existing-service extension (including Weekly notification hooks in phase 6) requires asking the user and receiving explicit approval first; mandatory contract applies to all phases.
- Adapter source refresh/profile changes, missing data vs known zero: phases 3 and 11.
- IPC sender authorization, bounded validation, concurrent writes and migration idempotency: phase 4.
- Concurrent World/Weekly edits, hidden-Weekly history policy and auto/manual provenance: phase 6.
- Unknown expiry/exclusivity and notification opt-out/no repeat reminders: phase 7.
- Invalid rank/quantity rejection without mutation: phases 3, 8 and 9.
- Catalog-scale responsiveness with thousands of entries and no automatic goal creation: phase 10.
- Keyboard, focus, theme, locale, small-window layouts and unexpected error capture: each feature phase, audited in phase 11.
- Upstream SHAs, merge conflicts, retired overlapping extension behavior and migration compatibility: phase 12.

## Explicitly deferred

No browser/Tailscale listener, curl-based desktop E2E, Go/wf-watcher code, OAuth, public hosting, replacement application, parallel inventory authority or rewrites of existing upstream screens. New scopes require an explicit follow-up decision.
