# Phase 10: Long Term Catalog and Progression

## Plan reference

Source plan §§2–2.2 and F6; Long Term catalog and Mastery/Prime/Arcane acceptance. Read `testing-and-delivery-contract.md`. Depends on phases 3–5 and 8–9.

## Tasks

- [ ] Write failing tests for canonical base/Prime separation, owned/mastered/crafted/needed/unknown states, Arcane rank/copy progression, collections and explicit goal creation versus passive browsing.
- [ ] Implement Catalog/Mastery/Prime/Arcanes/Collections tabs using existing supported item DB, native search/category filters and upstream item-detail routes.
- [ ] Add paginated or virtualized large-result rendering with responsive filtering, keyboard/focus behavior and no auto-created goal for every discovered item.
- [ ] Reuse upstream mastery pins and crafting dependencies through established utilities; reconcile shared notifications without a contradictory independent mastery-pin ledger or copied native explorer.
- [ ] Add explicit Long Term target creation and identity-preserving reclassification to/from Short Term, retaining history and references when goals also serve builds.
- [ ] Render supported collection completion, base/Prime variants and Arcane rank goals with accurate source freshness; inventory/profile refresh cannot erase user goals or turn unknowns into confirmed gaps.
- [ ] Complete `long-term.spec.ts`: search/filter an unpinned item, open native detail, create goal, inspect variants/progression, reclassify both directions, inspect IDs on disk and verify navigation/reload/full restart without duplicates.
- [ ] Add changed-inventory/mastery E2Es, unsupported category/availability, failed saves and an audited generated catalog with thousands of entries; prove a representative interaction/navigation stays responsive within a documented stable-runner budget.
- [ ] Verify native accessibility/layout and unchanged Mastery/Relics/Foundry controls, run focused logic/Electron tests and applicable gates, and record realistic catalog-scale evidence.

## Acceptance and verification

Browsing thousands of entries does not persist thousands of goals. A selected target moves between Short Term and Long Term as the same goal ID, survives restart and keeps its history. Base and Prime remain distinct; mastered is not conflated with currently owned or crafted. Collection denominators/unknown members are explained instead of showing false completion.

## Notes

Performance acceptance should use a documented supported-runner budget and meaningful UI responsiveness, not arbitrary coverage percentages or brittle wall-clock sleeps. Public item constants and generated catalog-shaped data must not import a real player's account snapshot.
