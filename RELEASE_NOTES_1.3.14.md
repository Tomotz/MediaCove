# MediaCove 1.3.14 public beta

## Changes

- Fix automatic TV connection when an old incompatible MediaCove server is also on the LAN: skip protocol-v1 installations instead of stopping at their missing connection endpoint.
- Recover saved servers after HTTP failures and DHCP changes using the saved installation identity and bearer authorization; remember stable hostname/interface hints and sweep the full saved subnet. Retry automatically after an outage and when network connectivity returns, without erasing saved credentials on an HTTP error.
- Save a small catalog of ready Google Drive copies and preferred subtitle links on the TV. If the computer is unavailable, browse Cached episodes and stream directly from Google, including after app relaunch. Store viewing progress locally and reconcile it when the computer returns. No episode bodies are downloaded into TV browser storage; normal LAN playback remains independent of Google and internet access.

## Limits

Cloud copies require internet and TV support for the original codecs. Google may block public delivery. A disconnected TV can retain an evicted link until its next catalog synchronization. Near-5 GB, long-duration provider reliability and the oldest webOS physical baseline remain unverified. This update does not guarantee that every provider request succeeds.

## Install

Update both the computer server and LG TV app, then connect once while the computer is running to synchronize ready cloud-copy links. Use the complete [customer guide](https://mediacove-entitlements.mediacove.workers.dev/download/). Existing media, settings, paired clients, encrypted Google connection, and viewing progress are preserved.

The Windows installer is unsigned. The Linux/macOS portable server requires Node.js 22.x, FFmpeg, and ffprobe. Samsung remains a development preview; no Samsung customer package is published.
