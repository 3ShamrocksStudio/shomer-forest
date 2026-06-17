# 🌲 Shomer Forest — Solo Test Guide & Honest Status

**App:** Single-file PWA (`forest.html` + `index.html` + `manifest.json` + `sw.js` + icons)
**Live URL:** https://3shamrocksstudio.github.io/shomer-forest/
**Audience:** One person (Dave), testing solo in a real browser.

This is a step-by-step self-test. Work top to bottom. Each section lists **exact steps** and the
**expected result**. The honest *"What's real vs. needs a backend"* section at the end is the truth
about what this front-end-only app does and does not do.

> Shomer Forest is a **content/purpose re-skin of Shomergency** — the visual brand, layout, components,
> onboarding, alarm engine, map style and SOS safety logic are **identical** to the live Shomergency app.
> Only the *purpose* changed: location focus, event types (wildfire, dumping, injured/dead wildlife,
> poaching, hazard), and the relevant authorities (Fire 102, Nature & Parks, Environmental Protection).

> **Best device for the full experience:** a phone (Chrome on Android, or Safari on iOS) over the live
> HTTPS URL — that unlocks real geolocation, device-motion shake, the camera, push permission, and the
> install prompt. Everything except live shake and the OS install prompt can also be tested on desktop.

---

## 0. First-run reset (do this before each clean test)

1. Open the app. If you've used it before, open the menu (**☰**) → **הגדרות / Settings**.
2. Scroll to the bottom → **🔄 איפוס אפליקציה / נתוני דמו (Reset app / demo data)** and confirm.
   - **Expected:** page reloads to the **onboarding** screen with all data cleared.

---

## 1. Onboarding (persists, never loops)

1. An auto handle (e.g. `Forester01234`) is pre-filled — keep it or type your own name.
2. Type any phone number (e.g. `050-123-4567`). Blank also works (demo mode).
3. Tap **המשך / Continue** → advances to the **location** step.
4. Tap **📍 אפשר גישה למיקום / Allow location** and accept the prompt (or **דלג / Skip**).
   - **Expected (allow):** box turns green with your lat/long.
5. Continue → **emergency settings**: pick an alarm, set volume, tap **🔊 בדוק אזעקה / Test alarm** (hear it).
6. Tap **✓ כניסה לשומר היער / Enter Shomer Forest** → you enter the map; a welcome toast appears.
7. **Reload (Cmd/Ctrl-R).**
   - **Expected:** opens **straight to the map** — onboarding does **not** reappear. Name + alarm choice remembered.

✅ *Pass:* any phone reaches the map, onboarding survives reload, no loop.

---

## 2. Report a hazard (photo → submit → map pin + feed + persists)

Open menu → **📝 דיווח אירוע / Report incident** (or **🔔 Updates → +**).

1. Pick an incident **type**: 🔥 wildfire/smoke · 🗑️ illegal dumping · 🦌 injured wildlife · 💀 dead animal · 🚫 poaching · ⚠️ hazard/tree.
2. Type a **description** (required — empty submit prompts you).
3. **Photos:** tap **🖼️** (gallery) or **📷** (camera). Thumbnails + count appear (up to 5).
4. Tap **📍 שתף מיקום נוכחי / Share current location** and accept (required before sending).
5. Tap **📤 שלח דיווח / Send report**.
   - **Expected:** "report sent" toast; a **category-coloured pin** drops on the map (tap → detail sheet);
     the report appears in the **Updates** feed and in **Profile → הדיווחים שלי / My reports** with thumbnails.
6. **Reload.** Check Profile → My reports and the map.
   - **Expected:** the report, its thumbnails, **and its map pin** are all still there. *(This pin-on-reload
     path was specifically hardened — pins re-render even if the map library finishes loading late.)*

✅ *Pass:* report submits, shows on map + list + feed, **survives reload (incl. the map pin)**.

---

## 3. SOS safety / anti-abuse flow (alarm-first · cancel-no-trace · resolve · rate-limit · hold-to-arm)

Key rule: **false alarms and misuse must NEVER appear on the map.**

### 3a. Press → cancel within countdown = FALSE ALARM, no trace
1. On the map, press the big red **SOS** button.
   - **Expected:** the **loud alarm fires immediately** + a full-screen **cancel countdown** (ring + 10→0)
     with a green **"I'm safe — Cancel"**. At this point there is **NO map pin** and **NO feed card** yet.
2. Tap **"I'm safe — Cancel"** before it reaches 0.
   - **Expected:** alarm stops; toast *"Cancelled — false alarm. Nothing shared, nothing on the map."*
     Updates feed → **no SOS card**; map → **no pin**; SOS history → **no event** (false alarm leaves no trace).

