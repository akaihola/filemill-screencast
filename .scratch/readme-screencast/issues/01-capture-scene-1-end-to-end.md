# Capture scene 1 end to end from one command

Status: ready-for-agent

## What to build

The thinnest pipeline that works: one command builds the fixture, serves it with
the pinned Filemill, captures scene 1 (fold and unfold) in light mode, and writes
a GIF. Issues 02 and 03 add the other scenes, the dark variant and the checks.

- One file in this repo holds the Filemill commit sha. Start with
  `7ff1fd2ae5d441194bbc74d004d5d78c9d05c04e`, the commit the prototype used.
  Bumping it is a one-line edit.
- A fixture script writes the fixture into a fresh temporary directory, with
  fixed mtimes. Scene 1 needs only its own folders, but the whole fixture can go
  in now.
- The capture starts `uvx --from 'git+https://github.com/akaihola/filemill@<sha>#subdirectory=server' filemill`
  on a free loopback port, in the same process as Playwright.
- Chromium runs at 838 × 420 CSS px, DPR 1, with `has_touch`, locale `en-US`,
  timezone `UTC` and `color_scheme` forced. Each capture gets a fresh browser
  context, so the default sort holds.
- Touch input goes through CDP `Input.dispatchTouchEvent`. Drag steps follow a
  clock, not a fixed sleep per step, because CDP round trips made the
  prototype's 700 ms drags overrun. A finger lifted mid-motion does not fling, so
  scene 1 unfolds with two swipes.
- The touch indicator is an element injected with `add_init_script`. Capture-phase
  passive touch listeners move it. A tap pulses it.
- Frames come from CDP `Page.startScreencast` as PNGs with timestamps. The
  encoder resamples them onto a 12 fps grid.
- ffmpeg encodes with `palettegen=stats_mode=diff`, then
  `paletteuse=dither=none:diff_mode=rectangle`. The GIF loops forever.

## Acceptance

- [ ] One `uv run` command produces the GIF, with no manual steps.
- [ ] The first frame is the opening frame: the deep link to
      `1 Swipe right/…/you passed/README.md`, spines folded, README preview
      showing, no touch indicator.
- [ ] Scene 1 shows two swipes that unfold to the tour, a hold, two swipes that
      fold back, and a tap on the root spine. It lasts about 7 s.
- [ ] The touch indicator follows each drag and pulses on the tap.
- [ ] Still frames at 1:1 show no ghosts of earlier frames on flat backgrounds.

## References

- Spec: Artifact, Capture, scene 1 of the storyline, environment notes (proxy
  keys, loopback in one command, `UV_TOOL_DIR` in the sandbox, never
  `playwright install`).
- [ADR 0002](../../../docs/adr/0002-the-screencast-is-a-gif.md): the ffmpeg
  settings and why not gifski.
- The prototype did all of this in throwaway form. It lives only on the local
  branch `prototype/readme-screencast` in the clone on gogo:
  `git show prototype/readme-screencast:prototype/capture_prototype.py`.
  `Toucher`, `INDICATOR_JS`, `motion()` and `encode()` are the parts that
  worked. Rewrite them; the prototype has no structure and no checks.
