# Ice Level — Fantasy Hockey Draft Board

A single-page fantasy hockey draft prospect evaluator for points leagues: scoring settings, CSV import, a live multi-team draft tracker with a Sleeper-style draft board, and per-player projections.

## Running it

This is a static site — no build step, no server-side code.

- **Locally:** just open `index.html` in a browser.
- **GitHub Pages:** already enabled for this repo (see repo Settings → Pages). Live at the URL shown there.

## Files

- `index.html` — the entire app (HTML/CSS/JS in one file).
- `nhl_2025_26_stats.csv` — real final 2025-26 NHL stats for the top ~200 skaters and ~40 goalies, sourced from the NHL's own stats API. Load it from the app's Import panel, or paste your own CSV in the same column format.
- `player-ids.json` — maps `"player name|team"` (lowercased) to NHL player IDs, used to load real headshots from the NHL's mugshot CDN (`assets.nhle.com`). Falls back to a colored initials avatar for any player not in this file.
- `2026-27-outlook-notes.txt` — free-text context (trades, injuries, contract situations) surfaced in the app's Outlook panel.

## Why this isn't an Artifact anymore

The original version of this tool ran as a Claude Artifact, which sandboxes the page (strict CSP) and blocks loading images from arbitrary hosts and microphone access from an embedded iframe — so real player headshots, real team logos, and voice search couldn't work there. This plain static site has none of those restrictions:

- Team logos load live from ESPN's CDN (`a.espncdn.com`), with a colored fallback chip if a logo fails to load.
- Player headshots load live from the NHL's own CDN, keyed by `player-ids.json`, falling back to an initials avatar.
- Voice search (the 🎤 button) uses the browser's native Speech Recognition API directly on the page, which needs the browser's own microphone permission prompt — no iframe blocking it.

## Data notes

- Everything (scoring settings, imported players, your draft picks, team names) is saved to `localStorage` in your browser only — nothing is sent to a server.
- A few players who changed teams over the 2026 offseason are still listed under their 2025-26 team in the stats (see the Outlook panel for specifics) since that CSV reflects actual last-season performance.
- Rookie projections use a simple, transparent heuristic (a draft-pick-based scoring curve blended with this season's position averages) — not a licensed or precision projection system. Supply your own `proj_*` stat columns in a CSV import if you want to override it with real projections.
