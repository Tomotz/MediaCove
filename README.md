# MediaCove

MediaCove streams video files you already own from a Windows, Linux, or macOS
computer to a television on the same home network. LG webOS is the current
downloadable TV target. Samsung Tizen is available as an emulator-tested
development preview, not yet as a public Samsung TV package.

## Features

- Video streaming from a computer to an LG TV on the same private LAN
- Automatic English caption generation with local Whisper models
- Automatic same-language transcription
- Subtitle search and download from OpenSubtitles.org

This repository contains official MediaCove binary releases. The application
source repository is private and is not mirrored here.

## Public Beta

MediaCove is currently a free public beta. No payment method is required and
users are not charged automatically.

MediaCove 1.0.5 has one complete
[release page](https://github.com/Tomotz/MediaCove/releases/tag/v1.0.5) for all
platforms:

- [Windows x64 installer](https://github.com/Tomotz/MediaCove/releases/download/v1.0.5/MediaCove-Server-1.0.5-Windows-x64.exe)
- [Linux/macOS x64/ARM64 portable server](https://github.com/Tomotz/MediaCove/releases/download/v1.0.5/MediaCove-Server-1.0.5-Linux-macOS-Portable.tar.gz)
- [Portable-server checksum](https://github.com/Tomotz/MediaCove/releases/download/v1.0.5/MediaCove-Server-1.0.5-Linux-macOS-Portable.tar.gz.sha256)
- [LG webOS Developer Mode package](https://github.com/Tomotz/MediaCove/releases/download/v1.0.5/MediaCove-TV-1.0.5.ipk)
- [Complete SHA-256 manifest](https://github.com/Tomotz/MediaCove/releases/download/v1.0.5/SHA256SUMS.txt)
- [MediaCove 1.0.5 release notes](RELEASE_NOTES_1.0.5.md)

The TV package is for temporary Developer Mode testing. It is not yet available
as a permanent LG Content Store installation.

Samsung Tizen support currently includes the shared TV client, remote handling,
root exit confirmation, lifecycle handling, WGT signing, installation, launch,
and basic H.264/AAC HLS playback verified in Samsung's Tizen 10 TV emulator.
There is no public Samsung WGT or physical-TV support claim yet. See
[Samsung Tizen development preview](SAMSUNG_PREVIEW.md).

## Canonical Project Locations

- Public downloads and releases: <https://github.com/Tomotz/MediaCove>
- Private source repository: <https://github.com/Tomotz/MediaCove-Private>
- Customer download and setup guide: <https://mediacove-entitlements.mediacove.workers.dev/download/>

The source repository is private and is available only to authorized
collaborators. Release and installation changes are kept consistent across all
three locations.

## Requirements

- A 64-bit Windows 10/11 computer, or an x64/ARM64 Linux or macOS computer.
- An LG TV with webOS support on the same trusted household network for the
  public TV package.
- For the Samsung development preview, an authorized source checkout, Tizen
  Studio with the Samsung TV Extension, a Samsung certificate profile, and a
  Tizen TV emulator.

The Windows installer includes its Node.js runtime and offers to install FFmpeg
and ffprobe automatically through Microsoft WinGet. FFmpeg remains a separate
GPL-3.0 package, but customers do not need to find or configure it themselves.

The portable Linux/macOS server requires Node.js 22.x, npm, FFmpeg, and ffprobe.
Other Node.js major versions are not supported. On macOS, use Homebrew's
versioned formula because the unversioned `node` formula may install an
unsupported newer major:

```sh
brew install node@22 ffmpeg
echo 'export PATH="$(brew --prefix node@22)/bin:$PATH"' >> ~/.zprofile
export PATH="$(brew --prefix node@22)/bin:$PATH"
node --version
```

The Windows beta installer is currently unsigned, so Windows SmartScreen may
display an unknown-publisher warning.

After installation, Windows keeps a MediaCove icon in the notification area.
Double-click it to open Server Control, or right-click it for
**Open MediaCove** and **Exit MediaCove**. Exit stops the server as well as the
icon. The icon may initially appear under the taskbar's hidden-icons arrow.

Extract the portable beta to a permanent directory, run
`npm install --omit=dev`, then `npm run start`. It includes current-user systemd
and launchd startup helpers. Linux gets an application-menu entry with Open and
Stop actions; macOS gets `~/Applications/MediaCove.app`, which can be dragged
to the Dock. Native signed Linux/macOS installers and bundled automatic-caption
models are not yet available. See
[PORTABLE_BETA.md](PORTABLE_BETA.md) for complete setup.

## Complete Setup

The computer server and a TV client are both required. The canonical setup page
contains direct downloads and complete Windows, Linux, macOS, firewall, LG
Developer Mode, pairing, verification, and Samsung preview instructions:

<https://mediacove-entitlements.mediacove.workers.dev/download/>

After installing both apps, open `http://localhost:32480` on the computer, navigate to
**Server Control**, and enter the five-character pairing code on the TV.

If the library opens but playback reports that server access is inactive, open
**Server Control > MediaCove account**. Create free beta access and save its
recovery key, or connect with an existing key, then select **Refresh status**.
Library browsing can work while stream creation is blocked.

## Beta Account

Create one free household account in the computer dashboard and save the recovery
key. No email or payment method is required. The computer stays authenticated
across restarts and refreshes access automatically. The recovery key is needed when 
reinstalling or moving access to another computer.

Free beta access is temporary. MediaCove will provide at least 14 days' notice
before a planned beta-wide cutoff. One household can run one active MediaCove
server and pair multiple TVs. MediaCove can revoke an account or end beta access
centrally. New playback then stops at the next successful online check or no
later than the current signed lease's seven-day expiry. This offline window is
not a weekly login prompt.

## Verify Downloads

Each release includes SHA-256 integrity information. In PowerShell, compare the
Windows installer with the published value:

```powershell
Get-FileHash .\MediaCove-Server-1.0.5-Windows-x64.exe -Algorithm SHA256
```

On Linux, verify the portable archive with its companion file:

```sh
sha256sum -c MediaCove-Server-1.0.5-Linux-macOS-Portable.tar.gz.sha256
```

On macOS:

```sh
shasum -a 256 -c MediaCove-Server-1.0.5-Linux-macOS-Portable.tar.gz.sha256
```

## Support And Policies

- Support: [mediacove.support@gmail.com](mailto:mediacove.support@gmail.com)
- [Privacy policy](https://mediacove-entitlements.mediacove.workers.dev/privacy/)
- [Free beta terms](https://mediacove-entitlements.mediacove.workers.dev/terms/)
- [Account deletion](https://mediacove-entitlements.mediacove.workers.dev/deletion/)
- [Security reporting](https://mediacove-entitlements.mediacove.workers.dev/security/)
