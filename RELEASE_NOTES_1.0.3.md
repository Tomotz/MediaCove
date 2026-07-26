# MediaCove 1.0.3

MediaCove 1.0.3 supports Windows, Linux, and macOS server computers and
clarifies how free beta access works.

## Downloads

- [Windows x64 installer](https://github.com/Tomotz/MediaCove/releases/download/v1.0.3/MediaCove-Server-1.0.3-Windows-x64.exe)
- [Linux/macOS portable server](https://github.com/Tomotz/MediaCove/releases/download/v1.0.3/MediaCove-Server-1.0.3-Linux-macOS-Portable.tar.gz)
- [Portable-server checksum](https://github.com/Tomotz/MediaCove/releases/download/v1.0.3/MediaCove-Server-1.0.3-Linux-macOS-Portable.tar.gz.sha256)
- [LG webOS Developer Mode package](https://github.com/Tomotz/MediaCove/releases/download/v1.0.3/MediaCove-TV-1.0.3.ipk)
- [Complete SHA-256 manifest](https://github.com/Tomotz/MediaCove/releases/download/v1.0.3/SHA256SUMS.txt)

See the [complete installation guide](https://mediacove-entitlements.mediacove.workers.dev/download/)
for prerequisites, firewall setup, TV installation, and pairing.

## Changes

- The Windows installer offers to install FFmpeg and ffprobe automatically
  through Microsoft WinGet.
- Existing FFmpeg installations are detected and reused without another
  download.
- The installed launcher reliably locates WinGet-managed media tools even when
  the installer process has an older `PATH`.
- Linux and macOS use one portable archive that installs native Node
  dependencies for the destination CPU and operating system.
- Linux includes a current-user systemd helper; macOS includes a current-user
  LaunchAgent helper and native media-folder chooser.
- Account credentials use current-user DPAPI on Windows and AES-256-GCM with an
  owner-readable local key on Linux and macOS.
- Public documentation explains that registration is one-time, access refreshes
  automatically, and free beta access is centrally revocable.

FFmpeg remains separate software. Windows can install it through WinGet during
setup; Linux and macOS users install it with their platform package manager.
Native signed Unix installers and automated Whisper-model installation are not
included in this portable beta.
