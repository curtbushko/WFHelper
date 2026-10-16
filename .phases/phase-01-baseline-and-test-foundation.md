# Phase 1: Baseline and Test Foundation

## Plan reference

Source plan: `../wfhelper-native-fork-plan.md` §§1, 3, 5/F0, 6 and 7. Read `testing-and-delivery-contract.md`. Dependencies: none.

## Tasks

- [ ] Record the actual checked-out commit, upstream baseline/remote, supported platforms, package identity and updater behavior; verify the source plan's audited interfaces against this checkout before relying on them.
- [ ] Run and record unchanged upstream baseline build, Svelte/check, typecheck, lint, format, Vitest and real Electron E2E gates; distinguish preexisting failures and unavailable platform runners without weakening gates.
- [ ] Create `UPSTREAM_TOUCHPOINTS.md` and an extension architecture/testing note inventorying reusable WFHelper types/models and each data owner's store/service/typed API, allowed seams, no-touch original screens and extension-owned directories; explicitly prohibit duplicate source-file readers, website/feed fetches and collectors for WFHelper-managed data.
- [ ] Audit existing fixtures/spec builders and public data for accurate canonical IDs, schemas, reset rules and Arcane rank/copy mappings; record provenance, versions and privacy review before reuse.
- [ ] Test-first add shared invented fixture builders for minimal inventory/catalog, world/relic/acquisition data, build requirements and multiple fake profiles, including known-zero, stale, missing and unsupported variants.
- [ ] Reuse or narrowly extend native harness/offline scenario helpers for declared external responses, source refresh, isolated user-data and a controllable main/renderer clock that can cross reset/expiry boundaries without real sleeps.
- [ ] Add and run a real Electron foundation spec proving synthetic source input, unexpected-network rejection, real storage inspection and full-process same-sandbox restart without reseeding edited records.
- [ ] Establish privacy-safe artifact/error collection and a producer-only synthetic secret-sentinel check; ensure CI cannot read a developer's real inventory, credentials or screenshots.
- [ ] Verify extension Vitest/Electron spec discovery through default scripts, document supported CI runners and red/green evidence conventions, and rerun the foundation tests and applicable baseline gates.

## Acceptance and verification

A contributor can run a deterministic offline real-Electron scenario using audited synthetic data and restart the actual process while retaining an independently verified edit. Fixture schema/rule tests fail if quantities, canonical IDs or rank/copy assumptions are invalid. Main and renderer agree on injected time. No extension production feature is required yet: exercise an existing safe preference/storage path to prove the harness; do not claim that this verifies future planner persistence.

Planning checkout observed: `06fa689715b0488eb6345b3b1649425cd5a268c8`, with only an `origin` remote. This is not a verified upstream SHA; implementation must establish the correct pinned upstream baseline. The source plan's audit date is not proof of current compatibility.

## Notes

Candidate reuse: `tests/fixtures/world/circuitFixture.ts`, `e2e/offlineScenario.ts`, `e2e/electronTestHarness.ts`, existing World/Foundry/Mastery/acquisition specs. Review contents rather than assuming all existing logs/images are suitable. Keep upstream gate and synthetic-data evidence available for every later phase.
