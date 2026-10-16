# Phase 3: Shared Model and Native Adapters

## Plan reference

Source plan §§2.1, 3, 4.1 and 5/F1. Read `testing-and-delivery-contract.md`. Depends on phases 1–2.

## Tasks

- [ ] Write table-driven failing tests for canonical item/goal identity, one primary classification, target/owned separation, valid quantities/ranks, occurrence references and distinct known-zero/unknown states.
- [ ] Import/reuse existing WFHelper data models, TypeScript types and canonical identities; compose them into planner-specific goal, build, event, evidence and occurrence records only where new user intent requires it, documenting any new type rather than duplicating upstream fact models.
- [ ] Implement thin read-only projections of existing WFHelper inventory, item DB, relics, World, Mastery and Foundry stores/services/typed APIs; reuse upstream parsing/crafting/ownership utilities and refresh mechanisms, with no direct raw-source file reads, duplicate website/feed requests or independent collectors.
- [ ] Model freshness, unavailable/unsupported sources, verified expiry/recurrence, acquisition evidence and source timestamps; never derive confirmed unowned from missing inventory.
- [ ] Add source-refresh invalidation and profile/scope handling where upstream exposes identity, keeping user intent independent of source snapshots and preventing cross-profile ownership claims.
- [ ] Test-first implement shared reactive projections/selectors consumed by all five views, with no duplicate owned quantities or goal records when one item serves multiple views/builds.
- [ ] Expose minimal meaningful native UI status projections in the five shells and add real Electron input-refresh tests proving matching owned/unknown states across views, reload and full restart using the same synthetic source files.
- [ ] Assert producer-only private fields are excluded from projections/serialization and preserve unknown categories, availability and stale evidence; add realistic base/Prime, Arcane, mastery and resource fixtures.
- [ ] Run adapter/model Vitest and Electron tests plus applicable upstream gates; verify farming code uses the inventoried upstream types/data owners and introduces no duplicate source reads/fetches, and document genuine capability gaps for approval instead of bypassing existing services.

## Acceptance and verification

Existing WFHelper models and data-access owners are explicitly mapped and reused; farming production code does not read their raw source files or refetch their managed websites/endpoints. Assume access already exists and trace current consumers/exports before reporting a gap. Any proposed extension to an existing service requires asking the user and receiving explicit approval first; do not implement a service/API change or parallel reader as a workaround. Extension-owned planner file persistence remains permitted. Changing an invented inventory source updates all relevant views consistently; missing/stale sources remain explicit. Canonical identity survives refresh. No manual planning completion can mutate inventory. UI tests change producer input through native boundaries, not mocked selectors. Durable pin survival is verified in phase 4 once the production persistence path exists; do not invent temporary persistence solely to finish this phase.

## Notes

Use `src/extensions/farming/shared/{model,adapters,stores}/` and `tests/extensions/farming/`. Keep profile behavior conservative if upstream lacks a reliable identifier: explain limitations and do not treat a new snapshot as proof of another profile's completion.
