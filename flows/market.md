# Flow: Market (`market`)

> One self-contained file for converting this flow to another platform. Read https://kirtanyak-pe.github.io/lemonn-app-prototype/CONVERT.md first. Design system: Lemonnade V3 @ `84dd189` (components, tokens and rules: https://kirtanyak-pe.github.io/lemonnade-v3-docs/llms.txt).

The Market tab — stock lists (all, gainers, losers, under ₹100 / ₹500) and Filters.

Prototype: https://kirtanyak-pe.github.io/lemonn-app-prototype/#/flow/market · machine-readable: https://kirtanyak-pe.github.io/lemonn-app-prototype/flows/market.json

## Screens

| Id | Screen | Figma | Taps go to | Note |
|---|---|---|---|---|
| `market-01` | Market · All | — | — | The filter bar shows "Apply filters" until something is applied. |
| `market-02` | Market · Top gainers | — | — | Same layout — only the list changes. |
| `market-03` | Market · Top losers | — | — |  |
| `market-04` | Market · Under ₹100 | — | — |  |
| `market-05` | Filters | — | — | Side tabs on the left, every group in one scroll on the right. |
| `market-06` | Filters · indices | — | — | Scrolled to Indices — Nifty 500 picked; its side tab shows a dot. |
| `market-07` | Filters · sectors & sort | — | — |  |
| `market-08` | Market · filters applied | — | — | Applied filters as removable chips; the filter button gets a dot. |

## Spec (README.md)


> The spec for this flow. A developer should be able to build it on any platform (Flutter, iOS, web) from this file,
> the screens in this folder and the live app — without reading the rest of the repo.

- **Prototype:** https://kirtanyak-pe.github.io/lemonn-app-prototype/#/flow/market · one screen: `#/frame/market-01`
- **Source:** screenshots of the live app's Market tab and its filters (dark theme), shared 2026-10-17 — no Figma yet.
  Rebuilt in L3; the filter sheet is redesigned.
- **Status:** 🟨 lists + filters done · 2026-10-17
- **Entry:** the Market tab of the main navbar. **Exit:** a stock opens its page (not built yet); the navbar.

### Screens

| Id | Screen | What it shows / when |
|---|---|---|
| `market-01` | Market · All | Title, Search + Wallet, list tabs; "Apply filters" + filter button; stock cards |
| `market-02` | Market · Top gainers | Same layout — only the list (biggest risers first) |
| `market-03` | Market · Top losers | Same layout — biggest fallers first |
| `market-04` | Market · Under ₹100 | Same layout — stocks under ₹100 (Under ₹500 is the fifth tab) |
| `market-05` | Filters | The sheet: side tabs + all groups in one scroll; Cancel · Show n stocks |
| `market-06` | Filters · indices | Scrolled to Indices, Nifty 500 picked (its side tab has a dot) |
| `market-07` | Filters · sectors & sort | Scrolled further; sort picked too |
| `market-08` | Market · filters applied | Applied filters as removable chips; the filter button has a dot |

### Navigation

```
market-0x ─filter button─▶ Filters (sheet) ─Show n stocks─▶ the list, filtered (chips + dot)
                              └Cancel / backdrop / drag down─▶ unchanged
chip ✕ ─▶ that filter removed · list tab ─▶ same screen, another list (filters stay)
navbar Stocks / Market ─▶ the other tab (instant — a tab switch, no push)
```

### Data

`data.ts` — one set of stocks (`universe`), each with symbol, name, price, day change, sector, cap (small < ₹5,000 Cr,
mid ₹5,000–20,000 Cr, large > ₹20,000 Cr), market cap and the indices it's in. Every list comes from it:
- Tabs (`listFor`): All · Top gainers (rising, biggest first) · Top losers (falling, biggest first) · Under ₹100 ·
  Under ₹500.
- Filters (`filterGroups`): Quick filters · Indices · Market cap · Sectors · Sort by — one choice per group; groups
  combine (all must match); sort orders the result (`applyFilters`). Tickers and prices are samples from the live app.
- API: the list for a tab + filter params (index, cap, sector, quick filter, sort) → stocks; live prices on the list.

### Components (L3 → any platform)

| Where | L3 component (Figma name) | Props / variant |
|---|---|---|
| Top bar | Actionbar | `title="Market"` (L1, Heading/18), actions Search + Wallet, list tabs in `bottom` (sticky) |
| List tabs | Tabs | `underline` — hug + scroll (5 tabs) |
| "Apply filters" | text, Heading/16 | replaced by the applied filters' chips |
| Applied filter chip | Button | `secondary`, icon right ✕ — removes that filter (L3 has no removable chip: see gaps) |
| Filter button | Button + Badge | `tertiary` icon-only (`filter_alt`), Badge `dot` when filters are applied |
| Stock card | ListCell | `variant="card"`, `as="button"`; Avatar 40 left; symbol, company; price + PriceChange right |
| No results | EmptyState | "No stocks match" |
| Filters sheet | BottomSheet + BottomSheetHeader | heading "Filters", `trailing` = Clear all (ghost) |
| Side tabs | ListCell | plain, `size="sm"`, `as="button"`, `selected` for the group in view; Badge dot when picked |
| Group titles | SectionHeader | `headingLevel={3}` |
| Options | ListCell + Radio | `as="label"`, `size="sm"`, `multiline` (market cap shows its range) |
| Sheet buttons | ButtonGroup | horizontal: Cancel (secondary) · "Show n stocks" (primary — never the brand colour) |
| Main navbar | BottomNavbar (`MainNav`) | `value="market"` |

### Rules & behaviour

- All list tabs share one layout; switching a tab keeps the filters and the filter bar.
- Filters: one choice per group (radio); tapping the picked option again doesn't clear it — Clear all does.
- **The sheet:** side tab tap → the pane scrolls to that group; scrolling the pane → the side tab follows the group at
  the top. A side tab shows a dot when its group has a choice. The last group can scroll to the top too.
- "Show n stocks" counts what the choice would show, live; with 0 it reads "No stocks" and is off.
- Cancel, a backdrop tap or a drag down closes the sheet without applying.
- A chip ✕ removes just that filter; the dot on the filter button goes when nothing is applied.

### Differences from the live app

- **Filter sheet redesigned:** the live sheet had no header, a "Filter" title + Clear all repeated in the pane, one
  group visible at a time, lime brand buttons and outlined option cards. Now: an L3 sheet header "Filters" with Clear
  all, connected side tabs + one scrolling pane with every group, plain radio rows, and Cancel · "Show n stocks" in L3
  secondary / primary.
