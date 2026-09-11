# MediaCove 1.3.11 public beta

Includes the installed Google Drive cache controls introduced in 1.3.10 and
corrects a resumable-upload compatibility issue found during live verification.

## Changes

- Handle Google's HTTP 308 upload acknowledgements without following redirects. This fixes uploads that failed before transferring any bytes.
- Manage the Google account, total storage budget (8 GB default), and individual episode copies under **Server Control → Google Drive cache**.
- Preserve original video bytes up to 5 GB per file and one default subtitle. Resume interrupted uploads and expose retry, removal, and browser preview controls.
- Keep Google authorization encrypted on the computer and preserve existing Windows credentials with native DPAPI.

Google caching remains optional and experimental. Google can block direct
playback; the 5 GB file limit is not a delivery guarantee. Automatic TV cloud
fallback is not included. Normal LAN playback works without Google.

## Install

Update the server and LG TV together. The cache panel appears on the regular
server dashboard under Server Control. Google setup happens on the computer;
no Google login is required on the TV. Existing library and progress are preserved.

Use the complete customer guide:
<https://mediacove-entitlements.mediacove.workers.dev/download/>

The Windows installer is currently unsigned. Linux and macOS use the portable
archive and require Node.js 22.x plus FFmpeg and ffprobe. Store uploads remain
subject to the manual LG and Samsung release checklist.
