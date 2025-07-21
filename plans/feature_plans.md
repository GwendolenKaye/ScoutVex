# ScoutVex Feature Plans
This folder holds feature plans for ScoutVex, created by Grok to guide development and Kimi AI’s implementation in Bubble.io.

## Grading-Profit Calculator (Phase 1)
**Goal**: Upload a CSV of trading cards (e.g., year, brand, set, card number, player/title) and display a table with raw and PSA 9 median prices, expected profit, and recommendations (Grade, Sell Raw, Hold). This feature saves collectors hours by automating grading decisions.

### Step 1: Bubble.io Setup
- Create a new Bubble app named "ScoutVex" (blank template).
- Set time zone in Settings → General (e.g., America/New_York).
- **Checkpoint**: Preview the app; see a blank page with no errors.

### Step 2: Database Setup
- Create **Card** data type: slug (text, primary), year (number), brand (text), set_name (text), card_number (text), player_or_title (text).
- Create **ProfitRow** data type: card (Card), raw_median (number), psa9_median (number), expected_profit (number), recommendation (text).
- **Checkpoint**: Add a test card in Bubble’s App data (e.g., 1977 Topps Star Wars #1, slug: 1977-topps-starwars-1) and confirm it appears.

### Step 3: Connect eBay Insights API
- Install API Connector plugin in Bubble.
- Add eBayInsights API (OAuth 2 User-Agent Flow).
- Call: GetSoldPrices, URL: `https://api.ebay.com/buy/marketplace_insights/v1_beta/item_sales/search`, parameters: q=[keyword], category_ids=213 (Baseball, adjust for Star Wars), filter=priceCurrency:USD, limit=100.
- Get keys from developer.ebay.com; paste in Bubble.
- **Checkpoint**: Test API call in Bubble’s API Connector; see JSON with sold prices (e.g., itemSales array).

### Step 4: Build Index Page
- On index page, add FileUploader (upl_csv, .csv only), Button (btn_analyze, "Upload & Analyze"), RepeatingGroup (rg_results, type: ProfitRow).
- Display in RepeatingGroup: card slug, raw_median, psa9_median, expected_profit (green if >0, red if <0), recommendation.
- **Checkpoint**: Preview page; see uploader, button, and empty table.

### Step 5: Backend Workflows
- Enable Backend Workflows in Bubble (Settings → API).
- Create `process_csv` workflow: delete old ProfitRows, schedule `process_one_row` on CSV rows.
- Create `process_one_row`: create Card if needed, call eBay API for raw and PSA 9 prices, calculate expected_profit (psa9_median * 0.871 - 18 - raw_median), set recommendation (Grade if profit > 20, Sell Raw if > 0, else Hold).
- **Checkpoint**: Upload test.csv (e.g., 1977,Topps,Star Wars,1,Luke Skywalker); see ProfitRow data in App data.

### Step 6: Export Results
- Add "Download CSV" button; export ProfitRows as CSV.
- **Checkpoint**: Download CSV in preview; verify it matches table data.

## Household Card Tracker (Phase 2)
**Goal**: Track cards in boxes with privacy-first box-tagging and multi-user access, helping collectors find sold cards quickly without exposing home details.

### Step 1: Database Additions
- Create **FamilyGroup**: name (text), owner (User), members (list of Users).
- Create **StorageBox**: family_group (FamilyGroup), label (text), photo (file, optional), x_pct (number), y_pct (number).
- Add to **ProfitRow**: storage_box (StorageBox).
- **Checkpoint**: Add a FamilyGroup and StorageBox in App data.

### Step 2: Privacy Rules
- Set rules: Only users in the same FamilyGroup can view/edit ProfitRows and StorageBoxes.
- **Checkpoint**: Test with two users; confirm data is private.

### Step 3: Inventory Page
- Create inventory page with Image (for box photo) and RepeatingGroup (StorageBoxes).
- Add clickable icons for card locations (use x_pct, y_pct).
- **Checkpoint**: Upload a photo, add a StorageBox, see icon in preview.

### Step 4: Multi-User Access
- Add Signup/Login Popup on index page.
- Assign users to FamilyGroup on signup.
- **Checkpoint**: Two users in same FamilyGroup can edit shared data.

### Monetization Plan
- **Subscription**: $5–$10/month for premium features (via Bubble’s Subscription Payments plugin).
- **Concierge Service**: $20 per CSV analysis, manually run by you.
- **Checkpoint**: Test subscription flow in Bubble; confirm payment works.
- **Checkpoint**: Test subscription flow in Bubble; confirm payment works.