- **Applied chip** is an L3 secondary button with ✕; the filter button's dot is an L3 Badge.
- Market cap options show their ranges as descriptions; sort options read "Price: low to high".

### L3 gaps

- **Vertical side tabs** (a tab list down the side of a sheet) — composed from plain ListCells with `selected`.
- **Removable filter chip** — an L3 secondary Button with ✕ stands in.
- **Split sheet body** (two columns, fixed height, the right one scrolling) — local `--app-size-filter-body`.

### Changelog

- 2026-10-17 — first version: lists (5 tabs, one layout) and the redesigned Filters sheet.


## Source — this flow

### `src/flows/market/components/StockRow.module.css`

```css
/* Price over the day's change, right-aligned (numbers are Label, never Description). */
.price {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: var(--l3-spacing-02);
  white-space: nowrap;
}

.value {
  font: var(--l3-text-label-16);
}
```

### `src/flows/market/components/StockRow.tsx`

```tsx
import { Avatar } from '@l3/components/Avatar'
import { ListCell } from '@l3/components/ListCell'
import { PriceChange } from '@l3/components/PriceChange'
import { inr } from '../../../shared/data/format'
import type { MarketStock } from '../data'
import styles from './StockRow.module.css'

/** One stock in a Market list: logo, symbol, company, price and the day's change. Opens the stock page. */
export function StockRow({ stock, onOpen }: { stock: MarketStock; onOpen?: (symbol: string) => void }) {
  return (
    <ListCell
      variant="card"
      as="button"
      iconLeft={<Avatar initials={stock.initials} src={stock.logoUrl} size={40} label={stock.name} />}
      label={stock.symbol}
      description={stock.name}
      trailing={
        <span className={styles.price}>
          <span className={styles.value}>{inr(stock.price)}</span>
          <PriceChange value={stock.changePct} size="sm" arrow />
        </span>
      }
      onClick={() => onOpen?.(stock.symbol)}
    />
  )
}
```

### `src/flows/market/data.ts`

