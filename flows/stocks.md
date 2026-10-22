# Flow: Stocks (`stocks`)

> One self-contained file for converting this flow to another platform. Read https://kirtanyak-pe.github.io/lemonn-app-prototype/CONVERT.md first. Design system: Lemonnade V3 @ `cd84910` (components, tokens and rules: https://kirtanyak-pe.github.io/lemonnade-v3-docs/llms.txt).

The Stocks tab — the home after login: indices, holdings, products, movers, watchlists, ideas.

Prototype: https://kirtanyak-pe.github.io/lemonn-app-prototype/#/stocks · machine-readable: https://kirtanyak-pe.github.io/lemonn-app-prototype/flows/stocks.json

## Screens

| Id | Screen | Figma | Taps go to | Note |
|---|---|---|---|---|
| `stocks-01` | Home | — | `search-01` | Top of the page: search, indices, holdings, products, shortcuts. |
| `stocks-02` | Home · top movers & watchlists | — | `search-01` | Same page, scrolled. |
| `stocks-03` | Home · top losers | — | `search-01` | The "Top losers" chip picked. |
| `stocks-04` | Home · Smart Invest | — | `search-01` | Same page, scrolled: managed strategies, then the announcement. |
| `stocks-05` | Home · sectors & highlights | — | `search-01` | Same page, scrolled. |
| `stocks-06` | Home · trade ideas | — | `search-01` | Same page, scrolled: featured ideas, neighbours peek. |
| `stocks-07` | Home · idea baskets & disclosure | — | `search-01` | The end of the page. |

## Spec (README.md)


> The spec for this flow. A developer should be able to build it on any platform (Flutter, iOS, web) from this file,
> the screens in this folder and the live app — without reading the rest of the repo.

- **Prototype:** https://kirtanyak-pe.github.io/lemonn-app-prototype/#/stocks · one screen: `#/stocks-01`
- **Source:** screenshots of the live app's Stocks tab (dark theme), shared 2026-10-17 — there's no Figma for it yet.
  Rebuilt in L3.
- **Status:** 🟨 Home done; search is its own flow (`search`), opened from the top bar (stock page and lists come next) · 2026-10-19
- **Entry:** the app's home: after login (returning user), after account setup ("Explore stocks first"), after the
  first deposit ("Start investing"); the Stocks tab of the main navbar. **Exit:** everything on the page opens its
  own screen (not built yet).

### Screens

One long, scrolling L1 page. The board shows it as frames scrolled to each part, like the screenshots.

| Id | Screen | What it shows / when |
|---|---|---|
| `stocks-01` | Home | Top bar (search, wallet, profile) · index strip · current holdings · product cards · Do more with Lemonn |
| `stocks-02` | Home · top movers & watchlists | Top movers (chips + two rows of stock cards) · watchlists |
| `stocks-03` | Home · top losers | The "Top losers" chip picked |
| `stocks-04` | Home · Smart Invest | Strategies on the brand tint · the announcement · sectors |
| `stocks-05` | Home · sectors & highlights | Sectors · Highlights (Mutual funds, F&O) |
| `stocks-06` | Home · trade ideas | Trade ideas on the discover tint (one featured card, neighbours peek) · View all recommendations |
| `stocks-07` | Home · idea baskets & disclosure | Invest in ideas (baskets) · the broker's regulatory line |

### Page structure (top to bottom)

1. **Top bar (sticky):** search ("Search stocks") · Wallet · Profile. Tapping the search field opens Search
   (`search-01`, the Search flow: Stocks tab, keyboard up); nothing is typed on Home. Back on Search returns here.
