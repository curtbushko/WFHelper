# Phase 8: Builds and Shared Requirements

## Plan reference

Source plan §§2.1–2.2 and F4; build and multi-build inventory scenarios. Read `testing-and-delivery-contract.md`. Depends on phases 3–5; execute after phase 7 in the default sequence.

## Tasks

- [ ] Write failing deficit/rank/allocation tests for Warframe, weapon and companion builds, required mods, Arcanes, shards, Helminth and equipment using accurate public rank/copy rules.
- [ ] Implement Active/Planned/Archived build creation, editing, lifecycle changes and notes with stable IDs and validated quantities/rank targets through production persistence.
- [ ] Add grouped requirement selection and display using native item DB, mastery/crafting/Foundry utilities and existing item-detail navigation; do not clone a Foundry crafting tree.
- [ ] Derive owned/missing/unknown quantities from upstream evidence without user checkmarks claiming possession; distinguish mastered, crafted and owned where relevant.
- [ ] Define and test reusable equipment vs consumed resources/Arcane allocation semantics across two builds; show truthful aggregate/per-build requirements and no double allocation or duplicate goal ownership.
- [ ] Link build requirements to the shared engine and derived Short Term recommendations using stable references, accounting for active/planned/archived relevance without creating duplicate standalone goals.
- [ ] Complete `builds.spec.ts` with real create/edit/archive/re-enable actions, Arcane/mod/shard/Helminth/companion requirements, independent disk assertions and navigation/reload/full restart checks.
- [ ] Add multi-build changed-inventory E2Es, source/profile loss, invalid rank/negative quantity rejection without mutation, save-failure recovery and clear unknown states; verify user builds survive source refresh.
- [ ] Check native accessibility/layout and original Foundry/Mastery regressions, run focused tests and applicable gates, and record allocation rules and red/green evidence.

## Acceptance and verification

Two active builds referencing one Arcane cannot each independently spend the same consumable copies. Reusable equipment is not blindly summed as if consumed. Editing a build or refreshed synthetic inventory changes truthful shortages across views; archive/re-enable and notes survive restart. Invalid edits leave saved records unchanged.

## Notes

Make sharing/allocation assumptions visible in explanations. Linking an existing standalone Arcane target later must preserve its original stable ID/history and avoid double-counting. An unavailable inventory snapshot is not evidence of zero owned.