```ts
import type { Stock } from '../../shared/data/market'

/**
 * Market — every list on the tab comes from one set of stocks, so the tabs, filters and sort stay consistent.
 * Sample market data (tickers and prices from the live app, 2026-10-17; index / sector / cap tags are samples).
 */

export type Cap = 'small' | 'mid' | 'large'
export type MarketStock = Stock & { sector: SectorId; cap: Cap; marketCapCr: number; indices: IndexId[] }

export type IndexId = 'nifty-50' | 'nifty-500' | 'nifty-midcap-100' | 'nifty-auto' | 'nifty-pvt-bank' | 'nifty-fin-25-50' | 'nifty100-lowvol30' | 'finnifty' | 'nifty-growsect-15'
export type SectorId = 'oil-gas' | 'agriculture' | 'pharma' | 'auto' | 'infra' | 'building-materials' | 'engineering' | 'it' | 'banking' | 'fmcg' | 'finance' | 'consumer'

const s = (symbol: string, name: string, price: number, changePct: number, sector: SectorId, marketCapCr: number, indices: IndexId[] = []): MarketStock => ({
  symbol, name, price, changePct, sector, marketCapCr, indices,
  cap: marketCapCr < 5000 ? 'small' : marketCapCr <= 20000 ? 'mid' : 'large',
  initials: symbol.slice(0, 2),
})

export const universe: MarketStock[] = [
  s('TRICOMFRU', 'Tricom Fruit Products', 1.7, 4.94, 'agriculture', 4),
  s('IWP', 'Indian Wood Products', 35.62, -0.67, 'building-materials', 228),
  s('SAPPHIRE', 'Sapphire Foods India', 197.15, 0.41, 'consumer', 6335),
  s('RSSOFTWARE', 'R. S. Software (India)', 29.48, 1.66, 'it', 76),
  s('AGARWALEYE', 'Dr. Agarwal’s Health Care', 465.8, -2.38, 'pharma', 14720),
  s('WITS', 'Wherrelz IT Solutions', 135, 0, 'it', 17),
  s('CKKRETAIL', 'C K K Retail Mart', 258.65, 20, 'consumer', 412),
  s('DATAKOSA', 'Datakosa', 16.38, 20, 'it', 35),
  s('ONWARDTEC', 'Onward Technologies', 427.4, 19.99, 'it', 968),
  s('INFRAX', 'Infrax Renewable', 175.65, 19.98, 'infra', 288),
  s('COMFINTE', 'Comfort Intech', 7.74, 19.81, 'finance', 248),
  s('PECOS', 'Pecos Hotels and Pubs', 193.95, 19.72, 'consumer', 25),
  s('SBICARD', 'SBI Cards and Payment Services', 564.2, -1.57, 'finance', 53680, ['nifty-500', 'nifty-fin-25-50', 'finnifty']),
  s('WELSPUNLIV', 'Welspun Living', 219.3, 1.15, 'consumer', 21310, ['nifty-500']),
  s('DIVISLAB', 'Divi’s Laboratories', 9144.5, 1.27, 'pharma', 242760, ['nifty-50', 'nifty-500', 'nifty100-lowvol30']),
  s('ENRIN', 'Siemens Energy India', 2889, -2.3, 'engineering', 102880, ['nifty-500']),
  s('TMPV', 'Tata Motors Passenger Vehicles', 286.85, -0.07, 'auto', 105620, ['nifty-50', 'nifty-500', 'nifty-auto']),
  s('IOB', 'Indian Overseas Bank', 30.99, 0.81, 'banking', 59680, ['nifty-500']),
  s('ICICIBANK', 'ICICI Bank', 1342.6, 0.92, 'banking', 957300, ['nifty-50', 'nifty-500', 'nifty-pvt-bank', 'nifty-fin-25-50', 'finnifty', 'nifty100-lowvol30']),
  s('ITC', 'ITC', 412.35, 0.38, 'fmcg', 516200, ['nifty-50', 'nifty-500', 'nifty100-lowvol30', 'nifty-growsect-15']),
  s('INFY', 'Infosys', 1508.9, -1.12, 'it', 626400, ['nifty-50', 'nifty-500', 'nifty-growsect-15']),
  s('IEX', 'Indian Energy Exchange', 139.42, -6.12, 'infra', 12430, ['nifty-500', 'nifty-midcap-100']),
  s('ZOMATO', 'Eternal (Zomato)', 218.6, -4.85, 'consumer', 210800, ['nifty-50', 'nifty-500']),
  s('PAYTM', 'One 97 Communications', 712.3, -4.2, 'finance', 45300, ['nifty-500', 'nifty-midcap-100']),
  s('DELHIVERY', 'Delhivery', 341.75, -3.64, 'consumer', 25260, ['nifty-500', 'nifty-midcap-100']),
  s('YESBANK', 'Yes Bank', 18.42, 1.21, 'banking', 57800, ['nifty-500', 'nifty-midcap-100']),
  s('ONGC', 'Oil and Natural Gas Corporation', 236.9, -0.33, 'oil-gas', 298000, ['nifty-50', 'nifty-500']),
  s('MARUTI', 'Maruti Suzuki', 12480.5, 0.54, 'auto', 392400, ['nifty-50', 'nifty-500', 'nifty-auto']),
]

/** The Market tabs: same layout, a different list (and, for the movers, a different order). */
export type MarketTab = 'all' | 'gainers' | 'losers' | 'under-100' | 'under-500'
export const tabs: { value: MarketTab; label: string }[] = [
  { value: 'all', label: 'All' },
  { value: 'gainers', label: 'Top gainers' },
  { value: 'losers', label: 'Top losers' },
  { value: 'under-100', label: 'Under ₹100' },
  { value: 'under-500', label: 'Under ₹500' },
]

export const listFor = (tab: MarketTab): MarketStock[] => {
  switch (tab) {
    case 'gainers': return [...universe].filter((x) => x.changePct > 0).sort((a, b) => b.changePct - a.changePct)
    case 'losers': return [...universe].filter((x) => x.changePct < 0).sort((a, b) => a.changePct - b.changePct)
    case 'under-100': return universe.filter((x) => x.price < 100)
    case 'under-500': return universe.filter((x) => x.price < 500)
    default: return universe
  }
}

/** Filters: five groups, one choice in each (radio). Groups combine (all must match); sort orders the result. */
export type FilterGroupId = 'quick' | 'index' | 'cap' | 'sector' | 'sort'
export type FilterOption = { id: string; label: string; description?: string; test?: (x: MarketStock) => boolean }
export type FilterGroup = { id: FilterGroupId; label: string; options: FilterOption[] }

const inIndex = (id: IndexId) => (x: MarketStock) => x.indices.includes(id)
const inSector = (id: SectorId) => (x: MarketStock) => x.sector === id

export const filterGroups: FilterGroup[] = [
  {
    id: 'quick', label: 'Quick filters', options: [
      { id: 'nifty-midcap-100', label: 'NIFTY Midcap 100', test: inIndex('nifty-midcap-100') },
      { id: 'banking', label: 'Banking stocks', test: inSector('banking') },
      { id: 'fmcg', label: 'FMCG stocks', test: inSector('fmcg') },
      { id: 'it', label: 'IT stocks', test: inSector('it') },
    ],
  },
  {
    id: 'index', label: 'Indices', options: [
      { id: 'nifty-50', label: 'Nifty 50', test: inIndex('nifty-50') },
      { id: 'nifty-500', label: 'Nifty 500', test: inIndex('nifty-500') },
      { id: 'nifty-auto', label: 'Nifty Auto', test: inIndex('nifty-auto') },
      { id: 'nifty-pvt-bank', label: 'Nifty Pvt Bank', test: inIndex('nifty-pvt-bank') },
      { id: 'nifty-fin-25-50', label: 'Nifty Fin Services 25/50', test: inIndex('nifty-fin-25-50') },
      { id: 'nifty100-lowvol30', label: 'NIFTY100 LowVol30', test: inIndex('nifty100-lowvol30') },
      { id: 'finnifty', label: 'FINNIFTY', test: inIndex('finnifty') },
      { id: 'nifty-growsect-15', label: 'Nifty GrowSect 15', test: inIndex('nifty-growsect-15') },
    ],
  },
  {
    id: 'cap', label: 'Market cap', options: [
      { id: 'small', label: 'Small cap', description: 'Less than ₹5,000 Cr', test: (x) => x.cap === 'small' },
      { id: 'mid', label: 'Mid cap', description: '₹5,000 Cr to ₹20,000 Cr', test: (x) => x.cap === 'mid' },
      { id: 'large', label: 'Large cap', description: 'More than ₹20,000 Cr', test: (x) => x.cap === 'large' },
    ],
  },
  {
    id: 'sector', label: 'Sectors', options: [
      { id: 'oil-gas', label: 'Oil and gas', test: inSector('oil-gas') },
      { id: 'agriculture', label: 'Agriculture', test: inSector('agriculture') },
      { id: 'pharma', label: 'Pharma', test: inSector('pharma') },
      { id: 'auto', label: 'Auto', test: inSector('auto') },
      { id: 'infra', label: 'Infra', test: inSector('infra') },
      { id: 'building-materials', label: 'Building materials', test: inSector('building-materials') },
      { id: 'engineering', label: 'Engineering', test: inSector('engineering') },
      { id: 'it', label: 'IT', test: inSector('it') },
      { id: 'banking', label: 'Banking', test: inSector('banking') },
      { id: 'finance', label: 'Finance', test: inSector('finance') },
    ],
  },
  {
    id: 'sort', label: 'Sort by', options: [
      { id: 'mcap-asc', label: 'Market cap: low to high' },
      { id: 'mcap-desc', label: 'Market cap: high to low' },
      { id: 'price-asc', label: 'Price: low to high' },
      { id: 'price-desc', label: 'Price: high to low' },
    ],
  },
]

/** The picked option per group (none picked = the group isn't applied). */
export type Filters = Partial<Record<FilterGroupId, string>>

export const optionOf = (group: FilterGroupId, id: string | undefined) => filterGroups.find((g) => g.id === group)?.options.find((o) => o.id === id)

/** The tab's list with the filters applied and the sort order. */
export function applyFilters(list: MarketStock[], filters: Filters): MarketStock[] {
  const tests = (Object.keys(filters) as FilterGroupId[]).filter((g) => g !== 'sort').map((g) => optionOf(g, filters[g])?.test).filter(Boolean) as ((x: MarketStock) => boolean)[]
  const result = list.filter((x) => tests.every((t) => t(x)))
  const by: Record<string, (a: MarketStock, b: MarketStock) => number> = {
    'mcap-asc': (a, b) => a.marketCapCr - b.marketCapCr,
    'mcap-desc': (a, b) => b.marketCapCr - a.marketCapCr,
    'price-asc': (a, b) => a.price - b.price,
    'price-desc': (a, b) => b.price - a.price,
  }
  return filters.sort ? [...result].sort(by[filters.sort]) : result
}
```

