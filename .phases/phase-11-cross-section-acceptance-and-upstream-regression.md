# Phase 11: Cross-Section Acceptance and Upstream Regression

## Plan reference

Source plan F7, §§6.1 and 8. Read `testing-and-delivery-contract.md` and `coverage-matrix.md`. Depends on phases 1–10; this is an integration audit, not deferred feature E2E work.

## Tasks

- [ ] Audit every source-plan acceptance scenario against implemented spec names and actual evidence; fill missing tests with failing regressions before fixing behavior, with no skipped matrix rows.
- [ ] Complete a mixed-profile `priorities.spec.ts` using actual editors for incomplete Weekly, relevant verified expiring event, active-build Arcane, explicit Short Term pin and Long Term mastery; assert eligibility, tiers, reasons, stored preferences and restart consistency.
- [ ] Exercise the shared-item example: one event reward serves two builds and a standalone Arcane goal; complete the event objective, verify one occurrence/identity and no inventory claim, then refresh invented ownership and verify all deficits recompute.
- [ ] Exercise weekly completion/reset, event expiry, manual overrides and category weights across all five real views; require agreement on priority, missing quantities, source freshness and occurrence/expiry semantics.
- [ ] Extend source/profile conflict and persistence recovery E2Es across features, including corrupt/unsupported files, interrupted writes and invalid imports; retain last-good user-authored state and leave upstream inventory/World state intact.
- [ ] Complete privacy checks for producer-only secret sentinels in extension storage, export, errors, logs and publishable artifacts; inspect synthetic-only fixture provenance and unexpected-network rejection.
- [ ] Run unchanged upstream specs plus original World/Inventory/Relics/Foundry/Mastery/Dashboard controls under `upstream-regression.spec.ts` as needed; test controls, not only route screenshots.
- [ ] Audit keyboard/focus, locale/theme, small-window layouts, error capture and full-process restart for every meaningful feature; add focused missing behavior regressions and remove flaky timing assumptions without weakening assertions.
- [ ] Run all applicable build/check/typecheck/lint/format/Vitest/Electron gates on supported CI runners, reconcile the coverage matrix with executed evidence, inspect the full upstream-touchpoint diff and audit reused WFHelper models/data APIs; verify no production farming raw-source readers, duplicate managed-source requests or parallel collectors were introduced, while allowing extension-owned planner persistence.

## Acceptance and verification

Every matrix row has executable real Electron acceptance evidence including real storage and full restart where state is involved. No feature depends on the test mocking its UI, state engine or save/read path. Existing upstream tests remain unchanged in strength, and original screens behave correctly. Platform-blocked gates remain explicitly unresolved rather than being reported as successful.

## Notes

Use separate clean sandboxes per spec and a deliberately composed cross-section fixture for integration. One giant happy-path test does not replace feature-level negative cases. Keep only synthetic traces/screenshots and record exact commands/specs, not an unsubstantiated claim of complete coverage.
