# Phase 9: Short Term Targets and Activities

## Plan reference

Source plan §§2.1–2.2 and F5; Short Term and activity recommendation acceptance. Read `testing-and-delivery-contract.md`. Depends on phases 4–8.

## Tasks

- [ ] Write failing tests for deliberate standalone pins, target copies/ranks, manual priority overrides, archive status and derived-vs-owned goal identity.
- [ ] Implement Next Up/My Targets/Activities with native quick-add search over upstream items, deliberate standalone targets, editing, notes and archive actions; never auto-pin the whole catalog.
- [ ] Render build/weekly/event-derived acquisition suggestions with origin links and explanations, not duplicate editable ownership/checklist records.
- [ ] Implement source-backed acquisition alternatives and mission/activity overlap through shared adapters/engine; display unknown rates/availability explicitly without numerical efficiency or invented farm times.
- [ ] Implement identity-preserving Short Term/Long Term promotion/demotion through shared classification actions, retaining history, references, quantities and overrides; the full Long Term surface arrives in phase 10.
- [ ] Complete `short-term.spec.ts` for an independent Arcane/weapon pin, edited copies/priority, archive and restoration, persisted state, navigation/reload/full restart and no originating build deletion.
- [ ] Add real two-route activity comparisons where changing targets/overrides or completing a Weekly occurrence changes supported explanations/order; change upstream producer input for computed outputs.
- [ ] Exercise invalid quantity/rank, unsupported acquisition data, stale inventory, unlinking a suggestion, linking a standalone goal to a build and failed saves without losing records or claiming possession.
- [ ] Verify native usability and shared identity across existing views, run focused red/green tests and applicable gates, and record supported acquisition evidence and semantics.

## Acceptance and verification

A user can farm a standalone target without creating a build. User edits survive actual process restart. Derived suggestions remain linked to their owner and cannot delete it. Unknown odds do not become numeric rankings. Existing goals retain identity when linked to builds or reclassified.

## Notes

Cross-section ranking must call the shared engine, not a bespoke Next Up scorer. A manually archived farming goal is user intent, not a claim that its items are owned. Reclassification is fully exercised from both real view surfaces in phase 10.
