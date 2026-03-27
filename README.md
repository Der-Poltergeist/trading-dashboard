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

## Local Preview

Open `index.html` directly in a browser. Note that the economic calendar API may require a CORS proxy when served from `file://` - it works automatically when hosted on GitHub Pages or any HTTP server.

Alternatively, use any local server:

```bash
npx serve .
# or
python -m http.server 8000
```

## Data Sources

- Economic calendar: [ForexFactory / FairEconomy](https://nfs.faireconomy.media/ff_calendar_thisweek.json)
- Rollover dates: CME Group equity index futures (2nd Friday of quarterly months)
