<!-- markdownlint-disable -->

# Hardening Report: marocchino--tool-versions-action/v1.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **marocchino--tool-versions-action/v1.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses 'actions/checkout@v1', which is pinned to a mutable version tag rather than an immutable 40-character commit SHA. This means the action could be silently updated (or compromised) without the workflow noticing, enabling supply-chain attacks. It should be pinned to a full SHA, e.g. 'actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v1'.

Locations:

- `.github/workflows/test.yml:14`

### missing-permissions (severity: medium)

The workflow file '.github/workflows/test.yml' has no top-level 'permissions:' key and the only job ('test') also has no job-level 'permissions:' key. Without explicit permissions, the workflow inherits the default repository permissions (which may be overly broad, e.g. write access to contents). A minimal permissions block such as 'permissions: read-all' or specific scopes should be added.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

1. Pinned `actions/checkout@v1` to its full commit SHA `50fbc622fc4ef5163becd7fab6573eac35f8462e` with `# v1` comment for readability. 2. Added a top-level `permissions: contents: read` block — the minimal permission needed for a workflow that checks out repository code.

