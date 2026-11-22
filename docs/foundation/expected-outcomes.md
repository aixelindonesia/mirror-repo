---
status: draft
---

# Expected outcomes

## Success criteria

- A caller copied from the README passes repo-rules' workflow rules: `gha-pin-actions-to-sha`, `yaml-extension`, and `workflow-basics/permissions`.
- Every `uses:` in `action.yaml` is pinned by SHA with a version comment that `pinact run --check --verify-comment` accepts.
- An existing caller of `@v1` keeps working after each release, because `v1` moves to the newest `v1.x.y`.

## Non-goals

- Mirroring a subdirectory, or syncing in both directions.
