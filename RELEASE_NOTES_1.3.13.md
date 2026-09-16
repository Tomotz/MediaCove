# MediaCove 1.3.13 public beta

## Changes

- Restore the automatic-caption VAD timing correction removed in 1.3.9. Regenerate existing tracks to use the restored pipeline; transcription wording improvement is not claimed.
- Stop indefinite TV library loading when the computer is unreachable. Show Server unavailable with Retry after the connection timeout, bound discovery, and prevent stale requests from overwriting a successful retry.
- Include the household-tested Library Options menu, automatic Up Next cache selection, and capacity-checked LRU eviction previously applied locally to 1.3.12. Google cache delivery remains experimental and the TV still requires the LAN server.

## Install

Update both the computer server and LG TV app using the complete [customer guide](https://mediacove-entitlements.mediacove.workers.dev/download/). The web player is included in the server update. Existing media, settings, paired clients, and viewing progress are preserved.

The Windows installer is unsigned. The Linux/macOS portable server requires Node.js 22.x, FFmpeg, and ffprobe. Samsung remains a development preview; no Samsung customer package is published.
