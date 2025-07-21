# ScoutVex Feature Plans
This folder holds feature plans for ScoutVex, created by Grok.
## Grading-Profit Calculator (Phase 1)
**Goal**: Upload a CSV of trading cards (e.g., year, brand, set, card number, player/title) and display a table with raw and PSA 9 median prices, expected profit, and recommendations (Grade, Sell Raw, Hold).

### Step 1: Bubble.io Setup
- Create a new Bubble app named "ScoutVex" (blank template).
- Set time zone in Settings → General.
- **Checkpoint**: Preview the app; see a blank page with no errors.

### Step 2: Database Setup
- Create **Card** data type: slug (text, primary), year (number), brand (text), set_name (text), card_number (text), player_or_title (text).
- Create **ProfitRow** data type: card (Card), raw_median (number), psa9_median (number), expected_profit (number), recommendation (text).
- **Checkpoint**: Add a test card in App data (e.g., 1977 Topps Star Wars #1) and confirm it appears.

### Step 3: Connect eBay Insights API
- Install API Connector plugin.
- Add eBayInsights API (OAuth 2 User-Agent Flow).
- Call: GetSoldPrices, URL: `https://api.ebay.com/buy/marketplace_insights/v1_beta/item_sales/search`, parameters: q=[keyword], category_ids=213, filter=priceCurrency:USD, limit=100.
- **Checkpoint**: Test API call in Bubble, see JSON with sold prices.

### Step 4: Build Index Page
- Add FileUploader (upl_csv, .csv only), Button (btn_analyze, "Upload & Analyze"), RepeatingGroup (rg_results, type: ProfitRow).
- Display: card slug, raw median, PSA 9 median, expected profit, recommendation.
- **Checkpoint**: Preview page; see uploader, button, and empty table.

### Step 5: Backend Workflows
- Create `process_csv` workflow: delete old ProfitRows, schedule `process_one_row` on CSV rows.
- Create `process_one_row`: create Card, call eBay API, calculate profit (psa9_median * 0.871 - 18 - raw_median), set recommendation.
- **Checkpoint**: Run process_csv with a test CSV; see ProfitRow data in App data.

### Step 6: Export
- Add "Download CSV" button; export ProfitRows.
- **Checkpoint**: Download CSV; verify it matches table data.
