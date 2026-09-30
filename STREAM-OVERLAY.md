# LiveFrame Stream Overlay

Transparent 1920×1080 overlay with three scenes, a live Hyperliquid ticker, the sponsor logo band, a rotating Sponsor Spotlight with QR referral codes, and sponsor shout-outs.
It uses the same Supabase config as the Daily Show Builder (under its own `stream` key) and the same edit key.

## Pages (in this repo → https://jonah-blake.github.io/liveframe-overlays/…)
| Page | Use it for |
|---|---|
| `control.html` | **Stream Overlay Control** — edit titles, descriptions, name plates, sponsors, referral links/QR, ticker; switch the on-air scene; fire shout-outs |
| `builder.html` | Daily Show Builder (unchanged, now links to the control page and never overwrites the overlay's settings) |
| `overlay.html` | Evmux / single source — follows the **On-air scene** picked in the control page |
| `overlay-just-chatting.html` · `overlay-news.html` · `overlay-podcast.html` | Fixed-scene versions (one per OBS/Evmux scene) |

## Evmux
Layers → Add New Layer → **Web Source** → *Use link* → e.g. `https://jonah-blake.github.io/liveframe-overlays/overlay-news.html`
Frame 1920 × 1080, position 0,0, bring to front. One layer per scene with the matching page, or one layer with `overlay.html` switched from the control page.

## OBS
Sources → + → **Browser** → URL = the same GitHub Pages link (or tick *Local file* and pick a downloaded copy — it still syncs). 1920 × 1080, custom FPS 60, put it at the top of the source list, cameras underneath.

## Changing text
1. Open `control.html` (bookmark it; works on your phone too).
2. Enter your edit key once (top right — same key as the builder, remembered on the device).
3. Pick a scene tab, edit **Title**, **Description** (one line per row; rows rotate every 8 s) and window **name plates** — the preview updates as you type.
4. **Push to stream** (Ctrl/⌘ + Enter). Every overlay updates within ~3 seconds.

## Sponsors
- Toggle each sponsor on/off, reorder, rewrite taglines, add new ones (logo URL).
- **Referral link for QR code** → a scannable QR appears in the Spotlight and the shout-out. Pre-loaded: Markets.xyz (`…/u/jonahblake?ref=jonahblake`) and fomo (`fomo.family/r/JonahBlake`).
- **Promo / referral code** → shows a `CODE XXXX` chip (Spotlight, shout-out, next to the logo in the band).
- **Sponsor shout-out** buttons take over the green title bar for 8–30 s — use during reads.
- Markets.xyz brand rules followed: official kit files, lockup ≥160 px wide with clear space, always "Markets.xyz". Get their sign-off on the "Official partner" wording.

## Ticker
Live from Hyperliquid (websocket + REST fallback): the Markets.xyz by Kinetiq markets first, then top crypto perps by 24h volume or your pinned list.

## Camera window positions
| Scene | Window | x | y | w | h |
|---|---|---|---|---|---|
| Just Chatting | Main cam | 34 | 36 | 1380 | 776 |
| News | Screen share | 34 | 36 | 1306 | 776 |
| News | Host cam | 1366 | 36 | 520 | 375 |
| News | Bottom-right (when set to camera) | 1366 | 437 | 520 | 375 |
| Podcast | Host | 34 | 36 | 913 | 776 |
| Podcast | Guest | 973 | 36 | 913 | 776 |

## How it stores settings
`lf_update_config` replaces the whole config on every save, so both pages now read the current config first and only change their own part: the builder keeps `stream`, the control page only writes `stream`. No database changes were made.
