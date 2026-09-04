<h2 align="center"><b>OpenBoard-HB</b></h2>
<h4 align="center">A fork of OpenBoard, driven by head tracking.</h4>

> **This is a fork, not upstream OpenBoard.**
>
> OpenBoard-HB is maintained by the [Continuous Path Foundation](https://github.com/Continuous-Path)
> as the companion keyboard for [HeadBoard](https://github.com/Continuous-Path/HeadBoard),
> a head-tracking accessibility app. It accepts injected pointer and key events over
> broadcast Intents so someone who cannot touch the screen can still type and swipe on it.
>
> For the original project — its releases, F-Droid and Play Store listings, translation
> project and community chat — go to
> [openboard-team/openboard](https://github.com/openboard-team/openboard). Those channels
> support upstream, not this fork; please do not send them issues about OpenBoard-HB.
>
> Licence: **GPL-3.0**, inherited from OpenBoard and, beneath it, AOSP LatinIME. It cannot
> be relicensed. See [LICENSE](./LICENSE), [NOTICE](./NOTICE) for the full pedigree, and
> [CHANGES.md](./CHANGES.md) for what this fork changed, as GPL-3.0 §5(a) requires.

## Contribute

Issues and pull requests for **this fork** are welcome here; see
[CONTRIBUTING.md](./CONTRIBUTING.md). Changes that belong upstream are better sent
upstream.

### Create a dictionary
You can use [this tool](https://github.com/remi0s/aosp-dictionary-tools) to create a dictionary. You need a wordlist, as described [here](dictionaries/sample.combined). The output .dict file must be put in [res/raw](app/src/main/res/raw), its wordlist in [dictionnaries](/dictionaries).

For your dictionnary to be merged, you need to provide the wordlist you used, as well as its license if any.

### APK Development

#### Linux

Install java:
```sh
sudo pacman -S jdk11-openjdk jre11-openjdk jre11-openjdk-headless
```

Install Android SDK:
```sh
sudo pacman -S snapd
sudo snap install androidsdk
```

Configure your SDK location in your `~/.bash_profile` or `~/.bashrc`:
```bash
export ANDROID_SDK_ROOT=~/snap/androidsdk/current/AndroidSDK/
```

Compile the project. This will install all dependencies, make sure to accept
licenses when prompted.

```sh
./gradlew assembleDebug
```

Connect your phone and install the debug APK
```sh
adb install ./app/build/outputs/apk/debug/app-debug.apk
```

#### Generate KeyboardTextsTable.java
Make your modifications in [tools/make-keyboard-text/src/main/resources](tools/make-keyboard-text/src/main/resources)/values-YOUR LOCALE.

Generate the new version of [KeyboardTextsTable.java](app/src/main/java/org/dslul/openboard/inputmethod/keyboard/internal/KeyboardTextsTable.java):
```sh
./gradlew tools:make-keyboard-text:makeText
```

## Credits
- Icon by [Marco TLS](https://www.marcotls.eu)
- [AOSP Keyboard](https://android.googlesource.com/platform/packages/inputmethods/LatinIME/)
- [LineageOS](https://review.lineageos.org/admin/repos/LineageOS/android_packages_inputmethods_LatinIME)
- [Simple Keyboard](https://github.com/rkkr/simple-keyboard)
- [Indic Keyboard](https://gitlab.com/indicproject/indic-keyboard)
- Upstream OpenBoard's [contributors](https://github.com/openboard-team/openboard/graphs/contributors)

# Building (this fork)

- Requires **JDK ≤ 16** (Gradle 7.2). E.g. `JAVA_HOME=$(/usr/libexec/java_home -v 11) ./gradlew :app:assembleDebug`
- **Apple Silicon**: NDK v21 fails to recognize an arm64 host. Modify `<android-sdk>/ndk/21.3.6528147/ndk-build` so it looks like:

  ```
  #!/bin/sh
  DIR="$(cd "$(dirname "$0")" && pwd)"
  arch -x86_64 /bin/bash $DIR/build/ndk-build "$@"
  ```
