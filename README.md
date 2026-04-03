# customer-ui-dashboard
# FINVUE — Personal Finance Dashboard

> A sleek, fully responsive personal finance tracker built with **vanilla HTML, CSS, and JavaScript** — zero dependencies, zero build tools, runs entirely in the browser.

---

## Live Preview

Open `index.html` in any modern browser. No server, no install, no setup required.

---

## Project Structure

```
finvue/
├── index.html      # App shell & all page markup
├── style.css       # Design system, layout, components, responsive breakpoints
├── app.js          # State management, chart rendering, data logic
└── README.md       # You are here
```

---

## Features

### Dashboard Overview
- **4 live summary cards** — Total Balance, Income, Expenses, Savings Rate
- **Sparkline mini-charts** on each card showing 6-month trend
- **Month-over-month change badges** (▲/▼ with percentage)
- **Line chart** — Income vs Expense over last 6 months
- **Donut chart** — Spending breakdown by category with interactive legend
- **Recent transactions** preview table

### Transactions
- Full transaction table with **sort**, **search**, **filter by type & category**
- **Pagination** (10 per page)
- **Add / Edit / Delete** transactions via modal form
- **Admin / Viewer** role system — viewers cannot modify data
- **Export** to CSV or JSON

### Insights
- Monthly income vs expense bar chart
- Stacked category bar chart (last 6 months)
- Savings rate progress bar with contextual advice
- Average transaction value analysis

### UX
- **Dark / Light theme** toggle with persistent preference
- **Fully responsive** — works on desktop, tablet, and mobile (320px+)
- **LocalStorage persistence** — data survives page refresh
- Toast notifications for all user actions
- Smooth animations and transitions throughout

---

## Responsive Breakpoints

| Breakpoint | Behaviour |
|---|---|
| > 1100px | Full desktop layout, side-by-side charts |
| ≤ 1100px | Charts stack vertically |
| ≤ 900px | Sidebar narrows, 2-column insight grid |
| ≤ 768px | Sidebar becomes slide-in drawer, mobile layout |
| ≤ 480px | Compact cards, table columns hidden to fit |
| ≤ 380px | Single-column card grid, tighter padding |

---

## Tech Stack

| Layer | Technology |
|---|---|
| Markup | Semantic HTML5 |
| Styling | CSS3 — custom properties, grid, flexbox, animations |
| Logic | Vanilla ES6+ JavaScript |
| Charts | Native HTML5 Canvas API (no Chart.js, no D3) |
| Fonts | Google Fonts — Syne + DM Mono |
| Storage | Browser LocalStorage |
| Dependencies | **None** |

---

## Design System

All design tokens are defined as CSS custom properties in `:root` inside `style.css`, making theming and customisation straightforward.

```css
--accent:   #4f8eff   /* Primary blue */
--accent2:  #7c5cfc   /* Purple accent */
--green:    #22d98e   /* Income / positive */
--red:      #ff5c7a   /* Expense / negative */
--yellow:   #ffcb47   /* Warning */
--cyan:     #1ee8c8   /* Savings */
```

---

## Data & State

All application state lives in a single `state` object in `app.js`:

```js
state = {
  transactions: [...],   // Array of transaction objects
  role: 'admin',         // 'admin' | 'viewer'
  theme: 'dark',         // 'dark' | 'light'
  filters: { ... },      // Active search/filter/sort state
  currentPage: 1,        // Pagination
  nextId: 41             // Auto-increment ID counter
}
```

**Transaction shape:**
```js
{
  id: 1,
  desc: 'Monthly Salary',
  amount: 85000,
  type: 'income',           // 'income' | 'expense'
  category: 'Salary',
  date: '2026-04-05',       // ISO date string YYYY-MM-DD
  note: 'April pay'
}
```

Seed data is generated dynamically relative to the current date so charts always display data for the most recent 6 months.

---

## Key Implementation Notes

### Canvas Charts (No Library)
All charts are rendered using the native **HTML5 Canvas API** — no Chart.js, Recharts, or D3. This keeps the bundle size at zero and gives full control over rendering.

Charts use `getBoundingClientRect()` for sizing (not `offsetWidth`) and are drawn inside a double `requestAnimationFrame` callback to guarantee the DOM has fully painted before canvas dimensions are read — this is the fix for the common "blank chart on load" bug.

### Role-Based Access Control
The app has a simple two-role system:
- **Admin** — full CRUD access, can add/edit/delete transactions
- **Viewer** — read-only, all mutation controls are hidden via `.admin-only` CSS class

### Theme System
Switching themes updates a `data-theme` attribute on `<html>`. All colours are CSS variables, so the entire UI re-themes instantly. Charts redraw in the next animation frame to pick up the new computed colour values.

---

## Getting Started

**No build step needed.**

```bash
# Option 1: Just open the file
open index.html

# Option 2: Serve locally (avoids any browser CORS restrictions)
npx serve .
# or
python3 -m http.server 8080
```

Then visit `http://localhost:8080` in your browser.

---

## Browser Support

| Browser | Support |
|---|---|
| Chrome 90+ | ✅ Full |
| Firefox 88+ | ✅ Full |
| Safari 14+ | ✅ Full |
| Edge 90+ | ✅ Full |
| IE 11 | ❌ Not supported |

---

## Customisation

**Add a new transaction category:**
1. Add the category name to the relevant array in `CATEGORIES` (`app.js`)
2. Add a colour entry in `CAT_COLORS` (`app.js`)
3. Add a CSS class `.cat-yourname` in `style.css`

**Change the currency:**
Update the `fmt()` function in `app.js`:
```js
function fmt(n) {
  return '$' + Math.abs(n).toLocaleString('en-US', { maximumFractionDigits: 0 });
}
```

---

## Author

Built as a frontend evaluation project demonstrating responsive design, vanilla JS state management, and custom canvas-based data visualisation — all without external libraries or frameworks.
