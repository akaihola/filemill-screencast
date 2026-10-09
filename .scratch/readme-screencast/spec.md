# README screencast

Decisions from the 2026-10-09 grilling session. Vocabulary: [CONTEXT.md](../../CONTEXT.md).
Decisions with a recorded rationale: [docs/adr/](../../docs/adr/).

## Goal

One screencast at the top of the Filemill README ([akaihola/filemill](https://github.com/akaihola/filemill)).
Audience: a first-time GitHub visitor who has never used Miller columns.
Message: folders, Markdown headings, JSON, SQLite and zip archives all open as the same columns, with a rich preview.
Per-feature clips, keyboard demos and other surfaces (social preview, Pages) are out of scope; they can reuse the pipeline later.

## Artifact

- Animated GIF encoded with gifski. See [ADR 0002](../../docs/adr/0002-the-screencast-is-a-gif.md).
- Two variants, light and dark, from two captures with Playwright `color_scheme` forced.
- Committed to this repo with stable file names, linked from the Filemill README by raw URL on `main`. See [ADR 0003](../../docs/adr/0003-gifs-live-here-linked-by-raw-url.md). Theme switching uses fragments, not `<picture>`, which drops the GIF pause button:

  ```md
  ![<alt text>](https://raw.githubusercontent.com/akaihola/filemill-screencast/main/<path>/filemill-light.gif#gh-light-mode-only)
  ![<alt text>](https://raw.githubusercontent.com/akaihola/filemill-screencast/main/<path>/filemill-dark.gif#gh-dark-mode-only)
  ```

- Length: 35 s at most. If a scene does not fit, cut it, do not squeeze it.
- Budget: 3 MB at most per variant.
- Viewport: as wide as the README content column (about 830 CSS px, to be measured) and taller than 600 px (Filemill switches to its phone layout at landscape heights of 600 px or less, `ui/core/styles.css:58-62`). DPR 1, so text shows 1:1. Use DPR 2 only if it stays within budget.
- No browser frame. Bare viewport.
- Loops forever. After the last scene's hold, it cuts back to the opening frame.
- The opening frame must explain Filemill on its own: a viewer with autoplay off sees only that frame.

## Narration

All text the viewer reads is fixture content. No captions or overlays. See [ADR 0001](../../docs/adr/0001-narration-lives-in-the-fixture.md).

- Folder and file names state the action that reaches them. Preview contents state the benefit.
- Preview narration is an h1 or h2 of 8 words or fewer. Markdown body text renders at 12 px, headings at 24/18 px.
- No dot in any name except before the real extension. Filemill shows everything after the last dot as a grey "extension" (`ui/core/model/state.js:77-80`).
- Names fit a 240 px column: about 33 characters for a folder, 36 for a file (measured with DejaVu; Inter is narrower).
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
                           ## Long documents stay navigable
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

At an 830 px viewport only two full columns fit beside the preview, so the root column is folded at the start of most scenes. Returning to the root (root-spine tap or a drag right) is part of each scene's time.

## Capture

- Scripted with Playwright in Python, run with uv. Re-recording is one command, run by hand on gogo after bumping the pinned Filemill sha.
- Filemill's server edition, at a pinned commit of `origin/main`: `uvx --from 'git+https://github.com/akaihola/filemill@<sha>#subdirectory=server' filemill FIXTURE`. The sha is recorded in this repo and bumped on purpose. Never record the `~/prg/filemill` working tree; it holds other sessions' uncommitted work.
- The fixture is generated by a script in this repo into a fresh temporary directory per capture. Do not import Filemill's test fixture builders. Ideas can come from `ui_root` in Filemill's `server/tests/test_browser_new_ui.py:55-162`.
- Deterministic output: fixed mtimes, locale `en-US`, timezone `UTC` (the preview header shows the modified date, `ui/core/render.js:716`), theme forced with `color_scheme`, default sort (a non-default sort persists in local storage).
- Fonts: Inter for UI and JetBrains Mono for code, the only freely licensed fonts in Filemill's stacks (`ui/core/styles.css:13-16`). Noto Color Emoji covers the 🗃️ 📁 📄 glyphs.
- Input: one touch indicator for every action, driven by real touch events. A tap pulses it; a drag moves it along the drag. Taps stay under 500 ms, because a long press opens the context menu (`ui/core/render.js:55`). Touch emulation changes no Filemill CSS. Horizontal scroll of `#finder` is the fold dial (`ui/core/styles.css:355-376`).

## Publishing

- This repo is public at `akaihola/filemill-screencast`. The agent pushes feature branches and opens pull requests; the maintainer merges. A merge to `main` publishes a re-record.
- The Filemill README edit is a separate Filemill task (red-green TDD per its `AGENTS.md`), handed off from here once the GIFs exist. It carries the alt text.

## Prerequisites before the final capture

- Filemill [83]: hide the visible "Search file contents" label. It shows in every frame.
- Filemill [81]: horizontal-scrolling fold stops. Scene 1 shows this gesture.
- Filemill [86]: render Markdown inside archives. Scene 5 needs it.
- Filemill [87]: push `main`, make CI green, get <https://akaihola.github.io/filemill/> live. Scene 6 points there, and the pinned sha must exist on GitHub. Do not publish before the URL works.
- nixos-config `docs/backlog/install-ui-fonts-on-gogo.md`: Inter and JetBrains Mono on gogo.

Filemill [84] and [85] (JSON and images inside archives) do not show on screen and do not block.

## Open questions for the prototype

1. Does a `.gif` with a `#gh-dark-mode-only` fragment on a raw URL still get GitHub's pause button (`data-animated-image`)? If not, fall back to light only.
2. How wide is the README content column on a desktop repo page?
3. GIF size per variant at DPR 1 and DPR 2, and at which frame rate.
4. Are 12 px names, JSON scalar values and SQLite field values readable at 1:1?
5. Which frame source gives clean frames: Playwright `record_video` (low-bitrate VP8), CDP `Page.startScreencast`, or per-frame screenshots?
6. How to synthesize the touch drag (for example CDP `Input.dispatchTouchEvent` or `Input.synthesizeScrollGesture`) and draw the touch indicator.
7. Does `uvx --from git+…#subdirectory=server` install and run Filemill, `ui/` symlink included?
8. Is a `README.md` inside a folder of a zip archive auto-selected?

## Environment notes

- Browsers are pre-installed. Never run `playwright install`. Pin the Python `playwright` package to the version in `$UV_CONSTRAINT` (1.63 on 2026-10-09). If launch fails with "Executable doesn't exist at …/chromium_headless_shell-NNNN", re-pin; do not download.
- Chromium under Playwright drops the credentials in `$HTTPS_PROXY`, so external requests return 407. Pass `server`, `username` and `password` as separate keys of `launch(proxy=...)`, and set `proxy["bypass"]` from `$NO_PROXY`, or loopback servers answer 502.
- The Bash sandbox blocks loopback connections between commands. Run the Filemill server and Playwright in the same command.
- `$TMPDIR` is shared across sessions. Prefix scratch files with `screencast-`.
- ffmpeg 9 is installed. gifski is not; `nix-shell -p gifski` needs the sandbox disabled (the Nix daemon socket is blocked).
- Run renders and encodes with the harness's background mode, not a foreground timeout.
- Never record `filemill.service` (port 8334, Tailscale :8445): it serves the agent's whole home directory. The demo instance (`filemill-public.service`, `~/menu-public`) is not usable either: it symlinks the live Filemill working tree.
