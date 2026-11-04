# Diwali — the festive search bar

> For developers and their AI agents. A temporary, festival-only skin on one shared part of the app: the **search bar
> on Stocks home and F&O home**. It ships **on**. Turn it off after the festival and the regular app comes back
> unchanged. Each flow's file (`flows/stocks.md`, `flows/fno.md`) has the full source; this page is the spec. Last
> updated 2026-11-04.

## 1. What ships (the defaults)

| Setting (prototype: App settings → Diwali) | Ships as | What it does |
|---|---|---|
| **Diwali search bar** | **On** | On Stocks home and F&O home, the search bar's icon and label change to a lotus diya and "Happy Green Diwali" twice, with a sparkler round the bar |

There is one version, and you build only this. The launch splash stays the regular one. Earlier explorations are
archived in `src/shared/ui/archive/` and are **not** part of the app: the Garland splash (a night scene with a
marigold garland and the wish, archived 2026-11-04 — `DiwaliSplash.tsx`), a Lanterns splash, and a diya flip on the
profile avatar (`DiwaliArchive.tsx`).

In the app this is one remote flag (for example `diwali_search`), on for the festival window. Off = the regular search
bar, with nothing else changed.

## 2. The Diwali search bar

Source: `src/shared/ui/Diwali.tsx` (`useDiwaliSearch`, `DiwaliSearchFace`, `Sparkler`, `SlidingLabel`) +
`Diwali.module.css`. Used by the Stocks home top bar (`src/flows/stocks/components/TopBar.tsx`) and the F&O home header
(`src/flows/fno/components/Header.tsx`).

- **The bar never moves or changes size.** Only its content (the search icon and the "Search stocks" label) changes.
  Tapping it still opens Search, whichever face it shows.
- **When:** each time a home shows. If the splash is up, it waits until the splash starts to fade.
- **Timeline (from the home showing / the splash starting to fade):**

| At | What |
|---|---|
| 1.0 s | the icon and label fade out as they slide up 4 (160 ms, accelerate) |
| 1.16 s | the **lotus diya** (24 high, in the icon's place) and **"Happy Green Diwali"** (the label's style) slide up 4 and fade in (250 ms, decelerate). The label starts 60 ms after the diya. The sparkler starts. |
| 4.0 s | the Diwali face goes out the same way and the regular icon and label come back in the same way |
| 4.8 s → 7.8 s | the Diwali face again, for 3 s |
| 7.96 s | the regular content again, and it stays |

- **The sparkler** (only while the Diwali face shows): it is drawn on top of the bar and doesn't take taps.
  - A soft gold tint on the bar's 1px edge.
  - A **burning head** laps the edge clockwise every **2.8 s**, with a short fading trail behind it (three dashes:
    far / near / head) and a small blurred glow.
  - **Sparks:** 8 tiny dots (radius 1–1.6, orange and gold in turn) at the head. Each one rises out of the edge
    (about 6–13 out, drifting back a little), twinkles, and sinks back into the edge over **1.2 s**, 150 ms apart, so
    there are always a few. They come from the border and return to it; they never pop or fly off.
  - Colours: deeper orange / gold on the light theme's white bar, paler yellow on dark (`--app-diwali-sparkler-*`,
    with a dark block at the end of `src/local-tokens.css`).
- **A narrow bar (F&O home, beside the DASH pill):** if "Happy Green Diwali" doesn't fit, the text rests, slides left
  just far enough to show its end, and rests there (2.4 s in all, inside the 3 s). Where it fits (Stocks home), it
  doesn't move.
- **Reduced motion:** the content swaps instantly, the sparkler shows only the gold edge, and the label doesn't slide.
- Copy: `diwali.searchWish` = "Happy Green Diwali".

## 3. The lotus diya

`Diya` in `src/shared/ui/Diwali.tsx` (120 × 96 artwork): pink-hearted orange lotus petals, a purple bowl painted in
gold, a gold rim with oil, and a layered flame that flickers gently (1.4 s). Export it as one vector asset (SVG / vector
drawable). Only the flame animates.

## 4. Tokens and L3 gaps

L3 has no festive palette and no choreography timings, so the values are local tokens in `src/local-tokens.css`:
`--app-diwali-*` (colours, sparkler, timings) and `--app-splash-*` (the regular splash's logo entrance; the rest
belong to the archived Garland splash). They are built from L3's base
hues (honey, tangerine, yellow, purple), except the two lotus pinks (`--app-diwali-petal-heart`, `-petal-vein`), which
are raw hex. Everything else is L3: the theme, type styles, spacing, motion durations and easings.
