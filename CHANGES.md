# Changes from upstream OpenBoard

GPL-3.0 Section 5(a) requires that modified versions carry prominent notices
stating that they were changed, and the date. This file is that statement.

**Base:** OpenBoard 1.4.6 (`versionCode 19`), from
https://github.com/openboard-team/openboard — imported as a squashed drop-in,
so upstream's own history is not preserved in this repository.

**Modified by:** Continuous Path Foundation, 2026.

**Purpose of the fork:** to let the keyboard be driven by HeadBoard, a
head-tracking accessibility app, so that a user who cannot touch the screen can
still type and swipe on it. Everything below exists to serve that.

## Added

- `app/src/main/java/org/dslul/openboard/IMEEventReceiver.java` — receives
  broadcast Intents from HeadBoard: injected motion and key events, gesture-trail
  colour, long-press delay, key-bounds queries, key-popup and highlight control.
- `app/src/main/java/org/dslul/openboard/inputmethod/keyboard/SyntheticPointerTracker.java`
  — drives the keyboard from injected pointer events rather than real touches.

## Modified

25 existing source files, principally:

- `keyboard/PointerTracker.java` — emits `ACTION_IME_SWIPE_START` to HeadBoard
  when a gesture begins.
- `keyboard/MainKeyboardView.java`, `KeyDetector.java`,
  `KeyboardActionListener.java` — accept synthetic pointers.
- `keyboard/internal/GestureTrail*.java` — externally settable trail colour.
- `latin/LatinIME.java` — registers the HeadBoard receiver; plumbs the outbound
  broadcast.
- `latin/inputlogic/InputLogic.java`, `latin/settings/*`,
  `latin/suggestions/*` — supporting changes.
- `jni/src/suggest/core/layout/proximity_info_utils.h`,
  `jni/src/suggest/core/result/suggestions_output_utils.cpp` — native tuning for
  head-driven input.
- `AndroidManifest.xml` — declares the cross-app permissions and the receiver.

## Renamed

- References to the companion app moved off Google's namespace:
  `com.google.projectgameface` → `org.continuouspath.headboard`.

## Signing

- The committed AOSP platform test key was removed. Debug builds use the
  default debug keystore; release builds use the Continuous Path team key and
  are left unsigned if it is absent, rather than falling back to another key.

## Not changed

The dictionaries, layouts, emoji tooling and the suggestion engine's behaviour
are upstream's. This fork adds an input path; it does not attempt to change how
OpenBoard predicts or corrects text.
