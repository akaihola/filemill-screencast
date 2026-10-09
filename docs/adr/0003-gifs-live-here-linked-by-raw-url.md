# The GIFs live in this repo, linked by raw URL

The Filemill README embeds the screencast from
`https://raw.githubusercontent.com/akaihola/filemill-screencast/main/…` with
stable file names, not from a file committed to Filemill. Every re-record adds
megabytes of binary history. Keeping it here keeps Filemill's history clean, and
a re-record goes live without a Filemill commit. Raw URLs bypass GitHub's camo
image proxy and its 5 MiB cap, and keep the `#gh-light-mode-only` and
`#gh-dark-mode-only` fragments working. Re-records reach `main` only through a
pull request that the maintainer merges, so that merge is the publish gate.

## Consequences

Older Filemill commits show the newest screencast, not the one from their time.
