# Phase 5: Deterministic Shared Priority Engine

## Plan reference

Source plan §§2.1–2.2 and F7; priority engine moved earlier so feature views share one tested policy from the start. Read `testing-and-delivery-contract.md`. Depends on phases 3–4.

## Tasks

- [ ] Write failing table-driven tests for eligibility, stable tie-breaking, completed occurrence exclusion, expiry/reset boundaries, source freshness and unknown evidence before implementing the pure engine.
- [ ] Implement tier-first deterministic ordering: relevant source-backed expiring exclusive events; eligible incomplete weekly/limited-reset tasks; active-build needs and explicit Short Term pins; Long Term goals with category preferences.
- [ ] Return structured priority tiers, reason codes, decisive preference, freshness, missing quantities, occurrence/expiry and supported alternatives; keep unknown and not-currently-relevant distinct from zero/easy.
- [ ] Test-first implement source-backed activity/mission overlap without double-counting one goal/shared item, fabricated efficiencies, unverified exclusivity or speculative Prime/seasonal claims.
- [ ] Add extension-owned preference controls and persistence for category/weight settings and explicit overrides; document which preferences may affect within-tier ordering and how manual overrides are visibly represented.
- [ ] Wire the engine to shared reactive state and minimal real section recommendation UI; invalidate on user edits, inventory/source updates, resets and expiry without parallel view-specific engines.
- [ ] Add `priorities.spec.ts` for actual settings actions and changed synthetic producer data, independently assert stored preferences, navigate/reload/restart, and verify correct reasons/order and no invented numeric rates.
- [ ] Run focused logic/Electron tests and applicable gates; record the policy, override semantics and red/green evidence so later feature phases extend rather than fork the engine.

## Acceptance and verification

A fixed set of inputs always yields the same order and explanation. An unverified expiry or irrelevant exclusive event cannot displace a valid weekly by pretending certainty. Completed occurrences remain ineligible for their period. Weight changes have a demonstrated, policy-compliant effect and persist after actual process restart.

## Notes

Use source fixture inputs to exercise weekly/event/build projections before their full editors exist; do not claim these early specs replace each feature's later user-action acceptance. Add full mixed-feature workflows in phase 11. Preferences must not enter upstream Settings or mutate item metadata.
