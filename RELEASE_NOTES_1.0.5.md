# MediaCove 1.0.5 public beta

MediaCove 1.0.5 preserves the new platform launchers from 1.0.4 and completes
the release with multi-server TV testing support.

## Highlights

- Windows keeps a MediaCove icon in the notification area. Double-click it to
  open Server Control, or right-click for **Open MediaCove** and
  **Exit MediaCove**.
- **Start with computer** controls current-user autostart from Server Control on
  Windows, Linux, and macOS without stopping the current server.
- Linux service installation adds a desktop application entry with Open and
  Stop actions. macOS service installation creates
  `~/Applications/MediaCove.app`, which can live in the Dock.
- Windows checks the official beta channel and offers a visible,
  SHA-256-verified installer update. Unsigned beta installation is never silent.
- The portable server is pinned to Node.js 22.x and includes verified local
  automatic-caption setup for Linux and macOS.
- The TV can retain multiple test-server pairings and switch between them
  without revoking inactive servers.

## Install

Use the complete customer guide:
<https://mediacove-entitlements.mediacove.workers.dev/download/>

The Windows installer is currently unsigned. Linux and macOS use the portable
archive and require Node.js 22.x plus FFmpeg and ffprobe. The LG IPK is a
temporary Developer Mode package until MediaCove is approved for LG Content
Store.
