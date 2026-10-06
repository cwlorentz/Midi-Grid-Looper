# Grid Looper as an installable app (PWA)

This folder is the whole app. Upload the folder as it is; don't rename the files.

| File | What it is |
|---|---|
| `index.html` | The looper (drum grid, Bass/Lead/Pad synth tracks, looper deck), with the install links and service worker registration |
| `manifest.webmanifest` | App name, icons, fullscreen, landscape |
| `sw.js` | Offline cache (cache-first, versioned) |
| `icon.svg`, `icon-192.png`, `icon-512.png` | App icon |

The app has to be served over **https** for installing, offline mode and (later) MIDI to work. Opening `index.html` straight from the tablet's file manager still plays, but won't install or work offline.

## Hosting

**Netlify Drop (quickest)**
1. On a computer, go to https://app.netlify.com/drop.
2. Drag the `pwa` folder onto the page. You get an `https://….netlify.app` address.
3. Sign up (free) if you want the site to stay up; anonymous drops expire.
4. To update later: Site > Deploys, then drag the folder onto the deploy area again.

**GitHub Pages**
1. Create a public repository (e.g. `grid-looper`) and upload the contents of `pwa` to the root of the `main` branch.
2. Settings > Pages > Source: "Deploy from a branch", branch `main`, folder `/ (root)`. Save.
3. After a minute the app is at `https://<your-username>.github.io/grid-looper/`. All paths are relative, so the sub-folder address works.

## Installing on the Samsung tablet

1. Open the https address in **Chrome** (not Samsung Internet).
2. Tap the ⋮ menu > **Add to home screen** (or **Install app**), then **Install**.
3. Launch it from the home screen icon. It opens fullscreen, in landscape, with no browser bar.

## On a phone

Install it the same way and hold the phone sideways. On a landscape screen under 500px tall the looper switches to a compact layout: the top bar keeps Play, tempo, the instrument tabs, **Loops** and **•••**. Tap **•••** for Swing, Master, Rec, Loop export, Clear and full screen; tap anywhere outside the bar to close it. **Loops** swaps the grid for the loop pads (and back) instead of showing both. Use the installed app: in a normal Chrome tab the address bar takes height from the grid. Portrait still asks you to turn the phone.

## Updating after a change

Edit `sw.js` and bump `VERSION` (e.g. `looper-v1` → `looper-v2`), then re-upload the folder. Installed copies fetch the new version in the background; close and reopen the app (swipe it away from recent apps) to see it. If you forget to bump the version, the tablet keeps showing the old copy.

## What to test

- [ ] Chrome offers "Install app" / "Add to home screen", and the icon shows the amber grid (not a letter in a circle).
- [ ] Launched from the icon: fullscreen, landscape, no address bar, dark splash screen.
- [ ] Rotating the tablet to portrait keeps the app in landscape.
- [ ] Program a beat, close the app fully, reopen: the pattern is still there.
- [ ] Turn on airplane mode, close the app fully, reopen: it still loads and plays.
- [ ] Bump `VERSION`, re-upload a small visible change, reopen twice: the change appears.
- [ ] Timing still steady when launched from the home screen (same as in the browser tab).

## Instruments (Bass, Lead, Pad)

The **Drums / Bass / Lead / Pad** tiles in the top bar swap the grid area for that instrument. The looper deck stays put underneath.

- Each instrument shows a piano roll: one row per note of the scale, one octave plus the top root. The root rows are highlighted.
- **Tap** an empty cell to add a note (you hear it). **Drag right** while adding to make it longer. **Tap** a note to remove it, or press it and drag to change its length.
- **Tap: Vel** switches taps to loudness: each tap on a note steps it from loud to soft (brighter = louder). Tap **Note** to go back to adding notes.
- **Key** and **Scale** are shared by all three instruments. Every note snaps to the scale, and moving the key transposes everything. Chords stay chords.
- **Oct** moves the view up or down an octave. Notes in other octaves keep playing; a ▴ or ▾ next to the number says some are out of view.
- **Steps** sets that instrument's loop length (1 to 16). A 12-step bass against 16-step drums drifts around and lines up again every 3 bars.
- **M / S** and the slider are mute, solo and volume. Solo works across drums and instruments. **Clear** clears only what's on screen.
- To put an instrument on a loop pad, pick **In: Grid**: it records the drums and instruments together, so solo or mute tracks first to bounce just one.

