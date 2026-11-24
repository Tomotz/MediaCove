# MediaCove 1.3.17 public beta

## Changes

- Keep the player on “Episode finished” when a finished stream resets or
  reports a late error.
- Preserve backward seeking and Next Episode after playback ends.

## Install

Download the matching package from the [1.3.17 GitHub release](https://github.com/Tomotz/MediaCove/releases/tag/v1.3.17).
Use the [customer setup guide](https://mediacove-entitlements.mediacove.workers.dev/download/)
for installation steps. Update the LG app for the TV fix and the server
for the hosted web player update.

The Windows installer is unsigned. The Linux/macOS portable server requires
Node.js 22.x, FFmpeg, and ffprobe. The LG package installs through Developer
Mode. Samsung remains a development preview.
