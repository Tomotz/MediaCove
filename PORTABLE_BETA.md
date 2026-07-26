# Linux and macOS portable beta

The MediaCove portable server supports x64 and ARM64 Linux and macOS. It uses the
same direct-play-first LAN server and LG TV client as the Windows release.

## Install

1. Install [Node.js 22.x](https://nodejs.org/en/download/archive/v22), FFmpeg,
   and ffprobe. Other Node.js major versions are not supported. On macOS,
   install and activate Homebrew's versioned, keg-only Node.js formula:

   ```sh
   brew install node@22 ffmpeg
   echo 'export PATH="$(brew --prefix node@22)/bin:$PATH"' >> ~/.zprofile
   export PATH="$(brew --prefix node@22)/bin:$PATH"
   node --version
   ```

   `node --version` must begin with `v22.`. Do not use `brew install node`,
   because the unversioned formula may install an unsupported newer major.
2. Download the
   [portable archive](https://github.com/Tomotz/MediaCove/releases/download/v1.0.3/MediaCove-Server-1.0.3-Linux-macOS-Portable.tar.gz)
   and its
   [checksum](https://github.com/Tomotz/MediaCove/releases/download/v1.0.3/MediaCove-Server-1.0.3-Linux-macOS-Portable.tar.gz.sha256).
3. Verify the archive on Linux:

   ```sh
   sha256sum -c MediaCove-Server-1.0.3-Linux-macOS-Portable.tar.gz.sha256
   ```

   Or on macOS:

   ```sh
   shasum -a 256 -c MediaCove-Server-1.0.3-Linux-macOS-Portable.tar.gz.sha256
   ```

4. Extract the archive into a permanent directory:

   ```sh
   tar -xzf MediaCove-Server-1.0.3-Linux-macOS-Portable.tar.gz
   cd MediaCove-Server-1.0.3-Linux-macOS-Portable
   ```

5. In that directory, run:

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

For firewall examples, server configuration, LG TV installation, and pairing,
use the [complete installation guide](https://mediacove-entitlements.mediacove.workers.dev/download/).

## Canonical locations

- Public downloads: <https://github.com/Tomotz/MediaCove>
- Private source: <https://github.com/Tomotz/MediaCove-Private>
- Setup guide: <https://mediacove-entitlements.mediacove.workers.dev/download/>
