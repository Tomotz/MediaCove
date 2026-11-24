# MediaCove 1.3.18 public beta

## Changes

- Clear Playback unavailable when the active stream resumes advancing.
- Ignore stale LG media events while the video has playable frames.
- Preserve error feedback for genuine playback failures.

## Install

Download the matching package from the [1.3.18 GitHub release](https://github.com/Tomotz/MediaCove/releases/tag/v1.3.18).
Use the [customer setup guide](https://mediacove-entitlements.mediacove.workers.dev/download/)
for installation steps. Update the LG app for the TV fix and the server
for the hosted web player update.

The Windows installer is unsigned. The Linux/macOS portable server requires
Node.js 22.x, FFmpeg, and ffprobe. The LG package installs through Developer
Mode. Samsung remains a development preview.
