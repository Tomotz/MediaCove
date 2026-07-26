# MediaCove portable server for Linux and macOS

This package runs the MediaCove server on x64 or ARM64 Linux and macOS. It
installs Node dependencies on the destination computer so the native SQLite
module matches that operating system and CPU.

## Requirements

- Node.js 22.x, including npm; other Node.js major versions are not supported
- FFmpeg and ffprobe on `PATH`
- A private LAN shared with the LG webOS TV

On macOS, install Homebrew's versioned Node.js formula and put its keg-only
binary directory first on `PATH`:

```sh
brew install node@22 ffmpeg
echo 'export PATH="$(brew --prefix node@22)/bin:$PATH"' >> ~/.zprofile
export PATH="$(brew --prefix node@22)/bin:$PATH"
node --version
```

Keep the extracted directory in a permanent location, then run:

```sh
npm install --omit=dev
npm run captions:install
npm run start
```

The caption step is optional. On macOS it installs Homebrew's official
`whisper-cpp` formula when needed and downloads MediaCove's verified Turbo,
Small, and Silero models. Models are stored under
`~/Library/Application Support/MediaCove/whisper`. On Linux, install a
compatible `whisper-cli` on `PATH` before running the helper.
If Server Control still reports **Not installed**, run
`npm run captions:install -- --check` to verify every required path and model
hash.

Before extraction, compare the archive with its `.sha256` companion:

```sh
sha256sum -c MediaCove-Server-*-Linux-macOS-Portable.tar.gz.sha256
```

Open `http://localhost:32480` on the server computer. On macOS, **Browse** opens
the native folder chooser. On Linux, type the absolute media path and select
**Add folder**.

The first server run enables current-user autostart by default. Use
**Server Control > Start with computer** to enable or disable it. Disabling
autostart does not stop the current process. Start the server manually at any
time by opening a terminal in this extracted directory and running:

```sh
npm run start
```

The equivalent command-line autostart helper is:

```sh
sh scripts/install-unix-service.sh
```

The helper installs a user systemd service and MediaCove application-menu entry
on Linux. On macOS it installs a LaunchAgent and creates
`~/Applications/MediaCove.app`, which can be dragged to the Dock. It does not
require root access. On Linux, right-click the application entry and select
**Stop MediaCove Server** to stop it. Remove automatic startup and the launcher
without deleting library data with:

```sh
sh scripts/remove-unix-service.sh
```

MediaCove defaults to `~/.local/share/mediacove` on Linux and
`~/Library/Application Support/MediaCove` on macOS. Set `MEDIACOVE_DATA_DIR`
before the first start to choose another location.

Allow TCP port `32480` only from the TV or trusted private LAN. Never expose
that port through router port forwarding. Automatic-caption binaries and models
are not bundled; install them with `npm run captions:install`.

## Canonical locations

- Public downloads: <https://github.com/Tomotz/MediaCove>
- Private source: <https://github.com/Tomotz/MediaCove-Private>
- Setup guide: <https://mediacove-entitlements.mediacove.workers.dev/download/>

Release and installation changes must be kept consistent across all three
locations.
