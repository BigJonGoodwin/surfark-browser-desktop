# Surfark Browser (desktop): fork notes

<!--
Context for whoever reads this next (person or AI):
- This repository is J's fork of Zen Browser (github.com/zen-browser/desktop, branch dev). It is
  one half of a pair; the other half is github.com/BigJonGoodwin/surfark-browser-android
  (a fork of Sideraven).
- The overall plan lives outside this repo for now, at ClaudeProject/surfark-browser/docs/PLAN.md
  on J's laptop. This file records what has actually been changed in this fork and why.
- Add a dated entry below for every non-trivial change.
-->

## Rules for changes in this fork

- Keep this fork thin. Features that a WebExtension can do belong in the shared companion
  extension, not here.
- Our own files live in their own places (`docs/surfark/`, workflows named `surfark-*.yml`).
  Lines we add to or change in Zen's files carry the marker `SURFARK-HOOK`.
- Zen follows Firefox, which ships a new version about every two weeks. Merge Zen at least that
  often once this fork is in use.

## Status on 2026-11-12

Nothing in Zen's own files has been changed. The fork still builds "Zen Browser", with Zen's
name, profile folder and update server.

**Do not install a build of this fork yet.** Until it is renamed it would use the same profile
folder as a real Zen install on the same PC and would try to update itself from Zen's server.
The rename has to change the app's base name and binary name (so it gets its own profile and
install folders) and switch the updater off or point it at our own releases. That is the next
piece of work here, after the build test below.

## Change log

### 2026-11-12: build test workflow (branch `surfark/build-test`)

`.github/workflows/surfark-windows-build-test.yml` answers one question: can GitHub's standard
(free) machines build this fork for Windows? Zen's release builds use paid 8 to 16 core machines
and several optimisation passes. The test is Zen's Windows job cut down to one pass (x64 only, no
PGO, no LTO, no caches, no secrets). It uploads and publishes nothing; it only reports whether the
build finishes and how long it takes (see the run's summary).

Result: not known yet at the time of writing. Record it here.