## Looper deck (mic recording and loop layering)

The 8 big pads under the grid are audio loops, like a hardware looper. Everything is on one screen.

- **Tap an empty pad** to record into it. If the transport is stopped it starts playing, with one bar of clicks first when **Count** is on (mic only). If it's already playing, recording starts on the next bar line.
- **Bars** sets the take length: 1, 2, 4 or 8 bars, then it stops by itself and starts looping. **Free** keeps recording until you tap the pad again; it ends on the nearest bar line.
- **Tap a playing pad** to overdub onto that loop from the next bar. Tap it again to stop overdubbing on the next bar line.
- **In: Mic** records the tablet microphone. **In: Grid** records the drum grid and instruments instead, so you can bounce a beat to a pad, change the beat, and stack another.
- Under each pad: **M** mutes it, **✕** clears it (tap twice; while a pad is recording one tap cancels the take), and the slider sets its volume.
- **Delay** shifts mic recordings earlier to make up for the tablet's audio delay. It starts on an automatic guess; tap **Cal** with the speaker on (no headphones) in a quiet room to measure it, or drag the number up/down to fine-tune.
- Loops are saved on the tablet (browser storage) and come back after closing the app.
- On a keyboard, keys 1 to 8 tap the pads.

## Recording and exporting (Rec / Loop)

The **●** and **Loop** tiles next to Master save what you hear as a WAV file (16-bit stereo, the tablet's sample rate). Everything is in it: drums, instruments and loop pads, after the Master slider. Count-in clicks are left out.

- **● (Rec)** records everything you hear until you tap it again. The tile shows the time. If the transport is stopped, it starts playing and the recording begins on the first beat. Stopping the transport doesn't end the recording, so you can stop and restart within one take. It stops by itself at 15 minutes.
- **Loop** records exactly one pass of the loop, from bar line to bar line, so the file loops cleanly in other apps. The length covers the longest thing that repeats: 1 bar for drums, more when an instrument has an odd step count or a loop pad is longer (16 bars at most). It waits for a bar line (the tile says **Wait**, then shows bar 1/4, 2/4 ...). Tap it again to cancel.
- When the file is ready, a panel at the top right offers **Save** (to the Downloads folder), **Share** (send it to Drive, a DAW, email ...) and **✕** (discard; needs a second tap if you haven't saved or shared it).
- Live mic input is only in the recording once it's on a loop pad; the mic isn't monitored through the speaker.

## Known limitations

- The installed app and the Chrome tab share the same saved pattern only when they're the same address. A different host (Netlify vs GitHub Pages) means separate saved data.
- Orientation lock comes from the manifest and only applies to the installed app; in a normal Chrome tab, the ⛶ fullscreen button still does the locking.
- Updates take effect on the next launch, not instantly, so a re-upload never interrupts playback.
- The mic only works from the https address (GitHub Pages / Netlify) and only after Chrome asks for and gets microphone permission. If you said no, allow it again in Chrome ⋮ › Settings › Site settings › Microphone.
- Recording through the speaker also picks up the drums and other loops, and overdubbing then doubles them. Use wired headphones for clean takes.
- Bluetooth headphones add a large, variable delay (often 150 to 300 ms). Run **Cal** without them, then add the extra by dragging Delay, or use wired headphones.
- Changing BPM speeds loops up or down, so their pitch changes too.
- Instruments have one octave on screen at a time and 16 steps at most; the drum tracks are always 16 steps.
- Loops are mono and stay on this tablet. To take them elsewhere, use **●** or **Loop** to export a WAV of the mix; single loops can't be exported on their own yet.
- WAV files are big (about 10 MB a minute). The recording is held in memory until you save it, and only the latest one is kept: starting a new one replaces an unsaved one.
- The Android navigation bar may appear briefly on swipe from the edge; that's normal for fullscreen apps.
