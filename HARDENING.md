<!-- markdownlint-disable -->

# Hardening Report: marocchino--tool-versions-action/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **marocchino--tool-versions-action/v2.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files are pinned to mutable version tags instead of full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if the referenced tag is moved or overwritten.

In `.github/workflows/codeql.yml`:
- `actions/checkout@v6` (line 24)
- `github/codeql-action/init@v4` (line 27)
- `github/codeql-action/autobuild@v4` (line 33)
- `github/codeql-action/analyze@v4` (line 36)

In `.github/workflows/test.yml`:
- `actions/checkout@v6` (line 13)
- `actions/setup-node@v6` (line 17)

All should be pinned to full SHA digests, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/codeql.yml:24`
- `.github/workflows/codeql.yml:27`
- `.github/workflows/codeql.yml:33`
- `.github/workflows/codeql.yml:36`
- `.github/workflows/test.yml:13`
- `.github/workflows/test.yml:17`

### missing-permissions (severity: medium)

`.github/workflows/test.yml` has no top-level `permissions:` key and its only job (`test`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the default repository token permissions, which may be broader than necessary (e.g., `write` access to contents). A minimal `permissions:` block such as `contents: read` should be added at the top level or on the job.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 6 unpinned action references by pinning them to full 40-character SHA commit hashes (resolved via lookup_action_sha) while preserving the original tag as a comment. Added a top-level `permissions: contents: read` block to test.yml to address the missing-permissions finding. The codeql.yml already had appropriate job-level permissions (actions: read, contents: read, security-events: write).

