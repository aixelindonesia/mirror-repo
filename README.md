<!-- readme:product -->
# mirror-repo

A composite GitHub Action that force-pushes branches from the current repository to a mirror repository using an SSH deploy key.
It is the one source for the per-repository "sync to mirror" workflows; its sibling [`mirror-subtree`](https://github.com/aixelindonesia/mirror-subtree) mirrors one subdirectory.

<!-- readme:contents -->
## What is inside

| Path | What it holds |
| --- | --- |
| [`action.yaml`](action.yaml) | The composite action: its inputs and steps. |
| [`CHANGELOG.md`](CHANGELOG.md) | The releases, folded from `.changes/` by [`scripts/assemble-changelog.mjs`](scripts/assemble-changelog.mjs). |
| [`docs/`](docs/README.md) | What it is, why it exists, its glossary, how it works, and how to verify a change. |

<!-- readme:usage -->
## Use it

Add this caller as `.github/workflows/mirror-repo.yaml`:

```yaml
name: Mirror Repo

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions: {}

concurrency:
  group: mirror-repo-${{ github.ref }}
  cancel-in-progress: false

jobs:
  sync:
    runs-on: ${{ vars.CI_RUNNER || 'ubuntu-26.04' }}
    timeout-minutes: 10
    permissions:
      contents: read
    steps:
      - uses: aixelindonesia/mirror-repo@82f9b9c5f469ba11c6e8b468c6bdd2234f6aff60 # v1.5.0
        with:
          target-repo: git@github.com:OWNER/REPO.git
          sync-branches: main
          git-user-name: Aixel
          git-user-email: aixelindonesia@gmail.com
          ssh-private-key: ${{ secrets.PERSONAL_REPO_SSH_KEY }}
```

The action checks out the full history itself, so the caller has no checkout step.
The SSH secret name is the caller's: pass the secret the repository already has, such as `PERSONAL_REPO_SSH_KEY` or `PRIVATE_REPO_SSH_KEY`.
Take a later version by moving the pin to the commit of a newer `vX.Y.Z` tag; Renovate does this for a repository that extends the org preset.

### Inputs

| Input             | Required | Default                     | Description                                              |
| ----------------- | -------- | --------------------------- | -------------------------------------------------------- |
| `target-repo`     | yes      | —                           | SSH URL of the mirror, `git@github.com:owner/repo.git`   |
| `ssh-private-key` | yes      | —                           | Private key; pass the repo's existing SSH secret         |
| `sync-branches`   | no       | `main`                      | Space-separated source branches or `source:target` mappings, e.g. `"main staging"` or `"dev:main"` |
| `git-user-name`   | no       | — (skips commit)            | Committer name for the empty sync commit; needs `git-user-email` too    |
| `git-user-email`  | no       | — (skips commit)            | Committer email; needs `git-user-name` too                              |

### Branch mapping

- `sync-branches: dev` mirrors source `dev` to mirror `dev`.
- `sync-branches: dev:main` mirrors source `dev` to mirror `main`.
- Multiple entries are supported, e.g. `sync-branches: "dev:main staging:preview"`.

### One-time setup per consuming repo

1. Generate a keypair (no passphrase):
   ```bash
   ssh-keygen -t ed25519 -C "<repo>-sync" -f /tmp/<repo>-sync -N ""
   ```
2. **Mirror repo** → Settings → Deploy keys → add the **public** key with **write** access.
3. **Source repo** → Settings → Secrets and variables → Actions → add the
   **private** key as a secret (any name), and reference that name in
   `ssh-private-key`. If the repo already has a sync secret, reuse it — no
   rename needed.
4. Add the caller workflow above as `.github/workflows/mirror-repo.yaml`.

### Notes

- When both `git-user-name` and `git-user-email` are set, each run appends an
  empty `chore: sync` commit, so the mirror's history intentionally diverges
  from origin by these markers. Leave both empty to mirror branches verbatim
  with no extra commit.
- The push is `--force` — the mirror branch is overwritten; treat it as one-way.
- Keep `on.push.branches` in sync with the source branches in `sync-branches`.

<!-- readme:changing -->
## Changing this repository

[`AGENTS.md`](AGENTS.md) holds the rules for a change; [Verify a change](docs/how-to/verify-change.md) says how to prove one.
Releases are SemVer `vX.Y.Z` tags with a moving major tag `v1`, by [changelog's versioning guideline](https://github.com/aixelindonesia/changelog/blob/main/guidelines/versioning.md).
Which repository owns each neighbouring subject is in [Where guidance lives](https://github.com/aixelindonesia/how-we-build/blob/main/guidelines/where-guidance-lives.md).
