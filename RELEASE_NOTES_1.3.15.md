# MediaCove 1.3.15 public beta

## Changes

- Show the full title of the focused episode at the bottom of the library.
- Restore the exact subtitle track when reopening a title, including when
  multiple tracks share a language. Make match labels in subtitle search
  results larger and easier to read.
- Add up quick seek presses and recover when LG webOS reports a completed
  seek before playback settles. Two quick +10s presses target a 20-second jump.
- Use one loading message during episode transitions. Keep the status visible
  during a stalled resume, retry once, and offer a skip if playback still
  cannot advance.

## Install

Use the [customer download and setup guide](https://mediacove-entitlements.mediacove.workers.dev/download/).
Update the LG app for the TV fixes and the server for the hosted web-player
fixes.

The Windows installer is unsigned. The Linux/macOS portable server requires
Node.js 22.x, FFmpeg, and ffprobe. The LG package installs through Developer
Mode. Samsung remains a development preview.