### `src/flows/market/index.tsx`

```tsx
import { goToTab } from '../../shared/ui/MainNav'
import type { Flow } from '../types'
import { Market } from './screens/Market'

/**
 * Market — the Market tab: lists of stocks (All, Top gainers, Top losers, Under ₹100, Under ₹500), all with the same
 * layout, and Filters (quick filters, indices, market cap, sectors, sort). Built from the live app's screenshots
 * (shared 2026-10-17; no Figma yet), with the filter sheet redesigned in L3.
 */
export const market: Flow = {
  id: 'market',
  title: 'Market',
  description: 'The Market tab — stock lists (all, gainers, losers, under ₹100 / ₹500) and Filters.',
  figmaFile: '',
  sections: [
    { id: 'lists', title: 'Lists' },
    { id: 'filters', title: 'Filters' },
  ],
  screens: [
    { id: 'market-01', section: 'lists', name: 'Market · All', figma: '', note: 'The filter bar shows "Apply filters" until something is applied.', tab: true, render: (go) => <Market onTab={(t) => goToTab(go, t)} /> },
    { id: 'market-02', section: 'lists', name: 'Market · Top gainers', figma: '', note: 'Same layout — only the list changes.', tab: true, render: (go) => <Market tab="gainers" onTab={(t) => goToTab(go, t)} /> },
    { id: 'market-03', section: 'lists', name: 'Market · Top losers', figma: '', tab: true, render: (go) => <Market tab="losers" onTab={(t) => goToTab(go, t)} /> },
    { id: 'market-04', section: 'lists', name: 'Market · Under ₹100', figma: '', tab: true, render: (go) => <Market tab="under-100" onTab={(t) => goToTab(go, t)} /> },
    { id: 'market-05', section: 'filters', name: 'Filters', figma: '', note: 'Side tabs on the left, every group in one scroll on the right.', tab: true, render: (go) => <Market sheet onTab={(t) => goToTab(go, t)} /> },
    { id: 'market-06', section: 'filters', name: 'Filters · indices', figma: '', note: 'Scrolled to Indices — Nifty 500 picked; its side tab shows a dot.', tab: true, render: (go) => <Market sheet sheetSection="index" filters={{ index: 'nifty-500' }} onTab={(t) => goToTab(go, t)} /> },
    { id: 'market-07', section: 'filters', name: 'Filters · sectors & sort', figma: '', tab: true, render: (go) => <Market sheet sheetSection="sector" filters={{ index: 'nifty-500', sort: 'price-desc' }} onTab={(t) => goToTab(go, t)} /> },
    { id: 'market-08', section: 'filters', name: 'Market · filters applied', figma: '', note: 'Applied filters as removable chips; the filter button gets a dot.', tab: true, render: (go) => <Market filters={{ index: 'nifty-500', sort: 'price-desc' }} onTab={(t) => goToTab(go, t)} /> },
  ],
}
```

### `src/flows/market/screens/Market.module.css`

```css
/* Market — the filter bar over a list of stock cards (16 apart, inside the 16 page margin). */
.page {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-16);
  padding-block: var(--l3-spacing-16) var(--l3-spacing-24);
}

/* Applied filters (or the hint) on the left, the filter button on the right; the chips scroll if they don't fit. */
.filterBar {
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-12);
  padding-inline: var(--l3-spacing-16);
}

.filterHint {
  flex: 1;
  margin: 0;
  font: var(--l3-text-heading-16);
}

.applied {
  position: relative; /* keeps absolutely placed bits inside the scrolling row */
  display: flex;
  flex: 1;
  gap: var(--l3-spacing-08);
  min-width: 0;
  overflow-x: auto;
  scrollbar-width: none;
}

.applied::-webkit-scrollbar {
  display: none;
}

.applied > * {
  flex: none;
}

.filterButton {
  display: inline-flex;
  flex: none;
}

.list {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-16);
  margin: 0;
  padding: 0 var(--l3-spacing-16);
  list-style: none;
}
```

### `src/flows/market/screens/Market.tsx`

```tsx
import { useState } from 'react'
import { Actionbar, ActionbarAction } from '@l3/components/Actionbar'
import { Badge } from '@l3/components/Badge'
import { Button } from '@l3/components/Button'
import { EmptyState } from '@l3/components/EmptyState'
import { Icon } from '@l3/components/Icon'
import { Tabs } from '@l3/components/Tabs'
import { msAccountBalanceWallet, msClose, msFilterAlt, msSearch } from '@l3/icons/material'
import { MainNav, type Section } from '../../../shared/ui/MainNav'
import { Screen } from '../../../shared/ui/Screen'
import { StockRow } from '../components/StockRow'
import { applyFilters, listFor, optionOf, tabs, type FilterGroupId, type Filters, type MarketTab } from '../data'
import { FilterSheet } from '../sheets/FilterSheet'
import styles from './Market.module.css'

type Props = {
  tab?: MarketTab
  filters?: Filters
  /** Open with the Filters sheet up, scrolled to a group (board frames). */
  sheet?: boolean
  sheetSection?: FilterGroupId
  onTab?: (section: Section) => void
}

/**
 * market-01 … 08 · Market — an L1 tab. Every list tab (All, Top gainers, Top losers, Under ₹100, Under ₹500) has the
 * same layout; only the stocks change. Above the list: the applied filters as removable chips (or "Apply filters")
 * and the filter button, with a dot when something is applied.
 */
export function Market({ tab: initialTab = 'all', filters: initialFilters = {}, sheet = false, sheetSection, onTab }: Props) {
  const [tab, setTab] = useState<MarketTab>(initialTab)
  const [filters, setFilters] = useState<Filters>(initialFilters)
  const [open, setOpen] = useState(sheet)
  const base = listFor(tab)
  const list = applyFilters(base, filters)
  const applied = (Object.keys(filters) as FilterGroupId[]).map((g) => ({ group: g, option: optionOf(g, filters[g]) })).filter((a) => a.option)

  const remove = (group: FilterGroupId) => setFilters(({ [group]: _gone, ...rest }) => rest)

  return (
    <Screen
      top={
        <Actionbar
          title="Market"
          actions={
            <>
              <ActionbarAction icon={msSearch} label="Search" onClick={() => {}} />
              <ActionbarAction icon={msAccountBalanceWallet} label="Wallet" onClick={() => {}} />
            </>
          }
          bottom={<Tabs aria-label="Market lists" items={tabs} value={tab} onChange={(v) => setTab(v as MarketTab)} />}
        />
      }
      bottom={<MainNav value="market" onChange={onTab} />}
      sheet={open ? (
        <FilterSheet
          initial={filters}
          section={sheetSection}
          resultCount={(f) => applyFilters(base, f).length}
          onApply={(f) => { setFilters(f); setOpen(false) }}
          onClose={() => setOpen(false)}
        />
      ) : undefined}
    >
      <div className={styles.page}>
        <div className={styles.filterBar}>
          {applied.length ? (
            <div className={styles.applied} role="group" aria-label="Applied filters">
              {applied.map(({ group, option }) => (
                <Button key={group} variant="secondary" iconRight={<Icon icon={msClose} />} aria-label={`Remove ${option!.label}`} onClick={() => remove(group)}>
                  {option!.label}
                </Button>
              ))}
            </div>
          ) : (
            <p className={styles.filterHint}>Apply filters</p>
          )}
          <span className={styles.filterButton}>
            <Badge dot={applied.length > 0} label={`${applied.length} applied`}>
              <Button variant="tertiary" iconLeft={<Icon icon={msFilterAlt} />} aria-label="Filters" onClick={() => setOpen(true)} />
            </Badge>
          </span>
        </div>

        {list.length ? (
          <ul className={styles.list} aria-label={tabs.find((t) => t.value === tab)!.label}>
            {list.map((stock) => <li key={stock.symbol}><StockRow stock={stock} /></li>)}
          </ul>
        ) : (
          <EmptyState title="No stocks match" description="Try removing a filter, or pick another list." />
        )}
      </div>
    </Screen>
  )
}
```

