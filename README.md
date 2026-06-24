# Innovien World Cup — Standings Dashboard

A single-file, Vercel-hosted standings page for the Innovien World Cup contest
(June 2 – July 10, 2026). Mirrors the DLT dashboard workflow: drop the Power BI
workbook, run one script, push `index.html`.

## Files

- `index.html` — the deployed page. Self-contained (inline CSS + vanilla JS, no build step, no external CDNs). Standings live in an embedded JSON block between `/*DATA_START*/` and `/*DATA_END*/`.
- `refresh.py` — reads `data/pbi-source.xlsx`, computes standings, and patches `index.html` in place.
- `data/pbi-source.xlsx` — **canonical weekly input.** The "Recruiting and Sales Power BI Dashboard Data" export, saved with this exact filename (overwrite each week).

## Weekly refresh

1. Export the "Recruiting and Sales Power BI Dashboard Data" workbook from Power BI (recruiter tab current).
2. Save it as `world-cup-dashboard/data/pbi-source.xlsx`, overwriting last week's.
3. Run: `python3 refresh.py` (needs `openpyxl`: `pip install openpyxl`). It prints the top 5 and the Account-Breaker count.
4. Push the updated `index.html` to GitHub (see below). Vercel redeploys in ~30s.

`refresh.py` auto-detects the window end as the latest activity date in `Contact Activity` (capped at July 10) and applies 2x Power Week multipliers automatically once those weeks arrive. The "Refreshed" date uses the machine's local date.

## One-time GitHub + Vercel setup (new repo)

1. Create a new GitHub repo, e.g. `innovien-world-cup` (private).
2. Upload `index.html` (the only file Vercel needs to serve). Optional: add `refresh.py`, `README.md`, and `data/` for record-keeping, but **don't** commit `pbi-source.xlsx` if the repo is shared and the data is sensitive.
3. In Vercel: New Project → import the repo → Framework Preset **Other** → no build command, output dir = root → Deploy.
4. You get a URL like `https://innovien-world-cup.vercel.app/`. That's the shareable link.

### Updating each week (same as DLT)
Open `index.html` in the GitHub web UI → pencil/Edit → paste the new content → Commit to `main`. Vercel auto-deploys.

## Scoring (as implemented in refresh.py)

| Activity | Points | Source sheet |
|---|---|---|
| Meeting (set + gone off) | 35 (10 + 25) | Contact Activity (`Went Off = Y`) |
| Candidate screen | 10 | Recruiter Activity (weekly) |
| Sub (to client) | 25 | Recruiter Activity (weekly) |
| Start | 75 (both AM & recruiter) | ESF Details |
| Power Weeks (Jun 29–Jul 3, Jul 6–Jul 10) | 2x | applied by date |

**Still manual / flagged (not in the export):** DUB CLUB (75), reference calls (150), Pre-Stack bonuses (+25), and **Account Breaker** start upgrades (75 → 300) — the page lists the eligible Top-20/Federal starts but does not upgrade them until leadership confirms the official target-account list. To bake Account Breaker in, edit the start-scoring block in `refresh.py`.

Meetings are scored at 35 (set + gone off). To switch to held-only (25), change `PTS_MEETING_SET = 0` at the top of `refresh.py`.