### 3b. Press → let it fire = ACTIVE event
1. Press **SOS** and let the countdown reach 0.
   - **Expected:** a **pulsing red pin** drops; the red **SOS strip** appears (with **"✓ I'm safe"**);
     a **"🚨 Your active SOS"** feed card; after ~2s an auto-dial **Fire 102** confirm; responder count climbs.

### 3c. Resolve as accidental / test → removed from map; real → stays
1. With an active SOS, tap **"✓ I'm safe"** → prompt asks **Real / Accidental / Test**.
2. Choose **Accidental** (or **Test**) → **pin + feed card removed**; toast confirms it was not shared.
3. Repeat 3b, choose **Real** → the pin **stays** (confirmed events remain mapped).
4. Profile → SOS history tags each event **confirmed / accidental / test** (private log).

### 3d. Rate limit (max 3 active/day)
1. Activate **3 times** (resolve each). On the **4th**, let it activate.
   - **Expected:** alarm still sounds, strip shows **"שידור מושהה (הגבלה יומית) / broadcast paused (daily limit)"**;
     **no new pin/broadcast**. Settings → SOS safety shows **3/3**.

### 3e. Countdown duration & hold-to-arm
1. Settings → SOS safety → set **Cancel window** to **5s** → press SOS → countdown starts at **5**.
2. Toggle **Hold-to-arm** on → a single tap shows a *"press & hold 2s"* hint; **press & hold ~2s** → gold ring fills → SOS fires.

✅ *Pass:* alarm always first; cancel leaves zero trace; activation maps+pulses; accidental/test scrub; real stays;
rate limit blocks broadcast; duration + hold-to-arm work; all settings persist across reload.

---

## 4. Shake-to-SOS (toggle + sensitivity + simulate)

Menu → **Settings → Shake-to-SOS**.

1. **Toggle** off/on → toast confirms; persists across reload.
2. **Sensitivity** slider → label reads High / Medium / Low; persists.
3. **📳 סימולציית ניעור / Simulate shake (test)** → "shake detected" toast, then a full **SOS** fires
   (siren + countdown), exactly as a real 5-shake gesture would. Cancel when done.
4. **Real shake (phone only):** with the toggle on, shake ~5× hard → SOS fires. *(Real device + motion sensor;
   iOS asks for motion permission once.)*

✅ *Pass:* toggle + sensitivity persist; simulate fires SOS on desktop.

---

## 5. Dead Man's Switch (interval + countdown + check-in + auto-fire + resume)

Menu → **⏱️ Dead Man's Switch**.

1. Pick **1 / דמו (demo)** (also 5 and 10 min). Tap **▶ הפעל / Activate**.
   - **Expected:** a blue strip with a live **countdown**; menu badge "פעיל / Active".
2. At **60s left** the strip turns orange ("60 seconds to confirm").
3. Tap **✓ הכל בסדר / I'm OK** → countdown resets.
4. To watch it fire: let it reach `0:00` → **auto-triggers SOS** (siren + pin), logs a **DMS**-tagged event.
5. **Reload mid-countdown** → countdown **resumes** ("DMS resumed after reload"). If the deadline passed while
   the app was closed it clears safely ("expired while app was closed") — it does **not** blast the alarm on load.

✅ *Pass:* settable interval, visible countdown, check-in works, timeout fires SOS, state survives reload.

---

## 6. Map (live blue dot + forest anchors + interactive)

1. Tap the **📍** button (bottom-left) → accept the prompt → drops/centres a **blue "me" dot** and zooms in.
   On a moving phone the dot follows your real position (live watch).
2. Pinch / scroll or use **+ / −** to zoom; drag to pan.
3. Tap any colour-coded **forest anchor** (Carmel, Ben Shemen, Yatir, Biriya, Hula, Ramat HaNadiv).
   - **Expected:** a popup shows the category, the forest/reserve, an honest *area* note, and the source
     (KKL-JNF · Nature & Parks Authority). These are **aggregate overviews, not specific incidents.**
4. Check the **legend** (bottom-right) for the colour key.

✅ *Pass:* blue dot tracks real location, 6 forest/reserve anchors present, map interactive.

---

## 7. Language (live switch + persist + Hebrew RTL)

1. In onboarding or **Settings → שפה / Language**, tap **English** then **עברית**.
   - **Expected:** UI text switches **immediately**; Hebrew lays out **right-to-left**, English left-to-right.
     Event-type labels, legend, report types and emergency text all switch.
2. **Reload.** → comes back in your **last chosen language**.

✅ *Pass:* language applies live, RTL correct for Hebrew, choice persists. *(He / En only.)*

---

## 8. Authorities (one-tap dial)

Menu → **☎️ מספרי חירום / Emergency numbers**.

