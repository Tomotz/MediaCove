# Google Drive cache controls

MediaCove 1.3.14 places experimental cache controls in Google settings.
Google connection is optional and starts disconnected. Normal LAN playback
continues without Google or an internet connection.

## Server Control

Open **Server Control → Settings → Google settings** on the server dashboard.

- **Google account:** connect or disconnect the account. **Sign in again** appears only when
  authorization has expired or been revoked. Drive storage usage updates
  automatically while the dialog is open. Connection setup is available only
  on the server computer.
- **Total cache budget:** defaults to 8 GB for the entire household. Queued,
  uploaded, blocked, and pending-removal copies reserve their complete size.
  Remove entries before lowering the budget below that reservation.
- **Maximum video size:** 5,000,000,000 bytes per video. Videos retain their
  original bytes; no automatic compression or smaller rendition is created.
- **Subtitle preference:** automatic uses the default text track, then the first
  available text track. A preferred language is used when available. One WebVTT
  companion is uploaded and counted in the budget. Image-only subtitles require
  a text subtitle before the episode can be cached.
- **Add episodes:** open **Media → Library** and choose **Options → Cache episode** on
  an episode row. Its button then shows the cache status; select it to open
  Google settings. Uploads run one at a time and resume after a server restart.
- **Pause caching:** stops automatic selection and uploads, preserving queued jobs and existing cloud copies. Ready
  copies remain available for preview while uploads are paused.
- **Retry:** explicitly retry a failed upload or a Google-blocked playback check.
- **Remove from cache:** cancels an active resumable upload and removes the
  MediaCove-owned video and subtitle copies from Drive. Original media is never
  deleted. Failed removal remains visible and continues to reserve capacity.
- **Play cloud copy:** opens a browser preview using the public Drive video and
  its default subtitle. The browser must support the original codecs.

The TV playback path remains LAN-first. Update both the server and TV, then
connect once while the computer is running. The TV saves a catalog of ready
public video links and the selected subtitle link. If the computer becomes
unavailable, **Cached episodes** lets you browse and stream those copies
directly from Google, including after relaunch. Internet is required. Only
metadata and links are saved locally; no video bodies are downloaded into TV
storage. Reconnection runs automatically about every 15 seconds after a
failed connection and uploads pending viewing progress before refreshing the
online library. The catalog refreshes while connected, without contacting
Google in the normal LAN playback path. Removed or evicted cloud copies may
remain in a disconnected TV catalog until the next successful synchronization.
Original codecs must work on the TV; the unavailable computer cannot transcode.
Google can still block delivery, so unattended reliability remains experimental.

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

Live household verification passed for encrypted connection across restart,
an unchanged 8,150,964-byte video plus its English subtitle, anonymous range
checks, browser playback, and paired removal with the source preserved.
Long-running upload cancellation and unattended cloud reliability remain
experimental. The 1.3.14 candidate physically streamed the small cached video
and its subtitle on LG with the computer server stopped, retained its catalog
and resume progress after relaunch, then reconciled progress when the server
returned. Large-file, long-duration TV delivery remains unverified.

References: [desktop OAuth](https://developers.google.com/identity/protocols/oauth2/native-app),
[refresh-token lifetimes](https://developers.google.com/identity/protocols/oauth2),
[Drive resumable uploads](https://developers.google.com/workspace/drive/api/guides/manage-uploads),
[sharing permissions](https://developers.google.com/workspace/drive/api/guides/manage-sharing),
[application properties](https://developers.google.com/workspace/drive/api/guides/properties).

## Automatic selection and LRU (current source / household update)

Automatic caching uses the same shared Up Next selector as the TV library,
using household viewing progress. It checks about every 30 seconds while caching
is enabled. Device-specific Up Next dismissals do not hide episodes from the
shared household cache. Removed library entries are excluded.

Up Next considers the most recently played clip in each folder. It offers resume
after at least one minute of viewing when at least five minutes remain. It also
offers the next clip when the current clip is complete or has at most ten minutes
remaining after viewing has begun. The next clip must be unfinished and have
older viewing activity than the current clip. Candidates are ordered by most
recent triggering activity. The next-episode function sorts titles naturally
(2 before 10), then crosses into the next naturally sorted sibling folder when
a folder ends; it does not infer series relationships from metadata.

The cache processes that priority order, skipping episodes that cannot fit.
Higher-priority selected episodes are protected so an oversized candidate list
does not cause repeated upload/eviction cycles. Other copies are evicted least
recently used first, only as needed. Usage timestamps persist across restarts
and update on MediaCove cloud-playback requests and normal viewing-progress
reports. Direct public-link playback outside MediaCove cannot report usage.
Newly queued episodes count as recently used; legacy entries fall back to their
existing insertion order.

Before eviction, the original video must be within the 5 GB file limit and its
actual video-plus-selected-subtitle size must fit the entire cache budget.
MediaCove prepares and validates the subtitle before deleting anything. It also
plans enough reclaimable space in advance; active uploads and protected selected
episodes are excluded. If these checks fail, nothing is deleted. Video and
subtitle cache copies are removed together; original library files stay intact.
Google deletion/upload failures can still interrupt a replacement and are
reported rather than hidden.

Manually removing a cached episode suppresses automatic re-addition until newer
viewing activity makes it eligible again. A manual Cache episode action can
explicitly add it sooner. The settings dialog lists skipped candidates and reasons.
