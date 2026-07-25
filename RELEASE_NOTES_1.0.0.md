# MediaCove 1.0.0

MediaCove 1.0.0 is the first public beta candidate.

## Included

- `MediaCove-Server-1.0.0-Windows-x64.exe`: Windows server installer.
- `MediaCove-TV-1.0.0.ipk`: LG webOS TV companion package.
- `mediacove-1.0.0.cdx.json`: CycloneDX software bill of materials.
- `THIRD_PARTY_NOTICES-1.0.0.md`: dependency license notices.
- `SHA256SUMS.txt`: integrity hashes for all release artifacts.

The LG submission ZIP is retained for Seller Lounge review and is not needed by
normal Windows users.

## Beta Notes

- The beta is free and requires no payment method.
- One household account may activate one Windows server and pair multiple TVs.
- The computer and TV must be on the same trusted household network.
- FFmpeg and ffprobe are installed separately when transcoding, media inspection,
  embedded subtitles, or automatic captions are needed.
- The Windows installer is unsigned, so SmartScreen may display an
  unknown-publisher warning.
- Permanent LG TV installation depends on LG Seller Lounge approval. Developer
  Mode sideloading remains temporary.

## Verification

The release passed strict type checks, automated tests, production builds, TV
packaging, store-image validation, clean Windows install/uninstall, production
entitlement registration, signed lease verification, and revocation testing.
