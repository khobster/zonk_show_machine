# The Zonk Show — Overnight Production Board

A tiny browser app that turns a bedtime voice memo into a finished, leveled podcast episode
with your intro/outro bumpers already on it. No install, no Terminal, runs entirely in the browser
(ffmpeg via WebAssembly).

## Nightly use
1. Record on your phone, AirDrop the memo to your Mac.
2. Open the deployed page.
3. Drag the memo onto **Tonight's show** → **Assemble episode**.
4. Download the MP3, upload it to Spotify for Creators.

## One-time setup: make your bumpers
Open the **Set up bumpers** tab. Drop in a raw bumper + your rain track, set the rain level,
export. Do it twice, once saved as `intro.mp3`, once as `outro.mp3`, then commit both files
to the repo root next to `index.html`.

## Deploy
Flat static site. Push to GitHub, connect to Netlify (or enable GitHub Pages). Nothing to build.

## Important
- Use it on the live URL (or a local server), **not** by double-clicking the file. Opened as
  `file://` the browser blocks the audio engine from loading.
- First load pulls ~25MB of ffmpeg and may take ~20 seconds. It caches after that.

See `CLAUDE.md` for architecture notes and the decisions not to change.
