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

Two things are known:

1. **GitHub's free machines can build this fork.** Zen unchanged, Windows x64, one pass without
   PGO or LTO: 2 hours 36 minutes (setup 11, build 133, packaging about 10), inside the 6-hour
   limit. Such a build is slower to use than a real Zen release, which is optimised in several
   passes on bigger machines. Whether an optimised build fits is not tested.
2. **The rename is written but not proven.** See the change log entry below. It builds or fails
   on GitHub; nobody has started the result yet.

## Change log

### 2026-11-12: renamed to Surfark Browser (branch `surfark/rename`)

Based on Zen 1.24.2b (Firefox 160.0), commit deb01b6.

| What | Where | Was | Now |
|---|---|---|---|
| Product name | `surfer.json` `name` | Zen Browser | Surfark Browser |
| Vendor | `surfer.json` `vendor`, `src/toolkit/moz-configure.patch` | Zen OSS Team / Zen Team | Surfark |
| Program name | `surfer.json` `binaryName` | `zen` (zen.exe) | `surfark` (surfark.exe) |
| Brand names (release) | `surfer.json` `brands.release` | Zen, Zen Browser | Surfark, Surfark Browser |
| Base name | `configs/common/mozconfig` | Zen | Surfark |
| Profile folder | `src/toolkit/moz-configure.patch` (`MOZ_APP_PROFILE`), mozconfig `--with-user-appdir` | `zen` | `surfark` |
| Distribution id | mozconfig | `app.zen-browser` | `app.surfark.browser` |
| Release repository | `surfer.json` `brands.release.release.github.repo` | zen-browser/desktop | this repository |
| Update server | `surfer.json` `updateHostname` | updates.zen-browser.app | updates.surfark.invalid |
| Source link | mozconfig `MOZ_SOURCE_REPO` | Zen's repository | this repository |

Why: with its own program name, base name and profile folder, Surfark Browser installs next to a
real Zen and the two never share a profile (on Windows: `%APPDATA%\surfark` instead of
`%APPDATA%\zen`). The update server is a name that can never exist (`.invalid`), so this build
cannot update itself to a Zen build. It also means there are no automatic updates at all yet:
a new version is a new download.

Two of these live in one of Zen's patch files, `src/toolkit/moz-configure.patch`: the defaults for
`MOZ_APP_VENDOR` ("Zen Team" -> "Surfark") and `MOZ_APP_PROFILE` ("zen" -> "surfark"). A first
attempt set them in the mozconfig instead, to leave Zen's patch alone; Firefox's configure refuses
that ("can not be set by mozconfig"), so the patch is the only place. Patch files cannot carry a
marker comment: after merging a newer Zen, check that these two lines still say Surfark/surfark.

Not changed, on purpose or not yet:
- `surfer.json` `appId` is still `zen`. What the build tool uses it for was not checked.
- The "twilight" brand in `surfer.json` still says Zen. Only the release brand is used here.
- Icons and artwork are still Zen's. They need replacing before anything is shared with others.
- Zen's own texts (settings pages, welcome screens, "Zen Mods") still say Zen, and links still
  lead to Zen's sites and services. The mods store and docs are Zen's and keep working.
- Zen's release workflows are untouched and unused here; they need paid machines and secrets.

Not verified: whether the renamed build starts and behaves. The build test workflow now keeps
the installer and zip as run artifacts (5 days) so J can try them. First things to check when
trying it: it starts; `about:support` shows the profile under `%APPDATA%\surfark`; a real Zen
install on the same PC still opens its own windows and profile.

### 2026-11-12: build test workflow (branch `surfark/build-test`)

`.github/workflows/surfark-windows-build-test.yml` answers one question: can GitHub's standard
(free) machines build this fork for Windows? Zen's release builds use paid 8 to 16 core machines
and several optimisation passes. The test is Zen's Windows job cut down to one pass (x64 only, no
PGO, no LTO, no caches, no secrets). It uploads and publishes nothing; it only reports whether the
build finishes and how long it takes (see the run's summary).

Result: success on the first try, 2 hours 36 minutes (run 43194892023, Zen commit deb01b6).