| Number | Who | For |
|--------|-----|-----|
| **102** | Fire & Rescue (כבאות והצלה) | Wildfire, smoke, rescue *(primary)* |
| **\*3639** | Nature & Parks Authority (רשות הטבע והגנים) | Injured wildlife, reserves, poaching — *verify* |
| **\*6911** | Ministry of Environmental Protection (להגנת הסביבה) | Pollution, illegal dumping — *verify* |
| **100** | Israel Police (משטרה) | Poaching, illegal activity |
| **101** | MDA (מד"א) | Injury, medical emergency |

Tap any → opens your dialer with the number. *(Numbers marked "verify" still need final confirmation before relying on them in the field.)*

✅ *Pass:* each button opens the dialer with the right number.

---

## 9. Install (PWA)

- **Android / desktop Chrome (live HTTPS URL):** after a few seconds an **"Install Shomer Forest"** banner
  appears, or use the browser's install icon. → installs to home screen, opens standalone with the icon.
- **iOS Safari:** **Share → Add to Home Screen** (iOS has no auto-prompt API).

✅ *Pass:* installable and launches standalone.

---

## 10. Persistence summary (everything that survives a reload)

Stored under one LocalStorage key `shomer_state_v1`:

| Data | Survives reload? |
|------|------------------|
| Onboarding complete / profile (name, phone) | ✅ |
| Chosen language (He/En) | ✅ |
| Alarm type + volume | ✅ |
| Shake enabled + sensitivity | ✅ |
| Dead Man's Switch interval + active countdown | ✅ (resumes) |
| SOS safety settings (cancel window, hold-to-arm, daily count, trust) | ✅ |
| Submitted reports (incl. photo thumbnails **+ map pins**) | ✅ |
| SOS event history (with resolution tags) | ✅ |

The **Reset app / demo data** button wipes all of the above and returns to onboarding.

---

## ✅ What's REAL right now (front-end, no backend)

- Full onboarding, profile, He/En i18n, RTL.
- Real **Leaflet + OpenStreetMap** map, live geolocation blue dot, 6 forest/reserve anchors.
- Real **Web Audio** alarm engine (3 synthesized sirens, volume, test).
- Complete **SOS safety state machine**: alarm-first, cancel-no-trace, activate, Real/Accidental/Test
  resolution, per-day rate limit, hold-to-arm, trust-score simulation.
- **Shake-to-SOS** (real device motion + a desktop simulate button) and **Dead Man's Switch** (with reload-resume).
- **Report flow**: type, description, camera/gallery photos (downscaled thumbnails), GPS, map pin, feed card.
- Full **LocalStorage persistence** of everything above, incl. report map pins on reload.
- Installable PWA, offline-capable, strict CSP, **no `eval`**, XSS-safe (user data via `textContent`/escaping).

## ⚠️ What NEEDS a backend (honest limitations)

This is a **client-side-only** app. There is **no server**. As a result:

1. **No real multi-user delivery.** SOS alerts, reports and "guardians responding" are **local to this one
   device**. Nothing is actually sent to other people, rangers, fire dispatch, or the Nature & Parks Authority.
   Responder counts, "active guardians", and the live event ticker are **simulated** for demonstration.
2. **No real push notifications.** The app can show a *local* browser notification (with permission), and `sw.js`
   has a push handler, but there is **no push server** — no alerts arrive when the app is closed/backgrounded.
3. **SOS cannot force a phone call.** Browsers can't auto-dial. SOS offers to open the dialer (`tel:102`); you
   still tap call. Lock-screen / hardware SOS is the OS's feature, not this app's.
4. **Geolocation, shake & camera need a real device + permission + HTTPS.** On desktop these may be limited;
   use **Simulate shake** to test that path. iOS asks once for motion permission.
5. **Photos are stored as downscaled thumbnails** to stay within browser storage limits.
6. **Data is per-device, per-browser.** A different browser / private window / cleared site data starts fresh.
   No account sync.
7. **Map anchors are an aggregate area overview**, not real-time incident data. Forest/reserve notes describe
   general, publicly-known characteristics — no fabricated statistics. Real pins come only from user reports + live SOS.
8. **DMS / timers only run while a tab is open.** A backgrounded/closed tab won't fire the auto-SOS in real
   time; on reopening, an expired DMS is reported as expired (it does not retroactively blast the alarm).
9. **SOS anti-abuse is simulated locally and is bypassable.** Cancel-no-trace, the pulsing pin and the resolution
   prompt are real UX, but the broadcast, the per-day rate limit and the trust score live in LocalStorage —
   clearing site data resets them. A real deployment must enforce **broadcast/fan-out, rate-limiting and
   reputation on the server**, keyed to an authenticated account, and must never publish cancelled/accidental/test events.
10. **Emergency numbers marked "verify"** (`*3639`, `*6911`) need final confirmation before field use.

These are inherent to a no-backend app and would be resolved by adding a server, push infrastructure, an
authenticated account model, and a native wrapper.

---

*By 3Shamrocks.Studio · Shomer Forest™ · Verified in-browser (Chromium preview) prior to deploy.*
