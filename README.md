# MediaCove

MediaCove streams video files you already own from a Windows, Linux, or macOS
computer to an LG webOS TV on the same home network.

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

Download the current Windows installer or Linux/macOS portable beta from the
[Releases](https://github.com/Tomotz/MediaCove/releases) page. The LG TV package
is also archived for temporary Developer Mode testing. It is not yet available
as a permanent LG Content Store installation.

## Requirements

- A 64-bit Windows 10/11 computer, or an x64/ARM64 Linux or macOS computer.
- An LG TV with webOS support on the same trusted household network.

The Windows installer includes its Node.js runtime and offers to install FFmpeg
and ffprobe automatically through Microsoft WinGet. FFmpeg remains a separate
GPL-3.0 package, but customers do not need to find or configure it themselves.

The Windows beta installer is currently unsigned, so Windows SmartScreen may
display an unknown-publisher warning.

The portable beta requires Node.js 20 or newer plus FFmpeg and ffprobe. Extract
it to a permanent directory, run `npm install --omit=dev`, then `npm run start`.
It includes current-user systemd and launchd startup helpers. Native signed
Linux/macOS installers and bundled automatic-caption models are not yet
available. See [PORTABLE_BETA.md](PORTABLE_BETA.md) for complete setup.

## Complete Setup

The computer server and LG TV app are both required. The canonical setup page
keeps both download locations and their current installation status together:

<https://mediacove-entitlements.mediacove.workers.dev/download/>

After installing both apps, open `http://localhost:32480` on the computer, navigate to
**Server Control**, and enter the five-character pairing code on the TV.

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
Get-FileHash .\MediaCove-Server-1.0.3-Windows-x64.exe -Algorithm SHA256
```

On Linux or macOS, verify the portable archive with its companion file:

```sh
sha256sum -c MediaCove-Server-*-Linux-macOS-Portable.tar.gz.sha256
```

## Support And Policies

- Support: [mediacove.support@gmail.com](mailto:mediacove.support@gmail.com)
- [Privacy policy](https://mediacove-entitlements.mediacove.workers.dev/privacy/)
- [Free beta terms](https://mediacove-entitlements.mediacove.workers.dev/terms/)
- [Account deletion](https://mediacove-entitlements.mediacove.workers.dev/deletion/)
- [Security reporting](https://mediacove-entitlements.mediacove.workers.dev/security/)
