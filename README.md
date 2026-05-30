# The Zonk Show — Overnight Production Board

A tiny browser app that turns a bedtime voice memo into a finished, leveled podcast episode
with your intro/outro bumpers already on it. No install, no Terminal, runs entirely in the browser
(ffmpeg via WebAssembly).

## Nightly use
1. Record on your phone, AirDrop the memo to your Mac.
2. Open the deployed page. Make sure the **Show** field at the top is set to your show (default `zonk`).
3. Drag the memo onto **Tonight's show** → **Assemble episode**.
4. Download the MP3, upload it to Spotify for Creators.

## One-time setup: build your bumpers
Open the **Build bumpers** tab. Drop a **base track** (a music bed, or a finished clip on its own).
Optionally drop an **overlay** (a voice tag, sting, or sound) and use the sliders to set where it
comes in and how loud. Export. Do it twice, once as `<show>-intro.mp3`, once as `<show>-outro.mp3`,
then commit both files to the repo root next to `index.html`.

Example for The Zonk Show: intro = the music bed as the base + the "you're listening to the zonk
show" voice tag as the overlay, slid to the middle; outro = the closing clip dropped in as the base
with no overlay.

## More than one podcast
One machine runs every show. Build a client's two bumpers (`client-intro.mp3` / `client-outro.mp3`),
commit them, and type their name in the **Show** field. No second site, no code.

## Deploy
Flat static site. Push to GitHub, connect to Netlify (or enable GitHub Pages). Nothing to build.

## Important
- Use it on the live URL (or a local server), **not** by double-clicking the file. Opened as
  `file://` the browser blocks the audio engine from loading.
- First load pulls ~25MB of ffmpeg and may take ~20 seconds. It caches after that.

See `CLAUDE.md` for architecture notes and the decisions not to change.
