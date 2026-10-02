# MediaCove Installation

This guide installs the MediaCove server on a computer and the client on an LG
webOS TV. It also documents the emulator-tested Samsung Tizen development
preview. The server and TV must be on the same private network.

For release 1.3.14, update both the computer server and LG TV app. The web player
is included in the server update; existing media folders and viewing progress
are preserved.

After updating both apps, connect the TV once while the computer is running to
synchronize ready cloud-copy links. Automatic discovery skips incompatible old
servers, and saved connections recover by installation identity after DHCP
changes. The TV retries automatically after an outage; Try again remains
available on the remote.

After updating, open **Server Control → Settings → Google settings** on the regular
server dashboard for account connection, the total storage budget, and saved
episodes. Add episodes using **Cache episode** in **Media → Library**. Google setup is optional and must be completed on the
server computer; see [cache setup](https://github.com/Tomotz/MediaCove/blob/main/docs/GOOGLE_DRIVE_CACHE.md). The TV continues
using LAN playback and does not require a Google login.

The public
[download and setup page](https://mediacove-entitlements.mediacove.workers.dev/download/)
contains direct current-release links and customer-focused instructions for
Windows, Linux, macOS, firewall setup, LG Developer Mode, and TV connection.

## Canonical project locations

- [Public downloads and releases](https://github.com/Tomotz/MediaCove)
- [Private source repository](https://github.com/Tomotz/MediaCove-Private)
- [Customer download and setup guide](https://mediacove-entitlements.mediacove.workers.dev/download/)

When release files or installation requirements change, update all three
locations together.

## Recommended Windows installer

Download `MediaCove-Server-*-Windows-x64.exe` from the current public release
and run it. The installer includes the required Node.js runtime. Its recommended
**Media tools** task also installs FFmpeg and ffprobe automatically through
Microsoft Windows Package Manager (WinGet), so customers do not need to find or
configure those tools themselves.

FFmpeg is installed as the separate GPL-3.0 `Gyan.FFmpeg` package and is not
embedded in MediaCove. If both commands are already available, setup detects
them, omits the FFmpeg information and task pages, and does not download them
again. Setup also omits its startup task when MediaCove is already registered
to start at sign-in. If WinGet is unavailable, update
Microsoft's **App Installer** and rerun MediaCove setup. The installation log is
stored at `%LOCALAPPDATA%\MediaCove\ffmpeg-install.log`.

After setup, MediaCove runs in the Windows notification area beside the clock.
Double-click its icon, or right-click it and select **Open MediaCove**, to open
Server Control. Right-click and select **Exit MediaCove** to stop the server and
close the icon. Open **MediaCove** from the Start menu to start both again.
Continue with **Configure the Windows Firewall** below, then add media in the
dashboard.

The source-build instructions in the next sections are for developers and are
not required when using the public Windows installer.

## Portable Linux and macOS beta

Download `MediaCove-Server-*-Linux-macOS-Portable.tar.gz` from the current
public release. Install Node.js 22.x plus FFmpeg/ffprobe with the
platform package manager, extract the archive to a permanent location, and run:

On macOS, Homebrew's unversioned `node` formula may install an unsupported
newer major version. Install and activate the versioned formula:

```sh
brew install node@22 ffmpeg
echo 'export PATH="$(brew --prefix node@22)/bin:$PATH"' >> ~/.zprofile
export PATH="$(brew --prefix node@22)/bin:$PATH"
node --version
```

```sh
npm install --omit=dev
case "$(uname -s)" in
  Darwin) launchctl bootout "gui/$(id -u)/com.mediacove.server" 2>/dev/null || true ;;
  Linux) systemctl --user stop mediacove.service 2>/dev/null || true ;;
esac
npm run start
```

The portable package contains compiled MediaCove server code, but installs the
native SQLite dependency on the destination computer so it matches Linux or
macOS and the computer's x64 or ARM64 CPU. It does not bundle Node.js, FFmpeg,
Whisper, or automatic-caption models. To enable automatic captions after
`npm install --omit=dev`, run:

```sh
npm run captions:install
```

On macOS, the helper installs Homebrew's official `whisper-cpp` formula when
needed, downloads and verifies MediaCove's Turbo, Small, and Silero models under
`~/Library/Application Support/MediaCove/whisper`, and restarts the LaunchAgent
when it is already installed. A manually started server must be stopped and
started again.

## Update MediaCove

Managed Windows, Linux, and macOS servers check the official public beta
releases shortly after startup and every six hours. Open
**Server Control > Software updates** to see the installed and latest versions,
check immediately, or install an available update.

Selecting **Download and install** downloads only the exact versioned platform
package from the official `Tomotz/MediaCove` GitHub release, limits its size,
and verifies its published SHA-256 digest. On Windows, the normal setup window
remains visible, stops the previous server, preserves
`%LOCALAPPDATA%\MediaCove`, and starts the updated server. The beta installer is
currently unsigned, so review the Windows publisher warning before continuing.

On Linux and macOS, a managed portable installation first extracts the archive
and runs `npm install --omit=dev` in a sibling staging directory. A detached
updater then stops the user service, swaps in the prepared directory, recreates
the launcher/service, and waits up to 60 seconds for a healthy server. If that
fails, it restores and restarts the previous directory. Details are written to
the platform data directory under `updates/update.log`. Keep
`MEDIACOVE_DATA_DIR` outside the extracted application directory; MediaCove
disables automatic replacement when its data would be moved with the program.
Source checkouts and unmanaged portable launches update manually.
Portable releases from before this updater was added require one final manual
archive replacement; later managed portable releases can update themselves.

For a store release, TV updates are delivered by the applicable TV store rather
than by the MediaCove server. An LG Developer Mode IPK or Samsung development
WGT must still be rebuilt, signed, and reinstalled manually. The TV app version
is listed in **Settings**.

MediaCove compares the client and server release and protocol versions during
both automatic discovery and direct-IP connection. Compatible version differences
can still connect. The TV offers **Update server** only when the server's
verified release check confirms an available update; a numeric mismatch alone
does not suggest an update. Windows may still require confirmation in its
visible installer; managed Linux and macOS portable installations finish
through their user service. When the TV client is behind, update it through the
TV store. Developer Mode and other sideloaded clients must be reinstalled.

## 1. Install the source-build prerequisites

Install:

1. Node.js 22 LTS. Other Node.js major versions are not supported.
2. FFmpeg, including both `ffmpeg` and `ffprobe` (the Windows installer handles
   this automatically).
3. Git, if the repository is not already present.

Open a new PowerShell or terminal window after installation and verify:

```powershell
node -v
npm -v
ffmpeg -version
ffprobe -version
```

If FFmpeg is installed but not on `PATH`, set `FFMPEG_PATH` and `FFPROBE_PATH` to the full executable paths before starting MediaCove.

## 2. Install and build MediaCove

From the repository directory:

```powershell
corepack enable
corepack prepare pnpm@9.15.5 --activate
pnpm install --frozen-lockfile
pnpm check:prereqs
pnpm build
```

On Windows, installation also recommends enabling automatic server startup. It is optional and is never enabled silently.

Some Node.js installations do not include a usable `corepack` command. If `corepack` or `pnpm` is not recognized, use npm:

```powershell
npm install -g pnpm@9.15.5
pnpm install --frozen-lockfile
pnpm check:prereqs
pnpm build
```

Reopen PowerShell if a newly installed command is still not found.

## 3. Configure the Windows Firewall

MediaCove listens on TCP port `32480` and advertises its `.local` hostname over
mDNS UDP port `5353`. Scoped inbound firewall rules are necessary for the TV
and hostname discovery, but router port forwarding is neither necessary nor
recommended.

Run an elevated PowerShell window and allow the local `192.168.0.*` subnet:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\scripts\allow-tv-firewall.ps1
```

The equivalent command is:

```powershell
New-NetFirewallRule `
  -DisplayName "MediaCove LAN 192.168.0.0/24" `
  -Direction Inbound `
  -Protocol TCP `
  -LocalPort 32480 `
  -RemoteAddress 192.168.0.0/24 `
  -Action Allow `
  -Profile Private

New-NetFirewallRule `
  -DisplayName "MediaCove discovery 192.168.0.0/24" `
  -Direction Inbound `
  -Protocol UDP `
  -LocalPort 5353 `
  -RemoteAddress 192.168.0.0/24 `
  -Action Allow `
  -Profile Private
```

This rule is limited to:

- inbound TCP `32480` for the application and UDP `5353` for `.local` discovery;
- the `192.168.0.0/24` private subnet;
- Windows networks marked **Private**.

It allows devices on `192.168.0.*` to reach MediaCove while remaining limited
to the Private network profile. Do not create a broad Public-network rule and
do not expose port `32480` through the router. If your home LAN uses another
subnet, pass it explicitly, for example `-Subnet 192.168.1.0/24`.

Every permitted device can reach the server. A new attached-LAN player receives
a client token without a credential when no management password is configured;
once the owner sets one, new clients must enter it.
The firewall subnet remains a security boundary: include only a trusted LAN,
never `Any`, and use Wi-Fi client isolation only if those isolated devices do
not need MediaCove.

## 4. Start and configure the server

Start MediaCove:

```powershell
.\scripts\start-server.ps1
```

The script checks Node.js, FFmpeg, and ffprobe before starting. The direct command is:

```powershell
pnpm start:server
```

Leave that PowerShell window running and open:

```text
http://localhost:32480
```

Use **Open player** in the server header, **Play** beside an indexed movie, or
open `http://localhost:32480/player/` to use MediaCove in a browser on the
server computer. The localhost player authorizes automatically. A new player
on the attached private LAN asks for the management password when one is
configured. A client arriving from outside the computer's attached local
subnets is rejected even if it submits the correct password.

### Connect phones and other computers

Open **Server Control > Connection addresses** to see the preferred management and player
links. The preferred name uses mDNS, such as `http://toms.local:32480/`; use one
of the displayed IP fallbacks when `.local` names are not supported on the
other device. A bare Windows computer name such as `http://toms:32480/` is not
portable across phones and routers.

On the server computer, optionally create a management password under **Server
Control > Remote management**. Other private-LAN devices receive a password
screen at the management URL only when it is configured. They can scan and
organize the library, manage connected clients,
and control the server, but cannot pick native folders, change source paths or
startup registration, or initiate display capture. Open the `/player/` link to
watch from another computer and opportunistically from a phone.

To share the server computer's current screen, keep an idle MediaCove player
open on the receiving TV or computer. From the localhost dashboard select it
under **Share this computer's screen**, click **Start sharing**, and choose a
screen, window, or tab in the browser prompt. Capture permission is never
stored; either end can stop immediately, and abandoned sessions expire. This
first implementation is direct LAN WebRTC. Phone screen/audio capture is not
supported. Episode playback remains a direct-LAN operation and requires the
MediaCove server computer to be available.

### Start automatically with Windows

Autostart is enabled by default the first time the server runs. Open
**Server Control > Start with computer** to confirm its state or select
**Disable autostart**. Disabling it removes future startup registration without
stopping the server that is currently running.

If autostart is disabled, start an installed copy by opening **MediaCove** from
the Windows Start menu. For a source checkout, open PowerShell in the repository
and run:

```powershell
.\scripts\start-server.ps1
```

The dashboard control is recommended. A source checkout can also enable
per-user autostart from PowerShell:

```powershell
pnpm autostart:install
```

This creates a Windows Scheduled Task named **MediaCove Server**. It starts hidden whenever the current Windows user signs in, starts after a missed trigger, retries up to three times after an unexpected exit, and refuses to launch a second copy. Administrator rights are not required.

MediaCove deliberately runs as the signed-in user instead of as the all-powerful `SYSTEM` account. This preserves access to that user's media folders and allows the native Windows folder picker to appear. It starts at sign-in rather than at the pre-login boot screen.

The helper uses the source-checkout data directory `.mediacove-data`. To select a different data directory:

```powershell
.\scripts\install-autostart.ps1 -DataDirectory "D:\MediaCoveData"
```

Background logs are written to `mediacove-server.log` and `mediacove-server.error.log` in the selected data directory. If Node.js, FFmpeg, or ffprobe uses a custom location, configure its containing directory in the Windows user or system `PATH` before enabling autostart. Provider credentials must likewise be persistent user or system environment variables, not temporary `$env:` values from one PowerShell window.

To disable it from PowerShell without deleting the library:

```powershell
pnpm autostart:remove
```

Re-run `pnpm autostart:install` after moving the repository because the Scheduled Task records its absolute path.

### Start automatically with Linux or macOS

Autostart is enabled by default after the first server run. Open
**Server Control > Start with computer** to enable or disable the current-user
systemd service on Linux or LaunchAgent on macOS. Disabling autostart leaves
the currently running server and all library data untouched.

If autostart is disabled, open a terminal in the extracted portable directory
and run `npm run start`. In a source checkout, open a terminal in the repository
and run `pnpm start:server`.

The dashboard control is recommended. The equivalent command-line helper is:

```sh
sh scripts/install-unix-service.sh
```

The helper creates a current-user systemd service on Linux or a current-user
LaunchAgent on macOS, records absolute paths, starts it immediately, and
requires no root access. It also adds **MediaCove** to the Linux application
menu, with Open and Stop actions, or creates
`~/Applications/MediaCove.app` on macOS. The macOS app can be dragged to the
Dock. Move neither the checkout nor portable directory while the service is
installed. Remove automatic startup and the launcher without deleting the
library:

```sh
sh scripts/remove-unix-service.sh
```

### Add media

1. On Windows or macOS, press **Browse** to open the native folder picker. On
   any platform, you can type an absolute path such as `D:\Movies` or
   `/Users/me/Movies`.
2. Press **Add folder**.
3. Press **Scan library** when a manual rescan is needed.

The native picker works only from the server page opened on the same computer. MediaCove brings one picker to the foreground and prevents a second picker from opening concurrently. Cancelling the Windows dialog restores the Browse button; an abandoned picker times out after five minutes. If the server runs in a non-interactive Windows session, type the path manually instead.

MediaCove scans source folders recursively and preserves their hierarchy. Empty branches are omitted. Supported media extensions are:

```text
.mp4 .m4v .mov .mkv .avi .webm .ts .mts .m2ts .mpg .mpeg
.mp3 .m4a .aac .flac .wav .ogg .oga .opus
```

Sidecar `.srt` and `.vtt` subtitle files are discovered alongside matching video or audio files. Embedded subtitles and audio streams are read with ffprobe. Audio-only files use the same fullscreen player and subtitle controls; the picture remains black when the file has no artwork or video stream.

### Install automatic captions

Install `whisper.cpp`, MediaCove's two multilingual Whisper choices, and its speech-detection model.

From a source checkout on Windows, Linux, or macOS:

```powershell
pnpm captions:install
```

From an extracted Linux/macOS portable package:

```sh
npm run captions:install
```

On Windows, the helper downloads the official `whisper.cpp` `v1.9.1` BLAS x64 release. On macOS, it uses Homebrew's official `whisper-cpp` formula, which provides native Apple Silicon and Intel bottles. Linux requires a compatible `whisper-cli` on `PATH`. Every platform downloads the quantized multilingual `large-v3-turbo-q5_0` and `small-q5_1` Whisper models plus ggml-org Silero VAD `v6.2`, verifies fixed SHA-256 hashes, and installs them in MediaCove's platform data directory. The model download is about 785 MB. No administrator rights, account, API key, or cloud service is required. The managed Windows task, macOS LaunchAgent, or active Linux user service is restarted automatically when present.

On the TV, open **Subtitles** during playback and select **Automatic captions**. Choose the known spoken language for better recognition or leave **Detect automatically** selected, then choose original-language or English output when applicable. If an automatic track already exists, select **Regenerate automatic captions** to replace it. The same language and output choices are available through **Generate CC** beside every movie in the computer library.

Choose the transcription model in the computer dashboard under **Automatic captions queue**. **Turbo** is the default and generally provides stronger transcription. **Small** uses less CPU and memory. OpenAI's roughly 6 GB number describes GPU memory for its unquantized Python implementation; it is not the host-RAM requirement of MediaCove's quantized CPU build. The installed Turbo file is about 574 MB. On the tested CPU build, observed peak process working set was about 1.29 GB for Turbo and 0.85 GB for Small, although actual working memory and speed vary by computer. English translation always uses Small because Turbo is not trained for translation.

#### Language selection and support

MediaCove chooses the spoken language in this order:

1. The language selected manually in the caption menu.
2. A valid language tag on the selected audio stream, such as `eng` for English.
3. Whisper's automatic language detector.

Select the intended audio stream before generating captions when a movie contains dubbing, commentary, or multiple audio tracks. If its language tag is wrong, MediaCove will trust that tag and may generate incorrect text; choose the spoken language manually to override it. If the tag is missing or marked as undefined, Whisper detects the language from the audio. Detection can be unreliable when the opening contains silence, music, noise, very little speech, or several spoken languages.

The manual menu currently offers English, Hebrew, Spanish, French, German, Portuguese, Japanese, and Korean. **Detect automatically** can recognize and transcribe additional languages supported by the multilingual Whisper model, but accuracy varies by language. MediaCove uses valid audio-track language metadata immediately. Without metadata, silent chunks are ignored and two non-empty chunks must agree before later chunks are locked to a detected language. A short clip with only one useful chunk uses that result when generation finishes. Accents, dialects, overlapping speakers, quiet dialogue, background sound, proper names, and mixed-language recordings can all reduce recognition quality.

#### Translation behavior

Automatic captions transcribe the selected audio in its spoken language or translate supported non-English speech into English. Translation is English-only; MediaCove does not currently translate into arbitrary target languages. Turbo is used for transcription, while English translation automatically uses Small because Turbo does not support Whisper translation. A wrong source-language choice or the wrong audio stream also reduces translation quality.

Jobs run one at a time in the background, show percentage and ready-cue counts, can be canceled, and are cached by media version, audio track, Whisper model, VAD model, caption-pipeline version, language, and mode. MediaCove works through overlapping windows that start at 30 seconds, then grow to three, ten, and repeated ten-minute windows. FFmpeg prepares the next window while Whisper processes the current one, and the windows are merged into one growing subtitle track. Whisper runs Silero VAD once per window to remove non-speech, and MediaCove preserves Whisper segment timestamps with the audio-window offset. Starting a job pauses the movie at the progress dialog. Select **Continue playing** at any time to resume while the TV refreshes the growing caption track. Captions may not be generated fast enough to keep up with playback; if they fall behind, pause briefly while generation catches up.

Completed captions are written beside the movie as UTF-8 WebVTT sidecars, for example `Movie.en.auto-generated.mediacove.vtt` or `Movie.en.auto-translated.mediacove.vtt`, and are also cached by MediaCove. The server user therefore needs write permission to the movie folder. MediaCove-owned sidecars survive scans without duplicate menu entries.

#### Performance and caption quality

On this installation's Intel Core i7-10700F, three warmed alternating runs over the same ten-minute English segment took a 28.94-second median with Small and 59.62 seconds with Turbo. Median first-cue times were 7.03 and 13.72 seconds. Against the episode's authored non-SDH English text track, normalized word error rate was 29.33% for Small and 28.15% for Turbo. Manual review confirmed modest Turbo improvements on short, quiet, and accented lines, while both models still misheard some names and battle dialogue. The authored subtitle is not a verbatim speech transcript, so these WER values are comparative rather than absolute accuracy scores.

The full progressive-pipeline measurements used Small. Three warmed, rotated runs of an actual 53:52 episode had a 3:41.9 median and 3.32% run-to-run variation using 30-second, three-minute, and repeated ten-minute windows with eight Whisper CPU threads. Median first-cue time was 3.89 seconds and the merged output contained 3,094 words. A later Turbo diagnostic on the same sparse 13:30-33:30 battle interval found 51.88% comparative WER with ten-minute windows versus 56.45% with one twenty-minute window. The ten-minute run avoided pulling a later medical conversation several minutes earlier and duplicating part of it, while finishing the final 23 minutes in 66.48 seconds instead of 70.18. These measured results are not minimum guarantees. CPU speed, storage behavior, antivirus scanning, thread count, speech density, source audio, and other computer activity all affect processing time. Caption jobs are serialized so simultaneous requests do not compete for the processor. Use `MEDIACOVE_CAPTION_THREADS` to override the default thread count. Advanced deployments may set `MEDIACOVE_CAPTION_MODEL`, `MEDIACOVE_WHISPER_PATH`, `MEDIACOVE_WHISPER_MODEL`, and `MEDIACOVE_WHISPER_VAD_MODEL` instead of using the dashboard or Windows helper.

Silero VAD reduces recognition during silence and helps prevent repeated text, but it cannot eliminate every recognition or timing error. Quiet speech may be omitted, and lowering the speech threshold can add hallucinated dialogue instead of recovering it reliably. Long sparse windows can also associate later speech with an earlier timestamp; MediaCove limits windows to ten minutes to reduce that failure. Generated captions may omit or mishear dialogue and generally do not include reviewed accessibility details such as speaker names or sound descriptions. They are automatic subtitles rather than a replacement for authored closed captions.

### Enable OpenSubtitles (project owner only)

MediaCove users must not create or enter the application consumer key. The computer UI intentionally has no key field.

For OpenSubtitles, the MediaCove project owner creates one API consumer named `MediaCove` in the OpenSubtitles **API consumers** page, then supplies that same consumer key to approved MediaCove deployments as `MEDIACOVE_OPENSUBTITLES_CONSUMER_KEY`. The server sends `MediaCove v1.0` as its User-Agent. OpenSubtitles requires one key per application and warns that applications asking users for their own keys may be banned. Before releasing MediaCove publicly without OpenSubtitles user login, contact OpenSubtitles, describe the project, and ask them to register the released app. Anonymous download limits still apply.

Configure or replace the OpenSubtitles consumer key with a masked local prompt:

```powershell
pnpm subtitles:key
```

Re-run that command whenever the MediaCove project key is rotated. It stores the key as a persistent Windows user environment variable and restarts the **MediaCove Server** Scheduled Task when installed. Remove the key with `pnpm subtitles:key:clear`. The key is not written to PowerShell history, `.env.example`, source control, screenshots, support logs, or `%LOCALAPPDATA%\MediaCove\config.json`.

For a temporary manual launch without autostart, `MEDIACOVE_OPENSUBTITLES_CONSUMER_KEY` can be supplied as an environment variable before `.\scripts\start-server.ps1`. The consumer key is read when the server starts and is never sent to the TV. OpenSubtitles provides exact file-hash matching before filename/title fallbacks.

### Sign in to OpenSubtitles

Open `http://localhost:32480`, find **OpenSubtitles account**, and enter the OpenSubtitles.com username and password. Login is optional: anonymous downloads continue to work within the application consumer's allowance. Signing in makes searches and downloads use that account's separate daily allowance and is the recommended model for a published MediaCove release.

The browser sends the credentials only to the loopback-only MediaCove administration endpoint. The local server forwards them directly to OpenSubtitles over HTTPS and never stores the password. MediaCove stores only the returned bearer token, API host, expiry, and quota metadata in `opensubtitles-session.json` inside the data directory with user-only file permissions. OpenSubtitles tokens normally expire after about 24 hours; sign in again when the computer UI reports **Anonymous**. **Sign out** destroys the provider token when possible and always removes the local session.

Downloaded subtitle files remain in MediaCove's local data directory and are served only to authenticated clients through signed URLs. MediaCove does not upload or republish them. This design limits distribution, but it does not grant copyright permission: subtitle files may be protected works, and the user remains responsible for lawful use under provider terms and local law.

The Library section can create, rename, and move virtual folders. **Remove** on a clip or library folder hides it from the TV persistently across scans but keeps it in the computer library with a Removed label. Its action changes to **Add**, which returns it to the TV library. Folder removal is recursive and also covers files discovered under that folder by later scans. These actions never move or delete the original media files.

Removing an entry from the separate **Media folders** source list stops scanning that source and removes its records from the MediaCove index; it still does not delete files from disk.

The server data directory defaults to `%LOCALAPPDATA%\MediaCove` when started directly, while the Windows autostart helper defaults to `.mediacove-data` in the repository. It contains the `mediacove.sqlite` database, configuration, generated artwork/subtitles, and temporary HLS sessions. Override it with `MEDIACOVE_DATA_DIR`. Other configuration variables are documented in `README.md` and `.env.example`.

## 5. Prepare the LG TV

Developer installation requires webOS TV 3.0 or newer and an LG Developer account.

1. On the TV, install **Developer Mode** from LG Apps.
2. Open it, sign in, enable **Dev Mode Status**, and let the TV reboot.
3. Note the TV IP address from its network settings.
4. On the computer, install LG's CLI:

   ```powershell
   npm install -g @webos-tools/cli
   ares -V
   ```

5. Register the TV:

   ```powershell
   ares-setup-device
   ```

   Select `add` and use:

   ```text
   Name: myTV
   IP: the TV's IP address
   Port: 9922
   User: prisoner
   Password: none
   Profile: tv
   ```

6. Open Developer Mode on the TV and enable **Key Server**.
7. Retrieve the key and enter the passphrase displayed by the TV:

   ```powershell
   ares-novacom --device myTV --getkey
   ares-device --system-info --device myTV
   ```

The LG CLI may work better under a different Node version than the MediaCove server. If so, use NVM for Windows to switch versions when running the `ares-*` commands.

## 6. Package and install the TV client

Build, package, install, and launch:

```powershell
pnpm build:tv
pnpm package:tv
ares-install --device myTV .\artifacts\com.mediacove.app.tv_1.0.0_all.ipk
ares-launch --device myTV com.mediacove.app.tv
```

After TV-client source changes, repeat these commands. Installing a newer MediaCove IPK over the same app preserves normal app storage, including saved connections and language preferences.

## 6A. Samsung Tizen development preview

Samsung support is not yet a customer download or a supported physical-TV
release. The shared client, Tizen packaging, remote registration, root exit
confirmation, Home/resume lifecycle, and basic H.264/AAC HLS playback passed on
the Tizen 10 Samsung TV emulator. A real TV can use Samsung's standard
Developer Mode installation path below, but MediaCove has no physical Samsung
test result. The remaining playback matrix and physical-device evidence remain
open.

Do not redistribute the current signed WGT. Samsung signing includes an author
profile and distributor authorization for the intended emulator or TV. Each
developer must create a private Samsung certificate profile that includes their
target DUID.

For an authorized source checkout:

1. Install [Tizen Studio](https://developer.samsung.com/smarttv/develop/getting-started/setting-up-sdk/installing-tv-sdk.html).
   In Package Manager, install **Web CLI**, **Certificate Manager**, and
   **Emulator Manager** from Main SDK. Install the current **Samsung TV
   Extensions** and **Samsung Certificate Extension** from Extension SDK.
2. Choose a target:
   - **Emulator:** Open **Tools > Emulator Manager**, create and start a Tizen
     10 x86_64 Samsung TV virtual machine, then use **Tools > Device Manager >
     Permit to install applications**.
   - **Experimental physical TV:** Put the TV and computer on the same trusted
     LAN. On the TV open **Smart Hub > Apps > App Settings**, enter `12345`,
     enable **Developer mode**, enter the computer's LAN IPv4 address, and
     reboot. In **Tools > Device Manager > Remote Device Manager**, add the
     TV's LAN IPv4 address, connect it, and select **Permit to install
     applications**. Samsung Instant On TVs may require disconnecting and
     reconnecting power during the reboot.
3. Confirm its serial. If the commands are missing, add
   `C:\tizen-studio\tools` and
   `C:\tizen-studio\tools\ide\bin` to the current PowerShell `PATH`:

   ```powershell
   $env:PATH = "C:\tizen-studio\tools;C:\tizen-studio\tools\ide\bin;$env:PATH"
   sdb devices
   tizen version
   ```

4. With the target connected, open **Tools > Certificate Manager**. Create a
   **Samsung > TV** certificate profile and include the connected target's
   DUID in its distributor certificate. Certificate Manager can retrieve it
   from the connected target; Samsung also documents a physical TV's DUID
   under **Menu > Support > Contact Samsung > Unique Device ID**. Keep
   certificates, passwords, `profiles.xml`, and backups outside the repository.
5. Build, sign, install, and launch using the profile name and the serial
   reported by `sdb devices`:

   ```powershell
   pnpm build:tv
   node scripts/package-samsung-tv.mjs --profile YourSamsungProfile `
     --target emulator-26101 --install --run
   ```

   Replace `emulator-26101` with the exact serial from `sdb devices`. A
   physical TV target can resemble `192.168.1.50:26101`.
6. Keep MediaCove Server running on the computer. MediaCove searches the LAN
   and tries to connect automatically. If the connection page appears, enter
   the optional management password or the computer's LAN IPv4 address and
   port `32480`. Do not enter `localhost`: it refers to the target itself.
7. Run the automated signed-package lifecycle and playback gate. It performs a
   clean reinstall, authorizes the emulator as a local client, opens a real
   library clip through Samsung AVPlay, and fails unless the native playback
   clock advances:

   ```powershell
   $env:MEDIACOVE_TIZEN_PROFILE = "YourSamsungProfile"
   npm run release:test:samsung -- --server http://192.168.0.104:32480
   ```

   Replace the URL with this computer's LAN URL. The report is written under
   `artifacts/release/`.

The generated package is
`artifacts/samsung-tv/MediaCove-TV-<version>-Samsung.wgt`. See
[`release/samsung/README.md`](release/samsung/README.md) for the complete
Windows, macOS, and Linux setup, emulator and experimental physical-TV
connection steps, repeatable commands, certificate troubleshooting, firewall
guidance, and uninstall instructions,
and [`release/samsung/EMULATOR_VERIFICATION.md`](release/samsung/EMULATOR_VERIFICATION.md)
for the current evidence and limitations.

## 7. Connect the TV

On first launch, MediaCove searches the LAN and attempts a passwordless
connection automatically. If the connection page appears:

1. Keep the server running.
2. Leave Host or IP blank to search the local network again.
3. Enter the optional management password created under **Server Control >
   Remote management**, or leave it blank when the owner has not set one. The
   same password protects a new TV, computer, or phone on the attached private
   LAN.
4. Select **Connect**.

MediaCove assumes port `32480`. Enter a host/IP or a different port only when
automatic discovery is unsuitable. The server compares the request address
with its active private IPv4 interfaces and, when configured, verifies the
management password before issuing a revocable client token. Existing clients
keep using their saved tokens and do not repeatedly request the password.
Clients outside the attached local subnets are rejected; there is no pairing
code or alternate-subnet fallback.

To use more than one server, open **Settings > Switch server**. Choose a saved
server, select **Add server**, or disconnect one server without removing the
others. The TV reconnects to the last selected server at startup. Disconnecting
a server removes its saved credentials from this TV, then switches to another
saved server when one is available.

The firewall rule must include the TV's current subnet. The default helper
allows `192.168.0.0/24`; automatic discovery can find the server only while
both devices are on the same LAN and client isolation is disabled on the
Wi-Fi network.

## 8. Playback controls

Select an unwatched movie to begin full-screen playback immediately. A partially watched movie first opens **Playback options**, with **Resume from _time_** focused by default and **Play from beginning** underneath. The same screen exposes audio and subtitle controls, including online subtitle search and automatic-caption generation, before the stream starts. This is a compact start screen rather than a movie preview. During playback, controls disappear after eight seconds and return when a navigation button is pressed.

- **Search** reveals and focuses its text field only after you press the button. Highlighting it does not begin text entry.
- The single library screen shows **Recently added**, **Up next**, folders in the current location, and a multi-row **Movies** grid.
- **Folders** follow the imported and virtual server folder structure without skipping single-child levels. Folder cards show the first available movie preview found recursively. Select the **Back** folder tile or press the remote Back button to move up one level.
- **Up next** is per TV client. It shows the most recently played episode in each folder from 1:00 watched until 5:00 remains, and also shows the next naturally sorted episode after playback enters the final 10:00. Selecting an Up next card opens playback options, including **Remove from Up next**; dismissal lasts until newer progress makes that card relevant again.
- **Audio** lists available audio streams. Choosing one restarts the stream at the current position when transcoding is required.
- **Previous episode** and **Next episode** navigate within the current folder, then across sibling folders under the same parent. When a direction has no target, its button remains inactive.
- **Subtitles** lists Off and every discovered subtitle track using English language names. **Find subtitles online** searches OpenSubtitles and selects a download. **Automatic captions** transcribes locally or creates an English translation after language/output selection; the action offers regeneration when a matching automatic track already exists.
- MediaCove remembers the selected audio/subtitle language or label and applies it to later titles when a matching track exists.
- Progress is saved periodically and independently for each TV client.
- With playback controls hidden, Left/Right seeks backward/forward 10 seconds. The visible rewind/forward controls seek 30 seconds. Resumed HLS sessions can rewind before the previous saved point, and subtitle cues stay aligned with the absolute movie time.

### LAN playback and TV storage

Normal library browsing and playback use the running LAN server and need no
internet. With optional Google caching configured, the TV saves a small catalog
of ready copies and their subtitle links while connected. After a computer
outage or app relaunch, **Cached episodes** opens that catalog and streams
directly from Google with internet access. MediaCove never stores video bodies
in TV browser storage. The TV reconnects automatically and uploads pending
viewing progress when the computer returns. Google may block a public copy,
and an evicted copy can remain listed until the next successful catalog sync.
Cloud fallback plays original codecs and cannot transcode while the computer
is off.

## 9. Logs and troubleshooting

### Server logs

Run `.\scripts\start-server.ps1` in a visible PowerShell window. Requests, scans, transcoder failures, and shutdown messages appear there. Verify the server with:

```powershell
Invoke-RestMethod http://localhost:32480/api/health
Get-NetTCPConnection -LocalPort 32480 -State Listen
```

If port `32480` is already occupied, stop the old MediaCove process before starting another copy.

### TV logs and inspection

With the app running:

```powershell
ares-inspect --device myTV --app com.mediacove.app.tv --open
```

This opens the webOS inspector for console messages, network requests, and the current DOM.

### Common problems

- **`corepack` or `pnpm` not recognized:** install pnpm with `npm install -g pnpm@9.15.5`, then reopen PowerShell.
- **No MediaCove server found:** confirm the server is listening, both devices
  are on the same LAN, the firewall rule includes the TV's subnet (default
  `192.168.0.0/24`), and the network is marked Private. Try the computer IP
  manually.
- **Computer unavailable:** MediaCove does not store episode files on the TV.
  Start the computer and server, then use **Retry** or **Refresh**; internet
  access is not required for LAN reconnection.
- **Connection rejected:** confirm the device is on a subnet attached to the
  server computer and enter the management password if one is configured.
  Public and unrelated-subnet requests are deliberately blocked.
- **Library opens but playback says server access is inactive:** open
  **Server Control > MediaCove account** on the computer. Create a free beta
  account and save its recovery key, or connect with an existing recovery key,
  then select **Refresh status**. Connecting and browsing do not prove playback
  access; the server deliberately checks entitlement when a stream is created.
- **Playback remains on Loading:** close the player and try again; check the server terminal for FFmpeg errors and verify `ffmpeg -version`.
- **Automatic captions are not installed:** from source run `pnpm captions:install`; from the Linux/macOS portable package run `npm run captions:install`. Restart a manually launched server, then confirm the computer page reports **Automatic captions / Ready**.
- **Automatic captions still report Not installed on Linux/macOS:** from the portable package run `npm run captions:install -- --check`. The command verifies `whisper-cli`, Small, Turbo, and Silero VAD in MediaCove's platform data directory and names any missing or invalid file.
- **Caption generation fails:** check the server error log, verify FFmpeg still works, and rerun the installer with `--force` (`pnpm captions:install -- --force` or `npm run captions:install -- --force`) if the model or native files were removed.
- **Linux/macOS update failed:** check `updates/update.log` under MediaCove's platform data directory. A failed health check restores the previous application directory automatically. If a `.mediacove-backup` directory remains beside the installation, preserve it and inspect the log before retrying.
- **No subtitles:** confirm the media contains an embedded subtitle stream or has a matching sidecar `.srt`/`.vtt`, then rescan. For online results, the MediaCove build owner must enable at least one provider and check its account quota.
- **Developer app disappeared:** Developer Mode expired; enable it again, retrieve the key if required, and reinstall the IPK.

## 10. Keep Developer Mode active

Before the Developer Mode timer reaches zero, press **EXTEND** in the TV's Developer Mode app or run:

```powershell
ares-extend-dev --device myTV
```

If the session expires, webOS removes developer-installed applications.

## 11. Linux and macOS server notes

Linux uses `~/.local/share/mediacove` by default and honors `XDG_DATA_HOME`.
macOS uses `~/Library/Application Support/MediaCove`. Both can be overridden
with `MEDIACOVE_DATA_DIR`. Beta-account credentials are encrypted with a
separate owner-readable key in that directory; protect both files in backups.

Linux desktop environments vary, so media folders are entered as absolute paths
in the dashboard. macOS uses the native folder chooser. Firewall tools also
vary: permit TCP `32480` only from the intended TV or trusted private LAN, and
never configure router port forwarding.

Run `npm run captions:install` from the portable package or
`pnpm captions:install` from source to install automatic-caption dependencies.
The helper uses MediaCove's default platform paths, so
`MEDIACOVE_WHISPER_*` variables are needed only for advanced custom locations.

## 12. Permanent TV distribution

A permanent LG Content Store installation requires LG Seller Lounge registration, store assets and test information, and LG review. This external process is not required for private Developer Mode use.

Samsung public distribution separately requires Seller Office packaging,
model-group selection, testing, and certification. Emulator-only evidence is
not enough to advertise physical Samsung TV support.

Official LG references:

- https://webostv.developer.lge.com/develop/getting-started/developer-mode-app
- https://webostv.developer.lge.com/develop/tools/cli-installation
- https://webostv.developer.lge.com/develop/tools/cli-dev-guide
- https://webostv.developer.lge.com/develop/specifications/streaming-protocol-drm
- https://webostv.developer.lge.com/distribute/app-approval-process
