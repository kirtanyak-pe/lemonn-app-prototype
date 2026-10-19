# Flow: Search (`search`)

> One self-contained file for converting this flow to another platform. Read https://kirtanyak-pe.github.io/lemonn-app-prototype/CONVERT.md first. Design system: Lemonnade V3 @ `df57857` (components, tokens and rules: https://kirtanyak-pe.github.io/lemonnade-v3-docs/llms.txt).

Search stocks, F&O contracts, mutual funds and IPOs; Smart Options & Futures lists; add to watchlist.

Prototype: https://kirtanyak-pe.github.io/lemonn-app-prototype/#/flow/search · machine-readable: https://kirtanyak-pe.github.io/lemonn-app-prototype/flows/search.json

## Screens

| Id | Screen | Figma | Taps go to | Note |
|---|---|---|---|---|
| `search-01` | Stocks · before typing | [4912:13308](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-13308) | `stocks-01`, `search-18` | From the search field on Stocks home. Recent (last 5) and popular searches as chips, max 200 wide. Type to search; the tabs switch in place. |
| `search-02` | Stocks · results | [4912:13483](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-13483) | `stocks-01`, `search-18` | "Bank": 2 matching watchlists, then 8 stocks. A bookmark opens Add to watchlist; ✕ clears. |
| `search-03` | Stocks · only a watchlist matches | [4912:13671](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-13671) | `stocks-01` | "Penny": the watchlist, and "Stocks (0)" with nothing under it. |
| `search-04` | Stocks · no matching watchlist | [4912:13587](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-13587) | `stocks-01`, `search-18` | Same results without the watchlist group. |
| `search-05` | Stocks · nothing found | [4912:13703](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-13703) | `stocks-01` | L3 empty state with the L3 default illustration (Figma still has the old magnifier). Clear Search empties the field. |
| `search-06` | F&O · before typing | [4912:13728](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-13728) | `stocks-01`, `search-09` | Recent and popular contracts, then the Smart Options & Futures lists as tiles (a long name wraps; its row grows). |
| `search-07` | F&O · results | [4912:14199](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-14199) | `stocks-01` | "Bank": watchlists (no count on this tab), then the contracts. |
| `search-08` | F&O · nothing found | [4912:13902](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-13902) | `stocks-01` |  |
| `search-09` | Smart Options & Futures · indices | [4912:13925](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-13925) | `search-06` | The ↕ pill switches Indices / Stocks; pills pick the list; Price ↕ switches the sort to Volume. |
| `search-10` | Smart Options & Futures · stocks | [4912:14019](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-14019) | `search-06` |  |
| `search-11` | Smart Options & Futures · by volume | [4912:14113](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-14113) | `search-06` | Sorted by volume: each row shows its volume instead of price + change. |
| `search-12` | MF · before typing | [4912:13035](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-13035) | `stocks-01` | Recent and popular funds, then collections as tiles. |
| `search-13` | MF · results | [4912:12952](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-12952) | `stocks-01` | "Bank": watchlists, then funds with 3Y returns; the 3Y Returns ↕ switcher changes the period. |
| `search-14` | MF · no fund matches | [4912:13010](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-13010) | `stocks-01` | Other tabs have results ("Energy"), this one has none: the empty state on this tab only. |
| `search-15` | IPO · before typing | [4912:13116](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-13116) | `stocks-01` | Recent and popular IPOs, then Most Subscribed and Upcoming IPOs (View All each). |
| `search-16` | IPO · results | [4912:13240](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-13240) | `stocks-01` | "Energy": 2 IPOs with their price bands. |
| `search-17` | IPO · nothing found | [4912:13274](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-13274) | `stocks-01` |  |
| `search-18` | Add to watchlist | [4912:14274](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-14274) | `stocks-01`, `search-18`, `search-20`, `search-22`, `search-02` | From HDFC Bank’s bookmark. Nothing ticked yet: Save is off. The keyboard goes down while the sheet is open. |
| `search-19` | Add to watchlist · 2 ticked | [4912:14811](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-14811) | `stocks-01`, `search-18`, `search-20`, `search-22`, `search-02` |  |
| `search-20` | Added · toast | [4912:15015](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-15015) | `stocks-01`, `search-18`, `search-21` | Floating L3 toast with OK; it leaves on its own (L3 timing: 6 s with an action — Figma says 2 s). |
| `search-21` | Added · bookmark filled | [4912:15123](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-15123) | `stocks-01`, `search-18` | HDFC Bank’s bookmark is now filled with a tick. |
| `search-22` | Create watchlist | [4912:14650](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-14650) | `stocks-01`, `search-18`, `search-20` | From "+ Create" (or the first time, when there’s no watchlist yet). The keyboard sits under the dock. |
| `search-23` | Add to watchlist · few lists | [4912:14477](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-14477) | `stocks-01`, `search-18`, `search-20`, `search-22`, `search-02` | Someone with 4 watchlists: the sheet is only as tall as its rows. |

## Spec (README.md)


> The spec for this flow. A developer should be able to build it on any platform (Flutter, iOS, web) from this file,
> the Figma links and the screens in this folder — without reading the rest of the repo.

- **Prototype:** https://kirtanyak-pe.github.io/lemonn-app-prototype/#/flow/search · one screen: `#/frame/search-01`
- **Figma:** [Search → 🌙 Dark](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=4912-12950) (dev
  handoff file) — **final layouts** in the old D2 system. This flow keeps every layout as it is and swaps the parts for
  L3 components, tokens and text styles. Figma frames are dark; the code works in every L3 theme.
- **Status:** ✅ · last updated 2026-10-19
- **Entry:** the search field on Stocks home (`stocks-01`) · **Exit:** back → Stocks home; a result opens its page
  (stock / contract / fund / IPO page — not in this flow)

### Screens

| Id | Screen | Figma | What it shows / when |
|---|---|---|---|
| `search-01` | Stocks · before typing | 4912:13308 | Recent searches (last 5) and popular searches as chips; keyboard up, field focused |
| `search-02` | Stocks · results | 4912:13483 | "Bank": Your watchlist (2), then Stocks (8) with price, change and a bookmark each |
| `search-03` | Stocks · only a watchlist matches | 4912:13671 | "Penny": one watchlist, "Stocks (0)" |
| `search-04` | Stocks · no matching watchlist | 4912:13587 | "Bank" without the watchlist group |
| `search-05` | Stocks · nothing found | 4912:13703 | Empty state + Clear Search |
| `search-06` | F&O · before typing | 4912:13728 | Recent / popular contracts, Smart Options & Futures tiles (View All) |
| `search-07` | F&O · results | 4912:14199 | "Bank": watchlists (no count), Contracts (4) |
| `search-08` | F&O · nothing found | 4912:13902 | Empty state |
| `search-09` | Smart Options & Futures · indices | 4912:13925 | ↕ Indices pill, list pills, Filter + Price ↕, contracts with price + change |
| `search-10` | Smart Options & Futures · stocks | 4912:14019 | Same, for stock contracts |
| `search-11` | Smart Options & Futures · by volume | 4912:14113 | Sorted by Volume: rows show the volume |
| `search-12` | MF · before typing | 4912:13035 | Recent / popular funds, Collections tiles (View All) |
| `search-13` | MF · results | 4912:12952 | "Bank": watchlists, Funds (2) with 3Y returns, 3Y Returns ↕ |
| `search-14` | MF · no fund matches | 4912:13010 | Other tabs have results, MF has none → empty state on this tab |
| `search-15` | IPO · before typing | 4912:13116 | Recent / popular IPOs, Most Subscribed, Upcoming IPOs (View All each) |
| `search-16` | IPO · results | 4912:13240 | "Energy": IPOs (2) with price bands |
| `search-17` | IPO · nothing found | 4912:13274 | Empty state |
| `search-18` | Add to watchlist | 4912:14274 | Sheet: every watchlist as a checkbox row, + Create, Save off until one is ticked |
| `search-19` | Add to watchlist · 2 ticked | 4912:14811 | Save on |
| `search-20` | Added · toast | 4912:15015 | "Stock added to watchlist successfully" + OK |
| `search-21` | Added · bookmark filled | 4912:15123 | The toast has gone; the bookmark shows the tick |
| `search-22` | Create watchlist | 4912:14650 | Sheet: Watchlist Name + Create, keyboard under the dock |
| `search-23` | Add to watchlist · few lists | 4912:14477 | 4 watchlists: a short sheet |

(4912:15232 "Stock → Search results" in the watchlist section is the same screen as `search-02`.)

### Navigation

```
stocks-01 search field ─▶ search-01 ─type─▶ results in place (search-02 …) · tabs switch in place · ✕ / Clear Search ─▶ before typing
search-06 tile / View All ─▶ search-09 ─back─▶ search-06        (↕ pill, list pills and Price ↕ change the list in place)
search-02 bookmark ─▶ search-18 ─tick─▶ search-19 ─Save─▶ search-20 (toast) ─OK / auto─▶ search-21
                      search-18 + Create ─▶ search-22 ─Create─▶ search-20 · no watchlist yet: bookmark ─▶ search-22
any screen ─back─▶ stocks-01
```

### Data

`data.ts` (types = the API contract, values = examples from Figma):
- `search(query)` → `SearchResults` `{ query, counts: {stocks, fno, mf, ipo}, tabs: { [tab]: { watchlists, listTitle, items } } }`.
  A tab with no entry shows the empty state. Real app: one search call per (debounced) query.
