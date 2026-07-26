# MediaCove

MediaCove streams video files you already own from a Windows computer to an LG
webOS TV on the same home network. 

## Features

 - Video streaming from windows PC to LG TV - Easily share your full media library with your TV
 - Automatic English caption generation (powered by whisperAI models)
 - Automatic same language transcription
 - Automatic caption download from OpenSubtitles.org

This repository contains official MediaCove binary releases. The application
source repository is private and is not mirrored here.

## Public Beta

MediaCove is currently a free public beta. No payment method is required and
users are not charged automatically.

Download the current Windows installer from the
[Releases](https://github.com/Tomotz/MediaCove/releases) page. The LG
TV package is also archived for temporary Developer Mode testing. It is not yet
available as a permanent LG Content Store installation.

## Requirements

- A 64-bit Windows 10 or Windows 11 computer.
- An LG TV with webOS support on the same trusted household network.

The Windows installer includes its Node.js runtime and offers to install FFmpeg
and ffprobe automatically through Microsoft WinGet. FFmpeg remains a separate
GPL-3.0 package, but customers do not need to find or configure it themselves.

The Windows beta installer is currently unsigned, so Windows SmartScreen may
display an unknown-publisher warning.

## Complete Setup

The Windows server and LG TV app are both required. The canonical setup page
keeps both download locations and their current installation status together:

<https://mediacove-entitlements.mediacove.workers.dev/download/>

After installing both apps, open `http://localhost:32480` on Windows, navigate to
**Server Control**, and enter the five-character pairing code on the TV.

## Beta Account

Create one free household account in the Windows dashboard and save the recovery
key. No email or payment method is required. The computer stays authenticated
across restarts and refreshes access automatically. The recovery key is needed when 
reinstalling or moving access to another computer.

Free beta access is temporary. MediaCove will provide at least 14 days' notice
before a planned beta-wide cutoff. One household can run one active Windows
server and pair multiple TVs. MediaCove can revoke an account or end beta access
centrally. New playback then stops at the next successful online check or no
later than the current signed lease's seven-day expiry. This offline window is
not a weekly login prompt.

## Verify Downloads

Each release includes `SHA256SUMS.txt`. In PowerShell, compare a download with
the published value:

```powershell
Get-FileHash .\MediaCove-Server-1.0.3-Windows-x64.exe -Algorithm SHA256
```

## Support And Policies

- Support: [mediacove.support@gmail.com](mailto:mediacove.support@gmail.com)
- [Privacy policy](https://mediacove-entitlements.mediacove.workers.dev/privacy/)
- [Free beta terms](https://mediacove-entitlements.mediacove.workers.dev/terms/)
- [Account deletion](https://mediacove-entitlements.mediacove.workers.dev/deletion/)
- [Security reporting](https://mediacove-entitlements.mediacove.workers.dev/security/)
