# Changelog

## v1.5.1 - 2026-11-22

### Changed

- The action file is `action.yaml`; callers of `aixelindonesia/mirror-repo@v1` need no change.
- `actions/checkout` moves to v7.0.1 and `webfactory/ssh-agent` stays at v0.10.0, both pinned by commit SHA.
- The README caller is `.github/workflows/mirror-repo.yaml` with top-level `permissions: {}`, job permissions, and a SHA-pinned `uses:`.
