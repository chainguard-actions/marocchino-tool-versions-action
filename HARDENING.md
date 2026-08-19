<!-- markdownlint-disable -->

# Hardening Report: marocchino--tool-versions-action/v1.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **marocchino--tool-versions-action/v1.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference GitHub Actions using mutable version tags instead of full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if the tag is moved to a different (potentially malicious) commit.

.github/workflows/codeql.yml:
  - actions/checkout@v4
  - github/codeql-action/init@v3
  - github/codeql-action/autobuild@v3
  - github/codeql-action/analyze@v3

.github/workflows/test.yml:
  - actions/checkout@v4
  - actions/setup-node@v4

Locations:

- `.github/workflows/codeql.yml:22`
- `.github/workflows/codeql.yml:25`
- `.github/workflows/codeql.yml:30`
- `.github/workflows/codeql.yml:33`
- `.github/workflows/test.yml:13`
- `.github/workflows/test.yml:18`

### missing-permissions (severity: medium)

The workflow file .github/workflows/test.yml has no top-level `permissions:` key and its only job (`test`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g., write access to contents). A minimal permissions block should be added.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 6 unpinned action references by pinning them to full 40-character commit SHAs (with original tags preserved as comments). Added a top-level `permissions: {}` block to test.yml to deny all permissions by default, and a job-level `permissions: contents: read` for the test job (minimum required for actions/checkout). The codeql.yml already had appropriate job-level permissions and needed no permissions changes.

