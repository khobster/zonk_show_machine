# CLAUDE.md — The Zonk Show Overnight Production Board

## What this is
A single-file, no-build browser tool that assembles podcast episodes for *The Zonk Show*
(a late-night sports schmooze show). It runs ffmpeg compiled to WebAssembly entirely in the
browser — no server, no install. Deployed as a flat static site (Netlify or GitHub Pages).

## The workflow it serves
1. Host records a voice memo on their phone (iPhone .m4a, AAC).
2. AirDrops it to the Mac.
3. Opens this page, drags the memo in, hits ASSEMBLE EPISODE.
4. App prepends `intro.mp3`, appends `outro.mp3`, normalizes loudness to -16 LUFS, outputs one MP3.
5. Host uploads that MP3 to Spotify for Creators (Spotify removed in-app intro/bg-music tools in 2025; upload-only now).

## Two modes (tabs in index.html)
- **TONIGHT'S SHOW**: concat `intro.mp3` + voice memo + `outro.mp3`, then `loudnorm=I=-16:TP=-1.5:LRA=11`.
- **SET UP BUMPERS** (one-time): mixes a rain track UNDER a raw bumper at an adjustable level,
  exports a finished `intro.mp3` / `outro.mp3`. Rain lives only in the bumpers, never under the whole show.

## File layout (keep it flat — no folders)
```
index.html      <- the entire app
intro.mp3       <- produced once via the Set Up Bumpers tab, committed here
outro.mp3       <- produced once via the Set Up Bumpers tab, committed here
netlify.toml    <- minimal config
```
The app fetches `intro.mp3` / `outro.mp3` by RELATIVE path, so they must sit next to index.html at the repo root.

## DO NOT BREAK THESE DECISIONS (ask before changing)
- **ffmpeg version is pinned on purpose**: `@ffmpeg/ffmpeg@0.11.6` + `@ffmpeg/core@0.11.0`, the
  single-threaded UMD build loaded from unpkg. Reason: it works on plain static hosting with NO
  cross-origin-isolation headers. Do **not** upgrade to 0.12.x (the multi-threaded core needs
  SharedArrayBuffer, which forces COOP/COEP + CORP on every subresource and breaks the zero-config deploy).
- **Do not add COOP/COEP headers** to netlify.toml unless you've intentionally moved to the mt core
  and verified the cross-origin CDN subresources still load.
- `loudnorm` is intentionally single-pass — accurate enough for short nightly episodes, and fast.
- Single HTML file, no bundler, no npm build step. This is a deliberate preference.
- Aesthetic: matches the Arugula Motors house style (arugulamotors.com) — white background, near-black
  ink (#1a1a1a), muted slate-gray secondary text, a single blue accent (#1a6fd0), the system font stack,
  12px rounded cards, pill buttons, sentence-case copy, and the no-em/en-dash voice rule. (This replaced
  the original amber-on-black AM-radio skin at Kevin's request, 2026-05-30. No Google Fonts.)

## Known constraints / gotchas
- Must be served over http(s). It will NOT work opened as a `file://` from the desktop — the browser
  blocks the wasm core from loading. Test on the live Netlify URL or a local server (`python3 -m http.server`).
- Input is iPhone .m4a (AAC). The 0.11.0 core decodes AAC fine. Other formats (wav/mp3/aac) also work.
- ffmpeg.wasm is ~25MB on first load; it caches after.

## Reasonable next tasks
- Get this into a fresh GitHub repo and deploy to Netlify.
- After deploy, open the live URL and confirm the engine loads (watch the log line "board is live.").
- Produce intro.mp3 + outro.mp3 via the Set Up Bumpers tab and commit them.
- Optional niceties: favicon, a tiny "first load may take ~20s" hint, drag-and-drop polish.
