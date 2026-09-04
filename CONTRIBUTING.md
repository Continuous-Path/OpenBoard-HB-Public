# Contributing to OpenBoard-HB

Thanks for your interest. OpenBoard-HB is built by the Continuous Path Foundation, a
501(c)(3) nonprofit, for people who find ordinary touchscreens difficult or
impossible to use. That purpose shapes what we accept: **accessibility,
performance on inexpensive hardware, and backwards compatibility outrank
novelty.** A change that is elegant but drops older devices is usually the wrong
trade here.

## Signing off your work (DCO)

We do not ask you to sign a contributor licence agreement. Instead we use the
[Developer Certificate of Origin](https://developercertificate.org/) — a short
statement that you wrote the contribution, or otherwise have the right to submit
it under the project's licence.

Certify it by adding a `Signed-off-by` line to each commit:

```
Signed-off-by: Jane Doe <jane@example.com>
```

`git commit -s` adds it for you. Use your real name and an address where you can
be reached. By signing off you agree to the DCO, reproduced in full at the link
above.

## Licence

OpenBoard-HB is **GPL-3.0**, inherited from OpenBoard and, beneath that, AOSP
LatinIME (Apache-2.0). It cannot be relicensed: the OpenBoard contributions are
held by many authors who never signed an assignment. Contributions here are
accepted under GPL-3.0, and GPL-3.0 §5(a) requires that modifications be stated —
add a line to `CHANGES.md` describing yours.

## Before you open a pull request

```bash
./gradlew :app:assembleDebug
```

This is a fork carrying native code built with the NDK; see the README for the
JDK and Apple Silicon notes. It is a companion to HeadBoard and communicates
with it over broadcast Intents, so changes to those action names must be
coordinated with the HeadBoard repository.

Please also:

* Keep the change focused. One concern per pull request reviews far better.
* Match the surrounding code — its naming, its comment density, its idioms.
* Explain *why* in the commit message. The diff already shows what changed.
* Say what you tested, and on which device or Android version. "Tested on a
  Pixel Tablet, Android 15" is worth more than "works".

## Reporting bugs

Tell us what you expected, what happened, and how to reproduce it. For input or
prediction bugs, the exact key sequence and the on-screen result are the useful
details. Include the build identity shown at the foot of the settings screen —
it names the commit your build came from.

## Security and licensing

Please do not open a public issue for a security problem. Write to
**security@continuouspath.org** instead.

## Code of conduct

Participation is governed by our [Code of Conduct](./CODE_OF_CONDUCT.md).
Concerns go to **conduct@continuouspath.org**.
