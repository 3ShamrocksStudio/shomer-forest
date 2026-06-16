# 🌲 Shomer Forest — שומר היער

**Citizens For Nature.** A community guardian app for Israel's forests and wildlife — the second vertical of the **Shomer** platform (alongside [Shomergency](https://github.com/3ShamrocksStudio/shomergency)).

Spot a wildfire, illegal dumping, an injured animal, a fallen tree, or suspected poaching? Report it in seconds, see it on a live nature map, and reach the right authority with one tap.

## What it does

- **Report hazards** — wildfire / smoke 🔥, illegal dumping 🗑️, injured wildlife 🦌, dead animal 💀, poaching / illegal activity 🚫, fallen tree / hazard ⚠️. Each with its own legend colour, icon, photo capture, and precise GPS.
- **Live nature map** — real Leaflet + OpenStreetMap basemap (neutral / desaturated) with clear category-coded markers and a compact, collapsible legend. Forest & reserve anchors (Carmel, Ben Shemen, Yatir, Biriya, Hula, Ramat HaNadiv) give an honest *aggregate* overview — never fabricated incident data.
- **Instant SOS** — one button fires a loud synthesized alarm (Web Audio), shares your live location, and offers a one-tap call to **Fire & Rescue 102**. Also triggerable by **Shake-to-SOS** (5 hard shakes).
- **Dead Man's Switch** — for lone rangers and solo hikers in remote nature: confirm "I'm OK" at an interval, or an automatic SOS fires.
- **Authorities** — direct dial to **102** (Fire & Rescue), **\*3639** (Nature & Parks Authority), **\*6911** (Ministry of Environmental Protection), **100** (Police), **101** (MDA). *Numbers marked "verify" need final confirmation before relying on them.*
- **He / En**, light onboarding, real geolocation, installable PWA, fully offline-capable.

## Tech

Single self-contained `forest.html` — no build step, no framework, no backend. Vanilla JS, Leaflet (CDN-pinned via CSP), Web Audio alarm engine, `localStorage` persistence. XSS-safe (all user data via `textContent` / escaped). Network-first service worker with foreign-cache purge (safe on a shared `*.github.io` origin). Strict CSP, no `eval`.

## Run locally

```bash
python3 -m http.server 8000
# open http://localhost:8000/
```

## Files

| File | Purpose |
|------|---------|
| `forest.html` | The entire app (UI + logic) |
| `index.html` | Launcher / self-heal redirect to `forest.html` |
| `manifest.json` | PWA manifest |
| `sw.js` | Service worker (offline + push) |
| `logo-*.png`, `*-icon-96.png` | App icons (forest-guardian shield) |
| `3shamrocks.png` | Studio credit badge |

---

© 2026 **3Shamrocks Studio** · Shomer Forest™ · All rights reserved.
