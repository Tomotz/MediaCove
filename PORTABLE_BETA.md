# Linux and macOS portable beta

The MediaCove portable server supports x64 and ARM64 Linux and macOS. It uses the
same direct-play-first LAN server and LG TV client as the Windows release.

## Install

1. Install Node.js 20 or newer, FFmpeg, and ffprobe.
2. Download the `.tar.gz` archive and matching `.sha256` file from Releases.
3. Verify and extract the archive into a permanent directory.
4. In that directory, run:

```sh
npm install --omit=dev
npm run start
```

Open `http://localhost:32480` on the server computer. macOS provides a native
folder chooser; on Linux, enter an absolute media-folder path.

Install automatic startup for the current user with:

```sh
sh scripts/install-unix-service.sh
```

This creates a user systemd service on Linux or a LaunchAgent on macOS. Remove
it without deleting library data with:

```sh
sh scripts/remove-unix-service.sh
```

The default data locations are `~/.local/share/mediacove` on Linux and
`~/Library/Application Support/MediaCove` on macOS. Set
`MEDIACOVE_DATA_DIR` before first start to override the location.

Native signed installers and automated Whisper model installation are not part
of this initial portable beta. Compatible local automatic-caption tools can be
configured with the `MEDIACOVE_WHISPER_*` environment variables.
