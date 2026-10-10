# The screencast is a GIF

The screencast is an animated GIF, not animated WebP, APNG, SVG or mp4. GitHub
gives only `.gif` images a pause button, and only GIFs honour a viewer's
"Autoplay animated images" setting, which follows the system's reduced-motion
preference by default. WebP, APNG and SVG always play. An mp4 plays inline only
from an uploaded `user-attachments` URL, as a click-to-play, muted, non-looping
player that a script cannot produce. WebP or mp4 can be smaller; do not switch
without giving up the pause button. Checked against GitHub's renderer on
2026-10-09.

ffmpeg encodes it, with one palette for the whole capture
(`palettegen=stats_mode=diff`) and no dithering
(`paletteuse=dither=none:diff_mode=rectangle`). gifski was the first choice, but
in the 2026-10-09 prototype it left faint ghosts of earlier frames on Filemill's
flat backgrounds at every setting tried, `--motion-quality 100` included, and
its files were 15% larger. Screen content is flat colour, so a dither-free
palette loses nothing. Do not switch back to gifski or add dithering without
checking still frames at 1:1 for ghosts.
