# Phase 2: Five-Section Native Shell

## Plan reference

Source plan §§2, 4 and 5/F0; navigation and upstream-regression acceptance. Read `testing-and-delivery-contract.md`. Depends on phase 1.

## Tasks

- [ ] Write failing real Electron sidebar/navigation scenarios for exactly Weekly, Events, Builds, Short Term and Long Term alongside all original routes; add focused registry tests where practical.
- [ ] Add five extension-owned lazily loaded Svelte views with truthful empty/loading/unknown states and planned native sub-tabs, without inventing a sixth Farming Planner route.
- [ ] Register only the required view IDs, labels, icons, ordering and hideability through existing view registry conventions; document every upstream seam.
- [ ] Add required localized labels and use upstream theme, focus, keyboard, responsive tab/table and accessibility conventions rather than a separate app shell.
- [ ] Integrate sidebar visibility/order, selected view and small view preferences with existing validated preference patterns and extension-prefixed keys where appropriate.
- [ ] Complete `navigation.spec.ts` with real sidebar actions, independent preference assertions, navigate-away/back, renderer reload and full Electron restart checks.
- [ ] Cover locale/theme changes, keyboard focus, small windows and missing game data; assert no uncaught errors and that original Inventory, World, Mastery, Foundry, Relics and Dashboard navigation remains intact.
- [ ] Run focused red/green tests and applicable upstream gates, inspect lazy-loading/build output and touchpoint diff, and record acceptance evidence.

## Acceptance and verification

All five views open independently through the real sidebar. Optional visibility/order, theme and locale survive a full quit/relaunch. No original screen/component is rewritten. Original views still render with the synthetic fixture setup. Production shell styling uses native components; empty shells do not imply underlying features are complete.

## Notes

Keep implementation under `src/extensions/farming/`; integrate via `src/types/views.ts`, `src/lib/viewRegistry.ts` and existing i18n registration only as needed. Run Linux boot acceptance on a supported runner; record local platform limitations honestly. Fork updater protection is finalized in phase 12; disable unsafe official updating in any interim distributed build.
