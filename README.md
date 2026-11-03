# Trading Dashboard - Public View

A standalone, static HTML dashboard for traders showing world clocks, economic calendar, and futures contract rollover information.

## Features

- **World Clock** - Live clocks for Chicago, New York, Frankfurt, Dubai, Tokyo
- **Economic Calendar** - Weekly events from ForexFactory with impact filtering
- **Contract Rollover** - MNQ/MES futures rollover tracking with countdown

## Deploy to GitHub Pages

1. Create a new GitHub repository (e.g., `trading-dashboard`)
2. Push this folder's contents to the `main` branch:
   ```bash
   cd public-dashboard
   git init
   git add .
   git commit -m "Initial deploy"
   git remote add origin https://github.com/<USER>/trading-dashboard.git
   git push -u origin main
   ```
3. Go to **Settings > Pages** in the repository
4. Under **Source**, select `main` branch and `/ (root)` folder
5. Click **Save** - the site will be live at `https://<USER>.github.io/trading-dashboard/`

## Economic Calendar: news.json (GitHub Action)

The page does not call ForexFactory or any CORS proxy from the browser. It only loads `news.json` from its own
directory. The workflow `.github/workflows/news.yml` (part of this folder, i.e. at the root of the Pages repository)
fetches the weekly ForexFactory feed every 4 hours on GitHub's servers, validates it and commits `news.json`
(`fetched_at` = last successful run, shown as "Stand" on the page; `changed_at` = last change of the events).
A failed run (feed unreachable, empty or invalid) leaves `news.json` untouched, and the page keeps showing the news
it already has. If `news.json` is missing or invalid on the first load, the page shows "News derzeit nicht
verfügbar" and everything else keeps working.

Setup in the Pages repository (once):

1. Push the contents of this folder including `.github/workflows/news.yml` (see "Deploy to GitHub Pages").
   Pushing a file under `.github/workflows/` over HTTPS needs a token that may change workflows: a classic personal
   access token with the `workflow` scope (or a fine-grained token with "Workflows: Read and write"). Without it
   GitHub rejects the push; alternatively add the workflow file once through the GitHub web UI
   (**Add file > Create new file**, path `.github/workflows/news.yml`).
2. No repository-wide permission change is needed: the workflow requests `permissions: contents: write` for its
   own `GITHUB_TOKEN`; leave **Settings > Actions > General > Workflow permissions** at the default.
3. **Actions > "News aktualisieren" > Run workflow** once to create `news.json` immediately; afterwards it runs on
   its schedule (scheduled runs use the default branch and can be delayed during high load).
4. In a public repository GitHub automatically disables scheduled workflows after 60 days without repository
   activity. GitHub's documentation does not say whether the workflow's own `GITHUB_TOKEN` commits count as
   activity, so do not rely on them. If the "Stand" on the page stops moving, re-enable the workflow under
   **Actions > "News aktualisieren" > Enable workflow** (or `gh workflow enable news.yml`).

## Local Preview

Open the folder through a local HTTP server (below). Opened directly via `file://`, the browser blocks loading
`news.json` and the calendar shows "News derzeit nicht verfügbar". For a local test, download the feed once with
the workflow's script or copy a `news.json` from the Pages repository into this folder.

Alternatively, use any local server:

```bash
npx serve .
# or
python -m http.server 8000
```

## Data Sources

- Economic calendar: [ForexFactory / FairEconomy](https://nfs.faireconomy.media/ff_calendar_thisweek.json), fetched by the GitHub Action into `news.json`
- Rollover dates: CME Group equity index futures (2nd Friday of quarterly months)
