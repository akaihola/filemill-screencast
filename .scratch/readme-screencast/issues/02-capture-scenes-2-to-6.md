# Capture scenes 2 to 6

Status: ready-for-agent
Blocked by: 01

## What to build

Extend the capture to the whole storyline in the spec. Every action after scene 1
is a tap, on a row or on the root spine, through the same touch input. Taps stay
under 500 ms, because a long press opens the context menu.

Find a row by its text inside `#finder` and tap it. The root spine is the first
`.col` while it has the class `spine`. A folder opened by URL does not
auto-select its README, so every scene reaches its end state by tapping.

Scene lengths in the prototype (light, 838 × 420): scene 1 7.3 s, scene 2 3.7 s,
scene 3 5.2 s, scene 4 6.1 s, scene 5 4.3 s, scene 6 4.1 s with its 2 s hold.
30.7 s in total, 2.11 MB at 12 fps.

## Acceptance

- [ ] The GIF plays all six scenes in order, holds the scene 6 README for 2 s,
      and cuts back to the opening frame.
- [ ] Each scene ends on the preview the spec names: the "Each section has its
      own link" section, "No JSON viewer needed", "No SQL needed", the zip
      member's README, "Try Filemill in your browser".
- [ ] The storyline lasts 35 s or less.
- [ ] The fixture matches the spec's storyline draft, including "Long files stay
      navigable".

## References

- Spec: storyline and fixture draft, narration rules.
- Prototype: `storyline()`, `tap_text()` and `to_root()` in
  `git show prototype/readme-screencast:prototype/capture_prototype.py`, and
  `prototype/fixture_prototype.py` on the same branch.
