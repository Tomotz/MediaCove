# Samsung Tizen Development Preview

MediaCove's Samsung client is under active development. It is not yet a
customer download or a supported physical-TV release.

Samsung's normal Developer Mode workflow can install a target-signed MediaCove
WGT on a physical Tizen TV. MediaCove has no Samsung TV hardware for
verification, so the physical instructions below are an experimental,
best-effort path rather than a compatibility claim.

## Current status

The following pass on Samsung's Tizen 10 TV emulator:

- the shared MediaCove TV interface;
- Tizen runtime detection;
- D-pad navigation and registered media keys;
- Samsung-compliant root Back confirmation and application exit;
- Home/hide and resume lifecycle handling;
- signed WGT build, install, reinstall, uninstall, and launch.
- basic H.264/AAC HLS playback rendered by the Tizen 10 emulator.

The current build deliberately requests the server's verified H.264/AAC HLS
fallback instead of advertising unverified Samsung MP4 direct-play codecs.

## Why there is no WGT download

Samsung development packages are signed for an intended emulator or television
using a private author/distributor certificate profile and the target DUID. A
WGT signed for the MediaCove development emulator is not a universal package
for another emulator or TV.

MediaCove will not advertise physical Samsung TV support until the complete
playback matrix passes and either representative physical-device testing or
Samsung certification closes the emulator-only gap.

## Authorized emulator setup

1. Install [Tizen Studio](https://developer.samsung.com/smarttv/develop/getting-started/setting-up-sdk/installing-tv-sdk.html)
   and the [Samsung TV Extension](https://developer.samsung.com/smarttv/develop/tools/tv-extension/download.html).
2. Install Web CLI, Certificate Manager, and Samsung Certificate Extension.
3. Create and boot a Samsung TV emulator. Tizen 10 is the currently verified
   target.
4. Confirm the serial with `sdb devices`.
5. Create a private Samsung certificate profile that includes the emulator
   DUID. Never commit its certificates, password, `profiles.xml`, or DUID list.
6. From an authorized MediaCove source checkout using Node.js 22.x:

   ```powershell
   pnpm build:tv
   node scripts/package-samsung-tv.mjs --prepare-only
   node scripts/package-samsung-tv.mjs --profile YourSamsungProfile `
     --target emulator-26101 --install --run
   ```

Replace the profile and emulator serial with your own values.

## Experimental physical-TV installation

1. Update the TV firmware. Put the Samsung TV and development computer on the
   same trusted home network and record both LAN IPv4 addresses.
2. On the TV open **Smart Hub > Apps > App Settings** and enter `12345` with
   the remote or on-screen keypad.
3. Enable **Developer mode**, enter the development computer's LAN IPv4
   address, select **OK**, and reboot the TV. If Samsung Instant On is enabled,
   disconnect and reconnect TV power during this reboot. Reopen Apps and
   confirm that **Develop Mode** appears.
4. In Tizen Studio open **Tools > Device Manager > Remote Device Manager**,
   select **+**, add the TV's LAN IPv4 address, and switch its connection to
   **On**. If needed, connect from Tizen Studio's tools directory:

   ```powershell
   sdb connect 192.168.1.50
   sdb devices
   ```

   Replace the example address and keep the exact target serial reported by
   `sdb devices`; it can include `:26101`.
5. In Device Manager, right-click the connected TV or its filesystem and
   select **Permit to install applications**.
6. Open **Tools > Certificate Manager**. Create **Samsung > TV** author and
   distributor certificates, signing in with a Samsung Developer account.
   Add the connected TV's DUID to the distributor certificate. Samsung also
   documents it under **Menu > Support > Contact Samsung > Unique Device ID**.
   Back up the author certificate and never publish certificates or passwords.
7. From an authorized MediaCove source checkout using Node.js 22.x:

   ```powershell
   pnpm install --frozen-lockfile
   pnpm build:tv
   node scripts/package-samsung-tv.mjs --profile YourSamsungProfile `
     --target 192.168.1.50:26101 --install --run
   ```

   Replace the profile and target with the exact values from Certificate
   Manager and `sdb devices`.
8. Keep MediaCove Server running on the computer and allow private-LAN TCP port
   `32480`. In MediaCove on the TV, pair using the computer's LAN IPv4 address,
   port `32480`, and the current code from `http://localhost:32480`.

Official Samsung references:

- [Connect a TV to Tizen Studio](https://developer.samsung.com/smarttv/develop/getting-started/using-sdk/tv-device.html)
- [Create and authorize Samsung certificates](https://developer.samsung.com/smarttv/develop/getting-started/setting-up-sdk/creating-certificates.html)
- [Samsung TV SDK installation](https://developer.samsung.com/smarttv/develop/getting-started/setting-up-sdk/installing-tv-sdk.html)

## Remaining release gates

- direct-play profiles plus pause, repeated seeking, subtitle,
  automatic-caption, audio-only, title-switching, and network-loss coverage;
- oldest-supported and current Tizen emulator coverage;
- physical Samsung TV or Samsung certification evidence;
- Seller Office package, listing, model groups, and review.

The public [download and setup guide](https://mediacove-entitlements.mediacove.workers.dev/download/)
will add a Samsung customer download only after those gates pass.
