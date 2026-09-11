# Google Drive cache controls

MediaCove 1.3.10 adds experimental cache controls to the installed server.
Google connection is optional and starts disconnected. Normal LAN playback
continues without Google or an internet connection.

## Server Control

Open **Server Control → Google Drive cache** on the server dashboard.

- **Google account:** connect, reconnect, disconnect, and refresh the account's
  Drive storage usage. Connection setup is available only on the server computer.
- **Total cache budget:** defaults to 8 GB for the entire household. Queued,
  uploaded, blocked, and pending-removal copies reserve their complete size.
  Remove entries before lowering the budget below that reservation.
- **Maximum video size:** 5,000,000,000 bytes per video. Videos retain their
  original bytes; no automatic compression or smaller rendition is created.
- **Subtitle preference:** automatic uses the default text track, then the first
  available text track. A preferred language is used when available. One WebVTT
  companion is uploaded and counted in the budget. Image-only subtitles require
  a text subtitle before the episode can be cached.
- **Add episodes:** search the server's indexed library and select individual
  episodes. Uploads run one at a time and resume after a server restart.
- **Pause uploads:** preserves queued jobs and existing cloud copies. Ready
  copies remain available for preview while uploads are paused.
- **Retry:** explicitly retry a failed upload or a Google-blocked playback check.
- **Remove from cache:** cancels an active resumable upload and removes the
  MediaCove-owned video and subtitle copies from Drive. Original media is never
  deleted. Failed removal remains visible and continues to reserve capacity.
- **Play cloud copy:** opens a browser preview using the public Drive video and
  its default subtitle. The browser must support the original codecs.

The existing TV playback path remains LAN-first. These server controls do not
yet supply a persistent cloud catalog or automatic cloud fallback to the TV;
the previously released TV still needs the computer for browsing and playback.
Do not present the browser preview or earlier TV benchmarks as a shipped
computer-off playback feature.

## One-time Google setup

The optional cache uses a desktop OAuth client with the `drive.file` scope and
a project API key restricted to Google Drive API. The advanced **Google app
setup** form accepts a downloaded desktop-client JSON or the client ID and
secret. The credentials are encrypted in the server data directory, separate
from the library and normal server configuration. They are never returned by
the management status API or sent to a TV.

Google consent happens in a browser on the server computer. The callback uses a
temporary loopback listener, PKCE, and a single-use state value. Google tokens
renew on the server. An external OAuth app left in Testing has a seven-day
refresh-token lifetime for Drive access; move the app to Production before
claiming that weekly consent is unnecessary. Token revocation and Google
account policy can still require reconnection.

The cache folder stays private. Individual completed video and subtitle copies
are shared as **Anyone with the link / Viewer**, without discovery permission,
so direct playback uses no Google user login. Disconnecting the account removes
the server's refresh token but leaves uploaded copies and their public links
in place. Remove those copies before disconnecting if they should be deleted.
To switch accounts, first remove the previous account's managed cache entries.

Earlier uploads made through the Drive website are not automatically imported
into this managed cache. MediaCove only deletes files whose private application
properties identify both this installation and the selected media item.

## Readiness and integrity

Uploads use resumable 8 MiB chunks and persist their session addresses encrypted.
MediaCove compares the original file's MD5 with Google's uploaded-file checksum,
then verifies anonymous range bytes at the start and end and, for large videos,
beyond 4 GiB. The default subtitle has the same upload and playback checks.

If Google rejects anonymous access or returns something other than the requested
bytes, the entry becomes **Saved · Google playback blocked**, not Ready. Google
traffic restrictions are not bypassed. Original LAN playback remains available.
A 5 GB application limit does not guarantee that Google will serve every file
or sustain unattended playback under its quotas and traffic controls.

## Validation

Automated coverage includes original-byte integrity, paired subtitle removal,
full-size budget reservations, the 5 GB boundary, offsets beyond 4 GiB, canceled
sessions, resume across restart, ownership checks, OAuth state validation,
account-switch protection, management authorization, and responsive controls.
The Windows credential implementation uses native current-user DPAPI and reads
the previous .NET DPAPI format. Linux/macOS retain their existing encrypted
credential store.

Live Google connection, upload/cancellation, subtitle preview, and restart tests
remain required before claiming unattended cloud reliability. Repeat large-file TV tests only
after the managed cloud catalog and fallback are integrated.

References: [desktop OAuth](https://developers.google.com/identity/protocols/oauth2/native-app),
[refresh-token lifetimes](https://developers.google.com/identity/protocols/oauth2),
[Drive resumable uploads](https://developers.google.com/workspace/drive/api/guides/manage-uploads),
[sharing permissions](https://developers.google.com/workspace/drive/api/guides/manage-sharing),
[application properties](https://developers.google.com/workspace/drive/api/guides/properties).
