# RimRunnerConnecticut

Unofficial Connecticut HS boys basketball ratings. Not affiliated with the CIAC or MaxPreps.
Status: **HELD - waiting for hosting.** Planned home: https://rimrunnerconnecticut.web.app/ (Firebase Hosting). The Firebase project could not be
created yet (Google project quota on the hosting account) and Cloudflare Pages is at its project limit. Working preview: this repo's GitHub Pages copy.
Until the site's Firebase project exists, accounts, picks and follows save only in the visitor's browser.

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's Firebase project (accounts, picks, follows).

## Nightly scores

`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 09:30 and 16:00 UTC
(4:30 am / 11:00 am Eastern standard time) and on manual dispatch. Before the Connecticut boys season opens
(`SEASON_START` in index.html = Tue Dec 15, 2026, the CIAC first contest date) the script makes no requests.
From then on it reads boys basketball finals for the last 3 days from the MaxPreps Connecticut scores-by-date page
(team-page fallback), matches them to the schedule by MaxPreps team id + date, records them in `data/results.json` and
bakes scores and W-L records into `index.html`. The page recomputes Elo from those finals. It commits only when data
changed, then deploys (index.html + logos/) to Cloudflare Pages if the `CLOUDFLARE_API_TOKEN` repository secret is set.
