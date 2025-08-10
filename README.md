# Ultra System Elite — Multi-Sport Betting Platform (Starter Scaffold)

This repo rebuilds your multi-sport betting system with:
- Google Sheets dashboard powered by Google Apps Script (menu, sheets, automation)
- Python ingestion + ML ensemble scaffold (MLB + NFL/NBA/NHL/NCAAF, easily extended to more)
- VS Code automation via tasks (deploy Apps Script, fetch data, run predictions, publish)
- Config-driven sports registry (add new sports in config/sports.yml)

Core flow:
1) Python fetches sports data, computes predictions (6-model ensemble or baseline), and posts JSON to Apps Script.
2) Apps Script writes predictions to “<SPORT> Analysis” sheets and “Early Game Opportunities”.
3) Triggers/VS Code tasks keep the sheet up to date.

Quick start
1. Prereqs
   - Node 18+ (for clasp)
   - Python 3.10+
   - VS Code
   - Google account with Sheets access

2. Setup
   - Python env:
     - python -m venv .venv && source .venv/bin/activate
     - pip install -r python/requirements.txt
   - Node:
     - npm install
   - Copy .env.example to .env and fill WEBAPP_URL, ODDS_API_KEY, OPENWEATHER_API_KEY (optional for weather)

3. Google Apps Script (from VS Code)
   - npx clasp login
   - Update clasp.json with your scriptId or create a new GAS project
   - npx clasp push (or VS Code task: GAS: Push)
   - In the GAS UI: Deploy > New deployment > Web app > Execute as: Me; Access: Anyone with the link
   - Copy the Web App URL to WEBAPP_URL in .env

4. Initialize Sheets and triggers
   - Open the target spreadsheet (or let GAS create sheets on first post)
   - From the custom menu “🚀 Ultra System Elite”, click “🔄 Initialize Enhanced System”

5. Train models (optional)
   - Place training CSVs in data/{sport}_training.csv
   - Run: python scripts/train.py --sport MLB --input data/mlb_training.csv (repeat per sport)

6. Run predictions and publish
   - VS Code task: “Predict + Publish (Today)”

Docs
- docs/ARCHITECTURE.md — System design, data flow, ensemble strategy
- docs/TRAINING.md — Model training and artifacts
- docs/ADDING_SPORTS.md — Add/enable any sport via config
- docs/SHEETS_SCHEMA.md — Sheet columns/structure

Security
- Never commit .env
- Use GitHub Actions secrets for CI if you schedule runs
- Apps Script validates inbound JSON in doPost

Extending to other sports
- Add a sport in config/sports.yml
- Train a model for it (optional); if omitted, baseline + market calibration still computes EV/Kelly
- The Apps Script will auto-create a new “<SPORT> Analysis” sheet when data is posted
