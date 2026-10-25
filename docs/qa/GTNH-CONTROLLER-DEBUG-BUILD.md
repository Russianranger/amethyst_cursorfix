# Qualified debug APK — 2026-10-25

## Source and import

- Upstream branch: v3_openjdk.
- Upstream base: `330c6eae3164df64bdc4828e946a9e62cc5169e4`.
- Input fix: [4ca84b97c41a4bf7f1c534c933958c536ebfaa94](https://github.com/Russianranger/amethyst_cursorfix/commit/4ca84b97c41a4bf7f1c534c933958c536ebfaa94).
- Built source: `ac5c9b24eff65851e049c9d4976e1ca0690a7689`.
- [History verification run 40220286393](https://github.com/Russianranger/amethyst_cursorfix/actions/runs/40220286393): SUCCESS.
- 111 upstream branches and 140 original tag objects imported. The 110 unchanged
  branch tips and all tag objects match upstream; v3_openjdk retains both the
  upstream baseline and original destination history. Destination main is retained.
- All temporary tag-import branches were removed. No history rewrites or
  force-pushes were used. Upstream licenses, submodule gitlinks and URLs, debug
  key, and Gradle/AGP/SDK/NDK pins are unchanged.
- Historical workflow versions for ten older tags were introduced with the
  connector's workflow-write permission before normal GITHUB_TOKEN tag import.
  Actions was disabled during connector ref creation; no per-ref build fan-out.

## Build and artifact

[Debug build 40220816002](https://github.com/Russianranger/amethyst_cursorfix/actions/runs/40220816002): **SUCCESS**.

[Download Actions artifact](https://github.com/Russianranger/amethyst_cursorfix/actions/runs/40220816002/artifacts/12609363998)
(`amethyst-gtnh-cursorfix-debug-3`, retained until 2027-01-23).

- APK: `Amethyst-GTNH-cursorfix-debug.apk`.
- APK size: 168432704 bytes.
- APK SHA-256: `bf7d92698ca405ad91e677cb307f98487771d263c45f5b164aaecb915f1d6c34`.
- Package: `org.angelauramc.amethyst.debug`.
- Version code: `10000000`.
- Version name: `1.1.7-50-ac5c9b24e-debug-1.1.7`.
- Minimum Android API 21; target API 34; compiled against API 37.
- ARM64 libraries are present in the universal APK.
- APK signature verification passed (v1 and v2), using the unchanged upstream
  Android debug certificate:
  `d0d0886a0d7e3486e6627f9b8011027fe3c6b0fb09424530b6d7be14f8c2cc33`.
- Downloaded archive digest and extracted APK checksum were independently checked.
- No private release-signing key was used.

The first two debug attempts stopped before compilation: first because
sdkmanager was not on PATH, then because the current Google SDK repository
publishes the API 37 base platform as `platforms;android-37.0`. The final workflow
uses the SDK tool's explicit path and that package identifier; compileSdk=37
and all pinned toolchain versions remain unchanged.

## Checks and limits

- `git diff --check`: passed.
- Workflow YAML parsing: passed.
- Recursive pinned submodule checkout, JDK 21, upstream JRE8 download, Gradle
  9.6.1 and Android toolchain preparation: passed.
- `:app_pojavlauncher:assembleDebug`: passed.
- `:app_pojavlauncher:lintDebug`: completed under upstream's existing
  `abortOnError=false` policy. This is **not a clean lint result**: 64 Error,
  46 Fatal and 330 Warning entries. Error/Fatal categories are OnClick,
  MissingPermission, MissingConstraints, NewApi, WrongConstant,
  UnspecifiedRegisterReceiverFlag, StringFormatInvalid and ExtraTranslation.
  All Error/Fatal locations are in files unchanged from the upstream base;
  no lint issue references Gamepad.java. The complete reports are in the artifact.
- APK package, ARM64 and debug certificate assertions: passed.
- Physical AYN Thor installation, world entry and controller behavior: **not tested**.

See [installation and the five device acceptance checks](../GTNH-CONTROLLER-FIX.md).
An upstream recommended debug installation using the original run's package,
version code and signing key is update-compatible. The release package can
coexist with this debug package. Back up the full game/instance folders before
any uninstall or migration; do not delete the current installation just to test.
