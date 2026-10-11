# Write both variants to stable paths and check them

Status: ready-for-agent
Blocked by: 02

## What to build

One command captures and encodes both variants to `screencast/filemill-light.gif`
and `screencast/filemill-dark.gif`. The Filemill README will embed these raw
URLs, so the names never change.

The run checks each variant before it replaces a committed file:

- 3 MB or less, 35 s or less.
- Chromium rendered the UI in Inter and code in JetBrains Mono. Ask CDP
  `CSS.getPlatformFontsForNode` for a row and for a code block; it reports the
  font Chromium drew with. gogo has neither font yet, so a development capture
  needs an explicit override to accept fallback fonts, or a `FONTCONFIG_FILE`
  that points at a scratch font directory, as the prototype used.

It prints size, length and frame count for each variant.

## Acceptance

- [ ] One command writes both GIFs.
- [ ] A variant over budget or over length fails the run, and the committed
      file stays as it was. Try it by lowering the limit.
- [ ] A capture in fallback fonts fails unless the override is set.
- [ ] The two variants differ only in theme.
- [ ] A second run without changes gives GIFs that look the same. Equal bytes
      are not expected: screencast timestamps vary.

## References

- Spec: Artifact (budget, length, the embed), Capture (fonts).
- Prototype measurements: light and dark sizes stay within 0.1 MB of each other.
- nixos-config `docs/backlog/install-ui-fonts-on-gogo.md`: installs the fonts
  on gogo. Its "New Insights" section says which Inter files match "Inter".
