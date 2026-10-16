# Testing and delivery contract

Applies to every phase of **WFHelper Farming Planner**. Read this together with the selected phase before implementation. Source of scope: `../wfhelper-native-fork-plan.md`, especially §§1–4 and 6–8. See `coverage-matrix.md` for scenario ownership. No implementation or tests have been completed by creating these planning documents.

## Architecture and scope

- Use WFHelper's TypeScript/Svelte/Electron architecture, native components, i18n, item data, IPC and atomic-file conventions. No Go, daemon, HTTP API, SQLite, second app, replacement catalog pipeline or game-memory integration.
- Five peer sidebar views only: Weekly, Events, Builds, Short Term, Long Term. No sixth Farming Planner view.
- Keep new implementation under `src/extensions/farming/`, `services/extensions/farming/`, `ipc/extensions/farming/`, `tests/extensions/farming/`, and `e2e/extensions/farming/` where appropriate.
- Preserve original Inventory, World, Mastery, Foundry, Relics, Dashboard and Settings screens and their components. Prefer shared-service reads and a minimal shared save notification over editing existing UI.
- Record every changed preexisting file, rationale, narrow integration diff and migration impact in `UPSTREAM_TOUCHPOINTS.md`, including test/build configuration and any updater seam.
- Upstream inventory/game facts remain authoritative. Persist only extension-owned user input and bounded occurrence history; never infer ownership from a checked planning task.

## Required reuse of WFHelper models and data access

- Import and reuse existing WFHelper TypeScript types, domain models, canonical item identities, stores, selectors, services and utilities wherever they represent the required facts. New planner records may compose/reference those types; do not recreate parallel inventory, catalog, World, relic, Foundry or Mastery models. Add planner-specific types only for genuinely new user intent, relationships, evidence or projections, and document why an existing type is insufficient.
- Production farming code must consume game/account data through the existing WFHelper in-process models/stores/services or established typed IPC. Do not independently read inventory exports, logs, caches, raw World/catalog files or other source files already ingested by WFHelper. Do not scrape websites or fetch endpoints/feeds that WFHelper already manages, even if the extension would parse them differently. Refresh through the owning WFHelper service and subscribe to its supported changes rather than creating a second collector, parser or refresh loop.
- Adapters mean thin projections of WFHelper-owned data, not replacement importers or recreated services. Assume the required data is already available through existing code: trace current consumers, exported models, stores, utilities, subscriptions and typed APIs before declaring an access gap. Prefer direct reuse; do not add wrappers that merely duplicate an existing service's responsibilities.
- Any extension or behavioral change to an existing WFHelper service requires explicit user approval before implementation, including added accessors, API fields, refresh behavior or shared-state notification hooks. If existing access genuinely appears insufficient, pause the affected work and ask the user with the required datum/behavior, code paths inspected, smallest proposed change and why existing access does not suffice. A phase task or the original source plan is not approval. Do not recreate the service, read raw sources or fetch a duplicate feed to avoid asking.
- A genuinely new external source also requires a documented capability gap and explicit scope approval before implementation. Never duplicate a managed source under a supplemental-adapter label; approval of a new source does not implicitly approve changing an existing service.
- The extension may save and load its own validated planner documents, preferences, migrations, backups and imports using the approved native persistence path. This permission does not extend to reading raw upstream source files or maintaining a private copy of upstream inventory. Tests may seed producer files and independently inspect sandbox files to verify behavior; those test operations do not authorize production bypasses.
- Review each feature for reused types and data owners, and enforce the boundary with architecture/import checks where practical plus targeted tests: once a source is loaded by WFHelper, navigating/refreshing farming views must not introduce duplicate source-file reads, external requests or collectors.

## TDD delivery loop

1. Break a phase into small behavioral slices with explicit acceptance scenarios. Start each testable slice with a failing Vitest regression or real Electron acceptance spec. Confirm the failure is caused by missing behavior, not broken setup.
2. Implement the smallest working change, rerun the focused tests, then refactor with tests green. Use table-driven tests for identity, quantities, ranks, periods, priorities, validation and migration edge cases.
3. For UI slices, write the Electron acceptance scenario before implementing the interaction where practical. Early phases use the real slice available at that point; later phases extend the same contract rather than replacing it with mocks.
4. Record red/green commands and outcomes in task/PR evidence. For packaging, visual review or untestable setup, state why a test-first step is impractical and provide an executable verification instead. Do not manufacture failures or evidence.
5. Finish every feature's own acceptance and failure tests in its feature phase. Phase 11 audits integrated completeness; it is not permission to defer E2E.

## Real Electron acceptance protocol

