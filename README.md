# Grid Looper as an installable app (PWA)

This folder is the whole app. Upload the folder as it is; don't rename the files.

| File | What it is |
|---|---|
| `index.html` | The looper (Phase 1 grid plus the looper deck), with the install links and service worker registration |
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

## Looper deck (mic recording and loop layering)

The 8 big pads under the grid are audio loops, like a hardware looper. Everything is on one screen.

- **Tap an empty pad** to record into it. If the transport is stopped it starts playing, with one bar of clicks first when **Count** is on (mic only). If it's already playing, recording starts on the next bar line.
- **Bars** sets the take length: 1, 2, 4 or 8 bars, then it stops by itself and starts looping. **Free** keeps recording until you tap the pad again; it ends on the nearest bar line.
- **Tap a playing pad** to overdub onto that loop from the next bar. Tap it again to stop overdubbing on the next bar line.
- **In: Mic** records the tablet microphone. **In: Grid** records the drum grid instead, so you can bounce a beat to a pad, change the beat, and stack another.
- Under each pad: **M** mutes it, **✕** clears it (tap twice; while a pad is recording one tap cancels the take), and the slider sets its volume.
- **Delay** shifts mic recordings earlier to make up for the tablet's audio delay. It starts on an automatic guess; tap **Cal** with the speaker on (no headphones) in a quiet room to measure it, or drag the number up/down to fine-tune.
- Loops are saved on the tablet (browser storage) and come back after closing the app.
- On a keyboard, keys 1 to 8 tap the pads.

## Known limitations

- The installed app and the Chrome tab share the same saved pattern only when they're the same address. A different host (Netlify vs GitHub Pages) means separate saved data.
- Orientation lock comes from the manifest and only applies to the installed app; in a normal Chrome tab, the ⛶ fullscreen button still does the locking.
- Updates take effect on the next launch, not instantly, so a re-upload never interrupts playback.
- The mic only works from the https address (GitHub Pages / Netlify) and only after Chrome asks for and gets microphone permission. If you said no, allow it again in Chrome ⋮ › Settings › Site settings › Microphone.
- Recording through the speaker also picks up the drums and other loops, and overdubbing then doubles them. Use wired headphones for clean takes.
- Bluetooth headphones add a large, variable delay (often 150 to 300 ms). Run **Cal** without them, then add the extra by dragging Delay, or use wired headphones.
- Changing BPM speeds loops up or down, so their pitch changes too.
- Loops are mono and stay on this tablet; they aren't part of any export yet.
- The Android navigation bar may appear briefly on swipe from the edge; that's normal for fullscreen apps.
