# Not done

Handoff note for whoever continues [pull request #1](https://github.com/raffiihza/acapella-studio/pull/1)
on branch `reason/task-b5d59de1`. Written 2026-11-12. Delete this file before the pull request merges.

The task, in the owner's words: "review full, fix that can be fixed. change UI so it's not ai slop."

## Where things stand

- The branch holds a rebuilt `index.html` (bug fixes plus a redesigned interface) and a matching
  `README.md`, on top of `main` at `aa701a5`.
- The pull request is a draft and is not merged. Its one check (Cloudflare Pages) passed.
- Cloudflare built a branch preview at <https://reason-task-b5d59de1.acapella-studio.pages.dev>.
  Nobody has opened it yet to confirm the app works there.
- The work stopped because the owner ran out of usage, not because something was blocked.

`index.html` is the whole app, in three parts:

| Lines | What |
| --- | --- |
| 11-207 | CSS. Light and dark come from `prefers-color-scheme`; layout breakpoints at 900, 440 and 340 px. |
| 209-334 | Markup. |
| 335-341 | Bundled Mediabunny subset, minified. Unchanged from `main` (the large line has the same hash). Do not reformat it. |
| 342-1460 | The app script. |

## What is left

### 1. Sign off the redesign by eye

This is the main open item. The redesign was checked by script for layout, but only partly looked at.

Looked at and found acceptable (flat warm-grey palette, one red accent on Record, monospace numerals,
no gradients, glows or emoji): dark desktop, the settings panel in light, tablet in light, the phone
top, and the phone parts list.

Not looked at: the count-in overlay, the recording state, audio-only recording, the confirm dialog,
the export panel and its result, the file import states, and the empty project.

Things noticed in the views that were looked at, none of them fixed, each a judgement call:

- With the 9:16 frame on a desktop or tablet, the stage shows wide black bars either side of the grid.
- Each part row is about 195 px tall, so a 900 px desktop window shows four parts and a phone about
  three and a half. Consider tucking Delay, the two checkboxes and the four icon buttons behind a
  disclosure.
- The four per-part icon buttons (re-record, replace file, download, delete) have no visible text.
- The phone touch-target check only required 30 px. The usual guidance is 44 px.

The owner has not seen the redesign and should get screenshots before the pull request is marked ready.

### 2. Put the checks in the repository

There are no tests in the repository. The 85 scripted checks that passed (last run 01:57 UTC on
2026-11-12, against the committed `index.html`) lived in a temporary folder on the build machine,
and that folder is gone. They have to be written again. What they did:

- Served the repository on `127.0.0.1` and drove it with `puppeteer-core` and system Chromium, launched
  with `--use-fake-device-for-media-stream --use-fake-ui-for-media-stream
  --autoplay-policy=no-user-gesture-required`. Exports were inspected with `ffprobe`.
- Read app state from the page: `document.body.dataset.mode` (`idle`, `recording`, `playing`,
  `saving`), `#tracks .trk` rows, `.trk-name`, `.trk-len`, `.trk-note`, `#time`, `#total`, `#toast`,
  and the `tracks` store of the `acapella-studio` IndexedDB database. The app exposes no test hook.
- Covered, by section: first load; recording (video, audio-only, double press, take stopped during
  count-in); transport and seek; keyboard (`R`, `Space`, and leaving `Ctrl+R`/`Cmd+R` to the browser);
  delete and pending saves; file import and replace, including unreadable files; mix controls (delay,
  volume, pan, solo, match-first, waveform lanes); export (9:16 and 16:9 video, audio-only, cancel);
  persistence across reload; latency entry and calibration; new project; injected failures (denied
  permission, `MediaRecorder` throwing, storage write aborting); re-record safety; the real-time
  export fallback with WebCodecs removed; phone layout at 360 px.

### 3. Try it on real hardware and other browsers

Every check ran in headless Chromium on Linux with a fake camera and microphone. Never tried:

- A real camera, microphone and headphones, including whether latency calibration gives a usable number.
- Safari on macOS and iOS, Firefox, and Chrome on Android.
- Takes longer than a few seconds, and projects with more than seven parts.

### 4. Check the exported file's audio codec

`pickFormat` (around line 885) writes H.264 with AAC when the browser can encode AAC, and H.264 with
Opus inside an MP4 when it cannot. The test Chromium has no AAC encoder, so only the Opus path was ever
run, and the export check was loosened to accept it. Two things are unknown:

- whether the AAC path works at all, and
- whether an MP4 with Opus audio plays in phone galleries, QuickTime and the social apps the frame
  presets are named after. If it does not, falling back to WebM may be the better choice.

### 5. Finish the pull request

- Add screenshots to the pull request body, and rewrite the body to describe the finished work.
- Open the Cloudflare preview and record a take on it.
- Delete this file, mark the pull request ready, and merge it.
