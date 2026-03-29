# Invoice Audit Intelligence
### Mosaic Fellowship 2026 · Finance Challenge #1

A live CFO dashboard that fetches all invoice data from the Mosaic Wellness API, cross-references every line item against contracted rates, and surfaces every overcharge — with vendor profiles, trend analysis, and actionable recommendations.

---

## 🔍 What It Does

- **Fetches** all paginated data from three API endpoints (invoices, line items, rate card) entirely client-side
- **Cross-references** every line item against the vendor's contracted rate using exact + fuzzy service-type matching
- **Computes** the exact overcharge per line item: `(Billed Rate − Contracted Rate) × Quantity`
- **Classifies** severity: Critical (>₹50K), High (>₹10K), Medium, Credit
- **Visualises** overcharge by vendor, service category, monthly trend, and severity distribution
- **Surfaces** vendor profiles, key findings, and CFO-ready recommendations

---

## 📊 Key Findings (computed live from API)

| Metric | Value |
|--------|-------|
| **Total Overcharge** | Computed live (see dashboard) |
| **Discrepancy Count** | Computed live |
| **Top Offending Vendor** | Computed live |
| **Highest Single Overcharge** | Computed live |

> The numerical answer (to 2 decimal places) is displayed prominently in the dashboard header after data loads.

---

## 🚀 Deployment

**Live App:** `https://your-app.vercel.app` *(replace after deploy)*

### Deploy to Vercel in 3 steps

```bash
# 1. Clone this repo
git clone https://github.com/yourusername/invoice-audit-intelligence
cd invoice-audit-intelligence

# 2. Install Vercel CLI (optional)
npm i -g vercel

# 3. Deploy
vercel --prod
```

Or simply connect this repo to [vercel.com](https://vercel.com) and it will auto-deploy on every push.

---

## 🛠 Tech Stack

- **Vanilla JS + HTML/CSS** — zero dependencies, zero build step, instant load
- **Chart.js** — for all four data visualisations (loaded from CDN)
- **Google Fonts** — DM Serif Display, DM Mono, Syne
- **Vercel** — static hosting

No framework, no bundler, no backend. The entire audit runs in the browser.

---

## 🏗 Architecture

```
index.html
├── Data Layer      fetchAll() — paginated API fetch, all three endpoints in parallel
├── Analysis Engine analyse() — rate matching, overcharge computation, vendor profiling
└── Render Layer    renderApp() — KPI cards, findings, charts, vendor grid, table, recos
```

### Rate Matching Algorithm

1. Build a lookup map: `{ vendor_id → { service_type → contracted_rate } }`
2. For each line item, find the vendor's rate using:
   - **Exact match** on `service_type` key
   - **Fuzzy substring match** if exact fails
3. Overcharge = `(billed_unit_price − contracted_rate) × quantity`

---

## 📁 Repo Structure

```
/
├── index.html        # Entire app (self-contained)
├── vercel.json       # Vercel routing config
├── README.md         # This file
└── WRITEUP.md        # 500-word submission write-up
```

---

## 👤 Author

Lawrence | M.Com (Accounting & Finance), St. Xavier's College, Kolkata  
Mosaic Fellowship 2026 Application
