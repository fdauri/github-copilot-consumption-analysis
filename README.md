# GitHub Copilot Consumption Analysis

A fully client-side, single-file HTML dashboard for analysing GitHub Copilot AI credit usage from exported CSV reports. No server, no data upload — everything runs in your browser.

---

## File

### `Dashboard-Github-Copilot-consumption.html`

Open this file directly in any modern browser. No installation or build step required.

---

## How to use

1. Export one or more `AIUsageReport_*.csv` files from your GitHub Copilot billing dashboard.
2. Open `Dashboard-Github-Copilot-consumption.html` in your browser.
3. Drag and drop the CSV files onto the upload area, or click **Select CSV files** to browse.
4. The dashboard renders instantly. You can load additional files at any time — data is merged automatically.

**Required CSV columns:** `date`, `username`, `model`, `quantity`, `gross_amount`  
**Optional column:** `cost_center_name`, `total_monthly_quota` (needed for pool analysis)

---

## Features

### Data ingestion
- **Multi-file support** — load multiple CSV files simultaneously; rows from all files are merged into a single dataset.
- **Drag-and-drop or file picker** — accepts `.csv` files only; invalid or empty files are rejected with an inline error message.
- **Individual file removal** — remove any single file from the dataset without reloading the page.
- **Restart** — clears all loaded data and resets the dashboard.

### Summary KPI cards
| KPI | Description |
|-----|-------------|
| Total spend (gross) | Sum of `gross_amount` in USD across all premium requests |
| AI credits consumed | Sum of `quantity` (ai-credit units) |
| Active users | Count of distinct usernames |
| Models used | Count of distinct models (Auto variants consolidated) |
| Period | First and last date in the dataset |
| Assigned licenses | Business vs Enterprise licence count, with a proportional bar (requires `total_monthly_quota`) |

### Charts (Chart.js)
- **Spend by model** — horizontal bar chart ranking every AI model by total USD spend.
- **Spend share by model** — doughnut chart showing each model's percentage of total spend.
- **Daily trend** — stacked bar chart of daily USD spend for the top 8 models over the full date range.
- **Spend by organisation** — bar chart derived from the source filename (organisation name extracted automatically from `AIUsageReport_<org>_*.csv`).

### Sortable data tables
- **Breakdown by model** — columns: model name, spend (USD), % of total, credits, requests, users. Clickable column headers to sort ascending/descending.
- **Top 25 users by spend** — columns: username, spend (USD), credits, number of models used. Sortable.
- **Spend by cost center** — columns: cost center name, spend (USD), % of total, users. Sortable.

### AI credits pool analysis *(requires `total_monthly_quota` column)*
Activated automatically when quota data is present in the CSV.

- **Pool utilisation meter** — visual gauge showing consumed credits vs the total licence pool, with colour coding (green = within pool, red = over pool).
- **Pool KPIs** — total licence pool (credits & USD), consumed credits, utilisation percentage, credits beyond the pool.
- **Users split** — count and proportional bar of users over quota vs within quota.
- **Available pool vs actual consumption chart** — stacked bar breaking down each user's consumption into: within own quota / drawn from pool surplus (others' unused credits) / beyond the pool (paid extra) / unused pool.
- **Consumption composition chart** — doughnut chart with the same four categories at aggregate level.
- **Over-quota users table** — lists every user exceeding their monthly licence quota with: licence type, cost center, quota, consumption, credits drawn from pool surplus, credits beyond the pool, and associated USD cost.

### Controls & display options
| Control | Description |
|---------|-------------|
| **Language** | Toggle between Italian (IT) and English (EN). All labels, tooltips, and dynamic strings update immediately. |
| **Number format** | Switch between US format (`1,234.56`) and EU format (`1.234,56`) for all numeric values. |
| **Discount** | Enable and set a percentage discount (0–100 %) applied to all USD monetary values. |
| **Jun–Aug Promo** | Simulate the June–August promotional bonus (+1,100 credits per Business licence, +3,100 per Enterprise). Expands the pool and recalculates spend accordingly. |

### Export
- **Export aggregated data (JSON)** — downloads a `dashboard_data.json` file containing all computed aggregations (summary, by-model, by-date, by-user, by-cost-center, pool analysis).

### Theme
- Automatically matches the OS light/dark preference.
- Can be forced via the `?clawpilotTheme=dark` or `?clawpilotTheme=light` URL parameter.

---

## Privacy

All CSV parsing and computation runs entirely in the browser using [PapaParse](https://www.papaparse.com/) and [Chart.js](https://www.chartjs.org/) loaded from CDN. **No data is ever sent to any server.**

---

## Disclaimer

This project is an **independent, community-contributed open-source tool**. It is **not** affiliated with, endorsed by, sponsored by, or in any way connected to **GitHub, Inc.** or **Microsoft Corporation**. The GitHub Copilot name and related trademarks are the property of their respective owners and are used here solely for descriptive purposes.

The software is provided **"as is"**, without warranty of any kind, express or implied. The authors and contributors accept no responsibility for errors, omissions, or inaccuracies in the data displayed or computed by this tool. **It is the sole responsibility of the user to verify the accuracy and completeness of any data analysed through this dashboard** before making any business, financial, or operational decisions based on it.

---

## License

This project is released under the **[MIT License](https://opensource.org/licenses/MIT)**.

```
MIT License

Copyright (c) 2026 contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

> **Why MIT?** The tool is a single self-contained file with no server component and no proprietary dependencies. MIT is the most permissive and widely recognised licence for this kind of utility: it allows anyone to freely use, copy, modify, and redistribute the code — including in commercial contexts — while keeping the liability disclaimer that reflects the "as-is" nature of the project.