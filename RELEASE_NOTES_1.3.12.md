# MediaCove 1.3.12 public beta

## Changes

- Google Drive controls now open from Server Control → Settings → Google settings.
- Healthy Google connections show the account and Disconnect. Sign in again appears only after authorization expires or is revoked, and disappears after successful consent.
- Drive storage usage updates automatically while Google settings are open.
- Cache episode buttons now live in Media → Library. Queued and cached status buttons open the cache controls; the duplicate episode picker is removed.
- Cache limits, original-quality uploads, preferred subtitles, pause, retry, and paired removal remain available in Google settings.

Google caching is optional. Normal LAN playback remains available without Google.
The TV still uses the server for browsing and playback; automatic TV cloud
fallback and unattended cloud reliability remain experimental.

## Install

Update the computer server to see the revised controls. The web player is included.
Existing Google credentials, cache settings, media folders, and viewing progress
are preserved. Matching LG Developer Mode packages are included in this release.

Use the [download and setup guide](https://mediacove-entitlements.mediacove.workers.dev/download/).
The Windows installer is unsigned. Linux/macOS portable packages require Node.js
22.x, FFmpeg, and ffprobe. Samsung remains an emulator-tested development preview.
