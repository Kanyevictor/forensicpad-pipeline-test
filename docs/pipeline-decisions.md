
## 2026-05-12 — Action pinning strategy

For first-party actions (actions/checkout, actions/setup-node,
actions/upload-artifact, github/codeql-action), the project uses
major version tags (e.g., @v4, @v3) rather than commit SHAs.

Rationale:
  - These actions are maintained directly by GitHub
  - Major version tags are stable and follow semantic versioning
  - GitHub's own documentation uses tags for first-party actions

Risk accepted: a maintainer-level compromise at GitHub could in
theory rewrite these tags. The compensating controls are:
  - Workflow permissions default to read-only
  - persist-credentials: false prevents token leakage
  - All third-party actions WILL be pinned to commit SHAs when added
    (specifically: Sigstore actions, SLSA generator, Syft, etc.)

To convert to SHA pinning later:
  gh api /repos/<owner>/<action>/git/refs/tags/<tag> | jq -r .object.sha

