# Filemill screencast

Produces the animated screencast at the top of the Filemill README, from a
scripted browser session that can be re-run when Filemill's UI changes.

## Language

**Screencast**:
The finished animated file embedded at the top of the Filemill README.
_Avoid_: demo, animation, video, GIF

**Opening frame**:
The first frame of the screencast. A viewer whose autoplay is off sees only
this frame, so it must show Filemill without motion.
_Avoid_: poster, thumbnail

**Capture**:
The raw frames of one scripted browser session, before trimming and encoding.
_Avoid_: recording, take

**Storyline**:
The ordered scenes of the screencast.

**Scene**:
One step of the storyline: a path through the fixture that ends on a preview
saying what the viewer just did and why it helps.
_Avoid_: shot, clip, step

**Fixture**:
The purpose-built folder tree that Filemill serves during a capture.
_Avoid_: demo folder, sample data

**Narration**:
The text the viewer reads in the screencast. It exists only as fixture folder
names, file names and file contents, never as an overlay.
_Avoid_: caption, subtitle, overlay

**Touch indicator**:
The finger-sized circle that shows where the scripted touch taps and drags.
_Avoid_: cursor, pointer, ripple

**Demo instance**:
Filemill's public site at filemill.vempai.men. It is not the fixture.