### `src/flows/market/sheets/FilterSheet.module.css`

```css
/*
 * Filters body: side tabs (left) and one scrolling pane with every group (right), edge to edge in the sheet — it
 * steps out of the sheet content's padding (16 sides and top, 24 below) so the two columns meet the sheet edges.
 */
.split {
  display: grid;
  grid-template-columns: minmax(0, 2fr) minmax(0, 3fr);
  height: var(--app-size-filter-body);
  margin: calc(-1 * var(--l3-spacing-16)) calc(-1 * var(--l3-spacing-16)) calc(-1 * var(--l3-spacing-24));
}

/* Side tabs: plain rows; the picked one has L3's selected fill. A hairline separates them from the pane. */
.tabs {
  display: flex;
  flex-direction: column;
  padding-block: var(--l3-spacing-08);
  border-right: 1px solid var(--l3-border-light);
  overflow-y: auto;
}

.pane {
  position: relative; /* offsetTop of each group is measured from here */
  overflow-y: auto;
  overscroll-behavior: contain;
}

.group {
  padding-block: var(--l3-spacing-12);
}

/* The last group fills the pane, so a side tab can bring even it to the top. */
.group:last-child {
  min-height: 100%;
  box-sizing: border-box;
}

.group + .group {
  border-top: 1px solid var(--l3-border-light);
}

.groupHead {
  padding: 0 var(--l3-spacing-16) var(--l3-spacing-04);
}
```

### `src/flows/market/sheets/FilterSheet.tsx`

```tsx
import { useCallback, useId, useRef, useState } from 'react'
import { Badge } from '@l3/components/Badge'
import { BottomSheetHeader } from '@l3/components/BottomSheet'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { Radio } from '@l3/components/Checkbox'
import { ListCell } from '@l3/components/ListCell'
import { SectionHeader } from '@l3/components/SectionHeader'
import { Sheet } from '../../../shared/ui/Sheet'
import { filterGroups, type FilterGroupId, type Filters } from '../data'
import styles from './FilterSheet.module.css'

type Props = {
  /** The filters applied now (the sheet edits a copy until Apply). */
  initial: Filters
  /** Open scrolled to this group (board frames). */
  section?: FilterGroupId
  /** How many stocks a set of filters would show — the Apply button says it. */
  resultCount: (filters: Filters) => number
  onApply: (filters: Filters) => void
  onClose: () => void
}

/**
 * market-05 … 07 · Filters. Header "Filters" + Clear all. Body in two connected parts: side tabs for the five groups on
 * the left, and on the right every group as a section in ONE scroll — tap a side tab to jump to its section, or just
 * scroll and the side tab follows. One choice per group (radio). Footer: Cancel · "Show n stocks" (applies).
 */
export function FilterSheet({ initial, section, resultCount, onApply, onClose }: Props) {
  const headingId = useId()
  const [draft, setDraft] = useState<Filters>(initial)
  const [active, setActive] = useState<FilterGroupId>(section ?? filterGroups[0].id)
  const pane = useRef<HTMLDivElement>(null)
  const jumping = useRef(false)
  const picked = (Object.keys(draft) as FilterGroupId[]).length
  const count = resultCount(draft)

  const sectionEl = (id: FilterGroupId) => pane.current?.querySelector<HTMLElement>(`[data-group="${id}"]`)

  // The pane, when it's on screen (in the prototype the sheet's body is drawn by the screen's sheet host, a moment
  // later than this component). Board frames open with a group already at the top.
  const paneRef = useCallback((el: HTMLDivElement | null) => {
    pane.current = el
    const target = el && section ? el.querySelector<HTMLElement>(`[data-group="${section}"]`) : null
    if (el && target) el.scrollTop = target.offsetTop
  }, [section])

  // Scrolling the pane moves the side tab to the group at the top (not while a side-tab tap is scrolling it).
  const onPaneScroll = () => {
    const el = pane.current
    if (!el || jumping.current) return
    const top = el.scrollTop + 1
    const current = [...el.querySelectorAll<HTMLElement>('[data-group]')].filter((g) => g.offsetTop <= top).pop()
    if (current) setActive(current.dataset.group as FilterGroupId)
  }

  const jump = (id: FilterGroupId) => {
    const el = sectionEl(id)
    if (!el || !pane.current) return
    setActive(id)
    jumping.current = true
    pane.current.scrollTo({ top: el.offsetTop, behavior: 'smooth' })
    window.setTimeout(() => { jumping.current = false }, 500)
  }

  const pick = (group: FilterGroupId, id: string) => setDraft((d) => ({ ...d, [group]: id }))

  return (
    <Sheet
      labelledBy={headingId}
      onClose={onClose}
      header={
        <BottomSheetHeader
          headingId={headingId}
          heading="Filters"
          trailing={<Button variant="ghost" size="sm" disabled={picked === 0} onClick={() => setDraft({})}>Clear all</Button>}
        />
      }
      footer={
        <ButtonGroup aria-label="Filters" direction="horizontal">
          <Button variant="secondary" onClick={onClose}>Cancel</Button>
          <Button disabled={count === 0} onClick={() => onApply(draft)}>{count === 0 ? 'No stocks' : `Show ${count} ${count === 1 ? 'stock' : 'stocks'}`}</Button>
        </ButtonGroup>
      }
    >
      <div className={styles.split}>
        <nav className={styles.tabs} aria-label="Filter groups">
          {filterGroups.map((g) => (
            <ListCell
              key={g.id}
              as="button"
              size="sm"
              label={g.label}
              selected={g.id === active}
              trailing={draft[g.id] ? <Badge dot color="discover" label="picked" /> : undefined}
              onClick={() => jump(g.id)}
            />
          ))}
        </nav>
        <div ref={paneRef} className={styles.pane} onScroll={onPaneScroll}>
          {filterGroups.map((g) => (
            <section key={g.id} className={styles.group} data-group={g.id} aria-label={g.label}>
              <div className={styles.groupHead}><SectionHeader title={g.label} headingLevel={3} /></div>
              <div role="radiogroup" aria-label={g.label}>
                {g.options.map((o) => (
                  <ListCell
                    key={o.id}
                    as="label"
                    size="sm"
                    multiline
                    label={o.label}
                    description={o.description}
                    trailing={<Radio name={`filter-${g.id}`} checked={draft[g.id] === o.id} onChange={() => pick(g.id, o.id)} />}
                  />
                ))}
              </div>
            </section>
          ))}
        </div>
      </div>
    </Sheet>
  )
}
```

