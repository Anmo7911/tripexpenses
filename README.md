# Bharat Way — Trip Dashboard

First Draft was use in 25 October 2025
A single-page group-trip dashboard: expenses, itinerary/route tracking, squad (member) management, a chat with polls, a trip checklist, digital ID cards, POS receipts, and a public verification gateway for scanning those IDs/receipts — backed by Supabase and a single Vercel serverless function.

**Live demo:** https://bharatway.vercel.app/

---

## ✨ Pages

| File | Purpose |
|---|---|
| `index.html` | Main app — a tabbed single-page dashboard covering Spend, Places/Route, Squad Members, Notices, Trip Checklist, Transactions, POS receipts, Gallery, and Archives |
| `admin.html` | Admin control panel — manage members, places, transactions, budget, archives, checklist, broadcasts, and app settings |
| `verify.html` | Public "Official Verification Gateway" — scan/lookup a member ID card, POS invoice, or official notice via QR code or reference number |
| `announcement.html` | Fullscreen animated announcement screen for an upcoming trip |
| `loader.html` | Branded splash/loading screen (with multi-script font support for Indian languages) |
| `maintenance.html` | Static "system under maintenance" page |

> Note: `vite.config.js` still lists `squad.html` and `route.html` as separate build entry points, but those pages no longer exist as standalone files — that functionality now lives inside the "Squad" and "Places/Route" tabs in `index.html`. You'll likely want to update `vite.config.js` to match (see below).

## 🏗️ Project Structure

```
trip-dashboardv2/
├── api/
│   └── index.js              # Single Vercel serverless function — all backend actions
├── public/
│   └── manifest.webmanifest  # PWA manifest
├── scripts/
│   └── import-sheet-csv.mjs  # One-off importer: legacy Google Sheet CSV exports -> Supabase
├── sheet-export/              # CSV exports of the original Google Sheet (seed/reference data)
│   ├── Archives.csv
│   ├── Budget.csv
│   ├── Members.csv
│   ├── Messages.csv
│   ├── Places.csv
│   ├── Settings.csv
│   └── Transactions.csv
├── admin.html
├── announcement.html
├── index.html
├── loader.html
├── maintenance.html
├── verify.html
├── schema.sql                 # Supabase/Postgres table definitions
├── package.json
├── vercel.json                 # Vercel routing config
└── vite.config.js              # Vite build config (see note above)
```

## 🧰 Tech Stack

- **Frontend:** Vanilla HTML/JS, [Tailwind CSS](https://tailwindcss.com) (CDN), [Lucide](https://lucide.dev) icons, [Chart.js](https://www.chartjs.org/) (spend trends), [QRCode.js](https://davidshimjs.github.io/qrcodejs/) (ID card QR codes), [html2canvas](https://html2canvas.hertzen.com/) (exporting POS receipts as images) — bundled with [Vite](https://vitejs.dev)
- **Backend:** One Vercel serverless function (`api/index.js`) that dispatches on an `?action=` query param (e.g. `dashboard`, `login`, `expense`, `admin-save-member`, `verify`, etc.)
- **Database:** [Supabase](https://supabase.com) (Postgres) — see `schema.sql` for tables like `settings_sheet`, `members_sheet`, `transactions_sheet`, `places_sheet`, `budget_sheet`, `messages_sheet`, `archives_sheet`, `app_meta`
- **File storage:** Supabase Storage (signed URLs for photos, guidelines, trip reports, etc.)
- **External integrations:** Optional Google Apps Script endpoints for official notices and past-trip ledger lookups
- **Hosting:** [Vercel](https://vercel.com)

## 🚀 Getting Started

### Prerequisites
- [Node.js](https://nodejs.org/)
- A [Supabase](https://supabase.com) project

### Installation

```bash
git clone https://github.com/Anmo7911/trip-dashboardv2.git
cd trip-dashboardv2
npm install
```

### Database Setup

Run `schema.sql` against your Supabase project's Postgres database (via the Supabase SQL editor or CLI) to create the required tables.

### Environment Variables

Create a `.env` file (or set these in your Vercel project settings) with:

```
SUPABASE_URL=...
SUPABASE_SERVICE_ROLE_KEY=...
SUPABASE_STORAGE_BUCKET=trip-assets   # optional, defaults to "trip-assets"
GOOGLE_SCRIPT_URL=...                 # optional, for official notices
NOTICE_SCRIPT_URL=...                 # optional, for notice verification
```

### Importing Legacy Data (optional)

If migrating from the original Google Sheets version, export each sheet as CSV into `sheet-export/` and run:

```bash
node --env-file=.env scripts/import-sheet-csv.mjs Settings sheet-export/Settings.csv
node --env-file=.env scripts/import-sheet-csv.mjs Members sheet-export/Members.csv
# ...repeat for Transactions, Places, Budget, Messages, Archives
```

### Running Locally

```bash
npm run dev
```

### Deployment

Deployed on [Vercel](https://vercel.com); `vercel.json` rewrites `/api/*` requests to the serverless function in `api/`.

```bash
vercel deploy
```

## 🤝 Contributing

1. Fork the repo
2. Create a feature branch (`git checkout -b feature/my-feature`)
3. Commit your changes
4. Open a pull request

## 📜 License

© 2026 The Anch Technologies LTD. All rights reserved.
This project is proprietary software. See [LICENSE](./LICENSE) for details.
