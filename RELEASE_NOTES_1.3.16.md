# MediaCove 1.3.16 public beta

## Changes

- Show the title and buffering status together in the same font.
- Keep playback requested after an interrupted native LG startup.
- Retry a stalled LG remux once with a transcoded stream at the same position.

## Install

Download the matching package from the [1.3.16 GitHub release](https://github.com/Tomotz/MediaCove/releases/tag/v1.3.16).
Use the [customer setup guide](https://mediacove-entitlements.mediacove.workers.dev/download/)
for installation steps. Update the LG app for the TV fixes and the server
for the hosted web player update.

The Windows installer is unsigned. The Linux/macOS portable server requires
Node.js 22.x, FFmpeg, and ffprobe. The LG package installs through Developer
Mode. Samsung remains a development preview.
