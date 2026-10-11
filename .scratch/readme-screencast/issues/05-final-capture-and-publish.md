# Make the final capture and publish it

Status: ready-for-agent
Blocked by: 03, 04

Also blocked outside this repo:

- Filemill [81], horizontal-scrolling fold stops. Scheduled on 2026-10-11.
- Filemill [87]: push `main`, make CI green, publish
  <https://akaihola.github.io/filemill/>. In progress on 2026-10-11.
- nixos-config `docs/backlog/install-ui-fonts-on-gogo.md`: Inter and JetBrains
  Mono on gogo. Not started on 2026-10-11.

Filemill [83], [86], [88] and [89] were done on Filemill's local `main` by
2026-10-11, waiting for acceptance and not yet on GitHub. [87] pushes them.

## What to build

- Bump the pinned sha to a Filemill commit on GitHub `main` that contains [81],
  [83], [86], [87], [88] and [89].
- Rework scene 1 for the fold stops that [81] adds. Swipes then stop at fixed
  positions, so the number and length of the swipes change. Keep the fold-back,
  and keep scene 1 near 7 s.
- Check that <https://akaihola.github.io/filemill/> answers. Scene 6 points
  there; do not publish before it works.
- Capture both variants on gogo with the fonts installed, no override.
- Commit the GIFs and the sha bump on a branch and open a pull request. The
  maintainer's merge publishes.

## Acceptance

- [ ] No frame shows the search label, wrapped status hints, a second Raw button
      or raw Markdown in the zip scene.
- [ ] Both GIFs pass the checks from 03.
- [ ] The pull request is open with both GIFs, and the Pages URL works.

## References

- Spec: Prerequisites before the final capture, Publishing.
- [ADR 0003](../../../docs/adr/0003-gifs-live-here-linked-by-raw-url.md):
  re-records reach `main` only through a merged pull request.
