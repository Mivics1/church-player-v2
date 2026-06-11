# Church Program Auto-Player (v2 dev copy)

A no-install, full-screen player that streams selected sections of a recorded service video — in your chosen order — with the church logo animated between each section. Built for **Deeper Life Bible Church** auditorium playback.

Each service the program is pulled from one recorded video, but the parts that are shown (**GHS → Choir Song → GS Message**) are **not in video order**. This tool seeks to each section automatically, plays it start‑to‑end, fades the church logo in/out between them, and ends on the logo — no manual scrubbing.

---

## How it works

```
HOST (each week)                              AUDITORIUM (service)
n8n Form  ──►  builds a player link  ──►  redirects to this GitHub Pages player
(video, times)                            (full-screen, auto-advancing, animated logo)
```

- **Input form (n8n):** https://agbooladaramola7.n8n-wsk.com/form/church-setup-v2
- **Player (this site):** https://mivics1.github.io/church-player-v2/
- The form generates the player link and redirects to it; the player reads everything from the URL — nothing is stored.

---

## Running it (production)

### Weekly — before the service (~2 min)
1. Gather: the **video link** (recording, *not* a live stream), and the **start & end time** of GHS, Choir Song, GS Message as `mm:ss` (or `h:mm:ss`).
2. On the auditorium PC in **Chrome**, open the **form link** above.
3. Fill it in (the church logo is the default — leave the logo field blank), keep **Logo seconds = 3**, press **Submit**.
4. It redirects to the player. Leave the tab open.

### During the service
1. Click **▶ Start Program**.
2. It goes full-screen and plays automatically:
   **GHS → (logo in/out) → Choir Song → (logo in/out) → GS Message → ends on logo.**

### Operator keys
| Key | Action |
|-----|--------|
| `Space` | Pause / resume |
| `→` | Skip to next section |
| `Esc` | Exit full-screen |

---

## Player URL parameters

You can also open the player directly with a query string:

```
https://mivics1.github.io/church-player-v2/?video=<URL>&logo=<URL>&logoSec=3&ghs=<start>-<end>&choir=<start>-<end>&gs=<start>-<end>
```

| Param | Meaning | Example |
|-------|---------|---------|
| `video` | YouTube or direct/HLS recording URL | `https://youtu.be/XXXX` |
| `logo` | Logo image/animation URL (optional; defaults to bundled `logo.png`) | `logo.png` |
| `logoSec` | Seconds to hold the logo between sections | `3` |
| `ghs` | GHS section, **in seconds**, `start-end` | `600-660` |
| `choir` | Choir Song, in seconds | `90-105` |
| `gs` | GS Message, in seconds | `300-360` |

> Times in the **form** are entered as `mm:ss`; the form converts them to the seconds used above.

---

## The logo

- The player shows **`logo.png`** from this repo by default, fading/zooming **in and out** between sections and holding on it at the end.
- **To use the real church logo:** add your logo image to this repo named **`logo.png`** (see below). Until then, the player shows a clean "DEEPER LIFE BIBLE CHURCH" text card.
- A short animation (`.mp4`/`.webm`) also works — pass it as the `logo` parameter and it loops while shown.

### Replace the logo (drag-and-drop, ~30 sec)
1. Open this repo on GitHub → **Add file ▸ Upload files**.
2. Drag your logo, **rename it `logo.png`**, and **Commit**.
3. Done — it appears automatically (GitHub Pages updates within a minute).

---

## Important constraints

- ⚠️ **Recording only.** You can't seek to fixed times on a *live* stream — use an on-demand recording.
- ⚠️ **Ad-free** is only guaranteed with **your own non-monetized** YouTube upload (set it **Unlisted**) or a **direct/HLS** video link. There is no legal way to block ads on third-party videos.
- ⚠️ Use **Chrome**; YouTube seeking is accurate to ~±1s (direct video is exact).

---

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| Black screen after Start | Video isn't embeddable → use your own Unlisted upload or a direct `.mp4`/`.m3u8` link |
| A section is skipped | Ensure **end > start** for that section |
| No sound | Click **Start** (sound needs the click); raise PC volume |
| Logo shows as text card | Add your `logo.png` to this repo |
| Ads appear | Video is monetized/third-party → use your own non-monetized upload |

---

## Files

- `index.html` — the entire player (HTML + CSS + JS, no build step).
- `logo.png` — your church logo (add it).

Built with the [YouTube IFrame Player API](https://developers.google.com/youtube/iframe_api_reference) and [hls.js](https://github.com/video-dev/hls.js) for direct/HLS streams.
