# Kholhang eCashBook

A single-page **cashbook** for tracking **expenditure**, **income**, and **reports**. Data is stored as **XML** (with browser backup). Currency is **INR (₹)**. Sensitive changes are protected by a **4-digit confirmation code** (captcha-style).

---

## Quick start (open in browser)

| How | Link / action |
|-----|----------------|
| **Open locally (double-click)** | Open [`cashbook.html`](cashbook.html) in Chrome, Edge, or Firefox |
| **Sample data file** | [`cashbook-data.xml`](cashbook-data.xml) (optional; app also has built-in sample data) |

> **Tip:** Double-click works for daily use. **Import XML** and **Reload cashbook-data.xml** need a [local web server](#run-with-a-local-web-server-recommended-for-xml-files) (see below).

---

## Run with a local web server (recommended for XML files)

Some features use `fetch()` to read `cashbook-data.xml`. Browsers block that on `file://`. Use any of these from the project folder:

### Python (if installed)

```bash
cd "e:\ANDROID PROJECT\000 KHOLHANG PROJECTS\Kholhang eCashBook"
python -m http.server 8080
```

Then open: **http://localhost:8080/cashbook.html**

### Node.js (`npx serve`)

```bash
cd "e:\ANDROID PROJECT\000 KHOLHANG PROJECTS\Kholhang eCashBook"
npx --yes serve -p 8080
```

Then open: **http://localhost:8080/cashbook.html**

### VS Code / Cursor

Install **Live Server** (or similar), right-click `cashbook.html` → **Open with Live Server**.

---

## Run from GitHub

### Option A — GitHub Pages (clickable app URL)

1. Push this folder to a GitHub repository.
2. In the repo: **Settings → Pages**.
3. **Source:** Deploy from branch (e.g. `main`) → folder **`/ (root)`** or `/docs` if you move files there.
4. Save. After a minute, your site is live at:

   `https://<your-username>.github.io/<repo-name>/cashbook.html`

Replace `<your-username>` and `<repo-name>` with your values.

### Option B — Clone and run locally

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
python -m http.server 8080
```

Open: **http://localhost:8080/cashbook.html**

### Option C — View files on GitHub

- [`cashbook.html`](cashbook.html) — main app (download or clone to run)
- [`cashbook-data.xml`](cashbook-data.xml) — sample XML data

---

## Project files

| File | Purpose |
|------|---------|
| [`cashbook.html`](cashbook.html) | Full app (HTML, CSS, JavaScript in one file) |
| [`cashbook-data.xml`](cashbook-data.xml) | Sample / editable XML cashbook data |
| `README.md` | This documentation |

---

## Features

### Dashboard (top)

- **Closing balance (INR)** with opening balance, total income, and total expenditure
- Indian number format (`en-IN`) with **₹** symbol

### Three tabs

1. **Expenditure** — list of expense entries (cards), search
2. **Income** — list of income entries (cards), search
3. **Report** — filters, summaries, category breakdown, ledger with running balance, settings & XML tools

### New entry (FAB)

- Floating button: **New Income** / **New Expenditure** / **New Entry** (depends on active tab)
- **Income tab** → opens income entry only (type locked)
- **Expenditure tab** → opens expenditure entry only (type locked)
- **Report tab** → choose income or expenditure (uses last visited income/expenditure tab as default)

### Entry fields

- Date (**DD-MMM-YYYY** display + calendar picker), amount, description, category, payment (**Cash / Bank / UPI**), note
- **Edit** and **Delete** on each card
- **Move to Income** / **Move to Expenditure** — fix wrong-tab mistakes (with confirmation + code)

### Report

- **From / To** date filters (DD-MMM-YYYY + picker)
- Period income, expenditure, net, closing balance
- **By category** tables
- **Ledger** with running balance
- **Print report**

### Settings & data (Report tab)

- Opening balance (INR)
- Currency fixed to **INR (₹)**
- **Export XML** — download full cashbook
- **Import XML** — replace data from file
- **Export CSV** — spreadsheet export (dates as DD-MMM-YYYY)
- **Reload cashbook-data.xml** — load file from server folder
- **Reset to sample XML** — restore built-in demo data

### Data storage

- Auto-save to browser **localStorage** as XML (`kholhang_ecashbook_xml`)
- Load order on startup:
  1. Saved XML in localStorage (if present)
  2. Else `cashbook-data.xml` (if served over HTTP)
  3. Else built-in sample XML inside `cashbook.html`

---

## XML data format

Root element: `<CashBook>`.

```xml
<Settings>
  <BookName>...</BookName>
  <CurrencyCode>INR</CurrencyCode>
  <CurrencySymbol>₹</CurrencySymbol>
  <OpeningBalance>10000.00</OpeningBalance>
</Settings>
<Income>
  <Entry id="..." date="2026-07-01" category="Salary" payment="Bank">
    <Description>...</Description>
    <Amount>55000.00</Amount>
    <Note>...</Note>
  </Entry>
</Income>
<Expenditure>
  <!-- same Entry structure -->
</Expenditure>
```

- **`date`** in XML is always **`YYYY-MM-DD`** (for sorting and filters).
- UI shows dates as **`DD-MMM-YYYY`** (e.g. `10-Jul-2026`).

---

## Processes (step-by-step)

### Add income or expenditure

1. Open **Income** or **Expenditure** tab (or Report for both types).
2. Tap **New Income** / **New Expenditure** (FAB).
3. Fill the form; pick date with the calendar control.
4. Tap **Save**.
5. Enter the **4-digit confirmation code** shown in the captcha box → **Confirm**.
6. Entry appears on the correct tab; balance updates; XML saved to localStorage.

### Edit an entry

1. On a card, tap **Edit**.
2. Change fields; switch **Income / Expenditure** in the popup if you need to move type (or use **Move to…** on the card).
3. **Save** → confirmation code → **Confirm**.

### Delete an entry

1. Tap **Delete** on the card → confirm delete message.
2. Confirmation code → **Confirm**.

### Transfer (wrong tab)

1. Tap **Move to Income** or **Move to Expenditure** on the card.
2. Confirm the dialog.
3. Confirmation code → **Confirm**.

### Report for a date range

1. Open **Report**.
2. Set **From** / **To** (optional).
3. Review summary, categories, and ledger.
4. Use **Show all dates** to clear filters.

### Backup data

1. **Report** → **Export XML** (no confirmation code).
2. Keep the downloaded `.xml` file safe.

### Restore data

1. **Import XML** (file picker) → confirmation code, **or**
2. **Reload cashbook-data.xml** when running on a local server, **or**
3. **Reset to sample XML** for demo data (confirmation code required).

### Change opening balance

1. **Report** → enter **Opening balance (INR)** → **Save settings** → confirmation code.

---

## 4-digit confirmation code (captcha)

Required before **create, update, delete, transfer**, and **data replace** actions (import, reload, reset, save settings).

| Step | What happens |
|------|----------------|
| 1 | A popup shows a **confirmation code** (4 digits, captcha-style display). |
| 2 | Type the **same 4 digits** in the field below. |
| 3 | Tap **Confirm** to proceed, or **Cancel** to abort. |

The code is generated from the current time (**MM** + **SS**, frozen when the popup opens). It is **not** shown in export/print flows that only read data.

**No code required** for: viewing tabs, search, report viewing, **Export XML**, **Export CSV**, **Print**.

---

## Browser support

- Modern **Chrome**, **Edge**, or **Firefox** (desktop or mobile)
- JavaScript enabled
- localStorage enabled (private/incognito may clear data when the session ends)

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Reload / fetch XML fails | Run via [local web server](#run-with-a-local-web-server-recommended-for-xml-files), not `file://` |
| Still see old currency symbol | **Save settings** after refresh, or **Import** / **Reset** updated XML |
| Confirmation code fails | Re-type exactly 4 digits as shown; cancel and retry if the popup was open a long time |
| Data disappeared | Check if browser cleared localStorage; restore from **Export XML** backup |

---

## License & credits

**Kholhang eCashBook** — personal / business cashbook HTML app. Adjust book name and data in XML or Settings as needed.
