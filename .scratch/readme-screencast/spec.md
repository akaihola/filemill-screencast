# README screencast

Decisions from the 2026-10-09 grilling session. Vocabulary: [CONTEXT.md](../../CONTEXT.md).
Decisions with a recorded rationale: [docs/adr/](../../docs/adr/).

## Goal

One screencast at the top of the Filemill README ([akaihola/filemill](https://github.com/akaihola/filemill)).
Audience: a first-time GitHub visitor who has never used Miller columns.
Message: folders, Markdown headings, JSON, SQLite and zip archives all open as the same columns, with a rich preview.
Per-feature clips, keyboard demos and other surfaces (social preview, Pages) are out of scope; they can reuse the pipeline later.

## Artifact

- Animated GIF encoded with ffmpeg: one palette per capture, no dithering. See [ADR 0002](../../docs/adr/0002-the-screencast-is-a-gif.md).
- Two variants, light and dark, from two captures with Playwright `color_scheme` forced.
- Committed to this repo with stable file names, linked from the Filemill README by raw URL on `main`. See [ADR 0003](../../docs/adr/0003-gifs-live-here-linked-by-raw-url.md). Theme switching uses fragments, not `<picture>`, which drops the GIF pause button. Keep both images on one line; on separate lines GitHub puts a `<br>` between them:

  ```md
  ![<alt text>](https://raw.githubusercontent.com/akaihola/filemill-screencast/main/<path>/filemill-light.gif#gh-light-mode-only) ![<alt text>](https://raw.githubusercontent.com/akaihola/filemill-screencast/main/<path>/filemill-dark.gif#gh-dark-mode-only)
  ```

- Length: 35 s at most. If a scene does not fit, cut it, do not squeeze it.
- Budget: 3 MB at most per variant.
- Frame rate: 12 fps. The 30.7 s storyline is 2.11 MB at 12 fps; 15 fps came to 3.2 MB on the first, 36 s storyline.
- Viewport: 838 × 420 CSS px. 838 px is the README content column on desktop GitHub, at every viewport from 1280 to 2560 px wide (582 px at 1024). At 640 px tall the bottom third of every column and preview was empty. At landscape heights of 600 px or less Filemill sets `--column-ratio: 0.25` (`ui/core/styles.css:58-62`), so folder columns are 209 px wide instead of 240; nothing else changes. DPR 1, so text shows 1:1. DPR 2 is out: CDP screencast delivers 1× frames, and DPR 1 already fills the budget.
- No browser frame. Bare viewport.
- Loops forever. After the last scene's hold, it cuts back to the opening frame.
- The opening frame must explain Filemill on its own: a viewer with autoplay off sees only that frame.

## Narration

All text the viewer reads is fixture content. No captions or overlays. See [ADR 0001](../../docs/adr/0001-narration-lives-in-the-fixture.md).

- Folder and file names state the action that reaches them. Preview contents state the benefit.
- Preview narration is an h1 or h2 of 8 words or fewer. Markdown body text renders at 12 px, headings at 24/18 px.
- No dot in any name except before the real extension. Filemill shows everything after the last dot as a grey "extension" (`ui/core/model/state.js:77-80`).
- Names fit a 209 px column: 28 characters at most. In Inter, "Each section has its own link" (29) just fits and "Long documents stay navigable" (29) truncates.
- The root holds only folders, numbered with single digits. The server sorts folders first, then by lower-cased string, so `10` sorts before `2` (`server/src/filemill/vfs.py:19-21`). Archives and `.db` files sort with folders; `.md` and `.json` sort with files.
- Opening a folder auto-selects its `README.md` (not at the root, `ui/core/model/selection.js:2-10`), so a tap on a folder shows its narration at once.
- SQLite databases and tables preview nothing. Narration there is in table names, row labels (a text primary key is the label, `server/src/filemill/providers/sqlite.py:174-242`) and field values.

## Storyline and fixture draft

The prototype tunes the wording against real widths and legibility. The user reviews the final text.

```
Filemill tour/
  1 Swipe right/
    to unfold/
      the columns/
        you passed/
          README.md        # Swipe sideways to fold and unfold
  2 Open a Markdown file/
    Guide.md               # Guide (single title: its h2s form the column)
                           ## Headings open as columns
                           ## Each section has its own link
                           ## Long files stay navigable
  3 Open a JSON file/
    settings.json          {"Every key opens a column": {"all the way down": "No JSON viewer needed"}}
  4 Open a SQLite database/
    shop.db                table "Tables open as columns"
                             row with text primary key "Rows open as columns too"
                               field "and so do fields" = "No SQL needed"
  5 Open a zip archive/
    bundle.zip             No unpacking needed/README.md: # Archives open like folders
  6 Try it yourself/
    README.md              # Try Filemill in your browser
                           akaihola.github.io/filemill
```

Scenes:

1. **Fold and unfold** (about 7 s). The opening frame is the deep link to `1 Swipe right/…/you passed/README.md`: five columns, the left three folded into spines, README preview showing. Drag right: the spines unfold, the path reads as a sentence, and the root column shows the tour. Drag left: the columns fold again. Tap the root spine to unfold back to the tour (`ui/core/render.js:254`).
2. **Markdown headings** (about 5 s). Open `Guide.md`; its headings open as a column. Tap "Each section has its own link"; the preview shows that section alone.
3. **JSON** (about 4 s). Keys open as columns down to the scalar value.
4. **SQLite** (about 5 s). Database, table, row, field.
5. **Zip** (about 4 s). Archive members open as columns; the Markdown member previews rendered (needs Filemill [86]).
6. **Try it** (about 3 s, then hold 2 s). The README auto-selects.

At an 838 px viewport only two full columns fit beside the preview, so the root column is folded at the start of most scenes. Returning to the root (root-spine tap or a drag right) is part of each scene's time.

## Capture

- Scripted with Playwright in Python, run with uv. Re-recording is one command, run by hand on gogo after bumping the pinned Filemill sha.
- Filemill's server edition, at a pinned commit of `origin/main`: `uvx --from 'git+https://github.com/akaihola/filemill@<sha>#subdirectory=server' filemill FIXTURE`. The sha is recorded in this repo and bumped on purpose. Never record the `~/prg/filemill` working tree; it holds other sessions' uncommitted work.
- The fixture is generated by a script in this repo into a fresh temporary directory per capture. Do not import Filemill's test fixture builders. Ideas can come from `ui_root` in Filemill's `server/tests/test_browser_new_ui.py:55-162`.
- Deterministic output: fixed mtimes, locale `en-US`, timezone `UTC` (the preview header shows the modified date, `ui/core/render.js:716`), theme forced with `color_scheme`, default sort (a non-default sort persists in local storage).
- Fonts: Inter for UI and JetBrains Mono for code, the only freely licensed fonts in Filemill's stacks (`ui/core/styles.css:13-16`). Noto Color Emoji covers the 🗃️ 📁 📄 glyphs.
- Frames: CDP `Page.startScreencast` PNGs, resampled by timestamp onto a fixed frame-rate grid before encoding. It sends frames only when something changes, so holds cost nothing.
- Input: one touch indicator for every action, driven by real touch events: CDP `Input.dispatchTouchEvent`. A tap pulses it; a drag moves it along the drag. Drags scroll 1:1 after about 15 px of slop and never fling, so unfolding all the way from the opening frame (`scrollLeft` 990 of 1250 px) takes two swipes until Filemill [81]. Taps stay under 500 ms, because a long press opens the context menu (`ui/core/render.js:55`). Touch emulation changes no Filemill CSS. Horizontal scroll of `#finder` is the fold dial (`ui/core/styles.css:355-376`).

## Publishing

- This repo is public at `akaihola/filemill-screencast`. The agent pushes feature branches and opens pull requests; the maintainer merges. A merge to `main` publishes a re-record.
- The Filemill README edit is a separate Filemill task (red-green TDD per its `AGENTS.md`), handed off from here once the GIFs exist. It carries the alt text.

## Prerequisites before the final capture

- Filemill [83]: hide the visible "Search file contents" label. It shows in every frame.
- Filemill [81]: horizontal-scrolling fold stops. Scene 1 shows this gesture.
- Filemill [86]: render Markdown inside archives. Scene 5 needs it.
- Filemill [88]: status bar hints wrap and get clipped at 838 px. They show in every frame.
- Filemill [89]: Markdown previews show two Raw buttons. Several scenes show the toolbar.
- Filemill [87]: push `main`, make CI green, get <https://akaihola.github.io/filemill/> live. Scene 6 points there, and the pinned sha must exist on GitHub. Do not publish before the URL works.
- nixos-config `docs/backlog/install-ui-fonts-on-gogo.md`: Inter and JetBrains Mono on gogo.

Filemill [84] and [85] (JSON and images inside archives) do not show on screen and do not block.

## Prototype answers

Answered on 2026-10-09 by the prototype on the throwaway branch
`prototype/readme-screencast` (`prototype/README.md`), kept only in the clone on gogo,
against Filemill `7ff1fd2`.

1. Pause button with a theme fragment: yes. GitHub's Markdown API gives fragment URLs `data-animated-image`.
2. README column: 838 px.
3. Size: the first 36 s storyline at 838 × 420, DPR 1, ffmpeg, was 2.40 MB at 10 fps, 2.75 MB at 12 fps and 3.2 MB at 15 fps (light and dark within 0.1 MB). At 838 × 640 it was 2.82, 3.21 and 3.82 MB.
4. 12 px names, JSON scalar values and SQLite field values are readable at 1:1.
5. Frame source: CDP screencast. `record_video` blurs small text.
6. Touch: `Input.dispatchTouchEvent`; `Input.synthesizeScrollGesture` does nothing in headless Chromium. The indicator follows the page's own touch listeners.
7. `uvx --from git+…#subdirectory=server` installs and runs Filemill.
8. A `README.md` in a zip folder auto-selects after a tap. A folder opened by URL does not auto-select, so the capture taps.

Scene 1 first took 12.5 s: two swipes in, two out, the spine tap, and drag
steps that overran their timing. It keeps the fold-back with 450 ms swipes and
shorter holds, and now takes 7.3 s. The whole storyline takes 30.7 s: 1.86 MB at
10 fps, 2.11 MB at 12 fps (838 × 420, light).

Frame rate: 12 fps.

## Environment notes

- Browsers are pre-installed. Never run `playwright install`. Pin the Python `playwright` package to the version in `$UV_CONSTRAINT` (1.63 on 2026-10-09). If launch fails with "Executable doesn't exist at …/chromium_headless_shell-NNNN", re-pin; do not download.
- Chromium under Playwright drops the credentials in `$HTTPS_PROXY`, so external requests return 407. Pass `server`, `username` and `password` as separate keys of `launch(proxy=...)`, and set `proxy["bypass"]` from `$NO_PROXY`, or loopback servers answer 502.
- The Bash sandbox blocks loopback connections between commands. Run the Filemill server and Playwright in the same command.
- `$TMPDIR` is shared across sessions. Prefix scratch files with `screencast-`.
- ffmpeg 9 is installed; it is the only encoder needed.
- In the agent sandbox `uvx` cannot write `~/.local/share/uv/tools`; set `UV_TOOL_DIR` under `$TMPDIR`.
- Run renders and encodes with the harness's background mode, not a foreground timeout.
- Never record `filemill.service` (port 8334, Tailscale :8445): it serves the agent's whole home directory. The demo instance (`filemill-public.service`, `~/menu-public`) is not usable either: it symlinks the live Filemill working tree.
