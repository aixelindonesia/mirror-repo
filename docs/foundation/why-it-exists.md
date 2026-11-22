---
status: draft
---

# Why mirror-repo exists

Each entry says what goes wrong, who pays for it, how it is recognised, and the page that answers it.

## Hand-copied sync workflows drift

- Costs: each repository that mirrors carries its own copy of the sync steps, so a fix or a hardening reaches only the copy someone edits.
- Recognised by: `sync-to-personal.yml` and `sync-to-private.yml` files that differ between repositories.
- Answered by: [the README's Use it section](../../README.md#use-it), one caller of one action.
