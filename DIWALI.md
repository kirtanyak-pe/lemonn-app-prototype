# Diwali — the festive splash and search bar

> For developers and their AI agents. A temporary, festival-only skin on two shared parts of the app: the **launch
> splash** and the **search bar on Stocks home and F&O home**. Both ship **on**. Turn each one off after the festival
> and the regular app comes back unchanged. Each flow's file (`flows/stocks.md`, `flows/fno.md`) has the full source;
> this page is the spec. Last updated 2026-11-02.

## 1. What ships (the defaults)

| Setting (prototype: App settings → Diwali) | Ships as | What it does |
|---|---|---|
| **Diwali splash** | **On**, Garland art | The launch splash becomes a quiet night scene with a marigold garland and the wish |
| **Diwali search bar** | **On** | On Stocks home and F&O home, the search bar's icon and label change to a lotus diya and "Happy Green Diwali" twice, with a sparkler round the bar |

There is one version of each, and you build only these two. Earlier explorations (a Lanterns splash, and a diya flip on
the profile avatar) are archived in `src/shared/ui/archive/` and are **not** part of the app.

In the app these are two remote flags (for example `diwali_splash`, `diwali_search`), both on for the festival window.
Off = the regular splash and the regular search bar, with nothing else changed.

## 2. The Diwali splash

Source: `src/shared/ui/Splash.tsx` + `Splash.module.css`, art in `src/shared/ui/Diwali.tsx` (`Garland`, `Sparkles`) +
`Diwali.module.css`.

- **When:** the same as the regular splash. It shows when the app opens or reloads onto a home, for **1.5 s**, then
  fades (L3 motion: medium 250) into the home.
- **Always dark:** the splash takes L3's dark theme whatever the app theme is (it is a night scene). The status bar and
  the navigation bar go dark with it.
- **Layers, back to front:**
  1. Surface/default (dark) with a faint purple dusk from the top (`--app-diwali-dusk`).
  2. A soft warm glow behind the logo (`--app-diwali-glow`).
  3. Nine small warm lights scattered across the screen, each twinkling on its own beat (`Sparkles`, 2.4 s).
  4. **The garland** across the top edge: one thin gold string dipping to the middle, 9 small marigolds (orange and
     gold in turn, a darker centre) and 8 tiny gold drops between them (`Garland`, 392 × 120 artwork, scaled to the
     screen width).
  5. The Lemonn logo, centred (L3 `BrandLogo`, its name turns light on dark).
  6. The wish under the logo (32 below it, centred): "Team Lemonn wishes every trader a" (L3 label 14,
     content/secondary), and under that **"Happy Diwali"** (L3 heading 28 in gold, `--app-diwali-headline`, with a
     faint warm glow).
- **Entrance (all done by 1.35 s, inside the 1.5 s):**

| At | What | How long / easing |
|---|---|---|
| 0 ms | the glow swells in (opacity 0, scale 0.6 → 1); the garland's string draws across from the left | 400 ms, decelerate |
| 100 ms → | the drops and flowers pop on, one after another (50 ms apart), from scale 0 | 400 ms, a soft overshoot (`--app-splash-ease-settle`) |
| 50 ms | the logo rises in (opacity 0, scale 0.92, 8 lower → in place) | 400 ms, soft overshoot |
| 350 ms | the twinkling lights fade up | 400 ms |
| 400 ms | "Team Lemonn wishes every trader a" rises in (8 lower, fading in) | 400 ms, standard |
| 550 ms | "Happy Diwali" rises in | 400 ms, soft overshoot |
| 850 ms | a pale gold shine sweeps once across "Happy Diwali", right to left | 500 ms |

- **Reduced motion:** everything simply shows at once; no twinkle and no shine.
- Copy (`src/shared/ui/copy.json`): `splash.diwaliLead`, `splash.diwaliHeadline`, and `splash.diwali` (the splash's
  accessible label: "Team Lemonn wishes every trader a Happy Diwali").

## 3. The Diwali search bar

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

## 4. The lotus diya

`Diya` in `src/shared/ui/Diwali.tsx` (120 × 96 artwork): pink-hearted orange lotus petals, a purple bowl painted in
gold, a gold rim with oil, and a layered flame that flickers gently (1.4 s). Export it as one vector asset (SVG / vector
drawable). Only the flame animates.

## 5. Tokens and L3 gaps

L3 has no festive palette and no choreography timings, so the values are local tokens in `src/local-tokens.css`:
`--app-diwali-*` (colours, sparkler, timings) and `--app-splash-*` (the splash entrance). They are built from L3's base
hues (honey, tangerine, yellow, purple), except the two lotus pinks (`--app-diwali-petal-heart`, `-petal-vein`), which
are raw hex. Everything else is L3: the theme, type styles (label 14, heading 28), spacing, motion durations and
easings, and `BrandLogo`.
