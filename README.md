# 💰 Budget Buddy

A fully self-contained, offline-capable personal finance tracker with Bitcoin wallet support and a comprehensive multi-month budget planner — all in a **single HTML file**. No installation, no server, no dependencies to manage. Just open it in a browser.

---

## Features

### 💵 Transaction Tracker
- Add income and expense transactions with category, amount, and notes
- Tap any transaction to edit or delete it
- Live doughnut chart showing expense breakdown by category
- Running income, expense, and balance totals
- All data persists in browser `localStorage`

### 📋 Budget Planner
- Set up a multi-month budget across any date range
- Configure projected income with frequency support (monthly, weekly, bi-weekly, yearly)
- Adjust income for individual months or date ranges
- Create and manage expense categories with default budgets
- Override budgets per category for specific month ranges
- Track actual spending against budgeted amounts
- Per-month cards showing surplus/deficit and actual balance
- Variance colour-coded green/red at a glance

### ₿ Bitcoin Wallet
- Add multiple Bitcoin wallets with custom labels
- Full **8 decimal place** precision (down to 1 satoshi = 0.00000001 BTC)
- Live USD conversion via CoinGecko public API (no API key required)
- 24-hour price change badge (colour-coded green/red)
- Auto-refreshes price every 60 seconds while Online
- Last known price cached to `localStorage` — visible even when Offline
- Per-wallet USD value displayed alongside BTC amount

### 🔌 Online / Offline Mode
- Toggle pill in the top bar switches the app between modes
- **Offline** (default) — zero network requests, everything from local cache
- **Online** — enables live BTC price feed and auto-refresh
- Refresh button is safely blocked with a toast when in Offline mode

### 💾 Save / Export / Import
- **Save** — flushes all data (transactions + BTC wallets + price cache) to `localStorage`
- **Export** — downloads a unified `budgetbuddy-export.json` bundle containing everything
- **Import** — restores transactions, BTC wallets, and cached price from a bundle file
- Backwards-compatible with legacy transaction-only JSON files

---

## Getting Started

1. Download `BudgetBuddy.html`
2. Open it in any modern browser (Chrome, Brave, Firefox, Safari, DuckDuckGo)
3. That's it — no setup required

> **Mobile (Android):** Copy the file to your **Downloads** folder for the most reliable file picker access. Opening from nested folders like `Documents/Budget/` can cause import issues in some browsers due to Android `content://` URI restrictions.

---

## Usage Guide

### Adding a Transaction
Tap the large **＋** button → enter amount, select Income or Expense, pick a category, add an optional note → **Save**.

### Using the Bitcoin Panel
1. Tap **₿ Bitcoin** to open the panel
2. Enter a wallet label and your BTC balance → **+ Add**
3. Toggle **Online** in the top-right to fetch the live price
4. Your USD value updates automatically

### Exporting Your Data
Tap **Export** → saves `budgetbuddy-export.json` to your Downloads. This file contains your full financial snapshot including BTC wallets.

### Importing Your Data
Tap **Import** → select your `budgetbuddy-export.json`. The app restores all transactions and BTC wallets in one step.

### Using the Planner
Tap **Plan** → opens the Budget Planner modal. Work through the numbered sections top to bottom:
1. Set start year, number of months, and default income
2. Adjust income for specific months if it varies
3. Add expense categories (Rent, Groceries, etc.)
4. Set per-category budgets for specific month ranges
5. Log actual spending as you go
6. View the monthly overview cards at the bottom

---

## File Structure

Everything lives in a single file:

```
BudgetBuddy.html
├── <style>           — All CSS (tracker + Bitcoin panel + planner)
├── HTML              — App layout, Bitcoin panel, transaction sheet, planner modal
├── <script module>   — Tracker: transactions, chart, save/export/import (ES module)
├── <script>          — Bitcoin: wallets, live price, online/offline toggle
└── <script>          — Planner: multi-month budget planning engine
```

---

## Data Storage

All data is stored in your browser's `localStorage` under these keys:

| Key | Contents |
|---|---|
| `myBudgetBuddy.transactions.v1` | Transaction list |
| `myBudgetBuddy.btcWallets.v1` | Bitcoin wallet list |
| `myBudgetBuddy.btcPrice.v1` | Cached BTC price + timestamp |
| `budgetAppData` | Planner budget configuration |

> **Note:** `localStorage` is browser and origin-specific. Data saved in Brave won't appear in Firefox, and data from a file opened from `Downloads/` is separate from one opened from `Documents/`.

---

## Export File Format

```json
{
  "transactions": [
    {
      "id": "t1",
      "amount": 2770.33,
      "type": "income",
      "category": "Other",
      "note": "February Strike Balance",
      "date": "2026-02-25T21:09:26.061Z"
    }
  ],
  "btcWallets": [
    {
      "name": "WalletOfSatoshi (non custody)",
      "btc": 0.00224984
    }
  ],
  "btcPriceCache": {
    "price": 68318,
    "change24h": 6.59,
    "fetchedAt": "2026-02-25T23:25:42.933Z"
  },
  "exportedAt": "2026-02-25T23:41:37.857Z",
  "version": "2.1.0"
}
```

---

## External Dependencies

All loaded via CDN — the app requires an internet connection only for these on first load (and for the BTC price feed when Online):

| Library | Version | Purpose |
|---|---|---|
| [Chart.js](https://www.chartjs.org/) | 4.4.0 | Doughnut expense chart |
| [dayjs](https://day.js.org/) | 1.11.9 | Date formatting |
| [CoinGecko API](https://www.coingecko.com/en/api) | public | Live BTC/USD price |

> If you need the app to work fully offline including first load, you can download the Chart.js and dayjs scripts and inline them into the HTML file.

---

## Known Limitations

- **Android file picker** — importing files works best from the `Downloads` folder. Files in nested subdirectories may fail to read in some Android browsers due to `content://` URI restrictions. See v2.1.1 release notes.
- **localStorage limits** — browsers typically cap `localStorage` at 5–10 MB. For typical personal finance use this is not a concern.
- **No sync** — data lives only in the browser it was saved in. Use Export/Import to move data between devices or browsers.
- **BTC price only** — the Bitcoin panel currently supports BTC/USD only. Other cryptocurrencies are not supported.
- **No currency selection** — all fiat amounts are displayed in USD ($).

---

## Changelog

| Version | Description |
|---|---|
| **v2.1.1** | Android mobile import fix — `display:none` → visually hidden input, explicit UTF-8 encoding, loosened `accept` MIME types, hardened `FileReader` error handling |
| **v2.1.0** | Unified export/import — transactions + BTC wallets + price cache bundled into one JSON file |
| **v2.0.0** | Bitcoin integration — wallet panel, online/offline toggle, live CoinGecko price feed, 8dp precision |
| **v1.0** | Embedded Budget Planner — full multi-month planning modal with category CRUD and actual spend tracking |
| *(pre-git)* | Initial release — merged `app.js`, `index.html`, `style.css` into single self-contained HTML file |

---

## Privacy

- No data is ever sent to any server (except the CoinGecko price API call when Online, which sends no personal data)
- No analytics, no tracking, no ads
- Everything stays on your device

---

*Built as a single-file web app. No framework, no build step, no nonsense.*