- Build with upstream commands and launch `launchElectronTestHarness` with a fresh isolated sandbox. Specs live under `e2e/extensions/farming/*.spec.ts` and must be discovered by the default runner.
- Reuse `openView`, `evaluateInMain`, `writeHarnessInventory`, `restartElectronTestHarness` and artifact helpers after checking their current signatures. Use real sidebar navigation, actual Svelte UI, production IPC authorization, state engine and production storage implementation.
- Control external source responses and invented input at producer/native boundaries. Do not stub the feature save/read, recommendation engine, view or local persistence to claim E2E coverage.
- Assert meaningful initial UI, perform a user interaction (or change upstream input for read-only features), and assert both rendered output and independently read persisted extension documents or authoritative `world-dailies` progress. Reading back through the same selector alone is insufficient.
- Navigate away/back, reload the renderer, then quit/relaunch Electron with the same sandbox. Verify edited state without reseeding it. Source fixtures may be reattached after restart, but may not overwrite user edits.
- Include feature-relevant invalid input, missing/stale sources, source/profile changes, conflicts and persistence failures; retain last-good records and truthful unknown/error states. Cross-cutting corruption/authorization tests need not be copied into every spec, but each feature must demonstrate its own failure behavior.
- Check keyboard/focus, native theme, locale and constrained window sizes for each new view; fail on unexpected renderer/main errors. Use upstream traces/screenshots/log artifact helpers, and keep artifacts synthetic.
- Inject controllable time into both Electron main and renderer/source paths. Test reset/expiry immediately before, at and after the boundary without wall-clock sleeps; a renderer-only frozen Date does not prove main-process behavior.
- Isolate tests from real account files and real game/network sessions. Fail unexpected network requests; explicit fixture failures should be asserted, not ignored.

## Fake but accurate data

- Audit existing candidates before reuse: `tests/fixtures/world/circuitFixture.ts`, `e2e/world-dailies.spec.ts`, `e2e/offlineScenario.ts`, `e2e/foundry-buildable-sets.spec.ts`, `e2e/mastery-breakdown.spec.ts`, `e2e/drop-acquisition.spec.ts`, `e2e/scenarios/`, and upstream logic tests.
- `e2e/electronTestHarness.ts` already supports fixture inventory, sandbox user-data files, producer refresh and full-process restart. `e2e/offlineScenario.ts` demonstrates fixed-time, declared-response external-source isolation; its existing scenario names are limited, so extend narrowly instead of assuming arbitrary scenarios already work.
- Reuse safe schema/builders and public game constants, not unreviewed recordings, screenshots, logs or player exports. Invent account/profile identifiers, quantities, timestamps, personal choices, build names and notes. Canonical public item IDs, categories, valid rank/copy rules, reset functions and source payload shapes must match the pinned upstream data.
- Add extension fixture builders under `tests/extensions/farming/fixtures/` and share/import safely into E2E where supported. Use minimal fixtures for most tests plus a generated large public-catalog-shaped fixture for scale tests. Avoid maintaining a second catalog.
- Document provenance/version, schema, expected domain interpretation and safety review for each fixture family. Do not invent game-specific drop odds, farm duration, account rewards or exclusivity and present them as verified facts. If no supported evidence exists, use unknown; explicitly artificial event schedules are test scenario inputs, not claims about live events.
- Cover known owned, confirmed zero, unknown, stale, unavailable, unsupported and changed-profile states distinctly. Include shared Arcane demand, base/Prime pairs, mastered vs merely crafted, resources, shards, Helminth, companions and missing acquisition data.
- Test a deterministic synthetic secret sentinel in producer-only fields; it must not leak into extension files, exports, application logs/errors or published artifacts. Do not put a sentinel in user notes and then wrongly expect a legitimate note export to omit it.

## Quality gates and completion evidence

Use the checked-out `package.json`, not assumptions from the source plan's audit date. At planning time the following scripts exist:

```text
pnpm install --frozen-lockfile
pnpm run build
pnpm run check
pnpm run typecheck
pnpm run lint
pnpm run format:check
pnpm run test
pnpm run test:e2e
pnpm run test:packaged
pnpm run test:linux-boot
```

Use Corepack if required by the environment. Run focused checks during red/green and all applicable upstream gates before declaring a phase complete; preserve platform-specific regressions such as `test:dbwin`, `test:reward-scan` and `test:riven-scan` where relevant. Packaging and Linux/platform-specific gates require supported runners; explicitly record blocked checks and do not mark unsupported runs as passing. No arbitrary coverage quota, weakened assertions, global retries to conceal flakiness, or permanent skipped acceptance scenarios.

Each phase handoff records changed files/touchpoints, reused WFHelper types/models and owning data-access APIs, tested scenarios/spec names, actual commands/results, fixture provenance, residual risks and platform limitations. Keep `index.yaml` task counts/status synchronized with checkboxes. Only mark completed after acceptance evidence exists.
