# Wing Watch: Boise

A bird-themed dashboard of new, rare, and out-of-season wildlife near Boise, updated automatically
from iNaturalist and eBird. Hosted free on GitHub Pages.

## Set it up (about 5 minutes)

1. **Create a new GitHub repo** and push this folder to it.
2. **Get a free eBird API key** at https://ebird.org/api/keygen (log in with your eBird account).
   In the repo go to *Settings → Secrets and variables → Actions → New repository secret*, name it
   `EBIRD_API_KEY`, and paste the key. (iNaturalist needs no key.)
3. **Turn on Pages:** *Settings → Pages → Build from branch → `main` / `/docs`*.
4. **Run it once:** *Actions → Update sightings → Run workflow*. After that it refreshes twice a day.
   Your page will be at `https://<your-username>.github.io/<repo-name>/`.

The first run takes a few minutes because it builds a 5-year baseline for each group. Later runs reuse a cached baseline (refreshed weekly) and are much faster.

## How "unusual" is decided

For each group the script compares the last `window_days` of sightings to the previous `baseline_years` of
research-grade iNaturalist records inside the same circle:

| Flag | Meaning |
|---|---|
| First here! | no baseline record at all |
| Rare visitor | `rare_threshold` or fewer baseline records |
| Off-season | usually present (`offseason_min_baseline`+ records) but none in this time of year (±1 month) |
| eBird rarity | eBird's own "notable" list (birds only) |

"Needs ID" means no research-grade observation yet, so the ID could still change. These flags are a
nudge to go look, not verified records.

## Tweaking

Everything lives in `config.json`: location, radius, window, thresholds, and the groups (which iNaturalist
taxa go in each tab). Taxon names are looked up by name at runtime. Plants are not included.

Test locally with `EBIRD_API_KEY=... python3 scripts/fetch.py`, then open `docs/index.html` via
`python3 -m http.server -d docs`.

Data: iNaturalist (https://www.inaturalist.org) and eBird (https://ebird.org). Photos are shown only when the photographer used an open licence, and are credited on the card.

## Notes for this site

- Birds only (iNaturalist birds plus eBird rarities). Add more groups in `config.json` the same way the Santa Barbara site does.