- `WatchlistMatch` `{ id, name, initials, count? }` — `count` ("2 stocks") on the Stocks tab only.
- `SearchResult` `{ id, name, detail, initials?, logoUrl?, price?, changePct?, returnsLabel?, priceBand?, volume?, saved? }`
  — stocks: logo + price + change; contracts: no logo; funds: no logo, "3Y" + returns; IPOs: logo + price band.
- Before typing: `recent[tab]` (max 5), `popular[tab]` (1–2 rows), `signalCategories`, `collections`, `ipoLists`.
- Signals: `signalScopes` (Indices / Stocks, each with its lists), `indexSignals`, `stockSignals`, sort `price | volume`.
- Watchlists: `allWatchlists` (the sheet), `fewWatchlists`.
- API: search(query) · add item to watchlists(ids) · create watchlist(name) · signals list(scope, list, sort, filters).

### Components (L3 → any platform)

| Where | L3 component (Figma name) | Props / variant |
|---|---|---|
| Top bar | Actionbar ("L3: Actionbar", Base content Search / Searched) | `onBack`, `search={{ value, onChange, placeholder }}`, ✕ = `ActionbarAction variant="ghost"` (`close`) once typed |
| Tabs under the bar | Tabs ("L3: Tabs group", Flat, isSmall) | `size="sm"`, labels "Stocks (8)" once there's a query; fill the row |
| Group labels | — (text) | Label/12, `content/tertiary`, 16 tall, 16 from the edges; 24 between groups |
| View All | Button ("L3: Button" Ghost Small) | `variant="ghost" size="sm"`, chevron right |
| Recent / popular chip | Card (clickable) + Avatar | `padding="none"`, radius 8, no shadow (1px `border/light` only); inside: 8 / 12 / 8 / 8 padding, Avatar 24 rounded (initials), Label/12 `content/tertiary`; max 200 wide; rows scroll sideways |
| Watchlist match | ListCell ("L3: list cell", card, tappable, small) | `variant="card" size="sm" as="button"`, Avatar 32 rounded, label + "n stocks", chevron; 12 apart |
| Result row | ListCell (plain, small, breathable) + Avatar + PriceChange | `size="sm" density="breathable" as="button"` (70 tall); right: price (Label/12) over `PriceChange size="sm" arrow`; 1px `border/light` line under each row, 16 in |
| Bookmark | Button (Ghost, icon only) + L3 custom icons | `variant="ghost"` (lg, 24 icon): `watchlist_add` (outline bookmark with +) / `watchlist_added` (filled, ✓ cut out) when saved — Icons library → Custom icons; 16 from the edge |
| F&O / MF tiles | Card (clickable) | no shadow (1px `border/light` only); 2 columns, 12 apart; label Label/12 (MF: `content/tertiary`), 10 / 12 padding |
| Sort | Select ("L3: Select switcher") | `size="sm" icon="swap"`: Price ↕ / Volume, 3Y Returns ↕ |
| Filter | Button | `variant="ghost" size="sm"`, `filter_list` on the right |
| Indices / Stocks pill | Tab (action pill) | `action appearance="pill"`, ↕ icon right — switches the underlyings |
| Signals lists | Tabs (pill) | `appearance="pill"` (md), in the top bar's bottom slot, 12 above and below |
| Nothing found | EmptyState ("L3 → Empty state") | default illustration; "No search results found" / "Make sure you’ve spelled it correctly."; Clear Search = Button (lg, `delete_forever`) |
| Add to watchlist | BottomSheet + BottomSheetHeader + ListCell + Checkbox + ButtonGroup | header `trailing` = Tertiary Small "+ Create"; rows `as="label"`, Checkbox in `iconLeft`; Save (primary lg) off until one is ticked |
| Create watchlist | BottomSheet + TextField + ButtonGroup | "Watchlist Name" (typing), Create (primary lg) |
| Added | Aerobar (floating, primary) | `check` icon, heading, action OK |
| Keyboard | — | the phone's own keyboard (drawn in the prototype: `src/shared/ui/TextKeyboard`) |

### Rules & behaviour

- The field is focused on open and the keyboard is up; results update as you type. ✕ (or Clear Search) empties the
  field and brings back recent / popular searches. Tabs keep the query; each shows its own count.
- On a tab with no match: the empty state, even when other tabs have results.
- Recent searches: the last 5, newest first; a chip is at most 200 wide (the name ends in "…").
- Bookmark: opens Add to watchlist (or Create watchlist when the user has none). Filled with a tick = in at least one
  watchlist; tapping it again opens the sheet to change the lists.
- Add to watchlist: tick any number; Save adds the item to all ticked lists → toast → bookmark filled.
- Signals: the ↕ pill switches Indices / Stocks (each has its own lists); Price ↕ switches the sort to Volume (rows then
  show the volume). Filter opens filters (not designed yet).

### Differences from Figma