## Source — shared app parts it uses

### `src/shared/data/format.ts`

```ts
/** ₹ with Indian grouping: inr(123456.5) → "₹1,23,456.50"; inr(1000, 0) → "₹1,000". */
export const inr = (n: number, decimals = 2) =>
  '₹' + n.toLocaleString('en-IN', { minimumFractionDigits: decimals, maximumFractionDigits: decimals })

/** "9876543210" → "98765 43210" (how Indian mobile numbers are read out). */
export const phone = (digits: string) => (digits.length > 5 ? `${digits.slice(0, 5)} ${digits.slice(5)}` : digits)

/** Keep the last `visible` characters: mask('ABCPM1234K', 1, 5) → "ABCPM****K". */
export const mask = (s: string, keepStart: number, keepEnd: number) =>
  s.slice(0, keepStart) + '•'.repeat(Math.max(0, s.length - keepStart - keepEnd)) + s.slice(s.length - keepEnd)
```

### `src/shared/data/market.ts`

```ts
// Market data more than one flow uses (Stocks home, Market, F&O). Sample values, from the live app on 2026-10-17.

export type MarketIndex = { id: string; name: string; value: number; changePct: number }

export const indices: MarketIndex[] = [
  { id: 'nifty-50', name: 'NIFTY 50', value: 23146.15, changePct: 0.65 },
  { id: 'sensex', name: 'SENSEX', value: 74729.27, changePct: 0.56 },
  { id: 'nifty-bank', name: 'NIFTY BANK', value: 49812.4, changePct: -0.21 },
]

/**
 * A stock as lists show it. `logoUrl` comes from the API (the company logo); until it loads — and in this prototype —
 * the L3 Avatar shows `initials`.
 */
export type Stock = { symbol: string; name: string; price: number; changePct: number; initials: string; logoUrl?: string }

const stock = (symbol: string, name: string, price: number, changePct: number, initials: string): Stock => ({ symbol, name, price, changePct, initials })

export const stocks = {
  HBLENGINE: stock('HBLENGINE', 'HBL Engineering', 833.15, 17.27, 'HB'),
  SHRIPISTON: stock('SHRIPISTON', 'Shriram Pistons & Rings', 4629.9, 6.85, 'SP'),
  THANGAMAYL: stock('THANGAMAYL', 'Thangamayil Jewellery', 5098.05, 6.41, 'TJ'),
  VAML: stock('VAML', 'Vedanta Aluminium', 371.85, 3.42, 'VA'),
  WELCORP: stock('WELCORP', 'Welspun Corp', 2571.5, 3.4, 'WC'),
  LINDEINDIA: stock('LINDEINDIA', 'Linde India', 6184.55, 3.11, 'LI'),
  IEX: stock('IEX', 'Indian Energy Exchange', 139.42, -6.12, 'IE'),
  ZOMATO: stock('ZOMATO', 'Zomato', 218.6, -4.85, 'ZO'),
  PAYTM: stock('PAYTM', 'One 97 Communications', 712.3, -4.2, 'PT'),
  DELHIVERY: stock('DELHIVERY', 'Delhivery', 341.75, -3.64, 'DL'),
  NYKAA: stock('NYKAA', 'FSN E-Commerce (Nykaa)', 164.1, -3.08, 'NY'),
  POLICYBZR: stock('POLICYBZR', 'PB Fintech', 1532.4, -2.71, 'PB'),
  YESBANK: stock('YESBANK', 'Yes Bank', 18.42, 1.21, 'YB'),
  IDEA: stock('IDEA', 'Vodafone Idea', 8.12, -0.98, 'VI'),
  SUZLON: stock('SUZLON', 'Suzlon Energy', 54.2, 2.35, 'SZ'),
  IRFC: stock('IRFC', 'Indian Railway Finance', 128.65, 0.84, 'IR'),
  NHPC: stock('NHPC', 'NHPC', 78.9, 1.12, 'NH'),
  SOUTHBANK: stock('SOUTHBANK', 'South Indian Bank', 24.35, -0.41, 'SB'),
  NTPC: stock('NTPC', 'NTPC', 331.2, 0.62, 'NT'),
  COALINDIA: stock('COALINDIA', 'Coal India', 381.45, 0.45, 'CI'),
  HAL: stock('HAL', 'Hindustan Aeronautics', 4120.3, 1.08, 'HA'),
  ONGC: stock('ONGC', 'ONGC', 236.9, -0.33, 'ON'),
  SBIN: stock('SBIN', 'State Bank of India', 742.15, 0.27, 'SB'),
  JINDALSTEL: stock('JINDALSTEL', 'Jindal Steel & Power', 862.4, 1.46, 'JS'),
  COHANCE: stock('COHANCE', 'Cohance Lifesciences', 1084.6, 4.97, 'CL'),
  OIL: stock('OIL', 'Oil India', 412.75, 0.88, 'OI'),
  IOC: stock('IOC', 'Indian Oil', 128.3, 0.36, 'IO'),
  BPCL: stock('BPCL', 'Bharat Petroleum', 279.55, -0.52, 'BP'),
} satisfies Record<string, Stock>
```

