# MediaCove Releases

This repository contains official MediaCove binary releases. The application
source repository is private and is not mirrored here.

## Public Beta

MediaCove is currently a free public beta. No payment method is required and
users are not charged automatically.

MediaCove streams video files you already own from a Windows computer to an LG
webOS TV on the same home network. It supports recursive folders, TV search,
resume and Up next, audio and subtitle selection, direct play with local FFmpeg
fallback, and optional local automatic captions. MediaCove does not provide a
movie catalog or upload your library to the cloud.

Download the current Windows installer from the
[Releases](https://github.com/Tomotz/MediaCove-Releases/releases) page. The LG
TV package is also archived for temporary Developer Mode testing. It is not yet
available as a permanent LG Content Store installation.

## Requirements

- A 64-bit Windows 10 or Windows 11 computer.
- An LG webOS TV on the same trusted household network.
- FFmpeg and ffprobe installed separately for media inspection, transcoding,
  embedded subtitles, and automatic captions.
- Media files that you own or are authorized to use.

The Windows beta installer is currently unsigned, so Windows SmartScreen may
display an unknown-publisher warning.

## Complete Setup

The Windows server and LG TV app are both required. The canonical setup page
keeps both download locations and their current installation status together:

<https://mediacove-entitlements.mediacove.workers.dev/download/>

After installing both apps, open `http://localhost:32480` on Windows, select
**Server Control**, and enter its five-character pairing code on the TV.

## Beta Account

Create one free household account in the Windows dashboard and save the recovery
key. No email or payment method is required. The computer stays authenticated
across restarts and refreshes access automatically; users do not sign in every
week. The recovery key is needed when reinstalling or moving access to another
computer.

Free beta access is temporary. MediaCove will provide at least 14 days' notice
before a planned beta-wide cutoff. One household can run one active Windows
server and pair multiple TVs.

## Verify Downloads

Each release includes `SHA256SUMS.txt`. In PowerShell, compare a download with
the published value:

```powershell
Get-FileHash .\MediaCove-Server-1.0.1-Windows-x64.exe -Algorithm SHA256
```

MediaCove does not bundle movies, subtitle catalogs, FFmpeg, private entitlement
keys, or administrator credentials.

## Support And Policies

- Support: [mediacove.support@gmail.com](mailto:mediacove.support@gmail.com)
- [Privacy policy](https://mediacove-entitlements.mediacove.workers.dev/privacy/)
- [Free beta terms](https://mediacove-entitlements.mediacove.workers.dev/terms/)
- [Account deletion](https://mediacove-entitlements.mediacove.workers.dev/deletion/)
- [Security reporting](https://mediacove-entitlements.mediacove.workers.dev/security/)
