---
status: draft
---

# Glossary

| Term | Meaning here | Not to be confused with |
| --- | --- | --- |
| Mirror | The repository the action pushes to, named by `target-repo`. | A GitHub fork. |
| Branch mapping | One `sync-branches` entry, `source` or `source:target`. | |
| Sync commit | The empty `chore: sync` commit added when both git identity inputs are set. | A merge commit. |
| Moving major tag | `v1`, moved to each `v1.x.y` release so callers of `@v1` take it. | A release tag, `vX.Y.Z`. |
