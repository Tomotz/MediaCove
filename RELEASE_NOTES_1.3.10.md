# MediaCove 1.3.10 public beta

Adds the missing cache management panel to the installed server dashboard.

## Changes

- Add Google Drive account connection, storage usage, and a configurable household budget (8 GB by default) under Server Control.
- Add individual episode upload, pause, retry, removal, and browser preview. Preserve original video bytes up to 5 GB per file and one default-language subtitle companion.
- Resume interrupted uploads and verify checksums and anonymous byte ranges. Display Google-blocked copies explicitly and count queued or failed copies in the storage budget.
- Encrypt Google authorization on the server using native Windows DPAPI, preserving compatibility with existing encrypted credentials.

Google caching is optional and experimental. Google can block direct playback;
the 5 GB limit does not guarantee reliable delivery. Automatic TV cloud fallback
is not included. Normal LAN playback works without Google.

## Install

Update the computer server and LG TV package together. Open **Server Control →
Google Drive cache** on the regular server dashboard. Google setup happens on
the computer; the TV requires no Google login. Existing library configuration
and viewing progress are preserved.

Use the complete customer guide:
<https://mediacove-entitlements.mediacove.workers.dev/download/>

The Windows installer is currently unsigned. Linux and macOS use the portable
archive and require Node.js 22.x plus FFmpeg and ffprobe. Store uploads remain
subject to the manual LG and Samsung release checklist.
