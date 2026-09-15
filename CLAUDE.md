# CLAUDE.md

Guidance for Claude Code when working in this repository. What the package does
and how it is built is in `README.md`.

## Where it runs

This repo only builds the image. The cluster manifests live in
[mpdavis/homelab](https://github.com/mpdavis/homelab) under
`kubernetes/apps/gridiron/gridiron/`, which Flux reconciles.

## Release flow

1. Merge to `main` → `build.yml` publishes `ghcr.io/mpdavis/gridiron:1.1.<run_number>`
   (only when the image's build context changed; PRs build but never push).
2. Renovate in homelab sees the new tag and opens the pin bump there.

An image change and its pin bump can never land in one PR: the tag does not exist
until main publishes it. Keep tags on `1.1.x` — see the comment in `build.yml`.

## Checks

- `Tests / test` is the required check on `main`. It has no `paths` filter on
  purpose; a required check that gets skipped blocks the PR forever.
- Run the suite on the Python the image ships (`Dockerfile` `FROM`), not whatever
  is local: `pip install -e ".[dev]" && pytest -q`.
