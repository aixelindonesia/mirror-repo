---
status: draft
---

# What mirror-repo is

`mirror-repo` is a composite GitHub Action that force-pushes chosen branches of the calling repository to a mirror repository over SSH with a deploy key.
Its readers are the people who keep a copy of a repository in a second organisation or account.

## Current shape

- One [`action.yaml`](../../action.yaml) with five inputs: `target-repo`, `ssh-private-key`, `sync-branches`, `git-user-name`, and `git-user-email`.
- Releases tagged `vX.Y.Z` with a moving `v1`, recorded in [`CHANGELOG.md`](../../CHANGELOG.md).

## What it is for

- Mirroring whole branches one way, optionally under another branch name, with an optional empty `chore: sync` marker commit.

## What it is not for

- Mirroring one subdirectory, which [`mirror-subtree`](https://github.com/aixelindonesia/mirror-subtree) does.
- Two-way sync: the push is `--force`, so the mirror branch is overwritten.
