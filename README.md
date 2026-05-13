# 🏦 Bank Reconciliation Tool

> A lightweight, browser-based reconciliation tool for comparing the **NCC Bank Journal** against the **ICBC Bank Statement** — no server, no install, just open and run.

Built for **GREE Mexico · ICBC-MXN** accounting workflows.

---

## ✨ Features

- **Drag & drop upload** — NCC Journal (`.xlsx`) and Bank Statement (`.csv`, UTF-16)
- **Auto-detects headers and subtotals** — skips summary rows automatically using voucher number logic
- **Inflow & Outflow reconciliation** — sorted by amount, matched line-by-line
- **Nómina / Payroll toggle** — include or exclude payroll entries from outflow comparison on the fly
- **Live discrepancy stats** — matched count and gap count for both inflow and outflow
- **In-browser preview** — tabbed table with color-coded matched (green) and unmatched (yellow) rows
- **One-click Excel export** — downloads a `.xlsx` with two worksheets (`Inflow Reconciliation` / `Outflow Reconciliation`)
- **Zero dependencies** — runs entirely in the browser; only uses [SheetJS](https://sheetjs.com/) via CDN

---

## 🖥️ Usage

### Option A — Open directly

```
double-click bank_reconciliation_tool.html
```

No web server needed. Works in any modern browser (Chrome, Edge, Firefox, Safari).

### Option B — Serve locally (optional)

```
https://greemexico-bankreconciliation.netlify.app/
```
---

## 📂 File Format Requirements

### NCC Bank Journal — `.xlsx`

Column layout (auto-detected, header rows skipped):

| Col | Field |
|-----|-------|
| A | Month |
| B | Day |
| C | **Voucher No.** ← used to detect real transactions |
| D | Description / Summary |
| E | Counterpart |
| F | Debit (Inflow) |
| G | Credit (Outflow) |

> Rows without a voucher number (subtotals, headers, opening balances) are automatically skipped.

### Bank Statement — `.csv`

- Encoding: **UTF-16** (with BOM) or UTF-8
- Delimiter: **Tab-separated**
- First 7 lines are skipped (bank header block)
- Running balance in columns 10–14 is used to derive transaction direction and amount
- Rows containing `合计` or `Total` are skipped

---

## 🔄 Reconciliation Logic

1. Both lists are **sorted ascending by amount**
2. A two-pointer merge compares NCC vs Bank entries
3. Exact amount matches → ✅ Matched (green)
4. Amount present on one side only → ⚠️ Gap (yellow + note)

**Notes generated:**
- `Bank Inflow/Outflow but not yet in NCC`
- `NCC Inflow/Outflow but not yet in Bank`

---

## 💡 Nómina Toggle

Bank outflow entries whose summary contains `nomina` or `payroll` (case-insensitive) are tagged as Nómina. Use the toggle to include or exclude them from the outflow reconciliation without re-running.

---

## 📤 Export

The **Download Excel** button generates:

```
Bank_Reconciliation_YYYYMM.xlsx
├── Inflow Reconciliation
└── Outflow Reconciliation
```

Each sheet contains: NCC amount, NCC summary, Bank amount, Bank summary, Note, and a totals row.

---

## 🛠️ Tech Stack

| Library | Purpose |
|---------|---------|
| [SheetJS (xlsx 0.18.5)](https://sheetjs.com/) | Parse `.xlsx`, export reconciliation file |
| [Google Fonts — DM Mono + Fraunces](https://fonts.google.com/) | UI typography |
| Vanilla JS / HTML / CSS | Everything else |

No build step. No framework. No backend.

---

## 📁 Project Structure

```
bank_reconciliation_tool.html   ← entire app (single file)
README.md
```

---

## 🔒 Privacy

All file processing happens **entirely in your browser**. No data is uploaded to any server.

---

## 📋 Changelog

| Version | Notes |
|---------|-------|
| 1.0 | Initial release — inflow/outflow reconciliation, Nómina toggle, Excel export |
