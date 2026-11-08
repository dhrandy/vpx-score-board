# Pinball High Scores

A self-hosted dashboard that shows the last table played with its score, a high scores leaderboard, and recently played tables for a Visual Pinball cabinet.

## What it is

A single static page (`index.html`). No backend, no database. This page is a mockup with sample data. The live version pulls real scores from the cabinet and refreshes on its own (see Auto-populate below).

## Run it with Docker

```bash
docker compose up -d
```

Then open `http://<host>:8745`.

The compose file runs stock `nginx:alpine` on port 8745. The container fills the folder itself: on start it downloads `index.html` from this repo into `/DATA/AppData/vpx-score-board/` on the host, so you do not place the file by hand. Restart the stack to pull the latest page. On a CasaOS / Dockhand host, delete the old stack and create a fresh one with this compose file; applying over the old stack may keep the old container.

## Auto-populate from the cabinet

The board is meant to fill itself from the vpin server, not from manual entry. The vpin-bridge exposes the cabinet's high scores at `GET /api/highscores` (same host and `X-Api-Key` auth as its other endpoints). It forwards to the cabinet's VPin Studio highscores export and returns JSON. The fullstack app (`pinball-high-scores-app`) pulls from that endpoint and displays the live scores, refreshing on its own. Manual score editing stays available for tables where the bridge has no score or a score needs correcting.

## Login and score editing

The static page has no login. The fullstack app gates viewing behind one shared login (created on first open) and, once signed in, lets you change scores and tag player names and initials on tables that never record them. It also shows a cabinet status light (green when the cabinet is on, offline when it is off) that flips on its own when you power the cabinet up.

## Files

- `index.html` - the dashboard (single self-contained page)
- `docker-compose.yml` - nginx service on port 8745
- `README.md` - this file

## Notes

- All scores in the mockup are sample data, clearly labeled on the page.
- The page is designed to fit on one screen with no scrolling on a laptop, so it also works as a wall display.
