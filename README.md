# MediaCove Releases

This repository contains official MediaCove binary releases. The application
source repository is private and is not mirrored here.

## Public Beta

MediaCove is currently a free public beta. No payment method is required and
users are not charged automatically.

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
Get-FileHash .\MediaCove-Server-1.0.0-Windows-x64.exe -Algorithm SHA256
```

MediaCove does not bundle movies, subtitle catalogs, FFmpeg, private entitlement
keys, or administrator credentials.

## Support And Policies

- Support: [mediacove.support@gmail.com](mailto:mediacove.support@gmail.com)
- [Privacy policy](https://mediacove-entitlements.mediacove.workers.dev/privacy/)
- [Free beta terms](https://mediacove-entitlements.mediacove.workers.dev/terms/)
- [Account deletion](https://mediacove-entitlements.mediacove.workers.dev/deletion/)
- [Security reporting](https://mediacove-entitlements.mediacove.workers.dev/security/)
