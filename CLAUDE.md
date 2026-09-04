# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo contains

**OpenBoard-HB** — a fork of the open-source OpenBoard IME, modified for "swype"/gesture typing driven by the head-tracking cursor of **HeadBoard** (repo `Continuous-Path/HeadBoard`, app id `org.continuouspath.headboard`). App id `org.dslul.openboard.inputmethod.latin`. Has a native JNI keyboard engine (NDK).

This repo was split out of the HeadBoard repo (its former `openboard/` dir) in Aug 2026 with full history via `git filter-repo`. Integration with HeadBoard is **runtime-only** (broadcast Intents); there is no build-time dependency.

## Build & install

- Build: `./gradlew :app:assembleDebug` → `app/build/outputs/apk/debug/app-debug.apk`
- **Needs JDK ≤ 16** (Gradle 7.2 can't run on newer; Java 21 fails with "Unsupported class file major version 65"). E.g. `JAVA_HOME=$(/usr/libexec/java_home -v 11) ./gradlew :app:assembleDebug`.
- Install: `adb install -r <apk>`, then select OpenBoard as the IME in device settings. Head-tracking-driven typing also requires the HeadBoard app with its accessibility service enabled.

**Apple Silicon**: the NDK v21 build fails to recognize an arm64 host. Edit `<android-sdk>/ndk/21.3.6528147/ndk-build` so its last line is `arch -x86_64 /bin/bash $DIR/build/ndk-build "$@"`.

## Inter-app communication with HeadBoard

**Broadcast Intents are authoritative** — do not migrate to AIDL or remove broadcast paths.

- HeadBoard → OpenBoard (head cursor injected as touch/key events, trail color, key popups): actions namespaced `org.dslul.openboard.inputmethod.latin.ACTION_RECEIVE_MOTION_EVENT` / `ACTION_RECEIVE_KEY_EVENT` / `ACTION_CHANGE_TRAIL_COLOR` / etc. — received by `IMEEventReceiver.java` here, sent from HeadBoard's `KeyboardManager.java`.
- OpenBoard → HeadBoard: `org.continuouspath.headboard.ACTION_IME_SWIPE_START` / `ACTION_IME_LONGPRESS_ANIMATION` / `ACTION_IME_STATE_CHANGED`.
- Custom permissions: `org.dslul.openboard.inputmethod.latin.permission.RECEIVE_HEADBOARD_EVENT`, `org.continuouspath.headboard.permission.RECEIVE_IME_EVENT`.
- The authoritative action lists live in the source files, not docs. Changing `android:exported`, custom permissions, or broadcast actions silently breaks the on-device integration with HeadBoard — flag such changes before making them, and keep them in sync with the HeadBoard repo.

## Signing & releases

- **Debug builds** sign with the committed `app/platform.keystore` (AOSP platform test key, store password `android`) — test keys, not production keys.
- **Release builds** sign with the shared Continuous Path team key (same key as JustType and HeadBoard; keystore `~/.justtype/justtype-release.jks`, password in the macOS Keychain, both installed by JustType's `./jt signing-setup`). Machines without it fall back to the platform keystore. CI overrides via `JUSTTYPE_*` gradle properties from repo secrets.
- **Signature caveat:** our `RECEIVE_HEADBOARD_EVENT` permission is signature-protected. HeadBoard and OpenBoard must be signed with the same key to talk — install matching build types on a device (release↔release or debug↔debug), never mixed.
- **Release pipeline** (`.github/workflows/release.yml`): pushing a `v*` tag builds a signed release APK and publishes it to `Continuous-Path/OpenBoard-Releases` (stable link: `releases/latest/download/openboard.apk`). Mirrors JustType's pipeline (docs in JustType1's `docs/release.md`).
