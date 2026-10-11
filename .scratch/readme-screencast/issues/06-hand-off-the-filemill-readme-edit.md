# Hand off the Filemill README edit

Status: ready-for-agent
Blocked by: 05

## What to build

A Filemill task that puts the screencast at the top of Filemill's README.
Filemill's `TASKS.md` rules apply: the first unused number, a description file in
`docs/tasks/`, an entry in the ordered backlog, a link reference. A Filemill
session makes the edit, following Filemill's `AGENTS.md`.

The task carries:

- The embed. Keep both images on one line; on separate lines GitHub puts a
  `<br>` between them.

  ```md
  ![<alt text>](https://raw.githubusercontent.com/akaihola/filemill-screencast/main/screencast/filemill-light.gif#gh-light-mode-only) ![<alt text>](https://raw.githubusercontent.com/akaihola/filemill-screencast/main/screencast/filemill-dark.gif#gh-dark-mode-only)
  ```

- The alt text, approved by the user. It tells someone who cannot see the
  screencast what it shows.

## Acceptance

- [ ] Filemill's `TASKS.md` lists the task, and its description holds the embed
      and the approved alt text.
- [ ] Both raw URLs return the GIFs, which means 05's pull request is merged.

## References

- Spec: Artifact (the embed), Publishing.
- [ADR 0003](../../../docs/adr/0003-gifs-live-here-linked-by-raw-url.md).
