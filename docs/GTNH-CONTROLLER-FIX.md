# GTNH 2.9 controller correction

Modified 2026-10-25 for Russianranger/amethyst_cursorfix, an independent import of
AngelAuraMC/Amethyst-Android. Original licenses and attribution are retained.

## Source baseline

- Upstream default branch: `v3_openjdk`.
- Upstream commit: `330c6eae3164df64bdc4828e946a9e62cc5169e4`.
- Reported original Actions run: [35652635902](https://github.com/AngelAuraMC/Amethyst-Android/actions/runs/35652635902).
- Reported original source: `8d9f8f8f3465b3e58a5f556ef78b807aa95609e5`
  on `feat/misc`. This commit is no longer reachable from that branch's
  current tip, but can still be fetched by SHA.
- Gamepad.java, CallbackBridge.java, app build configuration and debug.keystore
  are identical between that reported commit and the selected upstream base.
- Full advertised branch/tag history was imported, with the destination's
  initial main history retained. See [import record](import/UPSTREAM.md).

## Correction

Gamepad.tick already computes per-frame mouse motion after sensitivity,
acceleration, and elapsed-time scaling. It updates mouseX by deltaX and mouseY
by negative deltaY, but the SDL captured-mouse route in sendCursorPos reads
the separate CallbackBridge.deltaX/deltaY fields.

Publish deltaX and negative deltaY alongside the position update, before
sendCursorPos. Both assignments are unconditional inside the existing movement
dispatch path, including a zero component or zero scaled movement. No new SDL
or grab condition is introduced. Existing joystick selection, menu clamping,
sensitivity, mappings, and dispatch cadence remain unchanged. Touchscreen and
captured physical mouse already follow this publication convention.

This addresses the source-level defect. Physical AYN Thor confirmation is
required; compilation does not establish runtime controller behavior.

## Build

Run **GTNH cursor fix debug APK** on `v3_openjdk`. It uses upstream JDK 21,
Gradle 9.6.1, Android Gradle Plugin 9.3.1, compile SDK 37, target SDK 34,
NDK 27.3.13750724, recursive pinned submodules, and the language-list generator.
The prebuilt JRE 8 is pinned to upstream successful run 33461312688, artifact
9783581184 (the artifact expires 2026-11-30; replace this pin with a verified
upstream artifact if rebuilding after expiry). Other JREs use upstream's
existing runtime management.

The workflow builds assembleDebug and runs lintDebug. Upstream sets
lint.abortOnError=false, so a successful lint task alone is not a zero-issue
lint guarantee. APK badging, ARM64 inclusion, signature, source commit and
SHA-256 are checked and included with the artifact. There are no existing
Java unit-test sources in the selected baseline. No release task or private
release-signing key is required. Without a separate CurseForge API key,
upstream disables CurseForge API access; Modrinth mrpack handling remains.

Local commands with JDK 21 and Android SDK available:

```sh
git clone https://github.com/Russianranger/amethyst_cursorfix.git
cd amethyst_cursorfix
git remote add upstream https://github.com/AngelAuraMC/Amethyst-Android.git
git remote set-url --push upstream DISABLED
git -c url.https://github.com/.insteadOf=git@github.com: submodule update --init --recursive
# Extract the pinned jre8-pojav artifact into:
# app_pojavlauncher/src/main/assets/components/jre/
./scripts/languagelist_updater.sh
./gradlew :app_pojavlauncher:assembleDebug :app_pojavlauncher:lintDebug --max-workers=2
```

## Installation and preservation

- App label: Amethyst (Debug).
- Package: `org.angelauramc.amethyst.debug`.
- Version code: `10000000`, retained from upstream.
- Normal checked-in Android debug key, identical to the reported original build.
- Certificate SHA-256:
  `D0:D0:88:6A:0D:7E:34:86:E6:62:7F:9B:80:11:02:7F:E3:C6:B0:FB:09:42:45:30:B6:D7:BE:14:F8:C2:CC:33`.

An upstream debug APK signed with this key and with an equal or lower version
code can be updated in place without clearing data. The reported run's source
uses the same package, version code and key, so its recommended debug artifact
is update-compatible by configuration. The actual installed APK has not been
inspected on the user's device.

The upstream release package `org.angelauramc.amethyst` has a different
identity: this debug APK installs alongside it, with separate private storage.
A debug installation signed with another certificate will reject the update.

Before any reinstall or migration, copy the existing launcher's full exposed
game folder using its Android DocumentsProvider/Files entry to Downloads or an
SD-card backup. Include hidden `.minecraft`, all profiles/custom instance
folders, saves, mods, configs, options and controlmap. Android 13 normally
stores the default game root at `Android/data/<package>/files`; use the app's
DocumentsProvider when direct Android/data access is restricted. Copy custom
game directories separately if outside that root. Verify the backup contains
the worlds and configs before changing installations. Private accounts,
runtimes and app preferences are separate and may require sign-in and setup
again after uninstalling. Keep the existing app installed until the backup is
verified. An alternate package build is another option for certificate
conflicts; no removal is needed for testing beside the release package.

## Physical AYN Thor acceptance (pending)

1. Launch the affected GTNH 2.9 beta 3 or RC1 mrpack instance. Check right-stick
   horizontal and vertical camera movement.
2. Open inventory/menu. Check left-stick cursor movement and menu buttons.
3. Return to the world. Check that right-stick camera movement resumes.
4. Alternate touchscreen look and right-stick look. Check for stale direction,
   unexpected movement and drift; release the stick and confirm movement stops.
5. Launch a previously working Minecraft instance and check controller behavior.