### `src/shared/ui/MainNav.tsx`

```tsx
import { BottomNavbar } from '@l3/components/BottomNavbar'
import { NavIcon } from '@l3/components/BottomNavbar/NavIcon'

export type Section = 'stocks' | 'market' | 'portfolio' | 'mutualFund' | 'fno'

const items: { value: Section; label: string }[] = [
  { value: 'stocks', label: 'Stocks' },
  { value: 'market', label: 'Market' },
  { value: 'portfolio', label: 'Portfolio' },
  { value: 'mutualFund', label: 'Mutual Fund' },
  { value: 'fno', label: 'F&O' },
]

/** The Lemonn app's main bottom navbar (L3: Bottom Navbar). Every L1 screen uses this — never a lookalike. */
export function MainNav({ value, onChange }: { value: Section; onChange?: (value: Section) => void }) {
  return (
    <BottomNavbar
      aria-label="Main"
      value={value}
      onChange={(v) => onChange?.(v as Section)}
      items={items.map((i) => ({ ...i, icon: <NavIcon name={i.value} />, selectedIcon: <NavIcon name={i.value} selected /> }))}
    />
  )
}

/** The first screen of each tab that's built so far (prototype navigation). Tabs not built yet stay where they are. */
const tabScreens: Partial<Record<Section, string>> = { stocks: 'stocks-01', market: 'market-01' }

/** The navbar's onChange in a flow: `onTab={(t) => goToTab(go, t)}`. */
export const goToTab = (go: (id: string) => void, section: Section) => {
  const id = tabScreens[section]
  if (id) go(id)
}
```

### `src/shared/ui/Raised.module.css`

```css
/*
 * EXPERIMENT — every screen and sheet of the app, not in L3 yet: "raised" buttons, a soft 3D look like YouTube's
 * recent buttons. On when <html data-buttons="raised"> (the default; the prototype controls switch it to "flat" to
 * compare with plain L3). On top of the L3 Button (nothing in the design system changes):
 * - a faint vertical sheen: a touch lighter at the top, a touch darker at the bottom, under L3's hover/press layer;
 * - a 1px inner highlight on the top edge and a 1px inner shade on the bottom edge (the "lit from above" bevel);
 * - a small soft drop shadow, so the button sits slightly above the page.
 * Pressed: the drop shadow goes and the bevel turns inward (pushed in). Disabled / loading / ghost stay flat.
 * If it's approved for L3, these become Button tokens (e.g. button/raise-sheen, -highlight, -shade) per variant.
 */

:root[data-buttons='raised'] .raised :is(button, a)[data-variant][data-state='default']:not([data-variant='ghost']) {
  --raise-sheen-top: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-10));
  --raise-sheen-bottom: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-5));
  --raise-highlight: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-60));
  --raise-shade: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-10));
  --raise-drop: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-10));
  background-image:
    linear-gradient(var(--btn-layer), var(--btn-layer)),
    linear-gradient(to bottom, var(--raise-sheen-top), var(--raise-sheen-bottom));
  box-shadow:
    inset 0 0 0 var(--btn-border-width) var(--btn-border),
    inset 0 1px 0 var(--raise-highlight),
    inset 0 -1px 0 var(--raise-shade),
    0 1px 2px var(--raise-drop),
    var(--l3-shadow-elevation-low);
}

/* Filled (primary, brand, buy, sell): a stronger sheen and bevel on the solid colour. */
/* (:not(ghost) only matches the base rule's specificity, so these values win.) */
:root[data-buttons='raised'] .raised :is(button, a):is([data-variant='primary'], [data-variant='brand'], [data-variant='buy'], [data-variant='sell'])[data-state='default']:not([data-variant='ghost']) {
  --raise-sheen-top: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-20));
  --raise-sheen-bottom: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-15));
  --raise-highlight: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-25));
  --raise-shade: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-30));
  --raise-drop: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-20));
}

/* Dark themes: the light glint on dark outlined buttons is kept faint (a bright line would look like a border). */
:root[data-buttons='raised'][data-mode='dark'] .raised :is(button, a):is([data-variant='secondary'], [data-variant='tertiary'])[data-state='default'] {
  --raise-sheen-top: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-10));
  --raise-sheen-bottom: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-20));
  --raise-highlight: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-10));
  --raise-shade: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-40));
  --raise-drop: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-40));
}

/* Pushed in while pressed: no drop shadow, the bevel turns inward. */
:root[data-buttons='raised'] .raised :is(button, a)[data-variant][data-state='default']:not([data-variant='ghost']):active {
  box-shadow:
    inset 0 0 0 var(--btn-border-width) var(--btn-border),
    inset 0 1px 2px var(--raise-shade);
}
```

### `src/shared/ui/Screen.module.css`

```css
/*
 * One phone screen: the OS status bar, then the page — top bar, a body that scrolls and fills the space, the bottom
 * bar / button dock — with an optional toast just above the bottom and a sheet over everything.
 */
.screen {
  position: relative;
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  overflow: clip; /* clip, not hidden: a sheet taking focus while it slides in can't scroll the screen */
  background: var(--l3-surface-default);
  color: var(--l3-content-primary);
}

.page {
  display: flex;
  flex-direction: column;
  flex: 1;
  min-height: 0;
}

.middle {
  position: relative;
  display: flex;
  flex: 1;
  min-height: 0;
}

.body {
  display: flex;
  flex-direction: column;
  flex: 1;
  min-height: 0;
  overflow-x: hidden; /* a screen never scrolls sideways — only rails inside it do */
  overflow-y: auto;
}

/* Floating toast: sits on the bottom edge of the body, above the dock / navbar (L3 Aerobar rule). */
.toast {
  position: absolute;
  inset: auto 0 0;
}

/* The prototype's modal L3 sheet is portalled into this screen — lift it above the bottom bars. */
.screen.screen > [data-contained] {
  z-index: var(--app-layer-sheet);
}
```

