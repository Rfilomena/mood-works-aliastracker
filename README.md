# Mood Works Alias QA

Small read-only dashboard for reviewing producer alias counts against Spotify's 35-alias cap.

## Replit Import

This repo is prepared for a non-technical Replit import. Import the repository into Replit, then click Run. The included `setup.js` bootstrap writes the full app files into the Replit workspace and starts the dashboard.

After the first run, Replit will contain the normal app structure:

- `src/`
- `public/`
- `data/app-state.json`
- `package.json`

## What It Does

- Reads the Spotify weekly report CSV and the Valid Artist Form CSV.
- Joins aliases to producers by Spotify artist URI first, then artist name.
- Groups the same producer name across multiple company/email rows.
- Shows all producer category totals without clicking.
- Sorts the global dashboard by `PRIORITIZED` aliases descending.
- Expands each producer to show artist-level detail.
- Keeps suspected scraper results in a review queue until accepted.
- Supports manual confirmed third-party counts per producer.

## Replit Setup

Upload the two CSV files into the Replit `data/` folder as:

- `data/valid-artist-form.csv`
- `data/weekly-report.csv`

In Replit Secrets, set:

```bash
VALID_ARTIST_CSV=data/valid-artist-form.csv
WEEKLY_REPORT_CSV=data/weekly-report.csv
ENABLE_WEEKLY_SCAN=false
YOUTUBE_API_KEY=
```

The YouTube key is optional. MusicBrainz and Wikidata can run without it.

## Scraper Notes

The scraper checks structured public sources and writes findings to the review queue:

- MusicBrainz artist aliases, relationships, and recording credits.
- Wikidata labels, aliases, and pseudonym claims.
- YouTube Data API search results when `YOUTUBE_API_KEY` is configured.

Scraper results are never counted immediately. They only become `SUSPECTED 3RD PARTY` after a reviewer accepts them.

## Production Note

The current first version stores review state in `data/app-state.json`. That is fine for the first Replit preview, but long-term production should move this state into Replit SQL/Postgres so review decisions persist safely across deploys.
