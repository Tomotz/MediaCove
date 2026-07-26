# Security Policy

## Supported Version

Security fixes are provided for the newest MediaCove public-beta release.
Windows, Linux, and macOS servers are supported; the Linux/macOS package remains
a portable beta rather than a signed native installer.

## Reporting

Email [mediacove.support@gmail.com](mailto:mediacove.support@gmail.com) with the
subject `MediaCove security report`. Include the affected version and
reproduction steps, but do not send recovery keys, account tokens, private media,
or full folder paths.

Avoid publishing exploit details while the issue is being assessed. Receipt will
be acknowledged when practical. The public beta does not currently offer a bug
bounty.

Keep MediaCove on a trusted private LAN and never expose TCP port `32480`
through router port forwarding. Account tokens use current-user DPAPI on Windows
and AES-256-GCM with a separate owner-readable local key on Linux and macOS.
