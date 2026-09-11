# MediaCove 1.3.9 public beta

Includes the changes prepared after the public 1.3.7 release.

## Changes

- Show the episode preparation cover until playback begins, including when switching episodes.
- Use the same cross-folder episode ordering for Up next and the Next episode button.
- Remove TV episode downloads, offline libraries, and related storage code. Playback continues over the LAN.
- Preserve generated subtitle segment timing and require confirmed file-hash matches in subtitle search.
- Automatically discover and connect new TVs to passwordless LAN servers, with connection recovery controls when needed.
- Offer server updates only when the verified release channel has an available update.
- Update the server, web player, and LG TV application together. Google Drive caching remains an experiment and is not included.

## Install

Use the complete customer guide:
<https://mediacove-entitlements.mediacove.workers.dev/download/>

The Windows installer is currently unsigned. Linux and macOS use the portable
archive and require Node.js 22.x plus FFmpeg and ffprobe. Store uploads remain
subject to the manual LG and Samsung release checklist.
