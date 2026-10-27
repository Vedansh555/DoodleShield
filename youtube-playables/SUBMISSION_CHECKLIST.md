# Doodle Shield – YouTube Playables submission package

## What is in this folder
| File | Purpose |
|---|---|
| `doodle-shield-playables.zip` | **Upload this** in the Playables Developer Portal (`index.html` is at the ZIP root, 16 KB). |
| `index.html` | The whole game (single file, no other assets). Same file that is inside the ZIP. |
| `thumbnails/` | Portrait, landscape and square cover images. |
| `metadata.md` | Title, descriptions, keywords to paste into the portal. |
| `SUBMISSION_CHECKLIST.md` | This file. |

## Requirements – how the game meets them, and how I checked
| Requirement | How it is met | Checked how |
|---|---|---|
| Bundle < 15 MB (hard cap 30 MB) | One 46 KB HTML file, ZIP ≈ 16 KB | File size |
| Fewer than 8,000 files, relative paths | 1 file | ZIP listing |
| `index.html` at ZIP root | Yes | ZIP listing |
| SDK script loads before game code | `<script src="https://www.youtube.com/game_api/v1">` is first | Source check |
| `firstFrameReady()` before `gameReady()` | Both called after the first rendered frame, in that order | **Mock SDK test** – call order recorded |
| Pause / resume | `system.onPause/onResume` stop all updates, drawing, timers, input and suspend audio | **Mock SDK test** – time frozen, input ignored, resumed |
| Do not rely on Page Visibility | Only used when the SDK is absent (local testing) | Source check |
| Audio follows system mute | `isAudioEnabled()` + `onAudioEnabledChange()` suspend/resume the AudioContext; no in-game mute button | **Mock SDK test** |
| Cloud save < 3 MB | `game.saveData()` stores `{"level":N}` (≈ 12 bytes); `loadData()` on start | **Mock SDK test** – a saved level 7 loaded correctly; win saved level + score |
| Score submission | `engagement.sendScore({value})` on each level clear | **Mock SDK test** |
| No external calls / links | Only the SDK URL appears in the file; no fetch/XHR/open/location | Source scan |
| No custom exit / mute / agreement buttons | None | Source check |
| Portrait + landscape, no lost state | Canvas letterboxes the 360×640 play area; resize redraws without resetting | Thumbnails rendered at 1080×1920, 1920×1080, 1024×1024 |
| Interactive quickly | Menu is interactive on the first frame (no assets to load) | Observed |
| Error reporting | `health.logError()` on uncaught errors | Mock SDK test (hook present) |

## NOT verified (needs YouTube / real devices)
- Google's **Test Suite** and official **certification review** – only you (with approved developer access) can run these.
- Real phone **touch drawing** feel, and real-device performance.
- How the sound actually *sounds* and its loudness on phones (levels were measured, not listened to by me).
- Payout / ads: **no ads are integrated**. Google's monetization terms were not published when I checked; add `requestInterstitialAd` / `requestRewardedAd` only after approval and after reading the current docs.
- Difficulty at very high levels was only checked with a simple test bot, not real players.

## How to submit
1. Fill in the **Playables interest form** (Google for Developers → YouTube Playables) with the game name and description from `metadata.md`.
2. Once approved, open the **Developer Portal**, choose *Add a new game*, and upload `doodle-shield-playables.zip` plus the thumbnails and metadata.
3. Open the portal's **Verify and test** tab and run the **Test Suite** on desktop web, mobile web, Android and iOS.
4. Fix anything it reports, then submit for certification.

## Notes on the third-party "submission guide" (checked against Google's own pages)
- Two stages are right: interest form first, Developer Portal after approval.
- The exact interest-form field list (experience, hosting link, etc.) comes from a third-party site; Google's page only says "game or company submission". Check the real form.
- Orientation: Google's guidance says the game must support both portrait and landscape and must not lock orientation. Do NOT choose "portrait only"; choose automatic/dynamic if asked.
- AdSense Publisher ID / earnings figures: unconfirmed. Google describes monetization as a pilot with no published terms. No ads are built in. Do not enter an ID until Google asks.
- Thumbnails: square 1:1 and 16:9 are included (plus portrait).
- Hosting link: some forms may want an HTTPS staging URL. The game is not hosted anywhere yet (GitHub Pages / Netlify would work: upload `index.html`).