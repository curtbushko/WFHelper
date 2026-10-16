# Phase 12: Packaging and Upstream Maintenance

## Plan reference

Source plan F8 and §§6–8. Read `testing-and-delivery-contract.md`. Depends on phases 1–11.

## Tasks

- [ ] Audit actual package/app identity, release channels and auto-update paths; define fork identity and user-data migration strategy, and test-first prevent official updates from silently replacing the fork without discarding existing user state.
- [ ] Build Linux and other supported Electron distributions with existing scripts/runners; narrowly extend packaged/Linux smoke helpers for the five extension routes and original app screens using offline invented fixtures.
- [ ] Launch the actual packaged binary, perform a representative persistent planner edit, inspect real user-data and quit/relaunch the packaged app with the same sandbox; verify all five sections and original routes render without uncaught errors.
- [ ] Verify extension backup/export/import and failure recovery through packaged UI, including invalid/corrupt data and identity/path migration; no raw inventory or credentials may enter backups/artifacts.
- [ ] Document supported install/update/recovery flows, optional-section controls, truthful history/source limitations and feature exclusions; keep browser/Tailscale/API access explicitly deferred.
- [ ] Establish the dedicated upstream-sync PR playbook: old/new SHAs, fetch/merge without force-rewriting shared history, conflicts, affected extensions, persisted-state changes and full test evidence.
- [ ] Rehearse a merge of an actual newer upstream revision in a dedicated integration branch when available; inspect IPC/types, task IDs/reset functions, item keys, Vite assets, locale/registry ordering and updater identity without committing copied upstream modules.
- [ ] After the merge, rerun unchanged upstream gates, every farming E2E and packaged/platform regressions; if upstream overlaps extension behavior, specify and test a compatible retirement migration instead of maintaining two authorities.
- [ ] Complete release acceptance with documented narrow touchpoints, synthetic-only CI artifacts, platform results and upstream compatibility evidence; synchronize all phase task counts/statuses only after actual verification.

## Acceptance and verification

The packaged fork boots on supported Linux CI, all five sections and original routes work offline with invented fixtures, and user edits survive a packaged full restart. Official updating cannot silently replace the fork. Backup/recovery works without leaking source payloads. An upstream integration demonstrates maintainable seams and unchanged behavior with exact before/after SHAs and test results.

## Notes

If no newer upstream revision exists, document that fact and prepare the integration procedure; do not fabricate a completed newer-revision merge. The source plan's final merge criterion remains pending until it can be exercised. Native platform runners are required for platform-specific tests; macOS-only local checks cannot certify Linux/Windows packaging. Do not push, publish or deploy as part of planning or without explicit release authorization.