### `src/shared/ui/Screen.tsx`

```tsx
import { Fragment, useState, type ReactNode } from 'react'
import { usePreview } from '../../preview/context'
import raised from './Raised.module.css'
import { SheetHost } from './Sheet'
import styles from './Screen.module.css'

type ScreenProps = {
  /** Actionbar (or nothing on full-bleed screens like the welcome screen). */
  top?: ReactNode
  /** Bottom navbar, button dock (ButtonGroup) or the keypad. */
  bottom?: ReactNode
  /** A floating Aerobar toast — shown just above `bottom`. */
  toast?: ReactNode
  /** A Sheet over the screen. */
  sheet?: ReactNode
  /** Screen content: scrolls when taller than the space between `top` and `bottom`. */
  children?: ReactNode
  /** A flow's own styling hook on the screen root (e.g. an experiment scoped to one flow). */
  className?: string
}

/**
 * The frame of every screen: top · body · bottom, a toast above the bottom, and a sheet over it all. It fills the
 * space between the OS status bar and the OS navigation bar (the preview draws those — real apps get them from the
 * OS, edge-to-edge insets), so nothing sits under the gesture handle. The screen is the sheet's container, so sheets
 * and their scrim stay in the app; its SheetHost keeps one sheet open while the screen's sheet changes (no scrim flash
 * between two sheets). Mark a bottom part with data-surface="primary" | "secondary" if the navigation bar should take
 * its colour (the button dock and the bottom navbar are recognised already). Buttons on every screen and its sheets
 * get the raised-button experiment (Raised.module.css) while <html data-buttons="raised">.
 */
export function Screen({ top, bottom, toast, sheet, children, className }: ScreenProps) {
  const [el, setEl] = useState<HTMLDivElement | null>(null)
  const { screenId } = usePreview()
  return (
    <div ref={setEl} className={[styles.screen, raised.raised, className].filter(Boolean).join(' ')}>
      <SheetHost container={el}>
        <div className={styles.page} data-part="page">
          {top}
          <div className={styles.middle}>
            <div className={styles.body}>{children}</div>
            {toast && <div className={styles.toast}>{toast}</div>}
          </div>
          {bottom}
        </div>
        <Fragment key={screenId}>{sheet}</Fragment>
      </SheetHost>
    </div>
  )
}
```

### `src/shared/ui/Sheet.module.css`

```css
/* Board only: the static sheet sits on the bottom edge of the screen, over the L3 Overlay — shown at rest, no fade-in. */
.scrim {
  z-index: var(--app-layer-scrim);
  transition: none;
}

.dock {
  position: absolute;
  inset: auto 0 0;
  z-index: var(--app-layer-sheet);
  display: flex;
  flex-direction: column;
  max-height: calc(100% - var(--l3-size-48));
}
```

### `src/shared/ui/Sheet.tsx`

```tsx
import { createContext, useCallback, useContext, useId, useLayoutEffect, useState, type ReactNode } from 'react'
import { BottomSheet, BottomSheetSurface } from '@l3/components/BottomSheet'
import { Overlay } from '@l3/components/Overlay'
import { usePreview } from '../../preview/context'
import styles from './Sheet.module.css'

/** Lets the L3 sheet play its exit before the next screen replaces it (≈ motion/duration-medium). */
const SHEET_EXIT_MS = 240

type SheetProps = {
  header?: ReactNode
  children?: ReactNode
  /** The button dock — a <ButtonGroup>. */
  footer?: ReactNode
  /** Id of the header's heading (BottomSheetHeader `headingId`). */
  labelledBy?: string
  /** Accessible name when there's no header (loaders). */
  label?: string
  /** Prototype: where a backdrop tap, drag down or Esc goes. Leave out for sheets that can't be dismissed (loaders). */
  onClose?: () => void
}

type Publish = (id: string, sheet: SheetProps | null) => void
const SheetHostContext = createContext<Publish | null>(null)

/**
 * An L3 bottom sheet over the screen.
 * Prototype: the real modal <BottomSheet> (focus trap, drag to close, scrim tap), drawn by the screen's SheetHost —
 * so when one sheet leads to another over the same page, the scrim stays and only the sheet's content changes.
 * Board: the same surface drawn statically with the L3 Overlay — dozens of open modals on one page would fight
 * over focus, so the board shows <BottomSheetSurface>, which is what the modal renders inside.
 */
export function Sheet(props: SheetProps) {
  const { interactive } = usePreview()
  const publish = useContext(SheetHostContext)
  const id = useId()
  const hosted = interactive && publish !== null

  // Hand the latest content to the host on every render; take it back when this sheet goes away.
  useLayoutEffect(() => { if (hosted) publish(id, props) })
  useLayoutEffect(() => (hosted ? () => publish(id, null) : undefined), [hosted, publish, id])

  if (hosted) return null
  const { header, children, footer, labelledBy, label } = props
  return (
    <>
      <Overlay open className={styles.scrim} />
      <div className={styles.dock} role="dialog" aria-labelledby={labelledBy} aria-label={label}>
        <BottomSheetSurface header={header} footer={footer}>{children}</BottomSheetSurface>
      </div>
    </>
  )
}

/**
 * Prototype: one modal L3 BottomSheet per screen, kept open while the screen's sheet changes (sheet → sheet swaps
 * the content in place, no scrim flash or slide). It closes with its exit animation when the screen has no sheet.
 */
export function SheetHost({ container, children }: { container: HTMLElement | null; children: ReactNode }) {
  const [shown, setShown] = useState<{ id: string; sheet: SheetProps; open: boolean } | null>(null)
  const publish = useCallback<Publish>((id, sheet) => {
    setShown((s) => (sheet ? { id, sheet, open: true } : s && s.id === id ? { ...s, open: false } : s))
  }, [])

  const close = () => {
    const onClose = shown?.sheet.onClose
    if (!onClose) return
    setShown((s) => s && { ...s, open: false })
    window.setTimeout(onClose, SHEET_EXIT_MS)
  }

  return (
    <SheetHostContext.Provider value={publish}>
      {children}
      {container && shown && (
        <BottomSheet
          open={shown.open}
          onClose={close}
          container={container}
          header={shown.sheet.header}
          footer={shown.sheet.footer}
          aria-labelledby={shown.sheet.labelledBy}
          aria-label={shown.sheet.label}
        >
          {shown.sheet.children}
        </BottomSheet>
      )}
    </SheetHostContext.Provider>
  )
}
```