- **Components only, same layout.** Every part is now L3: result rows are L3 list cells (Figma had a local "search
  stock listing" component), watchlist matches are card list cells, chips and tiles are clickable cards, the trend
  arrow is PriceChange, D2 icons are Material Symbols (the bookmark: L3 custom icons watchlist_add / watchlist_added), the bottom sheet is the current L3 sheet (Figma used the
  discontinued version).
- **Colours:** Figma's `content/secondary` at 60% opacity (labels, chip names) is `content/tertiary` (the same 36%).
  Chips and tiles are L3 clickable cards but, by the designer's call, **without the shadow** — only the 1px
  `border/light` outline (chips keep Figma's **8 radius**, tiles L3's 12). Watchlist rows keep L3's clickable look
  (`surface/primary` + `border/light` + `elevation-low`).
- **Empty state:** the L3 default illustration (a document with ✕) instead of the old magnifier; Clear Search is a
  Large button (app rule: Large buttons everywhere), Figma has Small.
- **Watchlist row gap:** L3's small list cell keeps 12 between the logo and the text (Figma result rows have 16).
- **Toast timing:** L3 keeps a toast with an action for 6 s (Figma's flow says 2 s).
- **Content:** obvious typos fixed (ICICIBANK, KOTAKBANK, AXISBANK, OI Gainers), and the repeated placeholder prices /
  changes replaced with varied sample values. Tab counts are the real number of results.
- Group labels stay Figma's small Label/12 labels (an L3 SectionHeader would be a bigger Heading/14 title and change
  the layout).

### L3 gaps

- **Chip** (recent / popular search): L3 has no chip component — a clickable Card with an Avatar stands in; its 200
  max width is a local token (`--app-size-search-chip-max`).
- **Divider:** no L3 divider — the 1px `border/light` line under result rows is drawn by the row.
- **Group label** (small Label/12 list label above a group): not an L3 component (SectionHeader is a bigger title).
- **Search field keyboard:** the Actionbar search input can't be told not to open the phone's own keyboard (the
  prototype draws one), so on a phone both may show.

### Changelog

- 2026-10-19 — bookmark uses the design's icons: L3 custom icons watchlist_add / watchlist_added (Icons 1.4.0).
- 2026-10-19 — chips: 8 radius, no shadow; tiles: no shadow; the focused search field no longer shows a line under the
  bar (L3 Actionbar 1.5.1).
- 2026-10-19 — first version (23 screens) from the final Figma layouts.


## Source — this flow

### `src/flows/search/components/GroupHeader.module.css`

```css
/* Figma "header": 16 tall, 16 from both screen edges, 8 between the label and its control. */
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--l3-spacing-08);
  min-height: var(--l3-size-16);
  padding-inline: var(--l3-spacing-16);
}

/* A header that starts a list straight under the top bar (IPO results, signals): 16 above it. */
.header[data-inset] {
  padding-top: var(--l3-spacing-16);
}

.title {
  margin: 0;
  font: var(--l3-text-label-12); /* L3/Label/12 */
  color: var(--l3-content-tertiary); /* Figma content/secondary at 60% = 36%, i.e. content/tertiary */
}
```

### `src/flows/search/components/GroupHeader.tsx`

```tsx
import type { ReactNode } from 'react'
import { Button } from '@l3/components/Button'
import { Icon } from '@l3/components/Icon'
import { msChevronRight } from '@l3/icons/material'
import styles from './GroupHeader.module.css'

/**
 * The small label over each group on the search screens — "Recent Searches", "Your watchlist (2)", "Stocks (8)" —
 * with an optional control on the right (View All, a sort switcher). Figma: Label/12 in content/tertiary, 16 tall,
 * 16 from the screen edges. Kept as the Figma label (not an L3 SectionHeader, which is a bigger Heading/14 title).
 */
export function GroupHeader({ title, action, inset = false }: { title: string; action?: ReactNode; inset?: boolean }) {
  return (
    <div className={styles.header} data-inset={inset || undefined}>
      <h2 className={styles.title}>{title}</h2>
      {action}
    </div>
  )
}

/** "View All ›" on a group header: an L3 Ghost Small button with a chevron. */
export function ViewAll({ onClick, label }: { onClick?: () => void; label: string }) {
  return (
    <Button variant="ghost" size="sm" iconRight={<Icon icon={msChevronRight} />} aria-label={`View all ${label}`} onClick={onClick}>
      View All
    </Button>
  )
}
```

### `src/flows/search/components/ResultList.module.css`

```css
.list {
  margin: 0;
  padding: 0;
  list-style: none;
}

/* Figma "search stock listing": the row (Info) and, on the right, the 56 wide bookmark column (16 · 24 · 16). */
.row {
  position: relative;
  display: flex;
  align-items: center;
}

/* Figma "sepe": a 1px border/light line under each row, 16 in from both edges. */
.row::after {
  content: '';
  position: absolute;
  inset: auto var(--l3-spacing-16) 0;
  height: 1px; /* hairline */
  background: var(--l3-border-light);
  pointer-events: none;
}

.cell {
  flex: 1 1 0;
  min-width: 0;
}

/* The row's own 16 right padding, the 24 icon, then 16 to the screen edge = Figma's 56 CTA column. */
.bookmark {
  flex: none;
  margin-inline-end: var(--l3-spacing-16);
}

.stack {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: var(--l3-spacing-02);
}

.value {
  font: var(--l3-text-label-12); /* L3/Label/12 */
  color: var(--l3-content-primary);
  white-space: nowrap;
}
```

### `src/flows/search/components/ResultList.tsx`

```tsx
import { Avatar } from '@l3/components/Avatar'
import { Button } from '@l3/components/Button'
import { Icon } from '@l3/components/Icon'
import { ListCell } from '@l3/components/ListCell'
import { PriceChange } from '@l3/components/PriceChange'
import { lmWatchlistAdd, lmWatchlistAdded } from '@l3/icons/lemonnade'
import { inr } from '../../../shared/data/format'
import type { SearchResult } from '../data'
import styles from './ResultList.module.css'

type ResultListProps = {
  items: SearchResult[]
  /** Accessible name of the list ("Stocks", "Contracts" …). */
  label: string
  /** Show the volume instead of price + change (a signals list sorted by volume). */
  showVolume?: boolean
  /** Bookmark on each row (search results). Signals lists have none. */
  bookmarks?: boolean
  /** Ids saved to a watchlist (filled bookmark) — overrides `item.saved`. */
  saved?: string[]
  onOpen?: (item: SearchResult) => void
  onBookmark?: (item: SearchResult) => void
}

/**
 * Search results: flat L3 list rows (ListCell plain, small, breathable — 70 tall, the asset-list density) with a
 * hairline between them, 16 in from the edges. Left: the logo (stocks, IPOs) as a 32 Avatar; then name + detail; on
 * the right the price over its change (L3 PriceChange), 3Y returns, an IPO price band or the volume; then the
 * bookmark (an L3 Ghost icon button with the L3 custom icons watchlist_add — outline + — and watchlist_added — filled,
 * ✓ cut out — once the item is in a watchlist).
 */
export function ResultList({ items, label, showVolume, bookmarks = true, saved, onOpen, onBookmark }: ResultListProps) {
  return (
    <ul className={styles.list} aria-label={label}>
      {items.map((item) => {
        const isSaved = saved ? saved.includes(item.id) : Boolean(item.saved)
        return (
          <li key={item.id} className={styles.row}>
            <ListCell
              className={styles.cell}
              size="sm"
              density="breathable"
              as="button"
              iconLeft={item.initials ? <Avatar initials={item.initials} src={item.logoUrl} size={32} /> : undefined}
              label={item.name}
              description={item.detail}
              trailing={<Trailing item={item} showVolume={showVolume} />}
              onClick={() => onOpen?.(item)}
            />
            {bookmarks && (
              <Button
                className={styles.bookmark}
                variant="ghost"
                iconLeft={<Icon icon={isSaved ? lmWatchlistAdded : lmWatchlistAdd} />}
                aria-label={isSaved ? `${item.name} is in a watchlist — change` : `Add ${item.name} to a watchlist`}
                aria-pressed={isSaved}
                onClick={() => onBookmark?.(item)}
              />
            )}
          </li>
        )
      })}
    </ul>
  )
}

/** Right side of a row: one value, or a value over the change. */
function Trailing({ item, showVolume }: { item: SearchResult; showVolume?: boolean }) {
  if (showVolume && item.volume !== undefined) return <span className={styles.value}>{item.volume.toLocaleString('en-IN')}</span>
  if (item.priceBand) return <span className={styles.value}>{item.priceBand}</span>
  const top = item.returnsLabel ?? (item.price !== undefined ? inr(item.price) : undefined)
  if (top === undefined) return null
  return (
    <span className={styles.stack}>
      <span className={styles.value}>{top}</span>
      {item.changePct !== undefined && <PriceChange value={item.changePct} size="sm" arrow />}
    </span>
  )
}
```

### `src/flows/search/components/SearchChips.module.css`

```css
/* Figma "Section 1 - Past search" / "Sec 2 - Popular search": label, 12, chip rows (12 apart). */
.section {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-12);
}

/* Rows scroll sideways together; the first chip lines up with the 16 page margin. */
.scroll {
  position: relative; /* keeps absolutely placed bits inside the scrolling area */
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-12);
  overflow-x: auto;
  overscroll-behavior-x: contain;
  padding-inline: var(--l3-spacing-16);
  scroll-padding-inline: var(--l3-spacing-16);
  scrollbar-width: none;
}

.scroll::-webkit-scrollbar {
  display: none;
}

.row {
  display: flex;
  gap: var(--l3-spacing-12);
  width: max-content;
}

/* Figma "history card": hugs its content up to 200 wide. */
.chip {
  flex: none;
  width: max-content; /* a Card fills its container by default; a chip hugs its name */
  max-width: var(--app-size-search-chip-max);
}

/* Designer's call (2026-10-19), as in Figma's "history card": 8 radius and no shadow — only the 1px border/light
   outline, not L3's clickable-card lift. Doubled class so it wins over the Card's own [data-interactive] rule. */
.chip.chip {
  border-radius: var(--l3-radius-08);
  box-shadow: inset 0 0 0 1px var(--l3-border-light); /* 1px: Figma stroke weight has no variable */
}

.body {
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-08);
  min-width: 0;
  padding: var(--l3-spacing-08) var(--l3-spacing-12) var(--l3-spacing-08) var(--l3-spacing-08);
}

.label {
  min-width: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  font: var(--l3-text-label-12); /* L3/Label/12 */
  color: var(--l3-content-tertiary); /* Figma content/secondary at 60% */
}
```

### `src/flows/search/components/SearchChips.tsx`

```tsx
import { Avatar } from '@l3/components/Avatar'
import { Card } from '@l3/components/Card'
import type { SearchChip } from '../data'
import { GroupHeader } from './GroupHeader'
import styles from './SearchChips.module.css'

/**
 * Recent or popular searches: a label, then one or two rows of chips that scroll sideways together. A chip is an L3
 * clickable Card (one action: search for it) holding a 24 Avatar and the name — at most 200 wide, the name ends in
 * "…". Recent keeps the last 5 searches (Figma dev note).
 */
export function SearchChips({ title, rows, onPick }: { title: string; rows: SearchChip[][]; onPick?: (chip: SearchChip) => void }) {
  return (
    <section className={styles.section} aria-label={title}>
      <GroupHeader title={title} />
      <div className={styles.scroll}>
        {rows.map((row, i) => (
          <div key={i} className={styles.row}>
            {row.map((chip) => (
              <Card key={chip.label} padding="none" className={styles.chip} aria-label={`Search ${chip.label}`} onClick={() => onPick?.(chip)}>
                <span className={styles.body}>
                  <Avatar initials={chip.initials} src={chip.logoUrl} size={24} shape="rounded" />
                  <span className={styles.label}>{chip.label}</span>
                </span>
              </Card>
            ))}
          </div>
        ))}
      </div>
    </section>
  )
}
```

### `src/flows/search/components/Tiles.module.css`

```css
/* Figma "Container": 2 columns, 12 apart both ways, inside the 16 page margin. */
.grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: var(--l3-spacing-12);
  padding-inline: var(--l3-spacing-16);
}

.cell {
  display: flex;
}

.tile {
  flex: 1;
}

/* Designer's call (2026-10-19): no shadow — only the 1px border/light outline, not L3's clickable-card lift.
   Doubled class so it wins over the Card's own [data-interactive] rule. */
.tile.tile {
  box-shadow: inset 0 0 0 1px var(--l3-border-light); /* 1px: Figma stroke weight has no variable */
}

/* Figma "option": 10 above and below, 12 at the sides — 36 tall with one line. */
.label {
  display: block;
  padding: var(--l3-spacing-10) var(--l3-spacing-12);
  font: var(--l3-text-label-12); /* L3/Label/12 */
  color: var(--l3-content-primary);
  text-align: left;
}

.label[data-muted] {
  color: var(--l3-content-tertiary); /* Figma content/secondary at 60% */
}
```

### `src/flows/search/components/Tiles.tsx`

```tsx
import { Card } from '@l3/components/Card'
import styles from './Tiles.module.css'

/**
 * A grid of shortcut tiles, 2 per row, 12 apart (F&O: Smart Options & Futures lists; MF: collections). Each tile is an
 * L3 clickable Card with one label; a long label wraps to two lines and its row grows with it. `muted` = the MF
 * collections' quieter label (content/tertiary).
 */
export function Tiles({ items, label, muted, onPick }: { items: string[]; label: string; muted?: boolean; onPick?: (item: string) => void }) {
  return (
    <div className={styles.grid} role="list" aria-label={label}>
      {items.map((item) => (
        <div key={item} role="listitem" className={styles.cell}>
          <Card padding="none" className={styles.tile} onClick={() => onPick?.(item)}>
            <span className={styles.label} data-muted={muted || undefined}>{item}</span>
          </Card>
        </div>
      ))}
    </div>
  )
}
```

### `src/flows/search/components/WatchlistMatches.module.css`

```css
/* Figma "watchlist": label, 12, card rows 12 apart inside the 16 page margin. */
.section {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-12);
}

.rows {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-12);
  padding-inline: var(--l3-spacing-16);
}
```

### `src/flows/search/components/WatchlistMatches.tsx`

```tsx
import { Avatar } from '@l3/components/Avatar'
import { Icon } from '@l3/components/Icon'
import { ListCell } from '@l3/components/ListCell'
import { msChevronRight } from '@l3/icons/material'
import type { WatchlistMatch } from '../data'
import { GroupHeader } from './GroupHeader'
import styles from './WatchlistMatches.module.css'

/**
 * "Your watchlist (n)": the user's watchlists that match the query, above the results. Each is a tappable L3 card row
 * (ListCell card, small): a 32 initials Avatar, the name, how many stocks (Stocks tab only) and a chevron; it opens the
 * watchlist.
 */
export function WatchlistMatches({ watchlists, onOpen }: { watchlists: WatchlistMatch[]; onOpen?: (id: string) => void }) {
  return (
    <section className={styles.section} aria-label="Your watchlists">
      <GroupHeader title={`Your watchlist (${watchlists.length})`} />
      <div className={styles.rows}>
        {watchlists.map((w) => (
          <ListCell
            key={w.id}
            variant="card"
            size="sm"
            as="button"
            iconLeft={<Avatar initials={w.initials} size={32} shape="rounded" />}
            label={w.name}
            description={w.count === undefined ? undefined : `${w.count} stocks`}
            iconRight={<Icon icon={msChevronRight} />}
            onClick={() => onOpen?.(w.id)}
          />
        ))}
      </div>
    </section>
  )
}
```

### `src/flows/search/data.ts`

```ts
// Sample data for Search — the shapes are the API contract; the values are examples (from the Figma frames).

/** The four search tabs, in order. */
export type SearchTab = 'stocks' | 'fno' | 'mf' | 'ipo'

export const tabLabels: Record<SearchTab, string> = { stocks: 'Stocks', fno: 'F&O', mf: 'MF', ipo: 'IPO' }

/** What the empty search field says on each tab. */
export const placeholders: Record<SearchTab, string> = {
  stocks: 'Search stocks',
  fno: 'Search contracts',
  mf: 'Search mutual funds',
  ipo: 'Search IPOs',
}

/** A recent or popular search, shown as a chip (max 200 wide; at most 5 recent ones are kept). */
export type SearchChip = { label: string; initials: string; logoUrl?: string }

/** One of the user's watchlists that matches the query. `count` is shown on the Stocks tab only. */
export type WatchlistMatch = { id: string; name: string; initials: string; count?: number }

/**
 * One search result row. `price` / `changePct` for stocks and contracts, `returnsLabel` + `changePct` for funds
 * (e.g. 3Y returns), `priceBand` for IPOs, `volume` when a signals list is sorted by volume.
 */
export type SearchResult = {
  id: string
  name: string
  /** Second line: "Small Cap • BANKBARODA", "Stock • Fut • 30JUN2026", "Sectoral • Banking", "Opens 3 Jun". */
  detail: string
  /** Stocks and IPOs have a logo (initials until the logo loads); contracts and funds don't. */
  initials?: string
  logoUrl?: string
  price?: number
  changePct?: number
  returnsLabel?: string
  priceBand?: string
  volume?: number
  /** Already in one of the user's watchlists (filled bookmark). */
  saved?: boolean
}

/** What one tab shows for a query. */
export type TabResults = {
  watchlists: WatchlistMatch[]
  /** The list's heading, e.g. "Stocks", "Contracts", "Funds", "IPOs" — shown with the count. */
  listTitle: string
  items: SearchResult[]
}

/** The result of a search: per tab, and the counts the tabs show. */
export type SearchResults = { query: string; counts: Record<SearchTab, number>; tabs: Partial<Record<SearchTab, TabResults>> }

// ---- Before typing ---------------------------------------------------------------------------------------------

export const recent: Record<SearchTab, SearchChip[]> = {
  stocks: [
    { label: 'Bharat Coking Coal Ltd.', initials: 'B' },
    { label: 'Saregama India Ltd.', initials: 'S' },
    { label: 'NIFTY 50', initials: 'N' },
    { label: 'Reliance Industries Ltd', initials: 'R' },
    { label: 'Tata Motors Passenger Vehicles Ltd.', initials: 'T' },
  ],
  fno: [
    { label: 'Reliance Fut Finnifty 23500 CE', initials: 'R' },
    { label: 'Nifty 24500 CE', initials: 'N' },
    { label: 'Banknifty 52000 PE', initials: 'B' },
  ],
  mf: [
    { label: 'Nippon India Small Cap Fund', initials: 'N' },
    { label: 'HDFC Balanced Advantage Fund', initials: 'H' },
    { label: 'ICICI Prudential Bluechip Fund', initials: 'I' },
  ],
  ipo: [
    { label: 'HDB Financial IPO', initials: 'H' },
    { label: 'OYO IPO', initials: 'O' },
    { label: 'NSDL IPO', initials: 'N' },
  ],
}

/** Popular searches: one or two rows of chips that scroll sideways together. */
export const popular: Record<SearchTab, SearchChip[][]> = {
  stocks: [
    [
      { label: 'NIFTY 50', initials: 'N' },
      { label: 'Nippon India Silver ETF', initials: 'N' },
      { label: 'Bharat Coking Coal Ltd.', initials: 'B' },
    ],
    [
      { label: 'Reliance Industries Ltd', initials: 'R' },
      { label: 'Tata Motors Passenger Vehicles Ltd.', initials: 'T' },
      { label: 'Saregama India Ltd.', initials: 'S' },
    ],
  ],
  fno: [
    [
      { label: 'Nifty 24500 CE', initials: 'N' },
      { label: 'Banknifty 52000 PE', initials: 'B' },
      { label: 'Reliance Fut Finnifty 23500 CE', initials: 'R' },
    ],
  ],
  mf: [
    [
      { label: 'Parag Parikh Flexi Cap Fund', initials: 'P' },
      { label: 'Motilal Oswal Midcap Fund', initials: 'M' },
      { label: 'SBI Small Cap Fund', initials: 'S' },
    ],
    [
      { label: 'HDFC Balanced Advantage Fund', initials: 'H' },
      { label: 'ICICI Prudential Bluechip Fund', initials: 'I' },
      { label: 'Nippon India Small Cap Fund', initials: 'N' },
    ],
  ],
  ipo: [
    [
      { label: 'boAt IPO', initials: 'B' },
      { label: 'Tata Capital IPO', initials: 'T' },
      { label: 'Ather Energy IPO', initials: 'A' },
    ],
  ],
}

/** F&O: the Smart Options & Futures lists, as tiles (2 columns). Each opens its signals list. */
export const signalCategories = [
  'Top Traded', 'Most Active',
  'OI Gainers', 'OI Losers',
  'Top Gainers', 'Top Losers',
  'Top Picks', 'Long Buildup',
  'Volume Surge Highlights', 'Long Windup',
  'Short Covering', 'Short Buildup',
  'Momentum Breakouts',
]

/** MF: fund collections, as tiles (2 columns). */
export const collections = ['High Returns', 'Tax Savings', 'Low Risk', 'Large Cap']

/** IPO: lists shown before typing (each with View all). */
export const ipoLists: { title: string; items: SearchResult[] }[] = [
  {
    title: 'Most Subscribed',
    items: [
      { id: 'qline', name: 'Q-line biotech limited', detail: 'Subscribed 42X • Closing Tomorrow', initials: 'Q' },
      { id: 'ather', name: 'Ather Energy IPO', detail: 'Subscribed 17X • Closing Today', initials: 'A' },
      { id: 'nsdl', name: 'NSDL IPO', detail: 'Subscribed 19X • Closing 12 June', initials: 'N' },
    ],
  },
  {
    title: 'Upcoming IPOs',
    items: [
      { id: 'hdb', name: 'HDB Financial IPO', detail: 'To be announced', initials: 'H' },
      { id: 'ather-up', name: 'Ather Energy IPO', detail: 'To be announced', initials: 'A' },
      { id: 'rentomojo', name: 'Rentomojo IPO', detail: 'Opens 3 Jun', initials: 'R' },
    ],
  },
]

// ---- After typing ----------------------------------------------------------------------------------------------

const bankWatchlists: WatchlistMatch[] = [
  { id: 'banking', name: 'Banking stocks', initials: 'B', count: 2 },
  { id: 'large-cap-banking', name: 'large cap banking', initials: 'L', count: 2 },
]

const bankStocks: SearchResult[] = [
  { id: 'BANKBARODA', name: 'Bank of Baroda', detail: 'Small Cap • BANKBARODA', initials: 'BB', price: 248.35, changePct: -2.03, saved: true },
  { id: 'HDFCBANK', name: 'HDFC Bank', detail: 'Large Cap • HDFCBANK', initials: 'HB', price: 1964.56, changePct: -0.42 },
  { id: 'ICICIBANK', name: 'ICICI Bank', detail: 'Large Cap • ICICIBANK', initials: 'IB', price: 1428.9, changePct: 0.86 },
  { id: 'AXISBANK', name: 'Axis Bank', detail: 'Large Cap • AXISBANK', initials: 'AB', price: 1182.4, changePct: -1.12 },
  { id: 'KOTAKBANK', name: 'Kotak Mahindra Bank', detail: 'Large Cap • KOTAKBANK', initials: 'KM', price: 2104.75, changePct: 1.35 },
  { id: 'PNB', name: 'Punjab National Bank', detail: 'Mid Cap • PNB', initials: 'PN', price: 112.48, changePct: -2.03 },
  { id: 'AUBANK', name: 'AU Small Finance Bank', detail: 'Mid Cap • AUBANK', initials: 'AU', price: 742.1, changePct: 0.54 },
  { id: 'FEDERALBNK', name: 'Federal Bank', detail: 'Mid Cap • FEDERALBNK', initials: 'FB', price: 208.62, changePct: -0.77 },
]

const bankContracts: SearchResult[] = [
  { id: 'hdfc-fut', name: 'HDFC Bank', detail: 'Stock • Fut • 30JUN2026', saved: true },
  { id: 'hdfc-call', name: 'HDFC Bank', detail: 'Stock • Call • 30JUN2026' },
  { id: 'bn-call', name: 'BANKNIFTY 52000', detail: 'Index • Call • 30JUN2026' },
  { id: 'bn-put', name: 'BANKNIFTY 52000', detail: 'Index • Put • 30JUN2026' },
]

const bankFunds: SearchResult[] = [
  { id: 'hdfc-banking', name: 'HDFC Banking and Financial Services Fund', detail: 'Sectoral • Banking', returnsLabel: '3Y', changePct: 14.62, saved: true },
  { id: 'axis-banking', name: 'Axis Banking Fund', detail: 'Sectoral • Banking', returnsLabel: '3Y', changePct: 11.08 },
]

const noWatchlistCounts = (w: WatchlistMatch[]) => w.map(({ count: _count, ...rest }) => rest)

/** "Bank": results on the Stocks, F&O and MF tabs (no IPO matches). */
export const bank: SearchResults = {
  query: 'Bank',
  counts: { stocks: bankStocks.length, fno: bankContracts.length, mf: bankFunds.length, ipo: 0 },
  tabs: {
    stocks: { watchlists: bankWatchlists, listTitle: 'Stocks', items: bankStocks },
    fno: { watchlists: noWatchlistCounts(bankWatchlists), listTitle: 'Contracts', items: bankContracts },
    mf: { watchlists: noWatchlistCounts(bankWatchlists), listTitle: 'Funds', items: bankFunds },
  },
}

/** "Bank" for someone with no matching watchlist: only the stocks. */
export const bankNoWatchlist: SearchResults = { ...bank, tabs: { ...bank.tabs, stocks: { watchlists: [], listTitle: 'Stocks', items: bankStocks } } }

/** "Penny": a watchlist matches, no stock does. */
export const penny: SearchResults = {
  query: 'Penny',
  counts: { stocks: 1, fno: 0, mf: 0, ipo: 0 },
  tabs: { stocks: { watchlists: [{ id: 'penny', name: 'Penny stocks', initials: 'P', count: 7 }], listTitle: 'Stocks', items: [] } },
}

/** "Energy": two IPOs. */
export const energy: SearchResults = {
  query: 'Energy',
  counts: { stocks: 3, fno: 1, mf: 0, ipo: 2 },
  tabs: {
    ipo: {
      watchlists: [],
      listTitle: 'IPOs',
      items: [
        { id: 'nsdl-energy', name: 'NSDL Energy IPO', detail: 'Opens 3 Jun', initials: 'N', priceBand: '₹760–₹800', saved: true },
        { id: 'ather-energy', name: 'Ather Energy IPO', detail: 'To be announced', initials: 'A', priceBand: '₹304–₹321' },
      ],
    },
  },
}

/** A query nothing matches ("Vanf", "Adsf"): the empty state on every tab. */
export const nothing = (query: string): SearchResults => ({ query, counts: { stocks: 0, fno: 0, mf: 0, ipo: 0 }, tabs: {} })

/**
 * Prototype search: the sample result sets above, matched as you type ("Ba" → Bank). Anything else finds nothing.
 * Real app: a search API call per query (debounced), returning SearchResults. An empty query = nothing typed yet.
 */
export function search(query: string): SearchResults | undefined {
  const q = query.trim().toLowerCase()
  if (!q) return undefined
  const sample = [bank, penny, energy].find((s) => s.query.toLowerCase().startsWith(q))
  return sample ? { ...sample, query } : nothing(query)
}

// ---- Smart Options & Futures (signals) ---------------------------------------------------------------------------

/** Which underlyings a signals list shows — the ↕ pill in the bar switches between them. */
export type SignalScope = 'indices' | 'stocks'

export const signalScopes: Record<SignalScope, { label: string; categories: string[] }> = {
  indices: { label: 'Indices', categories: ['Top Traded', 'Most Active', 'OI Gainers', 'OI Losers', 'Top Gainers', 'Top Losers'] },
  stocks: { label: 'Stocks', categories: ['Top Traded', 'OI Gainers', 'OI Losers', 'Top Gainers', 'Top Losers'] },
}

/** How a signals list is sorted — the ↕ switcher on the right of the list header. */
export type SignalSort = 'price' | 'volume'

export const sortLabels: Record<SignalSort, string> = { price: 'Price', volume: 'Volume' }

export const indexSignals: SearchResult[] = [
  { id: 's1', name: 'SENSEX 61000', detail: 'Stock • Fut • 30JUN2026', price: 85.35, changePct: -2.03 },
  { id: 's2', name: 'NIFTY 18250', detail: 'Stock • Call • 30JUN2026', price: 66.4, changePct: 4.12 },
  { id: 's3', name: 'NIFTY 18250', detail: 'Stock • Put • 30JUN2026', price: 23.3, changePct: -6.4 },
  { id: 's4', name: 'SENSEX 61000', detail: 'Index • Call • 30JUN2026', price: 784.15, changePct: 1.86 },
  { id: 's5', name: 'NIFTY 18250', detail: 'Stock • Put • 30JUN2026', price: 766.5, changePct: -2.03 },
  { id: 's6', name: 'SENSEX 61000', detail: 'Index • Call • 30JUN2026', price: 29.6, changePct: 3.25 },
  { id: 's7', name: 'NIFTY 18250', detail: 'Stock • Put • 30JUN2026', price: 15.7, changePct: -8.14 },
  { id: 's8', name: 'SENSEX 61000', detail: 'Stock • Put • 30JUN2026', price: 49, changePct: -1.52 },
  { id: 's9', name: 'SENSEX 61000', detail: 'Stock • Put • 30JUN2026', price: 23.3, changePct: 0.43 },
]

export const stockSignals: SearchResult[] = [
  { id: 't1', name: 'VBL 525', detail: 'Stock • Fut • 30JUN2026', price: 85.35, changePct: -2.03, volume: 407925 },
  { id: 't2', name: 'COLPAL 2160', detail: 'Stock • Call • 30JUN2026', price: 66.4, changePct: 2.41, volume: 473250 },
  { id: 't3', name: 'MAXHEALTH 1090', detail: 'Stock • Put • 30JUN2026', price: 23.3, changePct: -3.6, volume: 374850 },
  { id: 't4', name: 'ASIANPAINT 2620', detail: 'Index • Call • 30JUN2026', price: 784.15, changePct: 1.08, volume: 336375 },
  { id: 't5', name: 'PAYTM 1150', detail: 'Stock • Put • 30JUN2026', price: 766.5, changePct: -2.03, volume: 1813225 },
  { id: 't6', name: 'ITC 302.5', detail: 'Index • Call • 30JUN2026', price: 29.6, changePct: 5.72, volume: 204750 },
  { id: 't7', name: 'ICICIPRULI 530', detail: 'Stock • Put • 30JUN2026', price: 15.7, changePct: -4.9, volume: 1821550 },
  { id: 't8', name: 'AUROPHARMA 1480', detail: 'Stock • Put • 30JUN2026', price: 49, changePct: 0.82, volume: 971500 },
]

// ---- Watchlists (Add to watchlist sheet) -------------------------------------------------------------------------

/** All of the user's watchlists, in the order they made them (the sheet lists them all). */
export const allWatchlists = [
  'Momentum Plays', 'Banking Giants', 'Long Term Bets', 'Buy the Dip', 'AI & Tech', 'Crypto Watch', 'Breakout Setup',
  'Emerging Stocks', 'High Risk High Reward', 'Research Pending', 'Growth Opportunities', 'Market Trends',
  'Investment Strategies', 'Portfolio Management',
]

/** Someone with only a few watchlists. */
export const fewWatchlists = allWatchlists.slice(0, 4)
```

### `src/flows/search/index.tsx`

```tsx
import { Aerobar } from '@l3/components/Aerobar'
import { Icon } from '@l3/components/Icon'
import { msCheck } from '@l3/icons/material'
import type { Flow } from '../types'
import { allWatchlists, bank, bankNoWatchlist, energy, fewWatchlists, nothing, penny } from './data'
import { Search } from './screens/Search'
import { Signals } from './screens/Signals'
import { AddToWatchlistSheet, CreateWatchlistSheet } from './sheets/WatchlistSheets'

/** HDFC Bank after it's been added to a watchlist (plus Bank of Baroda, already in one). */
const savedAfter = ['BANKBARODA', 'HDFCBANK']

/**
 * Search — the search screen for Stocks, F&O, MF and IPO (one screen, four tabs), F&O's Smart Options & Futures lists,
 * and adding a result to a watchlist. Figma: "Search → 🌙 Dark" (4912:12950) in the dev handoff file — final D2
 * layouts, rebuilt with L3 components and tokens without changing the layout.
 */
export const search: Flow = {
  id: 'search',
  title: 'Search',
  description: 'Search stocks, F&O contracts, mutual funds and IPOs; Smart Options & Futures lists; add to watchlist.',
  figmaFile: 'sG24GPizTdwbMBxLm0IpTA',
  sections: [
    { id: 'stocks', title: 'Stocks' },
    { id: 'fno', title: 'F&O' },
    { id: 'mf', title: 'Mutual funds' },
    { id: 'ipo', title: 'IPO' },
    { id: 'watchlist', title: 'Add to watchlist' },
  ],
  screens: [
    // ---- Stocks
    { id: 'search-01', section: 'stocks', name: 'Stocks · before typing', figma: '4912:13308', note: 'From the search field on Stocks home. Recent (last 5) and popular searches as chips, max 200 wide. Type to search; the tabs switch in place.', render: (go) => <Search tab="stocks" onBack={() => go('stocks-01')} onBookmark={() => go('search-18')} /> },
    { id: 'search-02', section: 'stocks', name: 'Stocks · results', figma: '4912:13483', note: '"Bank": 2 matching watchlists, then 8 stocks. A bookmark opens Add to watchlist; ✕ clears.', render: (go) => <Search results={bank} onBack={() => go('stocks-01')} onBookmark={() => go('search-18')} /> },
    { id: 'search-03', section: 'stocks', name: 'Stocks · only a watchlist matches', figma: '4912:13671', note: '"Penny": the watchlist, and "Stocks (0)" with nothing under it.', render: (go) => <Search results={penny} onBack={() => go('stocks-01')} /> },
    { id: 'search-04', section: 'stocks', name: 'Stocks · no matching watchlist', figma: '4912:13587', note: 'Same results without the watchlist group.', render: (go) => <Search results={bankNoWatchlist} onBack={() => go('stocks-01')} onBookmark={() => go('search-18')} /> },
    { id: 'search-05', section: 'stocks', name: 'Stocks · nothing found', figma: '4912:13703', note: 'L3 empty state with the L3 default illustration (Figma still has the old magnifier). Clear Search empties the field.', render: (go) => <Search results={nothing('Vanf')} onBack={() => go('stocks-01')} /> },
    // ---- F&O
    { id: 'search-06', section: 'fno', name: 'F&O · before typing', figma: '4912:13728', note: 'Recent and popular contracts, then the Smart Options & Futures lists as tiles (a long name wraps; its row grows).', render: (go) => <Search tab="fno" onBack={() => go('stocks-01')} onSignals={() => go('search-09')} /> },
    { id: 'search-07', section: 'fno', name: 'F&O · results', figma: '4912:14199', note: '"Bank": watchlists (no count on this tab), then the contracts.', render: (go) => <Search tab="fno" results={bank} onBack={() => go('stocks-01')} /> },
    { id: 'search-08', section: 'fno', name: 'F&O · nothing found', figma: '4912:13902', render: (go) => <Search tab="fno" results={nothing('Vanf')} onBack={() => go('stocks-01')} /> },
    { id: 'search-09', section: 'fno', name: 'Smart Options & Futures · indices', figma: '4912:13925', note: 'The ↕ pill switches Indices / Stocks; pills pick the list; Price ↕ switches the sort to Volume.', render: (go) => <Signals scope="indices" onBack={() => go('search-06')} /> },
    { id: 'search-10', section: 'fno', name: 'Smart Options & Futures · stocks', figma: '4912:14019', render: (go) => <Signals scope="stocks" onBack={() => go('search-06')} /> },
    { id: 'search-11', section: 'fno', name: 'Smart Options & Futures · by volume', figma: '4912:14113', note: 'Sorted by volume: each row shows its volume instead of price + change.', render: (go) => <Signals scope="stocks" sort="volume" onBack={() => go('search-06')} /> },
    // ---- Mutual funds
    { id: 'search-12', section: 'mf', name: 'MF · before typing', figma: '4912:13035', note: 'Recent and popular funds, then collections as tiles.', render: (go) => <Search tab="mf" onBack={() => go('stocks-01')} /> },
    { id: 'search-13', section: 'mf', name: 'MF · results', figma: '4912:12952', note: '"Bank": watchlists, then funds with 3Y returns; the 3Y Returns ↕ switcher changes the period.', render: (go) => <Search tab="mf" results={bank} onBack={() => go('stocks-01')} /> },
    { id: 'search-14', section: 'mf', name: 'MF · no fund matches', figma: '4912:13010', note: 'Other tabs have results ("Energy"), this one has none: the empty state on this tab only.', render: (go) => <Search tab="mf" results={energy} onBack={() => go('stocks-01')} /> },
    // ---- IPO
    { id: 'search-15', section: 'ipo', name: 'IPO · before typing', figma: '4912:13116', note: 'Recent and popular IPOs, then Most Subscribed and Upcoming IPOs (View All each).', render: (go) => <Search tab="ipo" onBack={() => go('stocks-01')} /> },
    { id: 'search-16', section: 'ipo', name: 'IPO · results', figma: '4912:13240', note: '"Energy": 2 IPOs with their price bands.', render: (go) => <Search tab="ipo" results={energy} onBack={() => go('stocks-01')} /> },
    { id: 'search-17', section: 'ipo', name: 'IPO · nothing found', figma: '4912:13274', render: (go) => <Search tab="ipo" results={nothing('Adsf')} onBack={() => go('stocks-01')} /> },
    // ---- Add to watchlist (from a result's bookmark on search-02)
    { id: 'search-18', section: 'watchlist', name: 'Add to watchlist', figma: '4912:14274', note: 'From HDFC Bank’s bookmark. Nothing ticked yet: Save is off. The keyboard goes down while the sheet is open.', render: (go) => <Search results={bank} onBack={() => go('stocks-01')} onBookmark={() => go('search-18')} keyboard={false} sheet={<AddToWatchlistSheet lists={allWatchlists} onSave={() => go('search-20')} onCreate={() => go('search-22')} onClose={() => go('search-02')} />} /> },
    { id: 'search-19', section: 'watchlist', name: 'Add to watchlist · 2 ticked', figma: '4912:14811', render: (go) => <Search results={bank} onBack={() => go('stocks-01')} onBookmark={() => go('search-18')} keyboard={false} sheet={<AddToWatchlistSheet lists={allWatchlists} checked={['Momentum Plays', 'Banking Giants']} onSave={() => go('search-20')} onCreate={() => go('search-22')} onClose={() => go('search-02')} />} /> },
    { id: 'search-20', section: 'watchlist', name: 'Added · toast', figma: '4912:15015', note: 'Floating L3 toast with OK; it leaves on its own (L3 timing: 6 s with an action — Figma says 2 s).', render: (go) => <Search results={bank} onBack={() => go('stocks-01')} onBookmark={() => go('search-18')} saved={savedAfter} toast={<Aerobar floating emphasis="primary" icon={<Icon icon={msCheck} />} heading="Stock added to watchlist successfully" action={{ label: 'OK', onClick: () => go('search-21') }} onDismiss={() => go('search-21')} />} /> },
    { id: 'search-21', section: 'watchlist', name: 'Added · bookmark filled', figma: '4912:15123', note: 'HDFC Bank’s bookmark is now filled with a tick.', render: (go) => <Search results={bank} onBack={() => go('stocks-01')} onBookmark={() => go('search-18')} saved={savedAfter} /> },
    { id: 'search-22', section: 'watchlist', name: 'Create watchlist', figma: '4912:14650', note: 'From "+ Create" (or the first time, when there’s no watchlist yet). The keyboard sits under the dock.', render: (go) => <Search results={bank} onBack={() => go('stocks-01')} onBookmark={() => go('search-18')} keyboard={false} sheet={<CreateWatchlistSheet name="Banking" onCreate={() => go('search-20')} onClose={() => go('search-18')} />} /> },
    { id: 'search-23', section: 'watchlist', name: 'Add to watchlist · few lists', figma: '4912:14477', note: 'Someone with 4 watchlists: the sheet is only as tall as its rows.', render: (go) => <Search results={bank} onBack={() => go('stocks-01')} onBookmark={() => go('search-18')} keyboard={false} sheet={<AddToWatchlistSheet lists={fewWatchlists} onSave={() => go('search-20')} onCreate={() => go('search-22')} onClose={() => go('search-02')} />} /> },
  ],
}
```

### `src/flows/search/screens/Search.module.css`

```css
/* Figma "Body" / "resut": groups 24 apart, 16 above the first and below the last. */
.body {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-24);
  padding-block: var(--l3-spacing-16);
}

/* A label over its tiles (Smart Options & Futures, Collections): 12 between. */
.group {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-12);
}

/* Figma "results": the label sits right on the list — the rows bring their own 16 above. */
.results {
  display: flex;
  flex-direction: column;
}

/* IPO lists (Most Subscribed, Upcoming IPOs): 8 more above, a bigger break than the chip groups (24 + 8 = 32). */
.ipoList {
  display: flex;
  flex-direction: column;
  padding-top: var(--l3-spacing-08);
}

/* No match: the L3 empty state fills the space above the keyboard and centres itself. */
.empty {
  display: flex;
  flex: 1;
}
```

### `src/flows/search/screens/Search.tsx`

```tsx
import { useState, type ReactNode } from 'react'
import { Actionbar, ActionbarAction } from '@l3/components/Actionbar'
import { Button } from '@l3/components/Button'
import { EmptyState } from '@l3/components/EmptyState'
import { Icon } from '@l3/components/Icon'
import { Select } from '@l3/components/Select'
import { Tabs } from '@l3/components/Tabs'
import { msClose, msDeleteForever } from '@l3/icons/material'
import { usePreview } from '../../../shared/prototype'
import { Screen } from '../../../shared/ui/Screen'
import { TextKeyboard, typeText } from '../../../shared/ui/TextKeyboard'
import { GroupHeader, ViewAll } from '../components/GroupHeader'
import { ResultList } from '../components/ResultList'
import { SearchChips } from '../components/SearchChips'
import { Tiles } from '../components/Tiles'
import { WatchlistMatches } from '../components/WatchlistMatches'
import {
  collections, ipoLists, placeholders, popular, recent, search, signalCategories, tabLabels,
  type SearchResult, type SearchResults, type SearchTab,
} from '../data'
import styles from './Search.module.css'

const TABS: SearchTab[] = ['stocks', 'fno', 'mf', 'ipo']

type SearchProps = {
  /** The tab it opens on. */
  tab?: SearchTab
  /** What's typed when it opens ('' = before typing: recent + popular searches). */
  query?: string
  /** Results to show for `query` instead of the sample search (e.g. "no matching watchlist"). */
  results?: SearchResults
  /** Ids in a watchlist (filled bookmarks) — the sample data's `saved` otherwise. */
  saved?: string[]
  /** The phone's keyboard is up (it is while searching; it goes down when a sheet opens). */
  keyboard?: boolean
  sheet?: ReactNode
  toast?: ReactNode
  onBack?: () => void
  /** A result's bookmark: open "Add to watchlist". */
  onBookmark?: (item: SearchResult) => void
  /** F&O: a Smart Options & Futures tile (or View All) opens its signals list. */
  onSignals?: (category: string) => void
}

/**
 * Search — one screen for Stocks, F&O, MF and IPO. The top bar is the search field (back · field · ✕ once something is
 * typed) with the four tabs in its bottom slot; each tab shows its result count once there's a query.
 * Before typing: recent and popular searches (chips), plus F&O's Smart Options & Futures, MF's collections or the IPO
 * lists. After typing: the matching watchlists, then the results (with a bookmark each). No match on a tab: the L3
 * empty state with "Clear Search". The phone keyboard stays up while searching.
 */
export function Search({ tab: initialTab = 'stocks', query: initialQuery, results, saved, keyboard = true, sheet, toast, onBack, onBookmark, onSignals }: SearchProps) {
  const { interactive } = usePreview()
  const startQuery = initialQuery ?? results?.query ?? ''
  const [tab, setTab] = useState<SearchTab>(initialTab)
  const [query, setQuery] = useState(startQuery)
  const shown = results && query === startQuery ? results : search(query)

  const tabItems = TABS.map((t) => ({ value: t, label: shown ? `${tabLabels[t]} (${shown.counts[t]})` : tabLabels[t] }))
  const clear = () => setQuery('')

  return (
    <Screen
      top={
        <Actionbar
          onBack={onBack}
          search={{ value: query, onChange: setQuery, placeholder: placeholders[tab], label: placeholders[tab], autoFocus: interactive && keyboard }}
          actions={query ? <ActionbarAction variant="ghost" icon={msClose} label="Clear search" onClick={clear} /> : undefined}
          bottom={<Tabs size="sm" aria-label="Search in" items={tabItems} value={tab} onChange={(v) => setTab(v as SearchTab)} />}
        />
      }
      bottom={keyboard ? <TextKeyboard onKey={(k) => setQuery((q) => typeText(q, k))} /> : undefined}
      toast={toast}
      sheet={sheet}
    >
      {!shown ? (
        <BeforeTyping tab={tab} onSignals={onSignals} />
      ) : shown.tabs[tab] ? (
        <Results tab={tab} results={shown} saved={saved} onBookmark={onBookmark} />
      ) : (
        <div className={styles.empty}>
          <EmptyState
            title="No search results found"
            description="Make sure you’ve spelled it correctly."
            action={<Button iconLeft={<Icon icon={msDeleteForever} />} onClick={clear}>Clear Search</Button>}
          />
        </div>
      )}
    </Screen>
  )
}

/** Nothing typed yet. */
function BeforeTyping({ tab, onSignals }: { tab: SearchTab; onSignals?: (category: string) => void }) {
  return (
    <div className={styles.body}>
      <SearchChips title="Recent Searches" rows={[recent[tab]]} />
      <SearchChips title="Popular Searches" rows={popular[tab]} />
      {tab === 'fno' && (
        <section className={styles.group} aria-label="Smart Options & Futures">
          <GroupHeader title="Smart Options & Futures" action={<ViewAll label="Smart Options & Futures" onClick={() => onSignals?.(signalCategories[0])} />} />
          <Tiles items={signalCategories} label="Smart Options & Futures lists" onPick={onSignals} />
        </section>
      )}
      {tab === 'mf' && (
        <section className={styles.group} aria-label="Collections">
          <GroupHeader title="Collections" action={<ViewAll label="collections" />} />
          <Tiles items={collections} label="Fund collections" muted />
        </section>
      )}
      {tab === 'ipo' &&
        ipoLists.map((list) => (
          <section key={list.title} className={styles.ipoList} aria-label={list.title}>
            <GroupHeader title={list.title} action={<ViewAll label={list.title} />} />
            <ResultList items={list.items} label={list.title} />
          </section>
        ))}
    </div>
  )
}

/** Something typed and this tab has matches (watchlists, items, or both). */
function Results({ tab, results, saved, onBookmark }: { tab: SearchTab; results: SearchResults; saved?: string[]; onBookmark?: (item: SearchResult) => void }) {
  const { watchlists, listTitle, items } = results.tabs[tab]!
  return (
    <div className={styles.body}>
      {watchlists.length > 0 && <WatchlistMatches watchlists={watchlists} />}
      <section className={styles.results} aria-label={listTitle}>
        <GroupHeader
          title={`${listTitle} (${items.length})`}
          action={tab === 'mf' && items.length > 0 ? <Select size="sm" icon="swap" aria-label="Returns period" onClick={() => {}}>3Y Returns</Select> : undefined}
        />
        <ResultList items={items} label={listTitle} saved={saved} onBookmark={onBookmark} />
      </section>
    </div>
  )
}
```

### `src/flows/search/screens/Signals.module.css`

```css
/* Pill row in the top bar's bottom slot: 12 above and below, the first pill at the 16 page margin, 8 apart; scrolls
   sideways when the lists don't fit (L3: pill rows at the top of a screen). */
.pills {
  position: relative;
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-08);
  padding: var(--l3-spacing-12) var(--l3-spacing-16);
  overflow-x: auto;
  scrollbar-width: none;
}

.pills::-webkit-scrollbar {
  display: none;
}

.pills > * {
  flex: none;
}

.page {
  display: flex;
  flex-direction: column;
}

/* Figma "header": 16 above, 16 from both edges, Filter left and the sort right. */
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  min-height: var(--l3-size-16);
  padding: var(--l3-spacing-16) var(--l3-spacing-16) 0;
}
```

### `src/flows/search/screens/Signals.tsx`

```tsx
import { useState } from 'react'
import { Actionbar } from '@l3/components/Actionbar'
import { Button } from '@l3/components/Button'
import { Icon } from '@l3/components/Icon'
import { Select } from '@l3/components/Select'
import { Tab, Tabs } from '@l3/components/Tabs'
import { lmSwitchArrowVertical } from '@l3/icons/lemonnade'
import { msFilterList } from '@l3/icons/material'
import { Screen } from '../../../shared/ui/Screen'
import { ResultList } from '../components/ResultList'
import { indexSignals, signalScopes, sortLabels, stockSignals, type SignalScope, type SignalSort } from '../data'
import styles from './Signals.module.css'

type SignalsProps = {
  scope?: SignalScope
  category?: string
  sort?: SignalSort
  onBack?: () => void
}

/**
 * Smart Options & Futures — a signals list (Top Traded, OI Gainers …), opened from a tile on F&O search.
 * Top bar: back + title; in its bottom slot an L3 action pill (↕ Indices / Stocks — switches the underlyings) and the
 * lists as pill tabs. Then the list header — Filter (Ghost Small) on the left, the sort (L3 Select ↕: Price / Volume)
 * on the right — and the contracts (no bookmarks here). Sorted by volume, each row shows its volume instead of price +
 * change.
 */
export function Signals({ scope: initialScope = 'indices', category = 'Top Traded', sort: initialSort = 'price', onBack }: SignalsProps) {
  const [scope, setScope] = useState<SignalScope>(initialScope)
  const [list, setList] = useState(category)
  const [sort, setSort] = useState<SignalSort>(initialSort)
  const other: SignalScope = scope === 'indices' ? 'stocks' : 'indices'
  const { label, categories } = signalScopes[scope]
  const rows = scope === 'indices' ? indexSignals : stockSignals
  const sorted = sort === 'volume' ? [...rows].sort((a, b) => (b.volume ?? 0) - (a.volume ?? 0)) : rows

  return (
    <Screen
      top={
        <Actionbar
          title="Smart Options & Futures"
          onBack={onBack}
          bottom={
            <div className={styles.pills}>
              <Tab
                action
                appearance="pill"
                iconRight={<Icon icon={lmSwitchArrowVertical} />}
                aria-label={`Showing ${label}. Switch to ${signalScopes[other].label}`}
                onClick={() => setScope(other)}
              >
                {label}
              </Tab>
              <Tabs
                appearance="pill"
                aria-label="Signals lists"
                items={categories.map((c) => ({ value: c, label: c }))}
                value={categories.includes(list) ? list : categories[0]}
                onChange={setList}
              />
            </div>
          }
        />
      }
    >
      <div className={styles.page}>
        <div className={styles.header}>
          <Button variant="ghost" size="sm" iconRight={<Icon icon={msFilterList} />} onClick={() => {}}>
            Filter
          </Button>
          <Select size="sm" icon="swap" aria-label={`Sort by ${sortLabels[sort]}. Switch`} onClick={() => setSort(sort === 'price' ? 'volume' : 'price')}>
            {sortLabels[sort]}
          </Select>
        </div>
        <ResultList items={sorted} label={list} bookmarks={false} showVolume={sort === 'volume'} />
      </div>
    </Screen>
  )
}
```

### `src/flows/search/sheets/WatchlistSheets.module.css`

```css
/* Figma "content slot": 16 under the header (top space), 24 above the dock (bottom space); rows run edge to edge. */
.lists {
  display: flex;
  flex-direction: column;
  padding: var(--l3-spacing-16) 0 var(--l3-spacing-24);
}

/* The field inside the 16 page margin. */
.form {
  padding: var(--l3-spacing-16) var(--l3-spacing-16) var(--l3-spacing-24);
}
```

### `src/flows/search/sheets/WatchlistSheets.tsx`

```tsx
import { useId, useState } from 'react'
import { BottomSheetHeader } from '@l3/components/BottomSheet'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { Checkbox } from '@l3/components/Checkbox'
import { Icon } from '@l3/components/Icon'
import { ListCell } from '@l3/components/ListCell'
import { TextField } from '@l3/components/TextField'
import { msAdd } from '@l3/icons/material'
import { usePreview } from '../../../shared/prototype'
import { Sheet } from '../../../shared/ui/Sheet'
import { TextKeyboard, typeText } from '../../../shared/ui/TextKeyboard'
import styles from './WatchlistSheets.module.css'

type AddProps = {
  /** Every watchlist the user has, by name. */
  lists: string[]
  /** Already ticked (the lists the item is in, or a prototype state). */
  checked?: string[]
  onSave?: (lists: string[]) => void
  onCreate?: () => void
  onClose?: () => void
}

/**
 * Add to watchlist — from a result's bookmark. Header with "+ Create" (Tertiary Small) on the right; one row per
 * watchlist (L3 list cell, the checkbox on the left; tap anywhere on the row); Save in the dock, off until at least one
 * list is ticked. Long lists scroll inside the sheet.
 */
export function AddToWatchlistSheet({ lists, checked: initial = [], onSave, onCreate, onClose }: AddProps) {
  const id = useId()
  const [checked, setChecked] = useState<string[]>(initial)
  const toggle = (name: string) => setChecked((c) => (c.includes(name) ? c.filter((n) => n !== name) : [...c, name]))
  return (
    <Sheet
      labelledBy={id}
      onClose={onClose}
      header={
        <BottomSheetHeader
          headingId={id}
          heading="Add to watchlist"
          trailing={<Button variant="tertiary" size="sm" iconLeft={<Icon icon={msAdd} />} onClick={onCreate}>Create</Button>}
        />
      }
      footer={
        <ButtonGroup aria-label="Add to watchlist">
          <Button disabled={checked.length === 0} onClick={() => onSave?.(checked)}>Save</Button>
        </ButtonGroup>
      }
    >
      <div className={styles.lists} role="group" aria-label="Watchlists">
        {lists.map((name) => (
          <ListCell
            key={name}
            as="label"
            iconLeft={<Checkbox checked={checked.includes(name)} onChange={() => toggle(name)} />}
            label={name}
          />
        ))}
      </div>
    </Sheet>
  )
}

/**
 * Create watchlist — from "+ Create", or straight away when the user has no watchlist yet. One field (Watchlist Name,
 * typing) and Create; the phone keyboard sits under the dock while typing.
 */
export function CreateWatchlistSheet({ name: initialName = '', onCreate, onClose }: { name?: string; onCreate?: (name: string) => void; onClose?: () => void }) {
  const id = useId()
  const { interactive } = usePreview()
  const [name, setName] = useState(initialName)
  return (
    <Sheet
      labelledBy={id}
      onClose={onClose}
      header={<BottomSheetHeader headingId={id} heading="Create watchlist" />}
      footer={
        <>
          <ButtonGroup aria-label="Create watchlist">
            <Button disabled={!name.trim()} onClick={() => onCreate?.(name.trim())}>Create</Button>
          </ButtonGroup>
          <TextKeyboard onKey={(k) => setName((n) => typeText(n, k))} />
        </>
      }
    >
      <div className={styles.form}>
        <TextField label="Watchlist Name" value={name} placeholder="e.g. Banking" autoFocus={interactive} onChange={(e) => setName(e.target.value)} />
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

### `src/shared/prototype.ts`

```ts
// Prototype behaviour flows may use — the ONLY way a flow touches the preview. In a real app these are: interactive =
// always true; useAutoAdvance = "when the server answers, go on" (a loader / polling step).
export { LOADING_MS, usePreview, useAutoAdvance } from '../preview/context'
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

### `src/shared/ui/TextKeyboard.module.css`

```css
/* OS text keyboard mock (Figma "L3: System keyboard", 260 tall at 360): 5 rows of 40 tall keys on a grey tray. */
.keyboard {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-08);
  padding: var(--l3-spacing-12) var(--l3-spacing-06) var(--l3-spacing-16);
  /* On a real phone, clear the home indicator (like the L3 button dock does). */
  padding-bottom: calc(var(--l3-spacing-16) + env(safe-area-inset-bottom, var(--l3-spacing-00)));
  background: var(--l3-surface-secondary);
}

.row {
  display: flex;
  gap: var(--l3-spacing-04);
}

/* The A–L row is inset by about half a key, like a phone keyboard. */
.row[data-row='2'] {
  padding-inline: var(--l3-spacing-16);
}

.key {
  display: flex;
  flex: 1 1 0;
  align-items: center;
  justify-content: center;
  min-width: 0;
  height: var(--l3-size-40);
  padding: 0;
  border: 0;
  border-radius: var(--l3-radius-08);
  background: var(--l3-surface-primary);
  color: var(--l3-content-primary);
  font: var(--l3-text-description-18);
  cursor: pointer;
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
}

.key[data-wide] {
  flex-grow: 1.5;
}

.key[data-kind='space'] {
  flex-grow: 5;
  font: var(--l3-text-label-12);
}

.key[data-kind='fn'] {
  background: var(--l3-surface-tertiary);
  font: var(--l3-text-label-14);
}

/* The keyboard's action key (Search) — the phone's own accent, not an app colour. */
.key[data-kind='go'] {
  background: var(--l3-surface-accent-discover-default);
  color: var(--l3-content-inverted);
}

.key :is(svg, span) {
  width: var(--l3-icon-size-20);
  height: var(--l3-icon-size-20);
}

.key:active {
  transform: scale(var(--l3-motion-scale-press-default));
}

@media (prefers-reduced-motion: reduce) {
  .key:active {
    transform: none;
  }
}
```

### `src/shared/ui/TextKeyboard.tsx`

```tsx
import type { PointerEvent } from 'react'
import { Icon } from '@l3/components/Icon'
import { msArrowUpward, msBackspace, msKeyboardTab } from '@l3/icons/material'
import styles from './TextKeyboard.module.css'

type Key = { key: string; label?: string; kind?: 'letter' | 'fn' | 'space' | 'go'; wide?: boolean }

const ROWS: Key[][] = [
  '1234567890'.split('').map((k) => ({ key: k })),
  'QWERTYUIOP'.split('').map((k) => ({ key: k })),
  'ASDFGHJKL'.split('').map((k) => ({ key: k })),
  [{ key: 'Shift', kind: 'fn', wide: true }, ...'ZXCVBNM'.split('').map((k) => ({ key: k })), { key: 'Backspace', kind: 'fn', wide: true }],
  [{ key: '?123', kind: 'fn', wide: true }, { key: ',' }, { key: ' ', label: 'EN • LV', kind: 'space' }, { key: '.' }, { key: 'Enter', kind: 'go', wide: true }],
]

/**
 * The phone's own text keyboard (OS chrome, NOT part of the app or L3 — Figma "L3: System keyboard", isNumeric =
 * False). Drawn so search screens read like the phone; in the prototype the keys type into the screen. Real apps get the
 * system keyboard by focusing the text field (search: `enterKeyHint="search"`). `data-surface` tells the OS navigation
 * bar below it to take the keyboard's grey.
 */
export function TextKeyboard({ onKey }: { onKey?: (key: string) => void }) {
  return (
    <div className={styles.keyboard} role="group" aria-label="Keyboard" data-surface="secondary">
      {ROWS.map((row, i) => (
        <div key={i} className={styles.row} data-row={i}>
          {row.map((k) => (
            <button
              key={k.key}
              type="button"
              className={styles.key}
              data-kind={k.kind ?? 'letter'}
              data-wide={k.wide || undefined}
              aria-label={label(k)}
              onPointerDown={keepFocus}
              onClick={() => onKey?.(k.key)}
            >
              {k.key === 'Shift' ? <Icon icon={msArrowUpward} /> : k.key === 'Backspace' ? <Icon icon={msBackspace} /> : k.key === 'Enter' ? <Icon icon={msKeyboardTab} /> : (k.label ?? k.key)}
            </button>
          ))}
        </div>
      ))}
    </div>
  )
}

const label = (k: Key) =>
  ({ Shift: 'Shift', Backspace: 'Delete', Enter: 'Search', ' ': 'Space', '?123': 'Numbers and symbols' } as Record<string, string>)[k.key]

/** Like the OS keyboard, a key press doesn't take focus: the field being typed into keeps its Typing state. */
const keepFocus = (e: PointerEvent) => e.preventDefault()

/** Apply a keyboard key to a text value: letters, digits, space, comma, full stop and delete (Shift / ?123 do nothing here). */
export const typeText = (value: string, key: string) => {
  if (key === 'Backspace') return value.slice(0, -1)
  if (key.length !== 1) return value
  // Like a phone keyboard: the first letter is a capital, the rest lower case.
  return value + (value === '' ? key.toUpperCase() : key.toLowerCase())
}
```