2. **Summary area** (grey, `surface/secondary`):
   - **Index strip** (scrolls sideways): NIFTY 50 / SENSEX / NIFTY BANK — value, day change; each opens its chart.
   - **Current holdings:** the value (the page's biggest number) and the overall return; the block opens Portfolio.
   - **Products:** Mutual funds (240+ funds) · Equity F&O (0 slippage) · Commodities (0 brokerage, "New").
3. **Feed** on a rounded sheet over the summary area (radius 32 at the top), sections 32 apart:
   - **Do more with Lemonn:** Trade ideas · IPO ("8 new") · Watchlist (count) · Refer & earn ("New").
   - **Top movers:** chips Top gainers / Top losers / Under ₹100 → two rows of stock cards (logo, symbol, price,
     change). View all.
   - **Watchlist:** the user's watchlists (name, stock count). New watchlist.
   - **Smart Invest** (brand tint): strategy cards — name, min. deposit, last week's daily results, AUM, slots left;
     footer: monthly charges (₹0, was ₹300) and Invest now. Tag "NSE/BSE approved".
   - **Announcements:** one campaign card with its CTA.
   - **Invest in sectors:** sector cards — stock count and the day's change. View all.
   - **Highlights:** Mutual funds (tax saving: up to ₹46,800 a year) · F&O (Nifty 50 today).
   - **Trade ideas** (discover tint, tag "Live analysis"): idea cards — logo, symbol, potential profit, how it moved
     since; Details · Buy. Then "View all recommendations".
   - **Invest in ideas:** baskets — their stocks' logos, name, count.
   - **Disclosure:** the broker's registration line.
4. **Main navbar:** Stocks (selected) · Market · Portfolio · Mutual Fund · F&O.

### Data

- `src/shared/data/market.ts` — `indices[]` and `stocks` (symbol, name, price, day change %, initials, `logoUrl`).
  Shared with Market / F&O. Values are samples from the live app.
- `src/shared/data/portfolio.ts` — `holdings` (current value, return %) and `watchlists[]`. **Fictional** (the
  screenshots are a real account).
- `data.ts` — `products`, `shortcuts` (with badge count / text), `movers` (three lists), `strategies`,
  `announcement`, `sectors`, `highlights`, `tradeIdeas`, `ideaBaskets`, `disclosure`. Each is one API section.
- API: one home call (sections in order, each optional) + live prices for the index strip and the stock cards.

### Components (L3 → any platform)

| Where | L3 component (Figma name) | Props / variant |
|---|---|---|
| Top bar | Actionbar | `search` (placeholder "Search stocks") + 2 `ActionbarAction`s (Wallet, Profile) |
| Index strip, holdings, shortcuts | Card | `variant="flat"`, `padding="none"`, `onClick` (tap targets with no card look) |
| Changes (indices, holdings, stocks, sectors, ideas) | PriceChange | `sm` in cards, `lg` + `arrow` next to big numbers |
| Products, movers, sectors, baskets, highlights, ideas, strategies | Card | clickable (`onClick`); strategies `padding="none"` + `footerFilled`; highlights / ideas `footer` |
| Section titles | SectionHeader | View all / New watchlist = `action: view-all`; Smart Invest / Trade ideas = `tag` |
| Movers chips | Tabs | `appearance="pill"` (filter: hug + scroll), icon left |
| Stock / basket logos | Avatar | `src` = logo URL, falls back to `initials`; 40 (48 in ideas); baskets `shape="rounded"` |
| Watchlists | ListCell | `variant="card"`, `as="button"`, Avatar left, chevron right |
| Shortcut / highlight art | **Illustration slot** (`Spot`, src/shared/ui) | md 64, success / discover / brand tone |
| Counts / "New" | Badge (`count`) · Tag (`error`, `sm`) | over the shortcut's icon |
| Rows that scroll sideways | **Rail** (src/shared/ui) | `visible` = how many show (a fraction lets the next peek); `rows={2}` for movers |
| Buttons | Button | Invest now / Explore = tertiary · Details = secondary · Buy = `buy` · Explore now = `brand` |
| Main navbar | BottomNavbar (`MainNav`, src/shared/ui) | `value="stocks"` |
| Layout | Stack · Section · Inset · Grid (Figma auto-layout frames) | Page: Stack → summary Stack `gap="24"` on surface/secondary, feed Stack `gap="32"` on the rounded sheet; tinted bands Section `paddingBlock="24"`. Each block: Stack `as="section" gap="16"`, heading in an Inset `paddingInline="16"`; index strip a horizontal Stack `gap="16"`; shortcuts Inset 16 → Grid `columns={4} gap="00"`; strategy card body Section `padding="12" gap="12"`; trade-idea buttons Grid `columns={2} gap="12"`; disclosure Section `paddingInline="16" gap="04" align="start"`. Content inside clickable cards stays CSS (spans inside a button) |

### Rules & behaviour

- The top bar stays; everything else scrolls. Rails scroll sideways and snap; trade ideas snap to the centre.
- Index values and stock prices update live (prices tick; the change colour follows the sign — never by hand).
- Movers chips switch the list in place; the chip row and both card rows keep their scroll position per list.
- Badges: "8 new" (new IPOs), a count (watchlist stocks), "New" (referral offer) — from the API; hidden when empty.
- Smart Invest slots: show "x/y"; when 0 left, the CTA becomes "Join waitlist" (not drawn).
- Holdings: a user with no holdings sees "Start investing" instead of the value (not drawn).

### Differences from the live app

- **Top bar:** the live app puts the profile avatar on the left of the search field. The L3 Actionbar has no leading
  avatar, so Profile is the second action on the right, and the search field is L3's search mode.
- **Sections:** same order and content, rebuilt with L3 cards, tags and buttons; the grey summary area and the
  rounded feed sheet are kept (they read well in light and dark).
- **Smart Invest / Trade ideas tints** use the L3 brand and discover light surfaces (olive and navy in dark mode).
- **Shortcuts** use illustration slots (green) instead of the live app's icon artwork.
- **Announcement** is an L3 card with an illustration slot instead of the campaign image.
- **Sample data:** holdings and watchlists are fictional; tickers and prices are samples.

### L3 gaps

- **Rail / carousel:** no sideways-scrolling row in L3 — `Rail` (src/shared/ui) stands in.
- **Actionbar leading avatar** (profile on the left of the search field).
- **Illustrations** for shortcuts, highlights and campaigns — `Spot` slots stand in.

### Changelog

- 2026-10-21 — layout moved from CSS-module classes to the L3 layout blocks (Stack, Section, Inset, Grid); nothing moved (measured at 392). The home shortcuts sit edge to edge (gap 0).
- 2026-10-19 — the search field opens the Search flow (`search-01`).
- 2026-10-17 — first version: Home (7 frames of one page).


## Source — this flow

### `src/flows/stocks/components/Discover.tsx`

```tsx
import { useState } from 'react'
import { Avatar } from '@l3/components/Avatar'
import { Badge } from '@l3/components/Badge'
import { Card } from '@l3/components/Card'
import { Grid } from '@l3/components/Grid'
import { Icon } from '@l3/components/Icon'
import { Inset } from '@l3/components/Inset'
import { ListCell } from '@l3/components/ListCell'
import { PriceChange } from '@l3/components/PriceChange'
import { SectionHeader } from '@l3/components/SectionHeader'
import { Stack } from '@l3/components/Stack'
import { Tabs } from '@l3/components/Tabs'
import { Tag } from '@l3/components/Tag'
import { msChevronRight, msCurrencyRupee, msTrendingDown, msTrendingUp } from '@l3/icons/material'
import { inr } from '../../../shared/data/format'
import { watchlists } from '../../../shared/data/portfolio'
import { Rail } from '../../../shared/ui/Rail'
import { Spot } from '../../../shared/ui/Spot'
import { movers, shortcuts, type MoverList } from '../data'
import { t } from '../copy'
import styles from './Home.module.css'

/** "Do more with Lemonn": four feature shortcuts, with a count or "New" where there's something to see. */
export function Shortcuts({ onOpen }: { onOpen?: (id: string) => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('shortcuts.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('shortcuts.title')} /></Inset>
      <Inset paddingInline="16" paddingBlock="00">
        <Grid columns={4} gap="00">
          {shortcuts.map((s) => (
            <Card key={s.id} variant="flat" padding="none" onClick={() => onOpen?.(s.id)}>
              <span className={styles.shortcut}>
                <span className={styles.shortcutIcon}>
                  <Spot icon={s.icon} tone="success" size="md" />
                  {s.badge?.count !== undefined && (
                    <span className={styles.overIcon}><Badge count={s.badge.count} color="neutral" label={t('shortcuts.watchlistBadge', { count: s.badge.count })} /></span>
                  )}
                  {s.badge?.text && <span className={styles.overIcon}><Tag color="error" variant="primary" size="sm">{s.badge.text}</Tag></span>}
                </span>
                <span className={styles.shortcutLabel}>{s.label}</span>
              </span>
            </Card>
          ))}
        </Grid>
      </Inset>
    </Stack>
  )
}

const moverTabs: { value: MoverList; label: 'movers.gainers' | 'movers.losers' | 'movers.under100'; icon: string }[] = [
  { value: 'gainers', label: 'movers.gainers', icon: msTrendingUp },
  { value: 'losers', label: 'movers.losers', icon: msTrendingDown },
  { value: 'under-100', label: 'movers.under100', icon: msCurrencyRupee },
]

/** Top movers: gainers / losers / under ₹100 (chips), as two rows of stock cards that scroll together. */
export function TopMovers({ initial = 'gainers', onStock, onViewAll }: { initial?: MoverList; onStock?: (symbol: string) => void; onViewAll?: () => void }) {
  const [list, setList] = useState<MoverList>(initial)
  return (
    <Stack as="section" gap="16" aria-label={t('movers.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('movers.title')} action={{ type: 'view-all', onClick: () => onViewAll?.() }} /></Inset>
      <Inset paddingInline="16" paddingBlock="00">
        <Tabs
          aria-label={t('movers.tabsLabel')}
          appearance="pill"
          items={moverTabs.map((tab) => ({ value: tab.value, label: t(tab.label), iconLeft: <Icon icon={tab.icon} size={16} /> }))}
          value={list}
          onChange={(v) => setList(v as MoverList)}
        />
      </Inset>
      <Rail label={t(moverTabs.find((m) => m.value === list)!.label)} visible={2.15} rows={2}>
        {movers[list].map((stock) => (
          <Card key={stock.symbol} onClick={() => onStock?.(stock.symbol)}>
            <span className={styles.cardStack}>
              <Avatar initials={stock.initials} src={stock.logoUrl} size={40} label={stock.name} />
              <span className={styles.cardSymbol}>{stock.symbol}</span>
              <span className={styles.cardPrice}>{inr(stock.price)}</span>
              <PriceChange value={stock.changePct} size="sm" arrow />
            </span>
          </Card>
        ))}
      </Rail>
    </Stack>
  )
}

/** The user's watchlists, each opening its list. "New watchlist" creates one. */
export function Watchlists({ onOpen, onCreate }: { onOpen?: (id: string) => void; onCreate?: () => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('watchlists.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('watchlists.title')} action={{ type: 'view-all', label: t('watchlists.new'), onClick: () => onCreate?.() }} /></Inset>
      <Rail label={t('watchlists.railLabel')} visible={1.45}>
        {watchlists.map((w) => (
          <ListCell
            key={w.id}
            variant="card"
            as="button"
            iconLeft={<Avatar initials={w.name.slice(0, 1)} size={40} />}
            label={w.name}
            description={t(w.count === 1 ? 'watchlists.one' : 'watchlists.many', { count: w.count })}
            iconRight={<Icon icon={msChevronRight} />}
            onClick={() => onOpen?.(w.id)}
          />
        ))}
      </Rail>
    </Stack>
  )
}
```

### `src/flows/stocks/components/Home.module.css`

```css
/*
 * Stocks home — the insides of each section (the page layout is screens/Home.module.css). Tokens only. A section is
 * a Stack (heading → content 16, DESIGN_SYSTEM 2.3) with the heading in an Inset (the 16 page margin). Most of what
 * is left here sits inside clickable Cards, which are buttons: only spans go there, and the layout blocks render
 * block elements only.
 */

/* ---- Index strip: NIFTY 50 23,146.15 +0.65% › | SENSEX … — scrolls sideways (padding inside the scroller) ---- */
.indexStrip {
  position: relative; /* keeps absolutely placed bits inside the scrolling strip (see Rail) */
  overflow-x: auto;
  padding-inline: var(--l3-spacing-16);
  scrollbar-width: none;
}

.indexStrip::-webkit-scrollbar {
  display: none;
}

/* One index (divider + card): a horizontal Stack. It keeps its full width in the scrolling strip — a Stack sets
   min-width: 0, which would let it shrink. */
.indexItem {
  flex: none;
}

.index {
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-08);
  white-space: nowrap;
}

.indexName {
  font: var(--l3-text-label-14);
  color: var(--l3-content-secondary);
}

.indexValue {
  font: var(--l3-text-label-14);
}

.divider {
  flex: none;
  align-self: stretch;
  border-left: 1px solid var(--l3-border-light);
}

/* ---- Holdings: the biggest number on the page ---- */
.holdings {
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-12);
  padding-inline: var(--l3-spacing-16);
}

.holdingsText {
  display: flex;
  flex: 1;
  flex-direction: column;
  gap: var(--l3-spacing-04);
  min-width: 0;
}

.holdingsLabel {
  font: var(--l3-text-description-14);
  color: var(--l3-content-secondary);
}

.holdingsValue {
  display: flex;
  flex-wrap: wrap;
  align-items: baseline;
  gap: var(--l3-spacing-08);
  font: var(--l3-text-heading-28);
}

/* The round "→" that says the whole block opens Portfolio. */
.roundArrow {
  display: grid;
  flex: none;
  place-items: center;
  width: var(--l3-size-40);
  height: var(--l3-size-40);
  border-radius: var(--l3-radius-full);
  background: var(--l3-surface-primary);
  box-shadow: inset 0 0 0 1px var(--l3-border-light), var(--l3-shadow-elevation-low);
}

/* ---- Small cards (products, movers, sectors) ---- */
.cardStack {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-08);
}

.cardRow {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--l3-spacing-08);
}

.cardTitle {
  overflow: hidden;
  font: var(--l3-text-label-16);
  text-overflow: ellipsis;
  white-space: nowrap;
}

.productTitle {
  overflow: hidden;
  font: var(--l3-text-label-14);
  text-overflow: ellipsis;
  white-space: nowrap;
}

.cardSymbol {
  overflow: hidden;
  font: var(--l3-text-label-14);
  color: var(--l3-content-secondary);
  text-overflow: ellipsis;
  white-space: nowrap;
}

.cardPrice {
  font: var(--l3-text-label-16);
}

.cardMeta {
  font: var(--l3-text-description-12);
  color: var(--l3-content-secondary);
}

.cardRow > .cardMeta {
  white-space: nowrap;
}

.highlight {
  font: var(--l3-text-label-16);
  color: var(--l3-content-accent-success-default);
}

.chevron {
  display: inline-flex;
  flex: none;
  color: var(--l3-content-secondary);
}

/* "New" sits on the card's top edge, at the content's left (the products row leaves room above for it). */
.onEdge {
  position: absolute;
  top: calc(-1 * (var(--l3-spacing-12) + var(--l3-spacing-10)));
  left: 0;
}

.onEdgeRoom {
  padding-top: var(--l3-spacing-12);
}

/* ---- Do more with Lemonn: four shortcuts (an Inset around a 4-column Grid) ---- */
.shortcut {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--l3-spacing-08);
  text-align: center;
}

.shortcutIcon {
  position: relative;
  display: inline-flex;
}

/* The count / "New" over the icon's top edge. */
.overIcon {
  position: absolute;
  top: calc(-1 * var(--l3-spacing-08));
  left: 50%;
  transform: translateX(-50%);
  white-space: nowrap;
}

.shortcutLabel {
  font: var(--l3-text-label-12);
}

/* ---- Smart Invest strategy card: a Section (12 padding, 12 gap); the head is a horizontal Stack ---- */

/* The head's 12 below and its hairline: Section pads both sides alike and has no one-side border. */
.strategyHead {
  padding-bottom: var(--l3-spacing-12);
  border-bottom: 1px solid var(--l3-border-light);
}

.strategyName {
  flex: 1;
  min-width: 0;
  font: var(--l3-text-heading-16);
}

.strategyDeposit {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: var(--l3-spacing-02);
}

/* Three stats: left, centre and right-aligned in auto · 1fr · auto columns (Grid has equal columns only). */
.stats {
  display: grid;
  grid-template-columns: auto 1fr auto;
  gap: var(--l3-spacing-16);
}

.stat {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-04);
}

.stat:nth-child(2) {
  align-items: center;
}

.stat:last-child {
  align-items: flex-end;
}

.statValue {
  font: var(--l3-text-label-16);
  white-space: nowrap;
}

/* Last week: one small ring per trading day, profit or loss. */
.days {
  display: flex;
  gap: var(--l3-spacing-04);
}

.day {
  display: grid;
  place-items: center;
  width: var(--l3-size-20);
  height: var(--l3-size-20);
  border-radius: var(--l3-radius-full);
  font: var(--l3-text-label-10);
}

.day[data-result='profit'] {
  background: var(--l3-surface-accent-success-light);
  color: var(--l3-content-accent-success-default);
  box-shadow: inset 0 0 0 1px var(--l3-border-accent-success-default);
}

.day[data-result='loss'] {
  background: var(--l3-surface-accent-error-light);
  color: var(--l3-content-accent-error-default);
  box-shadow: inset 0 0 0 1px var(--l3-border-accent-error-default);
}

/* The footer's charges + Invest now: a horizontal Stack that fills the footer. */
.charges {
  flex: 1;
}

.chargesText {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-02);
}

.price {
  display: flex;
  align-items: baseline;
  gap: var(--l3-spacing-04);
  font: var(--l3-text-label-16);
}

.was {
  font: var(--l3-text-label-12);
  color: var(--l3-content-tertiary);
  text-decoration: line-through;
}

/* ---- Announcement ---- */
.announcement {
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-16);
}

.announcementText {
  display: flex;
  flex: 1;
  flex-direction: column;
  gap: var(--l3-spacing-04);
  min-width: 0;
}

.announcementText > :first-child {
  align-self: flex-start;
}

.announcementTitle {
  font: var(--l3-text-heading-18);
}

/* ---- Highlights ---- */
.highlightCard {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-08);
}

.highlightTitle {
  font: var(--l3-text-heading-20);
}

.highlightValue {
  font: var(--l3-text-heading-24);
  color: var(--l3-content-accent-success-default);
}

/* ---- Trade ideas ---- */
.idea {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--l3-spacing-08);
  padding-block: var(--l3-spacing-08);
  text-align: center;
}

.ideaSymbol {
  font: var(--l3-text-heading-18);
}

.ideaPotential {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--l3-spacing-02);
}

.ideaMoved {
  display: inline-flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: baseline;
  gap: var(--l3-spacing-04);
  font: var(--l3-text-description-14);
  color: var(--l3-content-secondary);
}

/* Details · Buy: a 2-column Grid that fills the footer. */
.ideaActions {
  flex: 1;
}

/* "Live" — a small pulsing dot before the tag's label. */
.liveDot {
  width: var(--l3-size-08);
  height: var(--l3-size-08);
  border-radius: var(--l3-radius-full);
  background: var(--l3-content-accent-error-default);
  animation: live-pulse var(--l3-motion-duration-long) var(--l3-motion-easing-standard) infinite alternate;
}

@keyframes live-pulse {
  to { opacity: var(--l3-opacity-40); }
}

@media (prefers-reduced-motion: reduce) {
  .liveDot {
    animation: none;
  }
}

/* ---- Invest in ideas: a basket's logos in a 4-across grid ---- */
.logos {
  display: grid;
  grid-template-columns: repeat(4, max-content);
  gap: var(--l3-spacing-08);
}

.basketName {
  font: var(--l3-text-heading-18);
}

/* ---- Disclosure ---- */
.disclosureText {
  margin: 0;
  font: var(--l3-text-description-12);
  color: var(--l3-content-tertiary);
}
```

### `src/flows/stocks/components/Ideas.tsx`

```tsx
import { Avatar } from '@l3/components/Avatar'
import { Button } from '@l3/components/Button'
import { Card } from '@l3/components/Card'
import { Grid } from '@l3/components/Grid'
import { Icon } from '@l3/components/Icon'
import { Inset } from '@l3/components/Inset'
import { ListCell } from '@l3/components/ListCell'
import { PriceChange } from '@l3/components/PriceChange'
import { Section } from '@l3/components/Section'
import { SectionHeader } from '@l3/components/SectionHeader'
import { Stack } from '@l3/components/Stack'
import { Tag } from '@l3/components/Tag'
import { msChevronRight } from '@l3/icons/material'
import { Rail } from '../../../shared/ui/Rail'
import { disclosure, ideaBaskets, tradeIdeas } from '../data'
import { t } from '../copy'
import styles from './Home.module.css'

/** Trade ideas: research picks, one featured card at a time (neighbours peek). Sits on the discover tint. */
export function TradeIdeas({ onDetails, onBuy, onViewAll }: { onDetails?: (id: string) => void; onBuy?: (id: string) => void; onViewAll?: () => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('ideas.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('ideas.title')} tag={<Tag color="discover" variant="primary" iconLeft={<span className={styles.liveDot} />}>{t('ideas.tag')}</Tag>} /></Inset>
      <Rail label={t('ideas.railLabel')} visible={1.15} snap="center">
        {tradeIdeas.map((idea) => (
          <Card
            key={idea.id}
            onClick={() => onDetails?.(idea.id)}
            footer={
              <Grid columns={2} gap="12" className={styles.ideaActions}>
                <Button variant="secondary" fullWidth onClick={() => onDetails?.(idea.id)}>{t('ideas.details')}</Button>
                <Button variant="buy" fullWidth onClick={() => onBuy?.(idea.id)}>{t('ideas.buy')}</Button>
              </Grid>
            }
          >
            <span className={styles.idea}>
              <Avatar initials={idea.stock.initials} src={idea.stock.logoUrl} size={48} label={idea.stock.name} />
              <span className={styles.ideaSymbol}>{idea.stock.symbol}</span>
              <span className={styles.ideaPotential}>
                <PriceChange value={idea.potentialPct} size="lg" arrow decimals={0} />
                <span className={styles.cardMeta}>{t('ideas.potential')}</span>
              </span>
              <span className={styles.ideaMoved}>{t('ideas.moved')} <PriceChange value={idea.movedPct} size="md" /> {t('ideas.movedIn', { period: idea.movedIn })}</span>
            </span>
          </Card>
        ))}
      </Rail>
      <ListCell as="button" label={t('ideas.viewAll')} iconRight={<Icon icon={msChevronRight} />} onClick={onViewAll} />
    </Stack>
  )
}

/** Invest in ideas: curated baskets — their stocks' logos, the name and the count. */
export function IdeaBaskets({ onOpen }: { onOpen?: (id: string) => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('baskets.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('baskets.title')} description={t('baskets.description')} /></Inset>
      <Rail label={t('baskets.railLabel')} visible={1.15}>
        {ideaBaskets.map((b) => (
          <Card key={b.id} onClick={() => onOpen?.(b.id)}>
            <span className={styles.cardStack}>
              <span className={styles.logos} aria-hidden="true">
                {b.stocks.slice(0, 8).map((s) => <Avatar key={s.symbol} initials={s.initials} src={s.logoUrl} size={40} shape="rounded" />)}
              </span>
              <span className={styles.basketName}>{b.name}</span>
              <span className={styles.cardRow}>
                <span className={styles.cardMeta}>{t('baskets.count', { count: b.stocks.length })}</span>
                <span className={styles.chevron}><Icon icon={msChevronRight} /></span>
              </span>
            </span>
          </Card>
        ))}
      </Rail>
    </Stack>
  )
}

/** The broker's regulatory line, in full (it's short enough not to need "Read more"). */
export function Disclosure() {
  return (
    <Section paddingInline="16" gap="04" align="start">
      <p className={styles.disclosureText}>{disclosure}</p>
    </Section>
  )
}
```

### `src/flows/stocks/components/Invest.tsx`

```tsx
import { Avatar } from '@l3/components/Avatar'
import { BrandLogo } from '@l3/components/BrandLogo'
import { Button } from '@l3/components/Button'
import { Card } from '@l3/components/Card'
import { Icon } from '@l3/components/Icon'
import { Inset } from '@l3/components/Inset'
import { PriceChange } from '@l3/components/PriceChange'
import { Section } from '@l3/components/Section'
import { SectionHeader } from '@l3/components/SectionHeader'
import { Stack } from '@l3/components/Stack'
import { Tag } from '@l3/components/Tag'
import { msChevronRight, msDeployedCode, msSavings, msShowChart, msVerified } from '@l3/icons/material'
import { inr } from '../../../shared/data/format'
import { Rail } from '../../../shared/ui/Rail'
import { Spot } from '../../../shared/ui/Spot'
import { announcement, highlights, sectors, strategies } from '../data'
import { t } from '../copy'
import styles from './Home.module.css'

/** Smart Invest: managed strategies that buy and sell for you. Sits on the brand tint (screens/Home: tinted). */
export function SmartInvest({ onInvest, onOpen }: { onInvest?: (id: string) => void; onOpen?: (id: string) => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('smartInvest.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('smartInvest.title')} tag={<Tag color="success" iconLeft={<Icon icon={msVerified} size={16} />}>{t('smartInvest.tag')}</Tag>} /></Inset>
      <Rail label={t('smartInvest.railLabel')} visible={1.08}>
        {strategies.map((s) => (
          <Card
            key={s.id}
            padding="none"
            onClick={() => onOpen?.(s.id)}
            footerFilled
            footer={
              <Stack direction="horizontal" align="center" justify="between" gap="12" className={styles.charges}>
                <span className={styles.chargesText}>
                  <span className={styles.cardMeta}>{t('smartInvest.monthlyCharges')}</span>
                  <span className={styles.price}>{inr(s.monthlyCharge, 0)} <span className={styles.was}>{inr(s.monthlyChargeWas, 0)}</span></span>
                </span>
                <Button variant="tertiary" onClick={() => onInvest?.(s.id)}>{t('smartInvest.investNow')}</Button>
              </Stack>
            }
          >
            <Section padding="12" gap="12">
              <Stack direction="horizontal" align="center" gap="12" className={styles.strategyHead}>
                <Avatar icon={<BrandLogo brand="lemonn" variant="icon" size={24} decorative />} size={40} />
                <span className={styles.strategyName}>{s.name}</span>
                <span className={styles.strategyDeposit}>
                  <span className={styles.statValue}>{inr(s.minDeposit, 0)}</span>
                  <span className={styles.cardMeta}>{t('smartInvest.minDeposit')}</span>
                </span>
              </Stack>
              <div className={styles.stats}>
                <span className={styles.stat}>
                  <span className={styles.cardMeta}>{t('smartInvest.lastWeek')}</span>
                  <span className={styles.days} aria-label={t('smartInvest.lastWeekLabel', { profit: s.lastWeek.filter((d) => d.result === 'profit').length, total: s.lastWeek.length })}>
                    {s.lastWeek.map((d) => <span key={d.day} className={styles.day} data-result={d.result} aria-hidden="true">{d.day}</span>)}
                  </span>
                </span>
                <span className={styles.stat}>
                  <span className={styles.cardMeta}>{t('smartInvest.aum')}</span>
                  <span className={styles.statValue}>{s.aum}</span>
                </span>
                <span className={styles.stat}>
                  <span className={styles.cardMeta}>{t('smartInvest.slotsLeft')}</span>
                  <span className={styles.statValue}>{t('smartInvest.slots', { left: s.slotsLeft, total: s.slots })}</span>
                </span>
              </div>
            </Section>
          </Card>
        ))}
      </Rail>
    </Stack>
  )
}

/** One announcement — a campaign (the real app shows its artwork; here an illustration slot). */
export function Announcement({ onOpen }: { onOpen?: () => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('announcements.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('announcements.title')} /></Inset>
      <Inset paddingInline="16" paddingBlock="00">
        <Card onClick={onOpen} footer={<Button variant="brand" fullWidth onClick={onOpen}>{announcement.cta}</Button>}>
          <span className={styles.announcement}>
            <span className={styles.announcementText}>
              <Tag color="success" size="sm">{t('announcements.new')}</Tag>
              <span className={styles.announcementTitle}>{announcement.title}</span>
              <span className={styles.cardMeta}>{announcement.description}</span>
            </span>
            <Spot icon={msDeployedCode} tone="brand" size="md" />
          </span>
        </Card>
      </Inset>
    </Stack>
  )
}

/** Sectors: how many stocks each has and how it moved today. */
export function Sectors({ onOpen, onViewAll }: { onOpen?: (id: string) => void; onViewAll?: () => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('sectors.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('sectors.title')} action={{ type: 'view-all', onClick: () => onViewAll?.() }} /></Inset>
      <Rail label={t('sectors.railLabel')} visible={1.75}>
        {sectors.map((s) => (
          <Card key={s.id} onClick={() => onOpen?.(s.id)}>
            <span className={styles.cardStack}>
              <span className={styles.cardRow}>
                <span className={styles.cardTitle}>{s.name}</span>
                <span className={styles.chevron}><Icon icon={msChevronRight} size={16} /></span>
              </span>
              <span className={styles.cardRow}>
                <span className={styles.cardMeta}>{t('sectors.count', { count: s.count })}</span>
                <PriceChange value={s.changePct} size="sm" arrow />
              </span>
            </span>
          </Card>
        ))}
      </Rail>
    </Stack>
  )
}

/** Highlights: product spotlights — mutual funds (tax saving) and F&O (the index today). */
export function Highlights({ onExplore }: { onExplore?: (id: string) => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('highlights.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('highlights.title')} /></Inset>
      <Rail label={t('highlights.railLabel')} visible={1.3}>
        {highlights.map((h) => (
          <Card key={h.id} onClick={() => onExplore?.(h.id)} footer={<Button variant="tertiary" onClick={() => onExplore?.(h.id)}>{h.cta}</Button>}>
            <span className={styles.highlightCard}>
              <Spot icon={h.id === 'fno' ? msShowChart : msSavings} tone={h.id === 'fno' ? 'discover' : 'success'} size="md" />
              <span className={styles.highlightTitle}>{h.title}</span>
              <span className={styles.cardMeta}>{h.description}</span>
              {h.id === 'mutual-funds' ? <span className={styles.highlightValue}>{h.value}</span> : <PriceChange value={h.changePct} size="lg" arrow />}
              <span className={styles.cardMeta}>{h.caption}</span>
            </span>
          </Card>
        ))}
      </Rail>
    </Stack>
  )
}
```

### `src/flows/stocks/components/Overview.tsx`

```tsx
import { Card } from '@l3/components/Card'
import { Icon } from '@l3/components/Icon'
import { PriceChange } from '@l3/components/PriceChange'
import { Stack } from '@l3/components/Stack'
import { Tag } from '@l3/components/Tag'
import { msArrowForward, msChevronRight } from '@l3/icons/material'
import { indices } from '../../../shared/data/market'
import { holdings } from '../../../shared/data/portfolio'
import { inr } from '../../../shared/data/format'
import { Rail } from '../../../shared/ui/Rail'
import { products } from '../data'
import { t } from '../copy'
import styles from './Home.module.css'

/** NIFTY 50 · SENSEX · … — today's index moves; each opens its chart. Scrolls sideways. */
export function IndexStrip({ onIndex }: { onIndex?: (id: string) => void }) {
  return (
    <Stack direction="horizontal" align="center" gap="16" className={styles.indexStrip} role="region" aria-label={t('indices.label')}>
      {indices.map((index, i) => (
        <Stack key={index.id} direction="horizontal" align="center" gap="08" className={styles.indexItem}>
          {i > 0 && <span className={styles.divider} aria-hidden="true" />}
          <Card variant="flat" padding="none" onClick={() => onIndex?.(index.id)}>
            <span className={styles.index}>
              <span className={styles.indexName}>{index.name}</span>
              <span className={styles.indexValue}>{index.value.toLocaleString('en-IN', { minimumFractionDigits: 2 })}</span>
              <PriceChange value={index.changePct} size="md" />
              <span className={styles.chevron}><Icon icon={msChevronRight} size={16} /></span>
            </span>
          </Card>
        </Stack>
      ))}
    </Stack>
  )
}

/** Current value of the holdings and the overall return — the page's headline number. The block opens Portfolio. */
export function HoldingsSummary({ onOpen }: { onOpen?: () => void }) {
  return (
    <Card variant="flat" padding="none" onClick={onOpen}>
      <span className={styles.holdings}>
        <span className={styles.holdingsText}>
          <span className={styles.holdingsLabel}>{t('holdings.label')}</span>
          <span className={styles.holdingsValue}>
            {inr(holdings.current)}
            <PriceChange value={holdings.returnPct} size="lg" arrow />
          </span>
        </span>
        <span className={styles.roundArrow} aria-hidden="true"><Icon icon={msArrowForward} /></span>
      </span>
    </Card>
  )
}

/** Mutual funds · Equity F&O · Commodities — each product's one-line promise. */
export function ProductCards({ onProduct }: { onProduct?: (id: string) => void }) {
  return (
    <Rail label={t('products.label')} visible={2.15} className={styles.onEdgeRoom}>
      {products.map((p) => (
        <Card key={p.id} onClick={() => onProduct?.(p.id)}>
          <span className={styles.cardStack}>
            {p.isNew && <span className={styles.onEdge}><Tag color="success" size="sm">{t('products.new')}</Tag></span>}
            <span className={styles.cardRow}>
              <span className={styles.productTitle}>{p.name}</span>
              <span className={styles.chevron}><Icon icon={msChevronRight} size={16} /></span>
            </span>
            <span className={styles.cardMeta}>
              <span className={styles.highlight}>{p.highlight}</span> {p.detail}
            </span>
          </span>
        </Card>
      ))}
    </Rail>
  )
}
```

### `src/flows/stocks/copy.json`

```json
{
  "search.placeholder": "Search stocks",
  "search.label": "Search stocks",
  "top.wallet": "Wallet",
  "top.profile": "Profile",
  "indices.label": "Market indices",
  "holdings.label": "Current holdings",
  "products.label": "Products",
  "products.new": "New",
  "shortcuts.title": "Do more with Lemonn",
  "shortcuts.watchlistBadge": "{count} in your watchlist",
  "movers.title": "Top movers",
  "movers.tabsLabel": "Movers",
  "movers.gainers": "Top gainers",
  "movers.losers": "Top losers",
  "movers.under100": "Under ₹100",
  "watchlists.title": "Watchlist",
  "watchlists.new": "New watchlist",
  "watchlists.railLabel": "Your watchlists",
  "watchlists.one": "{count} stock",
  "watchlists.many": "{count} stocks",
  "smartInvest.title": "Smart Invest",
  "smartInvest.tag": "NSE/BSE approved",
  "smartInvest.railLabel": "Strategies",
  "smartInvest.monthlyCharges": "Monthly charges",
  "smartInvest.investNow": "Invest now",
  "smartInvest.minDeposit": "Min. deposit",
  "smartInvest.lastWeek": "Last week",
  "smartInvest.lastWeekLabel": "Last week: {profit} profit days of {total}",
  "smartInvest.aum": "AUM",
  "smartInvest.slotsLeft": "Slots left",
  "smartInvest.slots": "{left}/{total}",
  "announcements.title": "Announcements",
  "announcements.new": "New",
  "sectors.title": "Invest in sectors",
  "sectors.railLabel": "Sectors",
  "sectors.count": "{count} stocks",
  "highlights.title": "Highlights",
  "highlights.railLabel": "Highlights",
  "ideas.title": "Trade ideas",
  "ideas.tag": "Live analysis",
  "ideas.railLabel": "Trade ideas",
  "ideas.details": "Details",
  "ideas.buy": "Buy",
  "ideas.potential": "Potential profit",
  "ideas.moved": "Moved",
  "ideas.movedIn": "in {period}",
  "ideas.viewAll": "View all recommendations",
  "baskets.title": "Invest in ideas",
  "baskets.description": "Curated industry-based stock offerings",
  "baskets.railLabel": "Idea baskets",
  "baskets.count": "{count} stocks"
}
```

### `src/flows/stocks/copy.ts`

```ts
import { makeCopy } from '../../shared/copy'
import copy from './copy.json'

/** Every user-facing string of Stocks is in copy.json: t('movers.title'), t('watchlists.count', { count }). */
export const t = makeCopy('stocks', copy)
```

### `src/flows/stocks/data.ts`

```ts
import { msBookmarks, msCampaign, msLightbulb, msRedeem } from '@l3/icons/material'
import { stocks, type Stock } from '../../shared/data/market'

/** Stocks home — what the API sends for each section, in page order. Sample values; market data from the live app. */

/** Product shortcuts under the holdings: one line on what's special about each. */
export type Product = { id: 'mutual-funds' | 'fno' | 'commodities'; name: string; highlight: string; detail: string; isNew?: boolean }
export const products: Product[] = [
  { id: 'mutual-funds', name: 'Mutual funds', highlight: '240+', detail: 'funds' },
  { id: 'fno', name: 'Equity F&O', highlight: '0', detail: 'slippage' },
  { id: 'commodities', name: 'Commodities', highlight: '0', detail: 'brokerage', isNew: true },
]

/** "Do more with Lemonn": feature shortcuts, each with an optional count or "New" flag. */
export type Shortcut = { id: string; label: string; icon: string; badge?: { count?: number; text?: string } }
export const shortcuts: Shortcut[] = [
  { id: 'trade-ideas', label: 'Trade ideas', icon: msLightbulb },
  { id: 'ipo', label: 'IPO', icon: msCampaign, badge: { text: '8 new' } },
  { id: 'watchlist', label: 'Watchlist', icon: msBookmarks, badge: { count: 3 } },
  { id: 'referral', label: 'Refer & earn', icon: msRedeem, badge: { text: 'New' } },
]

/** Top movers: three lists, picked with the chips. */
export type MoverList = 'gainers' | 'losers' | 'under-100'
export const movers: Record<MoverList, Stock[]> = {
  gainers: [stocks.HBLENGINE, stocks.SHRIPISTON, stocks.THANGAMAYL, stocks.VAML, stocks.WELCORP, stocks.LINDEINDIA],
  losers: [stocks.IEX, stocks.ZOMATO, stocks.PAYTM, stocks.DELHIVERY, stocks.NYKAA, stocks.POLICYBZR],
  'under-100': [stocks.IDEA, stocks.YESBANK, stocks.SOUTHBANK, stocks.SUZLON, stocks.NHPC, stocks.IRFC],
}

/** Smart Invest: managed strategies (auto buy & sell). Last week = one result per trading day. */
export type Strategy = {
  id: string
  name: string
  minDeposit: number
  lastWeek: { day: 'M' | 'T' | 'W' | 'Th' | 'F'; result: 'profit' | 'loss' }[]
  aum: string
  slotsLeft: number
  slots: number
  monthlyCharge: number
  monthlyChargeWas: number
}
export const strategies: Strategy[] = [
  {
    id: 'credit-spread-alpha', name: 'Credit Spread Alpha', minDeposit: 80000, aum: '₹1.28 Cr', slotsLeft: 324, slots: 500, monthlyCharge: 0, monthlyChargeWas: 300,
    lastWeek: [{ day: 'M', result: 'loss' }, { day: 'T', result: 'loss' }, { day: 'W', result: 'loss' }, { day: 'Th', result: 'loss' }, { day: 'F', result: 'profit' }],
  },
  {
    id: 'momentum-swing', name: 'Momentum Swing', minDeposit: 50000, aum: '₹86.4 L', slotsLeft: 112, slots: 300, monthlyCharge: 0, monthlyChargeWas: 250,
    lastWeek: [{ day: 'M', result: 'profit' }, { day: 'T', result: 'profit' }, { day: 'W', result: 'loss' }, { day: 'Th', result: 'profit' }, { day: 'F', result: 'profit' }],
  },
]

/** One announcement (a campaign; the real app shows the campaign's artwork). */
export const announcement = {
  title: 'Commodities are live',
  description: 'Trade gold, silver and crude on Lemonn — right next to your stocks.',
  cta: 'Explore now',
}

/** Sectors: how many stocks and how the sector moved today. */
export type Sector = { id: string; name: string; count: number; changePct: number }
export const sectors: Sector[] = [
  { id: 'agriculture', name: 'Agriculture', count: 8, changePct: -0.85 },
  { id: 'auto', name: 'Auto', count: 33, changePct: 0.74 },
  { id: 'banking', name: 'Banking', count: 41, changePct: 0.31 },
  { id: 'pharma', name: 'Pharma', count: 52, changePct: -0.18 },
]

/** Highlights: product spotlights with one number each. */
export type Highlight =
  | { id: 'mutual-funds'; title: string; description: string; value: string; caption: string; cta: string }
  | { id: 'fno'; title: string; description: string; changePct: number; caption: string; cta: string }
export const highlights: Highlight[] = [
  { id: 'mutual-funds', title: 'Mutual funds', description: 'Invest in tax-saving funds and save up to', value: '₹46,800', caption: 'in tax every year', cta: 'Explore' },
  { id: 'fno', title: 'F&O', description: 'Trade futures and options on the index', changePct: 0.65, caption: 'Nifty 50 today', cta: 'Explore' },
]

/** Trade ideas: research picks — the target move, and how it has moved since. */
export type TradeIdea = { id: string; stock: Stock; potentialPct: number; movedPct: number; movedIn: string }
export const tradeIdeas: TradeIdea[] = [
  { id: 'cohance', stock: stocks.COHANCE, potentialPct: 12, movedPct: 4.97, movedIn: 'last 7 days' },
  { id: 'welcorp', stock: stocks.WELCORP, potentialPct: 9, movedPct: 3.4, movedIn: 'last 5 days' },
  { id: 'linde', stock: stocks.LINDEINDIA, potentialPct: 15, movedPct: 6.1, movedIn: 'last 10 days' },
]

/** Invest in ideas: curated baskets of stocks. */
export type IdeaBasket = { id: string; name: string; stocks: Stock[] }
export const ideaBaskets: IdeaBasket[] = [
  { id: 'presidents-portfolio', name: 'President’s portfolio', stocks: [stocks.NTPC, stocks.IRFC, stocks.COALINDIA, stocks.HAL, stocks.ONGC, stocks.SBIN, stocks.NHPC] },
  { id: 'oil-and-gas', name: 'Oil & gas', stocks: [stocks.ONGC, stocks.OIL, stocks.IOC, stocks.BPCL, stocks.JINDALSTEL] },
]

/** The broker's regulatory line (shortened; "Read more" opens the full text). */
export const disclosure =
  'The online broking and mutual fund execution services are provided by NU Investors Technologies Pvt. Ltd. | CIN U67200MH2021PTC364704 | SEBI Registration No. INZ000304837 | SEBI DP Registration No. IN-DP-712-2022 | SEBI Research Analyst Registration No. INH000016764 | NSE member code 90251.'
```

### `src/flows/stocks/index.tsx`

```tsx
import type { Flow } from '../types'
import { goToTab } from '../../shared/ui/MainNav'
import { Home } from './screens/Home'

/**
 * Stocks — the Stocks tab, the app's home after login. Built from the live app's screenshots (shared 2026-10-17;
 * there's no Figma for it yet). One long page; the board shows it as frames scrolled to each part.
 */
export const stocks: Flow = {
  id: 'stocks',
  title: 'Stocks',
  description: 'The Stocks tab — the home after login: indices, holdings, products, movers, watchlists, ideas.',
  figmaFile: '',
  screens: [
    { id: 'stocks-01', name: 'Home', figma: '', note: 'Top of the page: search, indices, holdings, products, shortcuts.', tab: true, render: (go) => <Home onTab={(t) => goToTab(go, t)} onSearch={() => go('search-01')} /> },
    { id: 'stocks-02', name: 'Home · top movers & watchlists', figma: '', note: 'Same page, scrolled.', tab: true, render: (go) => <Home onTab={(t) => goToTab(go, t)} onSearch={() => go('search-01')} scrollTo="movers" /> },
    { id: 'stocks-03', name: 'Home · top losers', figma: '', note: 'The "Top losers" chip picked.', tab: true, render: (go) => <Home onTab={(t) => goToTab(go, t)} onSearch={() => go('search-01')} scrollTo="movers" movers="losers" /> },
    { id: 'stocks-04', name: 'Home · Smart Invest', figma: '', note: 'Same page, scrolled: managed strategies, then the announcement.', tab: true, render: (go) => <Home onTab={(t) => goToTab(go, t)} onSearch={() => go('search-01')} scrollTo="invest" /> },
    { id: 'stocks-05', name: 'Home · sectors & highlights', figma: '', note: 'Same page, scrolled.', tab: true, render: (go) => <Home onTab={(t) => goToTab(go, t)} onSearch={() => go('search-01')} scrollTo="sectors" /> },
    { id: 'stocks-06', name: 'Home · trade ideas', figma: '', note: 'Same page, scrolled: featured ideas, neighbours peek.', tab: true, render: (go) => <Home onTab={(t) => goToTab(go, t)} onSearch={() => go('search-01')} scrollTo="ideas" /> },
    { id: 'stocks-07', name: 'Home · idea baskets & disclosure', figma: '', note: 'The end of the page.', tab: true, render: (go) => <Home onTab={(t) => goToTab(go, t)} onSearch={() => go('search-01')} scrollTo="more" /> },
  ],
}
```

### `src/flows/stocks/screens/Home.module.css`

```css
/*
 * Stocks home — page layout. A grey summary area (indices, holdings, products) and, over its bottom edge, the feed on
 * a rounded sheet (radius 32 at the top), like the live app. The grey area is intentional and high-emphasis (the
 * account summary), so a large surface/secondary block is allowed here (DESIGN_SYSTEM 7.2). The layout is L3 Stack /
 * Section; what those blocks don't cover lives here: uneven padding (16 / 48, 32 / 24), the feed's overlap and
 * top-only radius, the default surface and the accent tints.
 */
.summary {
  padding: var(--l3-spacing-16) 0 var(--l3-spacing-48);
  background: var(--l3-surface-secondary);
}

.feed {
  position: relative;
  margin-top: calc(-1 * var(--l3-spacing-24));
  padding: var(--l3-spacing-32) 0 var(--l3-spacing-24);
  border-radius: var(--l3-radius-32) var(--l3-radius-32) 0 0;
  background: var(--l3-surface-default);
}

/* Sections on a tint, edge to edge: Smart Invest on the brand tint, Trade ideas on the discover tint. */
.tinted[data-tint='brand'] {
  background: var(--l3-surface-accent-brand-light);
}

.tinted[data-tint='discover'] {
  background: var(--l3-surface-accent-discover-light);
}

/* Wraps the top bar only to catch a tap on its search field (opens Search); adds no box of its own. */
.searchEntry {
  display: contents;
}
```

### `src/flows/stocks/screens/Home.tsx`

```tsx
import { useLayoutEffect, useRef, useState } from 'react'
import { Actionbar, ActionbarAction } from '@l3/components/Actionbar'
import { Section } from '@l3/components/Section'
import { Stack } from '@l3/components/Stack'
import { msAccountBalanceWallet, msAccountCircle } from '@l3/icons/material'
import { MainNav, type Section as NavSection } from '../../../shared/ui/MainNav'
import { Screen } from '../../../shared/ui/Screen'
import { Disclosure, IdeaBaskets, TradeIdeas } from '../components/Ideas'
import { Announcement, Highlights, Sectors, SmartInvest } from '../components/Invest'
import { HoldingsSummary, IndexStrip, ProductCards } from '../components/Overview'
import { Shortcuts, TopMovers, Watchlists } from '../components/Discover'
import type { MoverList } from '../data'
import { t } from '../copy'
import styles from './Home.module.css'

/** Where the page is scrolled to — the board shows the long page as several frames, like the live app's screenshots. */
export type HomePart = 'top' | 'movers' | 'invest' | 'sectors' | 'ideas' | 'more'

type Props = {
  scrollTo?: HomePart
  /** Which Top movers list is picked (board state). */
  movers?: MoverList
  /** The main navbar: go to another tab. */
  onTab?: (section: NavSection) => void
  /** Tapping the search field opens the Search screen (search-01), where the typing happens. */
  onSearch?: () => void
}

/**
 * stocks-01 … 07 · Stocks home — the app's first L1 screen (the Stocks tab).
 * Top bar: search + wallet + profile (sticky). The search field is the way into Search: tapping it opens the Search
 * screen (stocks tab, keyboard up) — nothing is typed here. Then, on a grey summary area: today's indices, the holdings and the
 * products; and on the rounded sheet over it, the feed: shortcuts, top movers, watchlists, Smart Invest (brand tint),
 * an announcement, sectors, highlights, trade ideas (discover tint), idea baskets and the broker's disclosure.
 */
export function Home({ scrollTo = 'top', movers, onTab, onSearch }: Props) {
  const [query, setQuery] = useState('')
  const page = useRef<HTMLDivElement>(null)

  // Board frames: open the page scrolled to a part (the screen's body is this page's scroll container).
  useLayoutEffect(() => {
    const root = page.current
    const body = root?.parentElement
    const target = root?.querySelector(`[data-home="${scrollTo}"]`)
    if (!body || !target || scrollTo === 'top') return
    body.scrollTop += target.getBoundingClientRect().top - body.getBoundingClientRect().top
  }, [scrollTo])

  return (
    <Screen
      top={
        // Focus on the field (a tap) opens Search. The wrapper is display: contents, so the bar's layout is untouched.
        <div className={styles.searchEntry} onFocus={(e) => { if ((e.target as HTMLElement).matches('input[type="search"]')) onSearch?.() }}>
          <Actionbar
            search={{ value: query, onChange: setQuery, placeholder: t('search.placeholder'), label: t('search.label') }}
            actions={
              <>
                <ActionbarAction icon={msAccountBalanceWallet} label={t('top.wallet')} onClick={() => {}} />
                <ActionbarAction icon={msAccountCircle} label={t('top.profile')} onClick={() => {}} />
              </>
            }
          />
        </div>
      }
      bottom={<MainNav value="stocks" onChange={onTab} />}
    >
      <Stack ref={page}>
        <Stack gap="24" className={styles.summary} data-home="top">
          <IndexStrip />
          <HoldingsSummary />
          <ProductCards />
        </Stack>
        <Stack gap="32" className={styles.feed}>
          <Shortcuts />
          <div data-home="movers"><TopMovers initial={movers} /></div>
          <Watchlists />
          <Section paddingBlock="24" className={styles.tinted} data-tint="brand" data-home="invest"><SmartInvest /></Section>
          <Announcement />
          <div data-home="sectors"><Sectors /></div>
          <Highlights />
          <Section paddingBlock="24" className={styles.tinted} data-tint="discover" data-home="ideas"><TradeIdeas /></Section>
          <div data-home="more"><IdeaBaskets /></div>
          <Disclosure />
        </Stack>
      </Stack>
    </Screen>
  )
}
```

## Source — shared app parts it uses

### `src/shared/copy.ts`

```ts
/**
 * UI copy. Every user-facing string of a flow lives in ONE file, `src/flows/<flow-id>/copy.json` — flat keys
 * ("empty.title") → the text. Screens read it through the flow's `t`:
 *
 *   t('empty.title')                       → "No search results found"
 *   t('group.results', { list, count })    → "Stocks (8)"   ({name} placeholders are filled in)
 *
 * Sample data (stock names, prices, watchlists) stays in the flow's data.ts — copy.json is only the words the app
 * itself says: titles, labels, buttons, empty states, accessible names. Porting a flow: this file is the string
 * table (Flutter ARB / iOS Localizable.strings).
 */
export type CopyVars = Record<string, string | number>

export function makeCopy<Copy extends Record<string, string>>(flow: string, copy: Copy) {
  return function t(key: keyof Copy & string, vars?: CopyVars): string {
    const template = copy[key] ?? key
    return vars ? template.replace(/\{(\w+)\}/g, (m, name: string) => (name in vars ? String(vars[name]) : m)) : template
  }
}
```

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

### `src/shared/data/portfolio.ts`

```ts
// The sample user's portfolio, shared by the Stocks home and Portfolio. FICTIONAL — never a real customer's holdings.

export type HoldingsSummary = {
  /** Current value of everything held, ₹. */
  current: number
  /** Overall return on what was invested, %. */
  returnPct: number
}

export const holdings: HoldingsSummary = { current: 24860.75, returnPct: -1.92 }

/** The user's watchlists (names are the user's own). */
export type Watchlist = { id: string; name: string; count: number }

export const watchlists: Watchlist[] = [
  { id: 'long-term', name: 'Long term', count: 12 },
  { id: 'banks', name: 'Banks', count: 6 },
  { id: 'to-research', name: 'To research', count: 0 },
]
```

### `src/shared/prototype.ts`

```ts
// Prototype behaviour the flows use — the ONLY way a flow touches the preview (src/preview). In a real app: interactive
// is always true, `go` is the app's navigation, and useAutoAdvance is "when the server answers, go on" (a loader /
// polling step).
import { createContext, useContext, useEffect } from 'react'

export type Preview = {
  /**
   * Prototype (true): real modal L3 BottomSheets, taps move to the next screen, loaders advance on their own.
   * Board (false): every screen frozen as in the Figma board — sheets are drawn as static surfaces.
   */
  interactive: boolean
  /** Go to another screen of the flow (by id). Does nothing on the board. */
  go: (id: string) => void
  /** The screen being shown — a new id gives the sheet a fresh mount while the page behind it stays put. */
  screenId?: string
}

export const PreviewContext = createContext<Preview>({ interactive: false, go: () => {} })
export const usePreview = () => useContext(PreviewContext)

/** Prototype only: after `ms`, go to `next` (loaders, "Verifying…"). */
export function useAutoAdvance(next: string | undefined, ms: number) {
  const { interactive, go } = usePreview()
  useEffect(() => {
    if (!interactive || !next) return
    const t = window.setTimeout(() => go(next), ms)
    return () => window.clearTimeout(t)
  }, [interactive, next, ms, go])
}

/** How long a loader or "Verifying…" state stays up in the prototype. */
export const LOADING_MS = 1600
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

### `src/shared/ui/Rail.module.css`

```css
/*
 * Sideways rail. Item width = the row (inside its 16 insets) shared by `--rail-visible` items with 12 between them, so
 * the same rail reads well from 360 to 412. --rail-visible is a count (set inline), not a size.
 */
.rail {
  /* Positioned, so absolutely placed bits inside (e.g. PriceChange's screen-reader text) are clipped by the rail
     instead of hanging off a far-away ancestor and making the whole page scroll sideways. */
  position: relative;
  display: grid;
  grid-auto-flow: column;
  grid-auto-columns: calc((100% - (var(--rail-visible) - 1) * var(--l3-spacing-12)) / var(--rail-visible));
  gap: var(--l3-spacing-12);
  overflow-x: auto;
  overscroll-behavior-x: contain;
  padding-inline: var(--l3-spacing-16);
  scroll-padding-inline: var(--l3-spacing-16);
  scroll-snap-type: x mandatory;
  scrollbar-width: none;
}

.rail::-webkit-scrollbar {
  display: none;
}

.rail > * {
  scroll-snap-align: start;
  min-width: 0;
}

.rail[data-snap='center'] > * {
  scroll-snap-align: center;
}

/* Two rows that scroll together. */
.rail[data-rows='2'] {
  grid-auto-flow: column;
  grid-template-rows: repeat(2, auto);
}
```

### `src/shared/ui/Rail.tsx`

```tsx
import type { CSSProperties, ReactNode } from 'react'
import styles from './Rail.module.css'

type RailProps = {
  children: ReactNode
  /** What the row holds, for screen readers ("Top gainers"). */
  label: string
  /** How many items show across the screen — a fraction (2.3) lets the next one peek in, so it reads as scrollable. */
  visible: number
  /** Two rows that scroll together (a grid of cards). */
  rows?: 1 | 2
  /** Snap each item to the start (lists) or the centre (one featured card at a time, neighbours peeking). */
  snap?: 'start' | 'center'
  /** Extra room inside the row (e.g. above the cards, for a tag sitting on a card's top edge). */
  className?: string
}

/**
 * A row that scrolls sideways — cards, baskets, ideas. Its first item lines up with the 16 page margin, items are
 * 12 apart and snap into place, and there's no scrollbar. Not L3 yet (a "carousel / rail" is missing there).
 */
export function Rail({ children, label, visible, rows = 1, snap = 'start', className }: RailProps) {
  return (
    <div className={[styles.rail, className].filter(Boolean).join(' ')} role="region" aria-label={label} data-rows={rows} data-snap={snap} style={{ '--rail-visible': visible } as CSSProperties}>
      {children}
    </div>
  )
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

/*
 * Secondary and tertiary: only a hint of the effect — no darker bottom (it read as grey), just a faint white light on
 * top, a soft top highlight and a small drop shadow (no elevation-low). The full effect stays on the filled buttons.
 */
:root[data-buttons='raised'] .raised :is(button, a):is([data-variant='secondary'], [data-variant='tertiary'])[data-state='default']:not([data-variant='ghost']) {
  --raise-sheen-top: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-10));
  --raise-sheen-bottom: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-0));
  --raise-highlight: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-50));
  --raise-shade: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-5));
  --raise-drop: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-5));
  box-shadow:
    inset 0 0 0 var(--btn-border-width) var(--btn-border),
    inset 0 1px 0 var(--raise-highlight),
    inset 0 -1px 0 var(--raise-shade),
    0 1px 2px var(--raise-drop);
}

