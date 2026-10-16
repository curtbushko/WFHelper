# WFHelper Native Fork — Five-Section Farming Planner

**Status:** Proposed implementation plan, replacing the earlier single “Farming Planner” navigation proposal

**Upstream:** [WFHelper/WFHelper](https://github.com/WFHelper/WFHelper)

**Upstream snapshot audited:** 2026-10-16; recheck interfaces at implementation time

**Intent:** Maintain a small, upstream-friendly fork that adds five native WFHelper sidebar views while leaving existing upstream views and workflows alone.

## 1. Decisions and constraints

1. **All code lives inside WFHelper.** Implement in TypeScript, Svelte and Electron using WFHelper's existing services, component conventions, styling, localization, item data, IPC and persistence. **No Go, wf-watcher backend, separate daemon, SQLite migration, HTMX site or replacement application.** No code from wf-watcher is incorporated.
2. **Five new top-level sidebar sections:** **Weekly**, **Events**, **Builds**, **Short Term**, and **Long Term**. Do not add a sixth “Farming Planner” navigation item. The five views are peers of WFHelper's existing views, not replacements for them.
3. **Do not modify the existing Inventory, World, Mastery, Foundry, Relics or Dashboard screens.** Read/reuse their established data services and utilities. Permit only the smallest necessary upstream-file *integration seams* for the five new sidebar entries, typed IPC, translation registration and reliable shared-state notification. Keep a recorded list of every upstream file touched.
4. **One shared model and priority engine** under an extension-owned directory. Each goal has one stable identity and one primary planning classification. It may affect multiple views without duplicating ownership, progress or checkmarks.
5. **Keep upstream data authoritative for game facts and inventory.** Read existing WFHelper inventory, item DB, relics, world state, Foundry and Mastery data. Only extension-owned user input—goals, plans, preferences, personal 1999 choices, manual completion history, notes—belongs in the new persistence layer. Never overwrite ownership because the user checked a planning box.
6. **Use upstream's native test stack.** Vitest for logic, Playwright's *real Electron* harness for end-to-end acceptance of **each feature**, upstream's build, typecheck, lint and formatting gates. No arbitrary coverage threshold.
7. **Maintain the fork for upstream merging.** Isolate most code in dedicated directories; avoid copying upstream views or implementing a second general-purpose inventory/database subsystem. Run full compatibility tests every time upstream changes are merged.
8. **Private-data boundary:** never check player exports, identifiers, tokens or personal screenshots into Git/CI. Test exclusively with invented profiles and fixtures. Offline, stale, unsupported and unknown must be explicit rather than silently treated as zero.

## 2. UI navigation and responsibilities

The existing WFHelper sidebar stays intact. Add these five optional, native-styled entries using the existing view registry, sidebar ordering, icons, focus, keyboard navigation and localization conventions:

| New section | Central question | Primary content | Things it must *not* duplicate |
|---|---|---|---|
| **Weekly** | What must I finish before the next reset? | Weekly/daily checklist, runs remaining, cycle countdown, completion history, weekly-first recommendations | A parallel conflicting `World → Dailies/Weeklies` progress store |
| **Events** | What opportunity will disappear? | Current/upcoming/expired time-limited rewards, seasonal opportunities, Prime Resurgence, event objectives and reminders | WFHelper's world-state collector or vendor/relic data service |
| **Builds** | What do my builds still require? | Active/planned/archived Warframe, weapon and companion build requirements; missing mods, Arcanes, shards, Helminth, gear | The existing Foundry crafting tree or Mastery Roadmap |
| **Short Term** | What should I farm next, independent of a build? | Pinned personal targets, required copies/resources, known acquisition routes, activity/mission overlap, actionable near-term queue | The full catalog, or automatic promotion of every discovered item into a goal |
| **Long Term** | What should I collect or master over time? | Searchable whole-item catalog, Mastery, Prime/base variants, Arcane rank progression, collection completion and long-horizon plans | Upstream inventory details and native relic/Foundry explorers |

**Suggested sub-tabs (not additional sidebar views):**

- **Weekly:** Due / Completed / History. Show daily-repeatable entries under a clearly labeled subgroup, rather than inventing a separate Daily view.
- **Events:** Active / Upcoming / History, with source-backed expiration/recurrence and personal participation notes.
- **Builds:** Active / Planned / Archived; select a build for grouped requirements and shortage calculations.
- **Short Term:** Next Up / My Targets / Activities; optionally a quick-add search against upstream items.
- **Long Term:** Catalog / Mastery / Prime / Arcanes / Collections. Use responsive native WFHelper tabs and tables, not a second application shell.

### 2.1 Classification and cross-view behavior

The five sections are **views of shared facts**, not five separate trackers:

- **Weekly** owns *recurring windowed task records*. A weekly reward important to a build may be highlighted in Builds and Next Up but its completion remains one occurrence in Weekly.
- **Events** owns *nonroutine, bounded availability windows* (seasonal, time-limited, special rotation). An expiring event can outrank normal Weekly tasks when its exclusivity/deadline is verified.
- **Builds** owns *user-defined build structures* and their item/copy/rank requirements. A missing Arcane may surface in Short Term as an acquisition suggestion, without creating a duplicate goal record.
- **Short Term** owns *standalone, deliberately pinned near-term goals*. It also displays derived actionable recommendations from other sections, clearly marked as suggestions vs user-owned goals.
- **Long Term** owns *discovery and enduring progression views*. Not every catalog item becomes a persistent goal; pins/targets created here reuse the same underlying goal model, and can be explicitly reclassified as Short Term.

**Example:** A Halloween mission offers an Arcane needed by two active builds. Its time window is recorded once under Events, the build shortages remain once under Builds, and Short Term may recommend that mission with a reason breakdown. Completing the event objective must update the shared goal/task state once; it must **not** automatically claim ownership of the Arcane unless WFHelper's inventory later verifies it.

**Example:** You target a max-rank Arcane outside any current build. Add it directly to Short Term; the Long Term Arcanes view can show its copy/rank progress. Linking it to a build later must preserve the original target and avoid double-counting copies.

### 2.2 Prioritization across all five sections

One pure, deterministic engine determines eligibility, reasons and ordered priority:

1. **Source-backed expiring exclusive events** can override weekly ordering where delay really loses access to a relevant reward.
2. **Eligible incomplete weekly/limited-reset activities** are the default highest ordinary tier; completed occurrences cannot reappear as outstanding before the next reset.
3. **Active build necessities** (including Arcane copy/rank gaps, mods, shards, weapons, companions, Helminth) and explicit Short Term pins follow.
4. **Long Term mastery/Prime/completion goals** follow, with user-configurable category preferences.
5. **Mission overlap and plausible acquisition alternatives** improve scores only where source-backed. Never fabricate drop odds, farm times, seasonal expiry, Prime vaulting certainty or account-specific 1999 rewards.

The engine returns **priority tier, reason codes, decisive user preference, source freshness, missing quantities, occurrence/expiry, and known alternatives**, not a black-box single score. A user can pin/reprioritize targets without modifying upstream item metadata. Items lacking sufficient evidence remain `unknown` or `not currently relevant`, not “free/easy”.

## 3. What WFHelper already provides: reuse, don't rebuild

The audited upstream repository includes:

- [`World/Dailies` task definitions and resets](https://github.com/WFHelper/WFHelper/blob/main/src/lib/world/dailies.ts): Netracells, Archon Hunt, Circuit, Archimedea, Calendar, vendor tasks, custom items, countdown keys and manual counts.
- [`dailiesAuto.ts`](https://github.com/WFHelper/WFHelper/blob/main/src/lib/world/dailiesAuto.ts): some verified inventory-assisted completion and protected partial progress; **not** universal completion for every task.
- [`DailiesTracker.svelte`](https://github.com/WFHelper/WFHelper/blob/main/src/components/world/DailiesTracker.svelte): the existing UI to remain unchanged.
- [`src/stores/data.ts`](https://github.com/WFHelper/WFHelper/blob/main/src/stores/data.ts): item database, inventory, ownership; [`src/lib/startupLoader.ts`](https://github.com/WFHelper/WFHelper/blob/main/src/lib/startupLoader.ts) pulls these from native IPC.
- [`src/lib/masteryPlanner.ts`](https://github.com/WFHelper/WFHelper/blob/main/src/lib/masteryPlanner.ts), [`src/stores/masteryPins.ts`](https://github.com/WFHelper/WFHelper/blob/main/src/stores/masteryPins.ts): mastery pinning, crafting trees and missing materials.
- Existing relics, Foundry, World sources, [`@wfcd/items`](https://www.npmjs.com/package/@wfcd/items) and `warframe-public-export-plus`: use currently supported feeds/data rather than creating a parallel catalog download pipeline by default.
- [`src/lib/persistence.ts`](https://github.com/WFHelper/WFHelper/blob/main/src/lib/persistence.ts), [`services/userDataPath.ts`](https://github.com/WFHelper/WFHelper/blob/main/services/userDataPath.ts), [`services/atomicFile.ts`](https://github.com/WFHelper/WFHelper/blob/main/services/atomicFile.ts): native preference and atomic-disk persistence patterns.
- [`playwright.config.ts`](https://github.com/WFHelper/WFHelper/blob/main/playwright.config.ts), [`vitest.config.ts`](https://github.com/WFHelper/WFHelper/blob/main/vitest.config.ts), [`e2e/electronTestHarness.ts`](https://github.com/WFHelper/WFHelper/blob/main/e2e/electronTestHarness.ts): the upstream Playwright Electron sandbox, fixture inventory, full app restart and Vitest infrastructure.

**Weekly interoperability constraint:** `world-dailies` in localStorage currently records *current period* progress but is not a permanent historical ledger. The new Weekly view must use the existing period IDs/functions and the authoritative current progress, and add its **own** per-occurrence history. Do not independently simulate a second `netracells` checkbox state. A write from either view must not overwrite a newer write; ensure that a current-period count from the existing World view is reflected when Weekly opens.

If retaining history of changes made inside the *unchanged* World screen requires a notification hook, first inspect whether an existing subscribe or IPC event suffices. If not, a **minimal hook in the shared state save function** is preferable to editing `DailiesTracker.svelte`. Without such a hook, don't promise perfect capture of intermediate World actions or completed past occurrences while Weekly is never opened; document this until solved and test it explicitly.

## 4. Upstream-friendly directory structure

All paths below are **proposals**, not existing WFHelper directories:

```text
WFHelper/
├── src/
│   └── extensions/
│       └── farming/
│           ├── shared/
│           │   ├── model/                  # stable goals, item references, periods, evidence
│           │   ├── adapters/               # read existing WFHelper stores/utilities
│           │   ├── priorities/             # pure scoring + explanation logic
│           │   ├── persistence/            # typed client side of native persistence
│           │   ├── stores/                 # reactive Svelte state
│           │   └── components/             # reusable WFHelper-styled controls
│           ├── weekly/
│           │   ├── WeeklyView.svelte
│           │   └── components/
│           ├── events/
│           │   ├── EventsView.svelte
│           │   └── components/
│           ├── builds/
│           │   ├── BuildsView.svelte
│           │   └── components/
│           ├── shortTerm/
│           │   ├── ShortTermView.svelte
│           │   └── components/
│           └── longTerm/
│               ├── LongTermView.svelte
│               └── components/
├── services/extensions/farming/           # privileged state storage/backup, if needed
├── ipc/extensions/farming/                # minimal typed IPC handler implementation
├── tests/extensions/farming/              # Vitest (*.test.ts)
└── e2e/extensions/farming/                # actual Electron Playwright (*.spec.ts)
```

**Minimal expected patches outside those directories** (record each in `UPSTREAM_TOUCHPOINTS.md`):

| Existing file(s) | Allowed change | Avoid |
|---|---|---|
| `src/types/views.ts` | Register five route IDs | Reorganizing WFHelper's built-in routes |
| `src/lib/viewRegistry.ts` | Lazy-load five views; add sidebar label/hideability | Rewriting Sidebar.svelte or route handling |
| Existing i18n dictionaries / registration | Add required labels and translations using upstream conventions | Replacing the upstream translation framework |
| Typed IPC/preload/main registration, only if required | One narrow registration for extension-owned durable user-data APIs | Broad window.api permissions or custom HTTP server |
| `src/lib/world/dailies.ts`, only if unavoidable | Tiny typed progress-change notification, preserving all existing calls/behavior | Replacing/rewriting the original tracker or modifying its UI |
| Existing test/package config, if required | Include new extension test/assets using existing commands | A second test runner, module system or packaging stack |

Design every integration seam for *upstream-first reading*, not a forked copy of a WFHelper module. Keep UI navigation and renderer code almost entirely under the extension directory; no patches to existing `WorldView`, `MasteryView`, `InventoryView`, `DashboardView`, `RelicsView` or their components are planned.

### 4.1 Persistence and synchronization

- **Small preferences:** use upstream-like validated localStorage helpers with extension-prefixed keys (`farming.*`) for tab selection, visible columns, sort, filter and weight presets.
- **Durable user records:** use a versioned, validated extension-owned file within Electron's `app.getPath("userData")`, written via WFHelper's atomic file helper and a sender-authorized, typed IPC bridge. Use backup/migration rather than disposable cache semantics. An extension-only storage folder (such as `userData/farming/`) is appropriate.
- **No cloned inventory:** store references to existing upstream canonical item identities and external-source timestamps/unknown flags; do not write copies of the complete private WFHelper inventory into extension state.
- **One logical goal:** stable goal ID, primary view classification (Short Term/Long Term/Build-related), target quantities/ranks, explicit user status, related build IDs and occurrence references. Explicit references allow a single item to serve multiple builds while remaining one owned inventory source.
- **Occurrences:** stable `(taskId, periodKey)` completion/history records with count, manual vs auto provenance, and timestamps. Retain old periods even after the current checklist resets; avoid unbounded indiscriminate logging and preserve archived user records.
- **Write integrity:** serialize concurrent writes, validate bounds, persist atomically and expose failure rather than silently discarding edits. Version migrations cannot erase existing goals or the original `world-dailies` state.
- **Profile/source changes:** keep goals as user-authored state separate from snapshots; explicitly identify scope/profile when WFHelper exposes it and prevent cross-profile accidental claims. Test stale and missing input as unknown.
- **Backups:** export/import only the extension's validated user-authored state (not account credentials/raw inventory), with idempotency and corruption recovery.

## 5. Feature phases and acceptance criteria

### F0 — Fork baseline, architecture and five-section shell

**Build:** Fork a pinned WFHelper commit, record `upstream` remote and current packaging/update behavior, run existing quality gates. Introduce extension directories and register five lazily loaded, native-styled sidebar entries. Show truthful empty/unknown states before game data is imported.

**Acceptance:** Native Linux Electron app opens with all five new sidebar entries; each opens independently. Existing Inventory, World, Mastery, Foundry and Relics screens still work. Sidebar visibility/order/theme/locale survive full quit/relaunch. Keep upstream touchpoint diff small and documented; avoid official auto-update overwriting the custom fork.

### F1 — Read-only native adapters and shared goal model

**Build:** Implement typed wrappers that project upstream inventory, catalog, relic, World, Mastery and Foundry facts. Model source freshness/unknowns, stable identities, target vs owned quantities, item category and recurrence/expiry. Add versioned user goal/build/event state and source-change invalidation; no parallel catalog importer unless audited gap warrants a targeted extension adapter.

**Acceptance:** Changing an invented inventory fixture updates the same owned/unknown status across all views; user pins survive source refresh; no raw personal payload leaks into extension files. A missing source must not appear as confirmed unowned.

### F2 — Weekly view and history

**Build:** Present upstream task definitions and current progress in an extension-owned Weekly view, with a weekly-first list, counters, reset timers, subgroups for daily repeatables, a History tab and stable occurrence records. Existing `World → Dailies/Weeklies` stays visually unchanged. Reuse upstream period calculation and auto-completion; add only the minimum notification/integration for two-way consistency. Add manual notes and per-cycle user actions without overwriting verified auto-count floors.

**Acceptance:** Mark 2 of 5 Netracell runs through Weekly; World reads the matching current count. Reopen Weekly, full-restart Electron, and confirm 2/5 remains. At the exact simulated reset, current progress becomes a fresh occurrence while last week's 2/5 remains in History; stale inventory cannot mark this week complete. If a user changes World progress, the next Weekly view reconciles without clobbering it and captures history per the documented hook policy.

### F3 — Events view

**Build:** Display active, upcoming and expired seasonal/time-limited opportunities from upstream World/vendor and supplemental source-backed metadata where necessary. Support user interest/pins, personal completion notes, optional local reminders using WFHelper's notification conventions, and 1999 Calendar *user-entered* details. Separate confirmed expiries from predictions/unknown availability and maintain event history.

**Acceptance:** A current event is shown with verified expiry. User pins an event and enters a personal objective; reload/restart preserves it. Clock advances past expiry: event moves to History and stops competing as an active farm; notes remain. An event with no verified expiration displays unknown instead of a fabricated countdown. A genuine expiring exclusive can outrank normal weekly tasks when relevant.

### F4 — Builds view

**Build:** Allow active, planned and archived Warframe/weapon/companion build records, with slots for required mods, Arcanes, rank targets, shards, Helminth, equipment and notes. Reuse upstream item DB and Foundry/Mastery parsing. Derive ownership and deficits without claiming automatic possession, and score multi-build overlap only once per goal where resource allocation is shared.

**Acceptance:** Create a build containing a missing rank-target Arcane and mod; verify deficit, editing, archive/re-enable and inventory-refresh updates. Reopen and full-restart: build persists. Two builds requiring the same item show a truthful shared need instead of duplicating inventory/copies. Invalid ranks and negative quantities are rejected without mutation.

### F5 — Short Term view

**Build:** Allow deliberate, standalone personal farming targets not tied to a build. Show a prioritized **Next Up** queue, required copies, known acquisition alternatives, likely activity/mission overlap and explicit manual overrides. Derived build needs can appear as suggestions with links, not duplicated Short Term tasks. Accept a promotion or demotion between Short Term and Long Term without losing history.

**Acceptance:** Pin an Arcane/weapon outside any build, edit desired copies, compare two source-supported activities, and explain why the recommended one changes after the user adjusts a target or completes a weekly opportunity. Reload/restart preserves target and override. Unknown drop rates must not render as numerical efficiencies; unlinking a suggestion cannot delete its originating build.

### F6 — Long Term view

**Build:** Catalog browsing over WFHelper's supported item DB with search, category filters, paginated/virtualized large-result rendering, owned/mastered/needed/unknown status, base vs Prime, Mastery, collection progress, Arcane completion and crafting dependencies. Reuse upstream mastery pins/crafting trees, without an independent contradictory pin ledger. Long-horizon catalog items are candidates, not automatically active goals.

**Acceptance:** Find an unpinned item, open its existing item detail, create a Long Term goal, track base vs Prime separately, and reclassify the target to Short Term. Verify the same stable item/goal appears without duplication and persists after full restart. Inventory import changes the item status without deleting the goal; unknown category/availability remains explicit. Handle thousands of catalog entries without blocking navigation.

### F7 — Shared priority engine, settings, cross-section coherence

**Build:** Implement deterministic eligibility and tier-first weighting in a pure TypeScript engine. Recompute from user edits, inventory updates, resets, event expirations and upstream source refresh. Emit traceable explanations and opportunity-level rankings (activities satisfying multiple relevant targets). Put shared weight/category settings inside extension-owned controls accessible from the new sections, *not* into WFHelper's preexisting Settings screen.

**Acceptance:** With an incomplete weekly, an expiring exclusive, an active-build item and a long-horizon mastery goal, the order and explanations match the documented policy. Completion removes the weekly from outstanding work for only that period. User changes a weight and ranking changes as expected; full restart retains it. Unknown source data must not become known zero. A shared item referenced in Events/Builds/Short Term is one identity, not three competing goals.

### F8 — Upstream merge, packaged release and optional future access

**Build:** Verify Linux and other supported Electron packaging with existing scripts. Preserve WFHelper’s app identity/update assumptions safely for a fork: prevent the official auto-updater from silently replacing fork builds; maintain own release identity and migration strategy. Integrate upstream periodically through dedicated sync PRs and re-run all native checks and E2Es. Provide in-app extension data backup/restore and clear failure recovery.

**Acceptance:** Built/packaged fork boots, all five views render with offline invented fixtures, and the existing WFHelper screens still work. Upstream sync changes are audited, integration diff remains narrow, and no private inventory or credentials appear in CI artifacts.

**Deferred, not silently included:** Cross-device/Tailscale access. Electron's native UI does **not** give browsers an HTTP API. If later explicitly required, scope an opt-in *WFHelper-native* listener inside its Electron main process and evaluate safe UI sharing; do not sneak in a Go service or second app. Only then would `curl`-based HTTP E2E be meaningful. Desktop E2E is Playwright-driven.

## 6. Mandatory E2E acceptance policy — for every feature

The same standard applies to **every meaningful user-visible feature in all five new sections**:

1. Build using WFHelper's own `pnpm`/Vite/Electron build, then launch the **real Electron process** via existing `launchElectronTestHarness` with a fresh isolated user-data sandbox. CI must not depend on a real Warframe session or the user's inventory.
2. Seed invented realistic inventory, world, catalog and relic fixtures via the existing harness/controlled native IPC boundaries. Mock **external sources** as necessary, but do not mock the planner’s state engine, storage path or Svelte UI under acceptance test.
3. Navigate through the real WFHelper sidebar to the feature and assert the initial visible content or accurate empty/unknown state.
4. Perform an actual user action—click checkbox, edit a build, change quantity/rank, pin an event or item, reclassify a goal, adjust preferences. For read-only computed features, change the invented upstream input and assert new derived output.
5. Assert **both the UI outcome and actual persisted extension state**, or real upstream progress state where appropriate. A successful click event alone is insufficient.
6. Navigate away and back, reload the renderer, **quit Electron and relaunch it with `restartElectronTestHarness` using the same sandbox**. Verify the data remains and the next read is correct without reseeding the edited state.
7. Exercise invalid input, source outage/staleness, conflicting source changes, corrupt persistence or failed writes according to the feature. Verify no false ownership/completion and preservation of the last good user-authored state.
8. Check native theme/keyboard/window sizing and absence of uncaught errors; capture trace/screenshot on failure with WFHelper's E2E artifact helpers. No real account, private file, secret, or raw player data in artifacts.

### 6.1 E2E coverage matrix

| Scenario/spec | Real app action / changed input | Independent assertion after restart and negative checks |
|---|---|---|
| Five-route smoke | Click Weekly, Events, Builds, Short Term, Long Term and original Inventory/World/Mastery routes | Sidebar config survives; all original routes still work |
| Weekly counts | Increment Netracells from 0 to 2/5; compare with World/Dailies | Same authoritative current progress; count survives quit/relaunch |
| Weekly reset/history | Advance injected clock from before to after valid weekly reset | New occurrence incomplete; previous partial/completed period available in History |
| Weekly auto-evidence | Change invented inventory from current to stale source signal | Auto floor applies only to current verified period; no false done or manual regression |
| Events | Pin an event, enter event-specific note; move fixture time through expiry | Event transfers to history; pin/note retained, unknown deadlines stay unknown |
| Personalized 1999 | Enter player's own choices and complete a calendar step | User choices persist; never overwritten by unrelated global world-state changes |
| Builds | Add/edit/archive build with Arcane rank, mod, shard and companion | Shared item shortfalls correct, no negative/invalid ranks, persisted after restart |
| Multi-build inventory | Two builds share missing copies; then change fixture-owned quantity | Shared ownership is not counted twice; needs and explanation recompute |
| Short Term | Pin an independent item, change amount/priority, mark target archived | Short Term record persists; builds and catalog references are not deleted |
| Activity recommendation | Two legitimate source-backed loot routes/mission overlaps | Goal changes produce explainable order; unknown rate not invented |
| Long Term catalog | Search/filter an unpinned item, add goal, classify as Short Term | Same stable ID, no duplicate goals, correct categorical state after restart |
| Mastery/Prime/Arcane | Inspect base and Prime separately and set Arcane rank target | Correct needed vs mastered/crafted/unknown; source changes do not erase goal |
| Global priority policy | Change weekly/event weights, complete weekly and switch view | All five views agree on eligibility, reason and priority; changes persist |
| Persistence recovery | Save, simulate interrupted/invalid persisted document, relaunch | Recovered/last-good state or clear failure; no silent data reset |
| Privacy | Seed invented secret sentinels in producer input | No raw player fields/secret sentinels in extension store, export, errors or logs |
| Upstream regression | Use original World, Inventory, Relics, Foundry and Mastery controls | Preexisting behavior and upstream Playwright specs still pass |
| Packaging | Launch packaged fork in supported Linux CI environment | Five sections and original app load correctly; no official auto-updater replacement |

Implement new specs under `e2e/extensions/farming/` using upstream naming/test conventions, for example `weekly.spec.ts`, `events.spec.ts`, `builds.spec.ts`, `short-term.spec.ts`, `long-term.spec.ts`, `priorities.spec.ts` and `recovery.spec.ts`. Add shared invented fixtures/helpers in the extension's test directories. Use upstream's `evaluateInMain`, `openView`, `writeHarnessInventory`, `restartElectronTestHarness` and artifact helpers as appropriate. Proposed view names and selectors must be added by the implementation, not assumed to exist already.

**Core acceptance principle:** *Every feature is considered done only after it is demonstrated through actual WFHelper UI and storage, including full-process restart.* Vitest unit tests are required as well, but cannot replace these end-to-end tests. Mocking Electron IPC responses for input feeds is acceptable; mocking the feature's actual save and read is not.

### 6.2 Use the existing native scripts

Commands verified in upstream `package.json` at audit time:

```bash
corepack enable
pnpm install --frozen-lockfile
pnpm run build
pnpm run check
pnpm run typecheck
pnpm run lint
pnpm run format:check
pnpm run test            # Vitest
pnpm run test:e2e        # build + Playwright against real Electron
pnpm run test:packaged   # packaged smoke, supported runner
pnpm run test:linux-boot # Linux boot smoke
```

Retain upstream `test:dbwin`, `test:reward-scan`, `test:riven-scan` and other platform-specific targeted regressions where relevant. Do not weaken upstream gate requirements or add code-coverage quotas. Optionally add a `test:e2e:farming` convenience script that selects our specs **using the existing runner**; the default `test:e2e` must continue to discover them.

## 7. Upstream update playbook

1. Keep an explicit `upstream` Git remote and pinned baseline SHA. Maintain `UPSTREAM_TOUCHPOINTS.md` documenting upstream file/path, reason, minimal diff, and any migration implication.
2. Maintain feature branches that primarily modify `src/extensions/farming/`, extension-specific privileged services and extension tests. Keep integration seams in separate, minimal commits.
3. Periodically fetch upstream and merge it through a dedicated integration PR. Avoid force-updating shared history and do not copy entire upstream modules into the fork to suppress merge conflicts.
4. Run unchanged upstream typecheck/lint/Svelte/format/Vitest/Electron E2E/packaged smoke **plus every farming acceptance spec** after each merge.
5. Verify shared IPC/types, task IDs, calendar reset behavior, inventory item keys, Vite assets, updater/package identity, locale files and view registry ordering after changes.
6. If upstream adds an overlapping feature (e.g., improved weekly history), retire the redundant fork code through a migration/compatibility test rather than maintain two diverging implementations.
7. For every upstream sync PR, report: old/new upstream SHAs, files conflicting, extensions affected, resolved compatibility changes, persisted-state migrations and test results.

## 8. Definition of done

The fork MVP is acceptable when a user can launch normal WFHelper and:

- Open all **five** additional first-class sidebar sections with the existing WFHelper look and behavior.
- Check remaining weekly runs and retrieve past-cycle history without conflicting with World/Dailies.
- Track expiring events and personal 1999 choices with known/unknown source status.
- Create active/planned builds and accurately inspect shortages of mods, Arcanes, weapons, shards, Helminth and companions.
- Pin independent Short Term goals and see legitimate acquisition/activity recommendations.
- Browse all supported WFHelper catalog items for Long Term mastery, Prime, Arcanes and collection planning, without forcing every item into a goal.
- See one explainable, weekly-first priority policy across sections, including rare justified Event overrides.
- Quit/restart the **real** Electron app without losing user-authored state and without corrupting WFHelper's own inventory or existing settings.
- Pass all original upstream CI checks and **real Electron/Playwright acceptance tests for every feature** with invented fixtures.
- Merge a newer WFHelper upstream revision with limited fork conflicts and no regression of existing views.

### Explicit exclusions

No wf-watcher or Go code. No new Go/HTTP backend or separate planning application. No rewrites of existing WFHelper Inventory/World/Mastery/Relics/Foundry views. No separate global app authentication, Google Sheets/OAuth, arbitrary public hosting, new game-memory access, or requirement to open the game just to browse existing public data. Tailnet/browser access and packaging beyond WFHelper's native flows are separate, explicitly decided follow-up scopes.

## 9. Verified upstream reference points

- [WFHelper repository](https://github.com/WFHelper/WFHelper)
- [package.json](https://github.com/WFHelper/WFHelper/blob/main/package.json), [Vitest config](https://github.com/WFHelper/WFHelper/blob/main/vitest.config.ts), [Playwright config](https://github.com/WFHelper/WFHelper/blob/main/playwright.config.ts)
- [Existing World task model](https://github.com/WFHelper/WFHelper/blob/main/src/lib/world/dailies.ts), [World daily/weekly UI](https://github.com/WFHelper/WFHelper/blob/main/src/components/world/DailiesTracker.svelte), [automatic completion](https://github.com/WFHelper/WFHelper/blob/main/src/lib/world/dailiesAuto.ts)
- [WFHelper E2E app harness](https://github.com/WFHelper/WFHelper/blob/main/e2e/electronTestHarness.ts), [World E2E](https://github.com/WFHelper/WFHelper/blob/main/e2e/world-dailies.spec.ts)
- [Mastery planner](https://github.com/WFHelper/WFHelper/blob/main/src/lib/masteryPlanner.ts), [Mastery pins](https://github.com/WFHelper/WFHelper/blob/main/src/stores/masteryPins.ts)
- [View ID registry](https://github.com/WFHelper/WFHelper/blob/main/src/types/views.ts), [lazy view registry](https://github.com/WFHelper/WFHelper/blob/main/src/lib/viewRegistry.ts)
- [Native preference storage](https://github.com/WFHelper/WFHelper/blob/main/src/lib/persistence.ts), [native user data path](https://github.com/WFHelper/WFHelper/blob/main/services/userDataPath.ts), [atomic file writes](https://github.com/WFHelper/WFHelper/blob/main/services/atomicFile.ts)