/* Dark themes: the light glint on dark outlined buttons is kept faint (a bright line would look like a border). */
:root[data-buttons='raised'][data-mode='dark'] .raised :is(button, a):is([data-variant='secondary'], [data-variant='tertiary'])[data-state='default']:not([data-variant='ghost']) {
  --raise-sheen-top: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-5));
  --raise-sheen-bottom: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-0));
  --raise-highlight: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-5));
  --raise-shade: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-20));
  --raise-drop: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-20));
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

/* Like a phone: no scrollbar that stays on screen — the page, rails and open sheets still scroll (touch, wheel, keys). */
.screen,
.screen * {
  scrollbar-width: none;
}

.screen ::-webkit-scrollbar {
  display: none;
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
import { usePreview } from '../prototype'
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
import { usePreview } from '../prototype'
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

### `src/shared/ui/Spot.module.css`

```css
/*
 * Illustration slot: a large icon on a soft accent disc with a faint outer ring. Stands in for artwork L3 doesn't have
 * yet (lemon mascot, PAN card, bank, coin …) — swap the whole <Spot> for the illustration when it exists.
 */
.spot {
  position: relative;
  display: inline-flex;
  flex: none;
  align-items: center;
  justify-content: center;
  border-radius: var(--l3-radius-full);
  background: var(--spot-surface);
  color: var(--spot-content);
}

.spot[data-size='md'] {
  width: var(--l3-size-64);
  height: var(--l3-size-64);
}

.spot[data-size='lg'] {
  width: var(--l3-size-96);
  height: var(--l3-size-96);
}

/* The halo: one hairline ring 12 outside the disc, in the same accent (lg only — the hero moments). */
.spot[data-size='lg']::after {
  content: '';
  position: absolute;
  inset: calc(-1 * var(--l3-spacing-12));
  border: 1px solid var(--spot-surface);
  border-radius: var(--l3-radius-full);
}

.glyph {
  width: var(--l3-size-32);
  height: var(--l3-size-32);
}

.spot[data-size='lg'] .glyph {
  width: var(--l3-size-48);
  height: var(--l3-size-48);
}

.spot[data-tone='brand'] { --spot-surface: var(--l3-surface-accent-brand-light); --spot-content: var(--l3-content-primary); }
.spot[data-tone='discover'] { --spot-surface: var(--l3-surface-accent-discover-light); --spot-content: var(--l3-content-accent-discover-default); }
.spot[data-tone='success'] { --spot-surface: var(--l3-surface-accent-success-light); --spot-content: var(--l3-content-accent-success-default); }
.spot[data-tone='warning'] { --spot-surface: var(--l3-surface-accent-warning-light); --spot-content: var(--l3-content-accent-warning-default); }
.spot[data-tone='error'] { --spot-surface: var(--l3-surface-accent-error-light); --spot-content: var(--l3-content-accent-error-default); }
.spot[data-tone='plain'] { --spot-surface: var(--l3-surface-default); --spot-content: var(--l3-content-primary); }
.spot[data-tone='neutral'] { --spot-surface: var(--l3-surface-secondary); --spot-content: var(--l3-content-primary); }
```

### `src/shared/ui/Spot.tsx`

```tsx
import { Icon } from '@l3/components/Icon'
import styles from './Spot.module.css'

export type SpotTone = 'brand' | 'discover' | 'success' | 'warning' | 'error' | 'neutral' | 'plain'

/**
 * ILLUSTRATION SLOT — a large icon on a soft accent disc (md 64 / lg 96 with a halo). Used where the app has artwork
 * (mascot, PAN card, bank, coin) until L3 has an illustration set; replace the <Spot> with the artwork 1:1.
 * Tone by meaning: brand = welcome moments, plain = on a brand panel, discover = info / in progress, success / warning / error = outcomes.
 */
export function Spot({ icon, tone = 'brand', size = 'lg' }: { icon: string; tone?: SpotTone; size?: 'md' | 'lg' }) {
  return (
    <span className={styles.spot} data-tone={tone} data-size={size} aria-hidden="true" data-illustration-slot="">
      <Icon icon={icon} className={styles.glyph} />
    </span>
  )
}
```
