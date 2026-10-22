# Flow: DASH AI · Trading rules (`dash`)

> One self-contained file for converting this flow to another platform. Read https://kirtanyak-pe.github.io/lemonn-app-prototype/CONVERT.md first. Design system: Lemonnade V3 @ `8d10ac0` (components, tokens and rules: https://kirtanyak-pe.github.io/lemonnade-v3-docs/llms.txt).

Describe a trading habit; DASH builds it into a rule, backtests it, saves it. Your active rules, on / off, delete.

Prototype: https://kirtanyak-pe.github.io/lemonn-app-prototype/#/dash · machine-readable: https://kirtanyak-pe.github.io/lemonn-app-prototype/flows/dash.json

## Screens

| Id | Screen | Figma | Taps go to | Note |
|---|---|---|---|---|
| `dash-02` | DASH landing | [5612:81961](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-81961) | `stocks-01`, `dash-03`, `dash-20`, `dash-14` | From the DASH pill in the Stocks home top bar (stocks-01; first visit: the coach mark, stocks-08). Back returns home. Set Trading Rules opens the rules chat; Create Custom Strategy is another flow (not designed yet). ☰ opens your rules. |
| `dash-03` | Rules · what to set | [5612:40261](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-40261) | `dash-02`, `dash-14`, `dash-04` | The chat’s empty state — the same screen as the conversation. A chip (or typing + Send) sends the message: it flies up into the chat as the greeting and examples fade away. History opens your rules. |
| `dash-04` | Thinking | [5612:40517](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-40517) | `dash-03`, `dash-14`, `dash-05` | Moves on by itself after 1.6 s in the prototype (the real app: when DASH replies). |
| `dash-05` | DASH asks the limit | [5612:40650](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-40650) | `dash-03`, `dash-14`, `dash-06` | The question card takes the input’s place. Any answer (or one typed) builds the preview. |
| `dash-06` | Rule preview | [5612:40816](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-40816) | `dash-03`, `dash-14`, `dash-05`, `dash-07` | The draft is pinned in the top bar’s bottom slot. Proceed runs the backtest; refine goes back to the question. |
| `dash-07` | Ready · would have saved | [5612:40971](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-40971) | `dash-03`, `dash-14`, `dash-10`, `dash-05`, `dash-13` | Backtest: would have saved ₹18,400 in 3 months (green strip). The description opens Read more. |
| `dash-08` | Weak · would have cost profit | [5612:41698](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-41698) | `dash-03`, `dash-14`, `dash-11`, `dash-13`, `dash-05` | Backtest: the rule would have cut ₹22,390 of profit — red glow and strip; DASH suggests improving it. |
| `dash-09` | Ready · no backtest data | [5612:41530](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-41530) | `dash-03`, `dash-14`, `dash-12`, `dash-05`, `dash-13` | Not enough trading history to test: a neutral strip. |
| `dash-10` | Read more · saved | [5612:41133](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-41133) | `dash-07` |  |
| `dash-11` | Read more · cost | [5612:41268](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-41268) | `dash-08` | Figma says ₹22,300 here and ₹22,390 on the card — one number (₹22,390) used for both. |
| `dash-12` | Read more · no data | [5612:41402](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-41402) | `dash-09` |  |
| `dash-13` | Rule created | [5612:41875](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-41875) | `dash-07`, `dash-14` | Over the ready screen it was saved from (Figma shows an older card behind it). |
| `dash-14` | Active rules | [5612:42076](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-42076) | `dash-02`, `dash-15`, `dash-03` | Switches turn a rule on / off in place. Delete asks first; Create New starts a new rule. |
| `dash-15` | Delete rule? | [5612:42250](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-42250) | `dash-14`, `dash-16` | Over the current Active rules (Figma shows an older list with Active / Paused tabs behind it). |
| `dash-16` | Rule deleted | [5612:42455](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-42455) | `dash-17` | The list behind already has 3 rules. |
| `dash-17` | Active rules · after delete | [5612:42076](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=5612-42076) | `dash-02`, `dash-15`, `dash-03` | Not a Figma frame: the list once the sheet closes (3 rules). |
| `dash-20` | Indicator · what to show | — | `dash-02`, `dash-21`, `dash-22`, `dash-23`, `dash-24`, `dash-25` | Third option on the DASH landing. Empty chat: example prompts; a chip or typed text flies up and DASH reads it. |
| `dash-21` | Indicator · which chart | — | `dash-02`, `dash-21`, `dash-22`, `dash-23`, `dash-24`, `dash-25` | DASH asks only what the prompt didn’t say: the chart… |
| `dash-22` | Indicator · which timeframe | — | `dash-02`, `dash-21`, `dash-22`, `dash-23`, `dash-24`, `dash-25` | …and the timeframe. |
| `dash-23` | Indicator · building | — | `dash-02`, `dash-21`, `dash-22`, `dash-23`, `dash-24`, `dash-25` | The card pins and builds live: a wave, then candles with the lines drawing in, the parts as chips, three steps ticking (~3.3 s in the prototype). |
| `dash-24` | Indicator · ready | — | `dash-02`, `dash-21`, `dash-22`, `dash-23`, `dash-24`, `dash-25` | BUY / SELL labels on the preview, notes from testing (fold open), Add to chart (loader → On chart). Ask for changes in plain words: the preview redraws (SELL labels turn orange). |
| `dash-25` | Indicator · on chart | — | `dash-02`, `dash-21`, `dash-22`, `dash-23`, `dash-24`, `dash-25` | Added: the tag and the button read On chart. |

## Spec (README.md)


> The spec for this flow. A developer should be able to build it on any platform (Flutter, iOS, web) from this file,
> the Figma links and the screens in this folder — without reading the rest of the repo.

- **Prototype:** https://kirtanyak-pe.github.io/lemonn-app-prototype/#/dash · one screen: `#/dash-03`
- **Figma:** [DASH AI - main flow](https://www.figma.com/design/nLs24dTJdPz9A5pu3AWMbe/?node-id=11491-7726) (F&O dev
  handoff file) — an **old design without library components or styles**. Rebuilt with L3 components and tokens, plus
  DASH's **own chat style** (orb, glow background, chat input, chips, rule card), which is different from the rest of
  the app on purpose.
- **Status:** ✅ · last updated 2026-10-22
- **Entry:** the DASH pill in the **Stocks home** top bar (`stocks-01`; first visit: the coach mark, `stocks-08`) · **Exit:** back → Stocks home

### Screens

| Id | Screen | Figma | What it shows / when |
|---|---|---|---|
| `dash-02` | DASH landing | 5612:81961 | Orb, "Hi Rohan!", what DASH can build: Set Trading Rules · Create Custom Strategy; ☰ = your rules |
| `dash-03` | Rules · what to set | 5612:40261 | Greeting, orb, example prompts (2 rows, scroll sideways), chat input |
| `dash-04` | Thinking | 5612:40517 | The user's message, "Understanding your request…" with pulsing dots |
| `dash-05` | DASH asks the limit | 5612:40650 | "Got it!" + a question card (₹2,000 / ₹5,000 / ₹10,000 / type your answer) in the input's place |
| `dash-06` | Rule preview | 5612:40816 | Draft rule pinned under the bar (name + ₹5,000 chip); "Rule preview!" + No, I want to refine / Proceed |
| `dash-07` | Ready · would have saved | 5612:40971 | Rule card with the backtest strip (green): would have saved ₹18,400 in 3 months; save? |
| `dash-08` | Weak · would have cost profit | 5612:41698 | Red glow + strip: would’ve cost ₹22,390 of profit; DASH suggests improving it |
| `dash-09` | Ready · no backtest data | 5612:41530 | Neutral strip: couldn’t test this, not enough history yet |
| `dash-10` | Read more · saved | 5612:41133 | Sheet: orb, rule name, full sentence, the backtest (green tint) · Okay |
| `dash-11` | Read more · cost | 5612:41268 | Same, red tint |
| `dash-12` | Read more · no data | 5612:41402 | Same, no backtest line |
| `dash-13` | Rule created | 5612:41875 | Sheet: tick + confetti, "Your rule is created", the rule · Cancel / View Rules |
| `dash-14` | Active rules | 5612:42076 | 4 rule cards: initial, name, what it does, on / off switch; footer: what it did lately + Delete; Create New |
| `dash-15` | Delete rule? | 5612:42250 | Sheet: "Are you sure…", the rule · No / Yes, Delete |
| `dash-16` | Rule deleted | 5612:42455 | Sheet: "Your rule has been deleted" · Okay (the list behind has 3) |
| `dash-17` | Active rules · after delete | (5612:42076) | Not a Figma frame: the list with 3 rules once the sheet closes |

### Navigation

```
stocks-01 DASH pill ─▶ dash-02 (first visit: stocks-08 coach mark ─Got it / pill─▶ dash-02) · dash-02 back ─▶ stocks-01
dash-02 Set Trading Rules ─▶ dash-03 ─chip / Send─▶ dash-04 ─(DASH replies)─▶ dash-05 ─answer─▶ dash-06
dash-06 Proceed ─▶ backtest ─▶ dash-07 (saved) | dash-08 (cost) | dash-09 (no data)     · refine ─▶ dash-05
dash-07/08/09 rule description ─▶ dash-10/11/12 (Okay ─▶ back) · Yes, save it / No, save as it is ─▶ dash-13
dash-08 Yes, improve my rule ─▶ dash-05
dash-13 View Rules ─▶ dash-14 · Cancel ─▶ back to the chat
dash-14 Delete ─▶ dash-15 ─Yes─▶ dash-16 ─Okay─▶ dash-17 · No ─▶ dash-14 · Create New ─▶ dash-03
dash-02 ☰ and the history action on every chat page ─▶ dash-14
```

### Data

`data.ts` (types = the API contract, values = examples):
- `examplePrompts: string[][]` — the example rows on `dash-03`; a tap sends the prompt as the user's message.
- DASH's turn: `RuleQuestion { question, options[], freeText }` (a follow-up), then a `DraftRule { title, value, description }`.
- `Backtest` on the user's last N months: `{ result: 'saved', amount, months }` · `{ result: 'cost', amount, months }` ·
  `{ result: 'none' }`. `saved` = the rule would have stopped losses; `cost` = it would have stopped trades that ended in
  profit (DASH calls the rule weak); `none` = not enough history.
- `SavedRule { id, title, summary, on, activity: { text, strong?, after? } }` — the Active rules list. `activity` is the
  footer line ("Stepped in **3 times** this month"), `strong` the bold part.
- First name for greetings comes from the app's user (`Rohan` — fictional; Figma's "Samaira" isn't used).

### Components

| Area | L3 / DASH part | Notes |
|---|---|---|
| Top bar | L3 `Actionbar` (see-through on DASH pages: `DashBar`) | Back, title "Trading rules", history (Tertiary action); the draft rule card sits in its **bottom slot** (pinned) |
| Background | DASH `DashScreen` | A blurred lime ring (angular gradient, 50%, blur 50) + white wave lines; `glow`: header / landing / bottom; `tone="loss"` = red |
| Mascot | shared `DashOrb` (src/shared/ui/dash; lg 96 · md 64 · sm 32 · xs 24) | The Figma vector parts (leaves, fill, ring) + a blurred halo copy |
| Chat input | DASH `ChatInput` | White pill, 56 tall, 1px DASH green edge, float shadow; Send = L3 Button primary md, icon-only, round |
| Examples | DASH `SparkleHeading` + `PromptChips` | Caps label 12 tracked 16% between two sparkles; chips 36 tall, DASH green edge, rows scroll sideways |
| Messages | DASH `UserMessage` · `AssistantMessage` · `Thinking` | User: grey pill on the right, Label/14. DASH: Heading/14 + Description/14, no bubble; earlier text fades to 40% |
| Quick replies | DASH `ReplyChips` | Pills, Label/12; DASH's suggestion has the green edge and a small shadow |
| Question | DASH `QuestionCard` | Radius 16 card: question + ✕ (L3 Ghost), numbered rows (24 disc), the pick gets a filled tick, dashed "Type your answer" row |
| Rule card | DASH `RuleCard` + `BacktestStrip` | Orb sm + name, dashed line, value chip (preview) or 2-line description + chevron (→ read more); strip tinted by result |
| Landing options | DASH `BuildOption` | Icon tile 56 (grey), name Heading/14, one line Label/12, chevron; DASH green edge at 50% |
| Entry pill | shared `DashEntry` (src/shared/ui/dash) | Orb + "DASH" on a white-to-lime fill, green-to-lime edge; lives in the Stocks home top bar |
| Sheets | L3 `BottomSheet` (via `Sheet`), `BottomSheetHeader`, `ButtonGroup` (horizontal for 2) | Read more · Rule created (L3 Spot success + confetti) · Delete? · Deleted |
| Rules list | L3 `Card` (filled footer) + `ListCell` + `Avatar` (initial) + `Switch`; Delete = Tertiary Small | Create New = Large primary in the dock |

### Rules and edge cases

- The chat dock shows the input, **or** DASH's question card instead of it while DASH waits for an answer.
- Once DASH has a draft, the rule card stays pinned under the top bar; only DASH's latest turn shows below it.
- The backtest strip: saved → green (trending up, amount in success green); cost → red (trending down, amount in error
  red) and the whole page glow turns red; none → grey (! icon), and the read-more sheet has no backtest line.
- A long rule summary on Active rules wraps to 2 lines, then "…".
- Switch changes are instant (no confirm). Delete always asks first.
- Create Custom Strategy (landing) is another DASH flow — not designed yet.

### Differences from Figma

- **L3 parts replace the old ones:** Actionbar (its Tertiary action instead of the grey 32 circles), L3 coach mark
  (light, with its own button) instead of the black tooltip, L3 bottom sheets and button docks, L3 Switch / Avatar
  (neutral initials — Figma's coloured discs aren't an L3 Avatar option).
- **Sheets over the current screens:** Figma's "Rule saved", "Delete rule" and "Rule deleted" sit over older versions
  (a checklist card; an Active / Paused list with Edit buttons). Here they sit over the current screens.
- **Copy fixes:** "No, I want refine" → "No, I want to refine"; the card's garbled "once your daily reach daily
  limit…" → the full sentence (cut at 2 lines); "I would suggest you to improve" → "I would suggest you improve";
  "Let me know what would you like…" → "what you would like…".
- **One loss number:** the card says ₹22,390, Figma's sheet ₹22,300 → ₹22,390 in both.
- **Entry:** the DASH pill and its first-visit coach mark live on the Stocks home (`stocks-01`, `stocks-08`), not in this flow; `dash-01` was removed (2026-10-22). Ids start at `dash-02`.
- **Status bar:** takes L3 brand-light on pages with the glow at the top (the nearest surface to the glow); Figma draws
  the glow under the status bar.
- The white 10% blur glass layer over the glow is left out (no visible difference).

### L3 gaps

- An **AI accent family** (DASH green `#50c83e`, glow lime / yellow / green) — local tokens `--app-dash-*`.
- Actionbar: no **see-through** option for decorative backgrounds (`DashBar` clears its surface and hairline).
- **Chat** components: chat input, message, quick-reply chip, prompt chip, question card.
- A **tracked caps** label style (Figma 1.92 = 16%) · a float shadow `0 4 6 / 6% + 0 1 1 / 4%`.
- Avatar with **coloured initials** (Figma: teal, indigo, yellow, purple).

### Motion (from the live app's recording, 2026-10-23, measured at 360 dp)

| Moment | What moves | Timing |
|---|---|---|
| Send (`dash-03 → 04`, one screen) | The tapped chip — or the typed text — flies up and becomes the user's message; the greeting, orb and examples lift and fade; the panel and the title fade in; the glow glides from the bottom to the header | fly 450 ms; intro out 300 ms; glow + panel 600 ms; the thinking dots wait for the message to land |
| Chat steps (`dash-03 … 13`) | ONE screen in different states, like ChatGPT / Claude (preview `group: 'chat'`): the empty state, the conversation, the pinned rule preview and the result all change in place — nothing is pushed in | — |
| A new message | Fades up 8 into place; earlier messages stay (the conversation builds up) and the panel scrolls to the newest | 250 ms, decelerate |
| DASH thinks | Three dots pulse in turn, after the request and after every answer / Proceed | ~1.6 s in the prototype (live 1.5–3 s) |
| DASH's question | The card rises 24 out of the input's place | 250 ms, decelerate |
| Answered | The question and the answer join the chat; the input drops back from where the card was | 150 ms |
| Draft ready | The rule card opens in the top bar's bottom slot and pushes the chat down | 300 ms (grid rows 0 → 1, fade) |
| Backtest back | The strip unfolds under the card; the description fades in | 300 ms / 250 ms |
| Rule created | Sheet up; green disc pops in (slight overshoot), the tick draws itself, confetti (green · yellow · white/lime) bursts out and drifts down | pop 400 ms from 150 ms; tick 250 ms from 350 ms; confetti 1.6 s |
| View Rules | The tick turns away like a coin, then the rules list cross-fades in (`enter: 'fade'`) | 150 ms + 250 ms |
| The orb (everywhere) | The ring's highlight goes round clockwise, steady; the leaves stay | 1 s per turn, linear |
| The glow | Turns once every 3.08 s (Figma motion on "Gradient") | ease-in, then a pause |

All of it is off with reduced motion. Values L3 doesn't have are local tokens (`--app-dash-*-duration`, …).
Not built yet: the live app's DASH mascot with a face and its "Checking 4 rules for you" bubble on the trading
screen (needs the mascot artwork), and the live landing's active-rules carousel.

### Changelog

- 2026-10-23 — The rules landing is the chat's empty state (one screen): the sent message flies up into place, the intro fades, the glow moves up; no page transition inside the chat.

- 2026-10-23 — Motion from the live app's recording: chat rises in, the conversation builds up, thinking between turns, question card rises, rule card pins, backtest unfolds, created-sheet tick + confetti, orb spin, rules list fades in.

- 2026-10-22 — Entry moved to the Stocks home top bar (DASH pill + coach mark there); `dash-01` removed; the orb and the pill are shared (`src/shared/ui/dash`).

- 2026-10-22 — First build: 17 screens from "DASH AI - main flow", DASH chat style as flow components, verified
  against every Figma frame at 392 wide.


## Source — this flow

### `src/flows/dash/assets/sparkle.svg`

```tsx
<svg preserveAspectRatio="none" overflow="visible" style="display: block;" width="11.9815" height="11.9815" viewBox="0 0 11.9815 11.9815" fill="none" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<g id="&#60;Path&#62;">
<use xlink:href="#stroke0_0_392" fill="url(#paint0_linear_0_392)"/>
<use xlink:href="#stroke0_0_392" fill="url(#paint1_linear_0_392)" fill-opacity="0.5"/>
</g>
<defs>
<linearGradient id="paint0_linear_0_392" x1="3.53456" y1="11.3605" x2="8.80644" y2="0.876619" gradientUnits="userSpaceOnUse">
<stop stop-color="#72E780" style="stop-color:#72E780;stop-color:color(display-p3 0.4476 0.9047 0.5009);stop-opacity:1;"/>
<stop offset="1" stop-color="#FDFFB5" style="stop-color:#FDFFB5;stop-color:color(display-p3 0.9903 1.0000 0.7091);stop-opacity:1;"/>
</linearGradient>
<linearGradient id="paint1_linear_0_392" x1="6.01563" y1="-0.00927739" x2="7.92143" y2="13.1031" gradientUnits="userSpaceOnUse">
<stop stop-color="#2BAA18" style="stop-color:#2BAA18;stop-color:color(display-p3 0.1686 0.6652 0.0941);stop-opacity:1;"/>
<stop offset="1" stop-color="#DFFA35" style="stop-color:#DFFA35;stop-color:color(display-p3 0.8745 0.9804 0.2078);stop-opacity:1;"/>
</linearGradient>
<path id="stroke0_0_392" d="M5.99117 0C5.36109 4.84984 4.84984 5.36031 0 5.99038C4.84984 6.62124 5.36109 7.13171 5.99117 11.9815C6.62124 7.13171 7.13249 6.62124 11.9815 5.99038C7.13249 5.36031 6.62124 4.84984 5.99117 0Z"/>
</defs>
</svg>
```

### `src/flows/dash/assets/waves.svg`

```tsx
<svg preserveAspectRatio="none" overflow="visible" style="display: block;" width="1329.47" height="613.674" viewBox="0 0 1329.47 613.674" fill="none" xmlns="http://www.w3.org/2000/svg">
<g id="Layer_1" clip-path="url(#clip0_0_306)">
<path id="Vector" d="M1.17172 237.102C9.05206 304.023 48.8528 366.47 106.189 401.87C163.524 437.27 237.185 444.877 300.544 421.941C343.148 406.519 380.505 378.437 423.764 364.957C467.022 351.478 522.55 358.03 545.305 397.211C564.995 431.115 552.511 474.527 562.038 512.558C573.816 559.571 619.276 592.657 666.871 601.809C714.466 610.961 763.69 600.079 809.531 584.34C855.37 568.601 899.647 547.938 946.762 536.567C1057.31 509.889 1179.88 539.101 1266.51 612.774" stroke-width="2.3588" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_2" d="M9.12267 229.198C13.223 268.734 27.9301 306.783 50.4389 338.951C66.3254 361.654 86.1413 381.622 109.833 396.669C137.852 414.425 170.358 424.634 203.338 427.852C237.578 431.194 272.598 427.089 305.061 415.421C314.412 412.06 323.523 408.125 332.508 403.895C364.701 388.735 395.32 369.798 429.322 359.923C437.566 357.532 446.231 355.864 455.016 355.003C492.572 351.297 532.288 362.456 550.37 394.505C554.225 401.335 556.802 408.541 558.512 416.001C565.368 445.918 558.36 479.987 566.402 510.424C572.187 532.616 585.695 551.453 603.25 565.93C620.483 580.132 641.624 590.169 663.574 595.273C666.238 595.893 668.915 596.441 671.598 596.913C718.912 605.253 767.798 594.206 813.431 578.526C859.067 562.844 903.212 542.466 950.135 531.222C956.914 529.599 963.737 528.181 970.595 526.971C1075.11 508.517 1187.62 537.929 1268.61 606.702" stroke-width="2.28213" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_3" d="M17.0736 221.294C20.657 261.134 34.4776 299.539 55.8345 332.264C70.9053 355.364 89.8169 376.021 113.48 391.466C141.39 409.609 174.342 419.351 207.46 422.124C241.837 425.006 276.93 420.554 309.58 408.899C318.984 405.542 328.161 401.638 337.219 397.465C369.672 382.508 400.698 364.098 434.883 354.883C443.18 352.656 451.876 351.152 460.677 350.458C498.266 347.456 537.717 359.457 555.433 391.792C559.209 398.686 561.716 405.947 563.339 413.457C569.845 443.593 562.404 477.823 570.763 508.283C576.794 530.261 590.495 548.725 608.127 562.689C625.448 576.385 646.514 585.834 668.347 590.518C670.997 591.087 673.656 591.586 676.324 592.012C723.354 599.543 771.904 588.329 817.328 572.71C862.765 557.085 906.776 536.992 953.506 525.874C960.256 524.268 967.051 522.864 973.88 521.665C1077.94 503.375 1190.03 532.217 1270.7 600.624" stroke-width="2.20547" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_4" d="M25.025 213.392C28.0894 253.537 41.0274 292.296 61.2285 325.579C75.4855 349.074 93.491 370.421 117.122 386.265C144.923 404.795 178.323 414.071 211.576 416.398C246.093 418.818 281.255 414.02 314.091 402.379C323.55 399.026 332.791 395.155 341.923 391.037C374.638 376.285 406.069 358.397 440.438 349.847C448.788 347.781 457.516 346.442 466.334 345.919C503.957 343.622 543.14 356.461 560.495 389.085C564.194 396.04 566.628 403.358 568.165 410.92C574.324 441.276 566.392 475.677 575.124 506.15C581.363 527.922 595.291 546.004 613.002 559.457C630.412 572.651 651.403 581.51 673.116 585.773C675.75 586.29 678.396 586.741 681.047 587.122C727.797 593.841 776.008 582.461 821.225 566.901C866.458 551.333 910.336 531.525 956.875 520.533C963.598 518.945 970.363 517.557 977.163 516.368C1080.78 498.241 1192.43 526.513 1272.8 594.556" stroke-width="2.12881" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_5" d="M32.9759 205.488C35.5214 245.936 47.5749 285.053 66.6241 318.893C80.0673 342.784 97.1686 364.821 120.768 381.063C148.461 399.983 182.309 408.789 215.698 410.67C250.353 412.628 285.587 407.484 318.61 395.857C328.124 392.508 337.43 388.669 346.633 384.608C379.607 370.058 411.445 352.697 445.997 344.807C454.4 342.904 463.157 341.731 471.995 341.377C509.649 339.785 548.569 353.464 565.561 386.373C569.183 393.39 571.544 400.765 572.995 408.376C578.805 438.95 570.38 473.524 579.485 504.01C585.929 525.577 600.091 543.276 617.879 556.216C635.377 568.906 656.295 577.176 677.886 581.017C680.505 581.482 683.135 581.885 685.773 582.22C732.239 588.126 780.115 576.582 825.122 561.081C870.154 545.57 913.9 526.047 960.246 515.181C966.941 513.61 973.677 512.238 980.449 511.059C1083.62 493.096 1194.84 520.797 1274.9 588.474" stroke-width="2.05019" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_6" d="M40.9272 197.586C42.9538 238.339 54.1227 277.809 72.0181 312.208C84.6475 336.496 100.809 359.27 124.413 375.862C151.958 395.224 186.292 403.509 219.818 404.944C254.611 406.44 289.916 400.948 323.128 389.337C332.695 385.991 342.065 382.186 351.343 378.18C384.576 363.832 416.821 346.998 451.556 339.771C460.011 338.029 468.801 337.021 477.656 336.836C515.344 335.948 553.999 350.466 570.626 383.664C574.17 390.742 576.46 398.174 577.824 405.837C583.285 436.631 574.365 471.376 583.849 501.873C590.493 523.236 604.894 540.553 622.758 552.978C640.344 565.165 661.188 572.845 682.659 576.266C685.264 576.68 687.878 577.034 690.498 577.323C736.683 582.42 784.221 570.711 829.02 555.266C873.851 539.814 917.464 520.576 963.617 509.834C970.285 508.283 976.992 506.923 983.734 505.755C1086.46 487.956 1197.25 515.086 1277 582.4" stroke-width="1.97353" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_7" d="M48.8782 189.683C50.3859 230.739 60.6722 270.566 77.4137 305.523C89.2274 330.206 104.479 353.678 128.057 370.661C155.486 390.42 190.274 398.228 223.938 399.218C258.87 400.252 294.246 394.414 327.645 382.819C337.266 379.477 346.702 375.703 356.052 371.756C389.547 357.611 422.197 341.302 457.117 334.737C465.624 333.158 474.446 332.315 483.319 332.299C521.04 332.116 559.43 347.474 575.692 380.957C579.159 388.099 581.376 395.586 582.654 403.301C587.767 434.313 578.347 469.231 588.213 499.74C595.055 520.899 609.694 537.833 627.636 549.745C645.311 561.429 666.081 568.521 687.432 571.516C690.02 571.882 692.621 572.185 695.226 572.427C741.13 576.712 788.329 564.837 832.922 549.454C877.551 534.059 921.029 515.106 966.992 504.489C973.63 502.956 980.309 501.612 987.022 500.454C1089.3 482.818 1199.66 509.378 1279.1 576.326" stroke-width="1.89686" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_8" d="M56.8292 181.779C57.8198 223.138 67.2197 263.32 82.8093 298.836C93.8092 323.914 108.151 348.082 131.703 365.458C159.016 385.61 194.258 392.946 228.058 393.49C263.127 394.062 298.576 387.878 332.161 376.297C341.838 372.959 351.337 369.218 360.762 365.326C394.516 351.384 427.573 335.602 462.676 329.699C471.236 328.283 480.089 327.605 488.98 327.757C526.733 328.278 564.857 344.477 580.757 378.247C584.146 385.449 586.292 392.993 587.483 400.759C592.248 431.992 582.325 467.081 592.576 497.602C599.615 518.558 614.496 535.107 632.515 546.505C650.278 557.686 670.973 564.189 692.206 566.764C694.781 567.078 697.364 567.334 699.953 567.528C745.574 571.001 792.437 558.962 836.822 543.637C881.248 528.299 924.595 509.632 970.365 499.141C976.976 497.625 983.625 496.294 990.309 495.149C1092.14 477.676 1202.07 503.666 1281.2 570.249" stroke-width="1.8202" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_9" d="M64.7805 173.877C65.2523 215.541 73.7675 256.077 88.2033 292.151C98.3894 317.624 111.821 342.485 135.348 360.257C162.547 380.802 198.241 387.666 232.178 387.764C267.385 387.874 302.905 381.344 336.677 369.777C346.407 366.445 355.972 362.734 365.468 358.899C399.484 345.159 432.945 329.903 468.231 324.663C476.844 323.409 485.729 322.895 494.638 323.218C532.423 324.442 570.282 341.481 585.819 375.538C589.131 382.803 591.205 390.404 592.309 398.22C596.724 429.67 586.296 464.934 596.937 495.465C604.168 516.216 619.294 532.382 637.388 543.268C655.24 553.945 675.86 559.858 696.973 562.012C699.532 562.276 702.101 562.482 704.674 562.632C750.014 565.295 796.538 553.088 840.716 537.823C884.939 522.542 928.155 504.159 973.732 493.796C980.315 492.298 986.936 490.983 993.59 489.847C1094.97 472.537 1204.48 497.957 1283.29 564.177" stroke-width="1.74354" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_10" d="M72.7294 165.973C72.6822 207.942 80.3149 248.833 93.5968 285.465C102.967 311.335 115.49 336.891 138.99 355.056C166.075 375.994 202.223 382.384 236.296 382.038C271.643 381.687 307.233 374.811 341.192 363.256C350.977 359.929 360.607 356.249 370.175 352.471C404.451 338.935 438.319 324.203 473.788 319.625C482.452 318.534 491.37 318.186 500.296 318.677C538.116 320.607 575.709 338.485 590.88 372.829C594.116 380.155 596.117 387.813 597.135 395.68C601.202 427.351 590.265 462.786 601.296 493.33C608.719 513.879 624.09 529.66 642.263 540.032C660.201 550.207 680.75 555.53 701.742 557.261C704.285 557.474 706.839 557.631 709.398 557.735C754.455 559.587 800.642 547.215 844.612 532.009C888.635 516.785 931.716 498.687 977.103 488.45C983.657 486.971 990.25 485.668 996.874 484.544C1097.81 467.399 1206.89 492.247 1285.39 558.101" stroke-width="1.66492" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_11" d="M80.6808 158.069C80.1147 200.34 86.8628 241.588 98.9929 278.778C107.549 305.043 119.163 331.293 142.637 349.855C169.609 371.184 206.208 377.105 240.418 376.31C275.903 375.497 311.566 368.273 345.711 356.734C355.549 353.41 365.244 349.764 374.887 346.041C409.422 332.708 443.697 318.502 479.351 314.587C488.068 313.657 497.016 313.494 505.962 314.135C543.807 316.847 581.141 335.486 595.95 370.119C599.107 377.508 601.037 385.221 601.969 393.138C605.688 425.027 594.238 460.637 605.664 491.192C613.273 511.538 628.896 526.933 647.146 536.795C665.173 546.464 685.647 551.199 706.518 552.511C709.048 552.672 711.586 552.78 714.127 552.839C758.903 553.881 804.752 541.342 848.514 526.196C892.334 511.029 935.282 493.218 980.479 483.105C987.005 481.644 993.568 480.357 1000.16 479.242C1100.65 462.261 1209.3 486.539 1287.49 552.029" stroke-width="1.58826" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_12" d="M88.6315 150.167C87.5465 192.743 93.4101 234.346 104.386 272.095C112.129 298.755 122.834 325.697 146.281 344.655C173.141 366.374 210.19 371.825 244.536 370.586C280.158 369.311 315.892 361.741 350.226 350.216C360.119 346.896 369.879 343.281 379.595 339.615C414.391 326.485 449.071 312.806 484.907 309.551C493.678 308.784 502.655 308.786 511.621 309.596C549.497 313.016 586.567 332.492 601.013 367.41C604.093 374.86 605.951 382.63 606.796 390.599C610.165 422.708 598.2 458.493 610.026 489.055C617.819 509.199 633.696 524.209 652.022 533.558C670.137 542.723 690.537 546.871 711.289 547.76C713.803 547.87 716.325 547.931 718.853 547.942C763.347 548.172 808.858 535.468 852.411 520.382C896.029 505.272 938.846 487.746 983.849 477.758C990.348 476.316 996.882 475.042 1003.45 473.939C1103.49 457.121 1211.71 480.829 1289.59 545.953" stroke-width="1.51159" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_13" d="M96.5828 142.263C94.9808 185.144 99.9598 227.101 109.782 265.408C116.709 292.465 126.508 320.096 149.925 339.452C176.674 361.56 214.173 366.543 248.656 364.859C284.418 363.121 320.222 355.205 354.743 343.694C364.691 340.378 374.514 336.797 384.303 333.188C419.358 320.26 454.446 307.105 490.464 304.515C499.288 303.911 508.297 304.078 517.28 305.055C555.188 309.181 591.995 329.494 606.075 364.701C609.078 372.212 610.863 380.041 611.622 388.059C614.643 420.387 602.157 456.348 614.384 486.918C622.36 506.862 638.491 521.484 656.895 530.32C675.099 538.983 695.424 542.541 716.056 543.007C718.556 543.066 721.062 543.077 723.574 543.042C767.786 542.46 812.961 529.591 856.308 514.563C899.723 499.512 942.408 482.27 987.219 472.408C993.688 470.985 1000.19 469.725 1006.73 468.632C1106.32 451.977 1214.11 475.115 1291.68 539.875" stroke-width="1.43493" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_14" d="M104.534 134.361C102.413 177.545 106.508 219.859 115.176 258.723C121.289 286.175 130.18 314.498 153.569 334.251C180.206 356.748 218.155 361.261 252.776 359.133C288.675 356.935 324.551 348.671 359.258 337.176C369.26 333.864 379.149 330.314 389.011 326.762C424.328 314.036 459.82 301.409 496.023 299.479C504.9 299.036 513.94 299.37 522.941 300.516C560.88 305.35 597.424 326.501 611.14 361.993C614.065 369.566 615.779 377.451 616.452 385.52C619.125 418.067 606.114 454.204 618.748 484.782C626.905 504.525 643.293 518.762 661.774 527.083C680.066 535.242 700.317 538.21 720.83 538.256C723.315 538.263 725.807 538.226 728.302 538.145C772.232 536.754 817.069 523.717 860.208 508.749C903.423 493.755 945.971 476.797 990.592 467.061C997.033 465.656 1003.51 464.411 1010.02 463.328C1109.16 446.836 1216.52 469.404 1293.78 533.799" stroke-width="1.35827" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_15" d="M112.485 126.457C109.845 169.945 113.055 212.614 120.572 252.037C125.871 279.885 133.856 308.898 157.216 329.05C183.742 351.934 222.141 355.982 256.898 353.407C292.935 350.747 328.883 342.135 363.777 330.656C373.334 327.512 382.799 324.18 392.243 320.855L393.722 320.334C429.299 307.813 465.198 295.71 501.584 294.443C510.512 294.163 519.584 294.661 528.604 295.978C566.575 301.515 602.853 323.505 616.208 359.286C619.056 366.92 620.697 374.862 621.283 382.982C623.606 415.748 610.071 452.063 623.113 482.647C631.447 502.191 648.097 516.04 666.654 523.847C685.035 531.503 705.213 533.882 725.605 533.505C728.073 533.461 730.55 533.377 733.031 533.249C776.678 531.045 821.179 517.844 864.109 502.937C907.122 487.999 949.539 471.327 993.967 461.718C1000.38 460.331 1006.83 459.1 1013.31 458.029C1112 441.7 1218.93 463.698 1295.88 527.729" stroke-width="1.28161" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_16" d="M120.436 118.553C117.277 162.346 119.605 205.371 125.968 245.35C130.453 273.593 137.532 303.296 160.86 323.847C187.277 347.118 226.124 350.698 261.017 347.677C297.193 344.555 333.211 335.598 368.293 324.132C378.402 320.828 388.421 317.343 398.43 313.903C434.266 301.584 470.572 290.006 507.141 289.401C516.122 289.283 525.225 289.947 534.261 291.431C572.262 297.672 608.278 320.503 621.27 356.571C624.039 364.269 625.61 372.265 626.109 380.436C628.084 413.422 614.016 449.917 627.473 480.504C635.984 499.85 652.893 513.309 671.53 520.604C689.999 527.757 710.102 529.546 730.374 528.748C732.829 528.653 735.29 528.52 737.753 528.347C781.119 525.331 825.282 511.965 868.003 497.116C910.814 482.236 953.099 465.85 997.334 456.366C1003.72 454.996 1010.14 453.781 1016.59 452.72C1114.84 436.556 1221.34 457.982 1297.98 521.648" stroke-width="1.20299" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_17" d="M128.387 110.651C124.709 154.749 126.152 198.127 131.361 238.667C135.031 267.307 141.205 297.696 164.504 318.648C190.813 342.304 230.106 345.42 265.136 341.953C301.45 338.369 337.54 329.066 372.809 317.614C382.974 314.313 393.058 310.862 403.138 307.477C439.235 295.361 475.948 284.31 512.698 284.367C521.732 284.412 530.866 285.241 539.92 286.894C577.952 293.839 613.705 317.512 626.331 353.865C629.024 361.623 630.522 369.678 630.934 377.899C632.562 411.103 617.959 447.78 631.833 478.372C640.517 497.521 657.691 510.589 676.404 517.37C694.962 524.02 714.99 525.219 735.142 524.001C737.581 523.855 740.027 523.672 742.476 523.454C785.561 519.629 829.386 506.095 871.899 491.306C914.509 476.483 956.66 460.382 1000.7 451.023C1007.06 449.673 1013.45 448.47 1019.87 447.42C1117.67 431.42 1223.74 452.273 1300.07 515.578" stroke-width="1.12632" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_18" d="M136.338 102.747C132.142 147.148 132.7 190.882 136.757 231.98C139.613 261.014 144.883 292.094 168.151 313.445C194.351 337.487 234.091 340.136 269.258 336.225C305.71 332.179 341.872 322.53 377.327 311.092C387.546 307.795 397.693 304.375 407.847 301.049C444.204 289.135 481.324 278.611 518.259 279.331C527.346 279.537 536.51 280.532 545.583 282.356C583.648 290.004 619.136 314.516 631.398 351.158C634.015 358.979 635.44 367.089 635.766 375.363C637.044 408.787 621.904 445.647 636.199 476.239C645.054 495.192 662.495 507.868 681.285 514.139C699.931 520.283 719.885 520.897 739.917 519.253C742.342 519.057 744.772 518.827 747.205 518.562C790.008 513.925 833.494 500.226 875.801 485.495C918.208 470.729 960.226 454.913 1004.08 445.681C1010.41 444.348 1016.77 443.161 1023.16 442.121C1120.52 426.283 1226.16 446.567 1302.17 509.506" stroke-width="1.04966" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_19" d="M144.29 94.8432C139.576 139.548 139.25 183.638 142.151 225.293C144.193 254.722 148.557 286.488 171.793 308.242C197.885 332.669 238.071 334.855 273.377 330.497C309.967 325.989 346.199 315.992 381.842 304.57C392.114 301.277 402.328 297.888 412.555 294.619C449.174 282.908 486.698 272.909 523.816 274.291C532.954 274.66 542.152 275.818 551.243 277.811C589.339 286.161 624.562 311.516 636.462 348.443C639 356.326 640.356 364.493 640.594 372.817C641.524 406.46 625.844 443.505 640.56 474.096C649.585 492.853 667.293 505.138 686.162 510.895C704.897 516.537 724.775 516.561 744.689 514.497C747.099 514.249 749.513 513.97 751.931 513.657C794.452 508.208 837.6 494.345 879.701 479.675C921.906 464.966 963.792 449.435 1007.45 440.328C1013.76 439.013 1020.09 437.84 1026.45 436.812C1123.36 421.137 1228.57 440.851 1304.27 503.426" stroke-width="0.973003" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_20" d="M152.241 86.9412C147.008 131.951 145.797 176.395 147.547 218.609C148.773 248.434 152.237 280.887 175.439 303.042C201.425 327.853 242.056 329.575 277.497 324.773C314.225 319.803 350.529 309.46 386.359 298.051C396.687 294.763 406.965 291.408 417.265 288.194C454.143 276.687 492.074 267.212 529.375 269.256C538.566 269.789 547.795 271.112 556.904 273.274C595.032 282.324 629.991 308.523 641.526 345.738C643.987 353.684 645.27 361.908 645.422 370.282C646.003 404.144 629.777 441.376 644.922 471.963C654.114 490.527 672.094 502.417 691.039 507.662C709.862 512.8 729.666 512.234 749.46 509.749C751.854 509.451 754.254 509.122 756.656 508.765C798.895 502.506 841.707 488.475 883.599 473.864C925.601 459.214 967.356 443.967 1010.82 434.985C1017.1 433.69 1023.4 432.53 1029.73 431.512C1126.2 416.001 1230.98 435.145 1306.37 497.354" stroke-width="0.896342" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_21" d="M160.192 79.0373C154.44 124.35 152.345 169.151 152.942 211.922C153.355 242.144 155.914 275.281 179.086 297.839C204.966 323.035 246.042 324.293 281.618 319.043C318.484 313.612 354.861 302.922 390.878 291.529C401.259 288.245 411.602 284.921 421.977 281.764C459.114 270.459 497.452 261.512 534.937 264.217C544.182 264.91 553.446 266.381 562.569 268.73C600.748 278.409 635.424 305.523 646.595 343.024C648.979 351.03 650.19 359.311 650.255 367.736C650.487 401.817 633.714 439.237 649.29 469.821C658.646 488.192 676.9 499.687 695.921 504.419C714.833 509.054 734.564 507.898 754.237 504.991C756.617 504.641 759.001 504.263 761.388 503.86C803.345 496.79 845.819 482.594 887.503 468.042C929.304 453.449 970.925 438.488 1014.2 429.633C1020.45 428.355 1026.72 427.209 1033.02 426.203C1129.04 410.855 1233.39 429.428 1308.48 491.274" stroke-width="0.819681" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_22" d="M168.143 71.1334C161.872 116.751 158.894 161.906 158.336 205.237C157.935 235.854 159.592 269.679 182.73 292.638C208.504 318.215 250.025 319.011 285.739 313.317C322.744 307.424 359.19 296.389 395.395 285.009C405.831 281.729 416.239 278.438 426.685 275.336C464.083 264.234 502.826 255.811 540.494 259.181C549.792 260.038 559.088 261.673 568.228 264.191C606.441 274.574 640.849 302.527 651.659 340.317C653.964 348.386 655.104 356.723 655.083 365.199C654.967 399.499 637.641 437.11 653.65 467.686C663.168 485.865 681.696 496.967 700.796 501.183C719.796 505.315 739.453 503.569 759.005 500.242C761.37 499.841 763.739 499.416 766.109 498.966C807.785 491.085 849.921 476.724 891.397 462.232L894.734 461.065C935.216 446.915 975.636 432.786 1017.57 424.29C1023.79 423.03 1030.03 421.9 1036.31 420.903C1131.87 405.721 1235.8 423.722 1310.57 485.202" stroke-width="0.741055" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_23" d="M176.094 63.2315C169.305 109.153 165.442 154.664 163.732 198.552C162.517 229.564 163.272 264.073 186.374 287.439C212.044 313.399 254.007 313.734 289.859 307.593C327.002 301.238 363.52 289.857 399.91 278.491C410.401 275.214 420.872 271.955 431.392 268.913C469.051 258.013 508.201 250.117 546.051 254.147C555.4 255.165 564.727 256.965 573.887 259.654C612.131 270.743 646.277 299.536 656.72 337.613C658.949 345.742 660.017 354.136 659.909 362.663C659.445 397.182 641.563 434.987 658.01 465.556C667.689 483.543 686.494 494.248 705.671 497.952C724.76 501.58 744.342 499.245 763.774 495.496C766.125 495.044 768.478 494.571 770.833 494.075C812.226 485.383 854.026 470.857 895.293 456.423C936.69 441.944 978.045 427.553 1020.94 418.95C1027.13 417.707 1033.34 416.593 1039.59 415.606C1134.71 400.586 1238.2 418.018 1312.67 479.134" stroke-width="0.664394" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_24" d="M184.045 55.3276C176.739 101.552 171.992 147.419 169.128 191.864C167.097 223.274 166.954 258.467 190.021 282.236C215.586 308.578 257.991 308.45 293.979 301.865C331.259 295.05 367.85 283.319 404.427 271.969C414.971 268.696 425.509 265.471 436.102 262.483C474.02 251.786 513.577 244.416 551.61 249.109C561.012 250.29 570.371 252.256 579.548 255.112C617.826 266.904 651.706 296.538 661.786 334.9C663.936 343.093 664.933 351.543 664.738 360.119C663.924 394.858 645.484 432.861 662.373 463.415C672.213 481.214 691.296 491.52 710.55 494.71C729.727 497.836 749.234 494.913 768.547 490.74C770.882 490.236 773.219 489.714 775.558 489.173C816.67 479.669 858.132 464.978 899.191 450.603C940.385 436.181 981.609 422.075 1024.31 413.597C1030.47 412.375 1036.66 411.272 1042.87 410.297C1137.55 395.44 1240.61 412.302 1314.76 473.053" stroke-width="0.587733" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_25" d="M191.996 47.4256C184.171 93.9548 178.539 140.175 174.521 185.181C171.677 216.986 170.676 252.822 193.665 277.037C219.175 303.713 261.974 303.172 298.099 296.141C335.519 288.864 372.178 276.787 408.944 265.451C419.543 262.184 430.144 258.988 440.812 256.057C478.991 245.562 518.953 238.718 557.171 244.072C566.626 245.417 576.016 247.548 585.211 250.573C623.522 263.069 657.137 293.542 666.853 332.193C668.927 340.447 669.851 348.954 669.569 357.582C668.408 392.539 649.406 430.74 666.739 461.28C676.736 478.892 696.098 488.797 715.43 491.477C734.696 494.097 754.128 490.586 773.323 485.992C775.643 485.438 777.966 484.868 780.289 484.278C821.118 473.965 862.244 459.106 903.096 444.79C944.09 430.425 985.178 416.605 1027.69 408.252C1033.82 407.047 1039.98 405.961 1046.17 404.995C1140.39 390.302 1243.03 406.593 1316.87 466.981" stroke-width="0.511072" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_26" d="M199.947 39.5216C191.603 86.3555 185.087 132.932 179.917 178.494C176.259 210.694 174.354 247.219 197.309 271.834C222.711 298.899 265.958 297.888 302.219 290.411C339.776 282.672 376.509 270.249 413.461 258.927C424.115 255.664 434.781 252.501 445.519 249.625C483.958 239.335 524.327 233.016 562.728 239.032C572.236 240.538 581.657 242.834 590.87 246.028C629.212 259.23 662.562 290.543 671.916 329.479C673.914 337.793 674.765 346.358 674.397 355.036C672.885 390.214 653.317 428.615 671.101 459.138C681.253 476.565 700.898 486.067 720.307 488.233C739.661 490.35 759.021 486.25 778.094 481.234C780.399 480.628 782.705 480.009 785.015 479.374C825.562 468.248 866.35 453.225 906.994 438.97C947.785 424.662 988.742 411.128 1031.06 402.9C1037.16 401.713 1043.29 400.639 1049.45 399.686C1143.23 385.156 1245.43 400.877 1318.97 460.899" stroke-width="0.434411" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_27" d="M207.898 31.6177C199.035 78.7543 191.636 125.686 185.311 171.809C180.837 204.403 178.028 241.619 200.954 266.632C226.246 294.085 269.941 292.609 306.337 284.685C344.032 276.484 380.835 263.715 417.975 252.407C428.682 249.148 439.414 246.016 450.225 243.198C488.925 233.11 529.699 227.317 568.283 233.996C577.842 235.665 587.297 238.126 596.528 241.49C634.903 255.395 667.987 287.547 676.976 326.772C678.895 335.149 679.675 343.769 679.221 352.498C677.361 387.894 657.223 426.502 675.459 457.003C685.767 474.246 705.695 483.345 725.18 484.998C744.623 486.612 763.908 481.922 782.861 476.485C785.151 475.828 787.443 475.16 789.735 474.48C830.001 462.542 870.449 447.353 910.886 433.157C951.475 418.908 992.3 405.658 1034.43 397.557C1040.5 396.388 1046.6 395.33 1052.73 394.387C1146.06 380.02 1247.84 395.171 1321.06 454.827" stroke-width="0.357751" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_28" d="M215.849 23.7157C206.467 71.157 198.184 118.445 190.707 165.124C185.419 198.115 181.704 236.019 204.6 261.431C229.784 289.271 273.925 287.327 310.459 278.959C348.294 270.296 385.168 257.179 422.494 245.887C433.256 242.632 444.051 239.532 454.937 236.77C493.896 226.885 535.077 221.617 573.846 228.958C583.458 230.788 592.944 233.415 602.191 236.949C640.6 251.56 673.418 284.551 682.044 324.061C683.886 332.5 684.593 341.178 684.053 349.957C681.845 385.573 661.135 424.387 679.825 454.864C690.286 471.924 710.497 480.618 730.061 481.758C749.592 482.869 768.802 477.589 787.635 471.732C789.911 471.024 792.188 470.308 794.464 469.581C834.448 456.834 874.559 441.478 914.788 427.341C955.177 413.149 995.868 400.183 1037.8 392.209C1043.85 391.059 1049.92 390.013 1056.02 389.081C1148.9 374.879 1250.25 389.459 1323.16 448.751" stroke-width="0.28109" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_29" d="M223.801 15.8118C213.901 63.5577 204.732 111.2 196.102 158.436C190.001 191.823 185.382 230.419 208.244 256.228C233.318 284.457 277.907 282.045 314.579 273.231C352.551 264.106 389.498 250.644 427.011 239.367C437.826 236.115 448.688 233.049 459.647 230.344C498.866 220.661 540.453 215.92 579.405 223.924C589.07 225.918 598.588 228.709 607.852 232.41C646.292 247.725 678.848 281.556 687.109 321.354C688.874 329.856 689.509 338.589 688.882 347.419C686.325 383.253 665.041 422.279 684.188 452.729C694.801 469.607 715.299 477.896 734.94 478.523C754.559 479.13 773.695 473.263 792.408 466.981C794.668 466.222 796.929 465.457 799.191 464.685C838.894 451.126 878.667 435.605 918.688 421.527C958.874 407.391 999.431 394.711 1041.17 386.862C1047.2 385.73 1053.24 384.7 1059.31 383.778C1151.74 369.739 1252.66 383.749 1325.26 442.675" stroke-width="0.202463" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_30" d="M231.752 7.90787C221.334 55.9565 211.281 103.956 201.496 151.751C194.579 185.533 189.056 224.819 211.889 251.027C236.855 279.643 281.89 276.763 318.699 267.505C356.809 257.919 393.827 244.11 431.528 232.847C442.398 229.599 453.323 226.564 464.355 223.917C503.835 214.436 545.827 210.222 584.962 218.886C594.68 221.043 604.227 223.999 613.511 227.869C651.985 243.888 684.273 278.56 692.171 318.646C693.857 327.208 694.422 335.999 693.708 344.881C690.803 380.936 668.89 420.208 688.548 450.597C699.285 467.313 720.097 475.177 739.815 475.289C759.522 475.394 778.583 468.936 797.177 462.233C799.421 461.424 801.668 460.61 803.913 459.792C843.334 445.423 882.77 429.735 922.582 415.718C962.566 401.64 1002.99 389.245 1044.54 381.521C1050.53 380.407 1056.55 379.391 1062.59 378.481C1154.58 364.605 1255.06 378.044 1327.36 436.607" stroke-width="0.125802" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
<path id="Vector_31" d="M239.701 0.00587115L206.89 145.066C199.159 179.245 192.729 219.219 215.533 245.826C240.389 274.829 285.874 271.484 322.817 261.777C372.094 248.831 419.447 229.068 469.062 217.487C518.678 205.905 572.438 203.031 619.172 223.327C665.904 243.622 703.088 291.592 698.538 342.338C695.285 378.61 672.837 418.069 692.91 448.456C703.823 464.976 724.895 472.449 744.691 472.048C764.488 471.647 783.476 464.602 801.949 457.479C888.315 424.172 974.332 386.884 1065.87 373.171C1157.42 359.459 1257.47 372.328 1329.45 430.525" stroke-width="0.0491416" stroke-miterlimit="10" stroke="white" stroke-opacity="0.3" style="stroke:white;stroke-opacity:0.3;"/>
</g>
<defs>
<clipPath id="clip0_0_306">
<rect width="1329.47" height="613.674" fill="white" style="fill:white;fill-opacity:1;"/>
</clipPath>
</defs>
</svg>
```

### `src/flows/dash/components/BuildOption.module.css`

```css
/* An L3 clickable Card (12 padding, radius 12, lift); DASH swaps its 1px edge for the DASH green at 50%. */
.option {
  --card-border: inset 0 0 0 1px color-mix(in srgb, var(--app-dash-green) 50%, transparent); /* 1px: Figma stroke */
}

/* Icon tile: 56, radius 8, grey (a small highlight — L3 grey-surface rule), DASH green glyph. */
.tile {
  display: grid;
  place-items: center;
  flex: none;
  width: var(--l3-size-56);
  height: var(--l3-size-56);
  border-radius: var(--l3-radius-08);
  background: var(--l3-surface-secondary);
  color: var(--l3-content-accent-success-default);
}

.text {
  flex: 1;
  min-width: 0;
  text-align: start;
}

.title {
  font: var(--l3-text-heading-14);
  color: var(--l3-content-primary);
}

.description {
  font: var(--l3-text-label-12);
  color: var(--l3-content-tertiary);
}

.chevron {
  flex: none;
  color: var(--l3-content-primary);
}

/* DASH: hover and press tint the card with the DASH orb green at 10% instead of L3's grey state layer (designer's call,
   2026-10-23). The card keeps its press scale. */
@media (hover: hover) {
  .option.option:hover {
    /* the DASH orb green at 10%, laid over the card's own surface */
    background-image: linear-gradient(color-mix(in srgb, var(--app-dash-glow-1) 10%, transparent), color-mix(in srgb, var(--app-dash-glow-1) 10%, transparent));
  }
}

.option.option:active {
  /* the DASH orb green at 10%, laid over the card's own surface */
  background-image: linear-gradient(color-mix(in srgb, var(--app-dash-glow-1) 10%, transparent), color-mix(in srgb, var(--app-dash-glow-1) 10%, transparent));
}
```

### `src/flows/dash/components/BuildOption.tsx`

```tsx
import { Card } from '@l3/components/Card'
import { Icon } from '@l3/components/Icon'
import { Stack } from '@l3/components/Stack'
import { msChevronRight } from '@l3/icons/material'
import styles from './BuildOption.module.css'

type BuildOptionProps = {
  icon: string
  title: string
  description: string
  onClick?: () => void
}

/**
 * "Choose what to build" on the DASH landing: one option per L3 clickable Card — an icon tile, the name, one line on
 * what it does, a chevron. DASH style on the card: its edge in the DASH green at 50% (Figma: lime 1px stroke).
 */
export function BuildOption({ icon, title, description, onClick }: BuildOptionProps) {
  return (
    <Card onClick={onClick} className={styles.option}>
      <Stack direction="horizontal" align="center" gap="16">
        <span className={styles.tile} aria-hidden="true">
          <Icon icon={icon} size={24} />
        </span>
        <Stack gap="04" className={styles.text}>
          <span className={styles.title}>{title}</span>
          <span className={styles.description}>{description}</span>
        </Stack>
        <Icon icon={msChevronRight} size={24} className={styles.chevron} />
      </Stack>
    </Card>
  )
}
```

### `src/flows/dash/components/Chat.module.css`

```css
/* DASH chat parts. Layout is L3 Stacks; this file holds the DASH look (edges, pills, motion). */

/* ---- Chat input: Figma "Input field", 56 tall, 16 / 8 padding, full radius, DASH green 1px edge, float shadow */
.input {
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-08);
  min-height: var(--l3-size-56);
  padding: var(--l3-spacing-08) var(--l3-spacing-08) var(--l3-spacing-08) var(--l3-spacing-16);
  border: 1px solid var(--app-dash-green);
  border-radius: var(--l3-radius-full);
  background: var(--l3-surface-default);
  box-shadow: var(--app-dash-float-shadow);
}

.input:focus-within {
  outline: 2px solid var(--app-dash-green);
  outline-offset: 1px;
}

.field {
  flex: 1;
  min-width: 0;
  border: 0;
  padding: 0;
  background: none;
  color: var(--l3-content-primary);
  font: var(--l3-text-label-14);
  outline: none;
}

.field::placeholder {
  color: var(--l3-content-tertiary);
}

/* Figma "Send Button": a 40 circle. */
.send.send {
  flex: none;
  width: var(--l3-size-40);
  border-radius: var(--l3-radius-full);
}

/* ---- "✦ CHOOSE WHAT TO BUILD ✦" */
.sparkleHeading {
  color: var(--l3-content-tertiary);
}

.sparkle {
  width: var(--l3-size-12);
  height: var(--l3-size-12);
}

.caps {
  margin: 0;
  font: var(--l3-text-label-12);
  letter-spacing: var(--app-dash-caps-tracking);
  text-transform: uppercase;
}

/* ---- Example prompts: rows scroll sideways together, cut at the screen edges like Figma */
.chipScroll {
  overflow-x: auto;
  overscroll-behavior-x: contain;
  scrollbar-width: none;
  padding-inline: var(--l3-spacing-16);
}

.chipScroll::-webkit-scrollbar {
  display: none;
}

.chipRows {
  width: max-content;
}

.chipRow {
  width: max-content;
}

/* Figma: the second row starts further left than the first — the rows don't line up, it reads as a stream. */
.chipRow[data-row='1'] {
  margin-inline-start: calc(-1 * var(--l3-spacing-24));
}

/* Figma 36 tall (L3 has 36 only as a spacing step). */
.promptChip {
  flex: none;
  min-height: var(--l3-spacing-36);
  padding: var(--l3-spacing-08) var(--l3-spacing-12);
  border: 1px solid var(--app-dash-green);
  border-radius: var(--l3-radius-full);
  background: var(--l3-surface-default);
  color: var(--l3-content-primary);
  font: var(--l3-text-label-12);
  white-space: nowrap;
  cursor: pointer;
  transition: transform var(--l3-motion-duration-short) var(--l3-motion-easing-standard);
}

.promptChip:active {
  transform: scale(var(--l3-motion-scale-press-default));
}

.promptChip:focus-visible,
.reply:focus-visible {
  outline: 2px solid var(--l3-content-primary);
  outline-offset: 2px;
}

/* ---- Messages */
.userMessage {
  margin: 0;
  max-width: 80%;
  padding: var(--l3-spacing-12) var(--l3-spacing-16);
  border-radius: var(--l3-radius-full);
  background: var(--l3-surface-secondary);
  color: var(--l3-content-primary);
  font: var(--l3-text-label-14);
}

.assistant p {
  margin: 0;
  color: var(--l3-content-primary);
}

.assistantTitle {
  font: var(--l3-text-heading-14);
}

.assistantText {
  font: var(--l3-text-description-14);
  text-wrap: pretty;
}

/* An earlier message scrolled up under the pinned rule card (Figma: faded). */
.assistant[data-faded] {
  opacity: var(--l3-opacity-40);
}

/* ---- Thinking */
.thinking {
  margin: 0;
  color: var(--l3-content-primary);
  font: var(--l3-text-label-14);
}

.dots {
  display: inline-flex;
  gap: var(--l3-spacing-04);
}

.dots span {
  width: var(--l3-size-08);
  height: var(--l3-size-08);
  border-radius: var(--l3-radius-full);
  background: var(--l3-content-primary);
  animation: pulse var(--l3-motion-duration-spin) var(--l3-motion-easing-standard) infinite;
}

.dots span:nth-child(2) { animation-delay: 150ms; }
.dots span:nth-child(3) { animation-delay: 300ms; }

@keyframes pulse {
  0%, 100% { opacity: var(--l3-opacity-30); }
  50% { opacity: var(--l3-opacity-100); }
}

@media (prefers-reduced-motion: reduce) {
  .dots span { animation: none; }
}

/* ---- Quick replies: Figma 12 / 8 padding, Label/12, border-light; DASH's suggestion has the green edge + lift */
.reply {
  padding: var(--l3-spacing-08) var(--l3-spacing-12);
  border: 1px solid var(--l3-border-light);
  border-radius: var(--l3-radius-full);
  background: var(--l3-surface-default);
  color: var(--l3-content-primary);
  font: var(--l3-text-label-12);
  white-space: nowrap;
  cursor: pointer;
  transition: transform var(--l3-motion-duration-short) var(--l3-motion-easing-standard);
}

.reply[data-suggested] {
  border-color: var(--app-dash-green);
  box-shadow: var(--l3-shadow-sm);
}

.reply:active {
  transform: scale(var(--l3-motion-scale-press-default));
}
```

### `src/flows/dash/components/Chat.tsx`

```tsx
import { useRef, useState, type ReactNode } from 'react'
import { Button } from '@l3/components/Button'
import { Icon } from '@l3/components/Icon'
import { Stack } from '@l3/components/Stack'
import { msArrowUpward } from '@l3/icons/material'
import sparkle from '../assets/sparkle.svg'
import { t } from '../copy'
import styles from './Chat.module.css'

/**
 * DASH's chat parts — its own look (designer's call): pills with the DASH green edge, plain assistant text, a grey
 * pill for what the user said. Text styles, spacing and surfaces are L3; only the DASH green and the float shadow are
 * local tokens.
 */

/** The message field at the bottom of every DASH chat page: a white pill with the DASH green edge and a round Send. */
export function ChatInput({ onSend, value: initial = '' }: { onSend?: (text: string, from: HTMLElement) => void; value?: string }) {
  const [value, setValue] = useState(initial)
  const field = useRef<HTMLInputElement>(null)
  /** `from`: where the text was typed — the sent message flies up from there. */
  const send = () => {
    const text = value.trim()
    if (!text || !field.current) return
    onSend?.(text, field.current)
    setValue('')
  }
  return (
    <form
      className={styles.input}
      onSubmit={(e) => {
        e.preventDefault()
        send()
      }}
    >
      <input
        ref={field}
        className={styles.field}
        value={value}
        onChange={(e) => setValue(e.target.value)}
        placeholder={t('input.placeholder')}
        aria-label={t('input.label')}
        enterKeyHint="send"
      />
      <Button type="submit" variant="primary" size="md" iconLeft={<Icon icon={msArrowUpward} />} aria-label={t('input.send')} disabled={!onSend} className={styles.send} />
    </form>
  )
}

/** "✦ HERE ARE SOME EXAMPLES ✦": a small caps heading between two DASH sparkles. */
export function SparkleHeading({ children }: { children: ReactNode }) {
  return (
    <Stack as="header" direction="horizontal" align="center" justify="center" gap="12" className={styles.sparkleHeading}>
      <img src={sparkle} alt="" className={styles.sparkle} />
      <h2 className={styles.caps}>{children}</h2>
      <img src={sparkle} alt="" className={styles.sparkle} />
    </Stack>
  )
}

/** Example prompts as rows of DASH chips that scroll sideways (Figma: two rows, cut at the screen edges). */
export function PromptChips({ rows, onPick }: { rows: string[][]; onPick?: (prompt: string, from: HTMLElement) => void }) {
  return (
    <div className={styles.chipScroll} role="group" aria-label={t('examples.label')}>
      <Stack gap="12" className={styles.chipRows}>
        {rows.map((row, i) => (
          <Stack key={i} direction="horizontal" gap="08" className={styles.chipRow} data-row={i}>
            {row.map((prompt) => (
              <button key={prompt} type="button" className={styles.promptChip} onClick={(e) => onPick?.(prompt, e.currentTarget)}>
                {prompt}
              </button>
            ))}
          </Stack>
        ))}
      </Stack>
    </div>
  )
}

/** What the user sent: a grey pill on the right. */
export function UserMessage({ children }: { children: ReactNode }) {
  return (
    <Stack direction="horizontal" justify="end">
      <p className={styles.userMessage}>{children}</p>
    </Stack>
  )
}

/** DASH's reply: a bold first line (Heading/14) and the text under it (Description/14), no bubble. */
export function AssistantMessage({ title, children, faded }: { title?: ReactNode; children?: ReactNode; faded?: boolean }) {
  return (
    <Stack gap="08" className={styles.assistant} data-faded={faded || undefined}>
      {title && <p className={styles.assistantTitle}>{title}</p>}
      {children && <p className={styles.assistantText}>{children}</p>}
    </Stack>
  )
}

/** DASH is working: three pulsing dots and what it's doing. */
export function Thinking({ children }: { children: ReactNode }) {
  return (
    <Stack direction="horizontal" align="center" gap="12" role="status">
      <span className={styles.dots} aria-hidden="true">
        <span />
        <span />
        <span />
      </span>
      <p className={styles.thinking}>{children}</p>
    </Stack>
  )
}

export type ReplyOption = { label: string; onClick?: () => void; suggested?: boolean }

/** Quick replies under DASH's question: pill chips; the one DASH suggests has the green edge. */
export function ReplyChips({ options }: { options: ReplyOption[] }) {
  return (
    <Stack direction="horizontal" gap="08" wrap role="group" aria-label={t('replies.label')}>
      {options.map((o) => (
        <button key={o.label} type="button" className={styles.reply} data-suggested={o.suggested || undefined} onClick={o.onClick}>
          {o.label}
        </button>
      ))}
    </Stack>
  )
}
```

### `src/flows/dash/components/DashScreen.module.css`

```css
/*
 * DASH background. Geometry is artwork, measured on the 360-wide Figma frame and scaled with the screen width
 * (cqw: 1cqw = 3.6 Figma px), so the glow sits in the same place on 360 / 392 / 412 screens.
 */
.frame {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: clip;
  container-type: size;
  background: var(--l3-surface-default);
}

.background {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

/* The screen itself is see-through so the background shows; its parts (panels, docks) bring their own surfaces. */
.screen.screen {
  background: transparent;
}

/*
 * Figma "Gradient": a ring (stroke 456 on a 342 radius → 114 … 570 from the centre) filled with an angular gradient
 * (lime → yellow → green → lime), 50% opacity, layer blur 50.
 */
.glow {
  --ring: 316cqw; /* outer diameter, 1140 on the Figma frame */
  --glow-1: var(--app-dash-glow-1);
  --glow-2: var(--app-dash-glow-2);
  --glow-3: var(--app-dash-glow-3);
  position: absolute;
  left: 50%;
  width: var(--ring);
  height: var(--ring);
  opacity: var(--l3-opacity-50);
  filter: blur(var(--app-dash-glow-blur)); /* on the wrapper, so the ring's cut edges blur too */
  /* The chat starting (bottom → header) glides the ring up rather than jumping. */
  transition:
    top var(--app-dash-glow-move) var(--l3-motion-easing-decelerate),
    width var(--app-dash-glow-move) var(--l3-motion-easing-decelerate),
    height var(--app-dash-glow-move) var(--l3-motion-easing-decelerate);
  transform: translate(-50%, -50%) rotate(-75.32deg);
  animation: dash-glow-turn 3.08s infinite;
}

/* Figma motion on "Gradient": one full turn (−75.3° → 284.7°), ease-in over the first 75% of 3.08 s, then a pause. */
@keyframes dash-glow-turn {
  0% {
    transform: translate(-50%, -50%) rotate(-75.32deg);
    animation-timing-function: ease-in;
  }
  75.38%,
  100% {
    transform: translate(-50%, -50%) rotate(284.68deg);
  }
}

@media (prefers-reduced-motion: reduce) {
  .glow {
    animation: none;
  }
}

.glow::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: var(--l3-radius-full);
  background: conic-gradient(from 90deg, var(--glow-1) 0deg, var(--glow-2) 124.6deg, var(--glow-3) 238.8deg, var(--glow-1) 360deg);
  mask: radial-gradient(circle closest-side, transparent var(--hole, 20%), black calc(var(--hole, 20%) + 1%));
}

/* Chat pages and the landing: the ring's centre 476 down (rule-created frames: 14 right of centre). */
.frame[data-glow='header'] .glow {
  --ring: 346cqw; /* 1246 */
  top: 132cqw;
}

/* DASH landing (5612:81961): the glow across the top only — a bigger white middle, low enough to sit behind the
   option cards and the bottom (Figma: an oval ring turned 88.7°, matched by eye). */
.frame[data-glow='landing'] .glow {
  --ring: 346cqw;
  --hole: 40%;
  top: 155cqw;
}

/* Rules landing: the ring's centre 466 below the bottom edge — only its top rises into the screen. */
.frame[data-glow='bottom'] .glow {
  top: calc(100cqh + 129cqw);
}

/* Loss: the same ring in the L3 reds (the backtest says the rule would have cost money). */
.frame[data-tone='loss'] .glow {
  --glow-1: var(--l3-base-hue-red-300);
  --glow-2: var(--l3-base-hue-red-100);
  --glow-3: var(--l3-base-hue-red-400);
}

/* Figma "Layer_1": white wave lines (30% stroke), 1329 × 614, turned −58.13°, in a 1223 × 1453 box at (−432, −120). */
.wavesBox {
  position: absolute;
  top: -33cqw;
  left: -120cqw;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 340cqw;
  height: 404cqw;
}

.frame[data-glow='header'] .wavesBox {
  top: -200cqw; /* −720: the lines cross the header */
}

.wavesBox {
  transition: top var(--app-dash-glow-move) var(--l3-motion-easing-decelerate);
}

.waves {
  flex: none;
  width: 369cqw;
  height: 170cqw;
  max-width: none;
  transform: rotate(-58.13deg);
}

/* DASH top bar: L3 Actionbar, see-through, no hairline (L3 gap: the Actionbar has no transparent option). */
.bar.bar {
  background: transparent;
  box-shadow: none;
}

@media (prefers-reduced-motion: reduce) {
  .glow,
  .wavesBox {
    transition: none;
  }
}
```

### `src/flows/dash/components/DashScreen.tsx`

```tsx
import type { ComponentProps } from 'react'
import { Actionbar, type ActionbarProps } from '@l3/components/Actionbar'
import { Screen } from '../../../shared/ui/Screen'
import waves from '../assets/waves.svg'
import styles from './DashScreen.module.css'

/** Where DASH's glow sits: behind the header (chat pages), across the top (DASH landing) or rising from the bottom (rules landing). */
export type DashGlow = 'header' | 'landing' | 'bottom'
/** brand = lime-green (every page); loss = the same glow in red, when the backtest says the rule would have cost money. */
export type DashTone = 'brand' | 'loss'

type DashScreenProps = ComponentProps<typeof Screen> & { glow?: DashGlow; tone?: DashTone }

/**
 * A DASH AI page: the app's <Screen> on DASH's own background — a soft blurred lime ring (Figma "Gradient", an angular
 * gradient at 50% with layer blur 50) and fine white wave lines over it (Figma "Layer_1"). The background is
 * decoration only; everything on it is a normal screen (top bar, body, bottom, sheet).
 * With the glow at the top, the OS status bar takes the brand-light surface (data-status-surface="brand"), the
 * nearest L3 surface to the glow; otherwise it stays surface/default.
 */
export function DashScreen({ glow = 'header', tone = 'brand', className, ...screen }: DashScreenProps) {
  return (
    <div className={styles.frame} data-glow={glow} data-tone={tone} data-status-surface={glow === 'bottom' ? undefined : 'brand'}>
      <div className={styles.background} aria-hidden="true">
        <span className={styles.glow} />
        <span className={styles.wavesBox}>
          <img src={waves} alt="" className={styles.waves} />
        </span>
      </div>
      <Screen {...screen} className={[styles.screen, className].filter(Boolean).join(' ')} />
    </div>
  )
}

/**
 * The L3 Actionbar on DASH's background: see-through (the glow runs under it) and without the hairline. Everything
 * else is the standard bar — back, title, Tertiary actions, the bottom slot for what stays pinned.
 */
export function DashBar(props: ActionbarProps) {
  return <Actionbar {...props} className={styles.bar} />
}
```

### `src/flows/dash/components/IndicatorCard.module.css`

```css
/* Indicator card — the same shell as the rule card (border-light, radius 12, 12 inside), with the mini chart. */
.card {
  overflow: clip;
  border: 1px solid var(--l3-border-light);
  border-radius: var(--l3-radius-12);
  background: var(--l3-surface-default);
}

/* Ready: the DASH green edge (wireframe: the ready card is outlined in lime). */
.card:not([data-state='building']) {
  border-color: color-mix(in srgb, var(--app-dash-green) 60%, transparent);
}

.body {
  padding: var(--l3-spacing-12);
}

.heading {
  min-width: 0;
}

.eyebrow {
  font: var(--l3-text-label-10);
  letter-spacing: var(--app-dash-caps-tracking);
  text-transform: uppercase;
  color: var(--l3-content-tertiary);
}

.title {
  margin: 0;
  font: var(--l3-text-heading-14);
  color: var(--l3-content-primary);
  animation: card-fade var(--l3-motion-duration-medium) var(--l3-motion-easing-decelerate) both;
}

.meta {
  font: var(--l3-text-label-12);
  color: var(--l3-content-tertiary);
}

.tag {
  flex: none;
  animation: card-pop var(--app-dash-pop-duration) var(--app-dash-pop-easing) both;
}

.chart {
  padding-block: var(--l3-spacing-04);
}

/* Parts: small pills tinted by what they are (wireframe: lime lines, blue labels). */
.part {
  padding: var(--l3-spacing-02) var(--l3-spacing-08);
  border-radius: var(--l3-radius-full);
  background: var(--l3-surface-accent-success-light);
  color: var(--l3-content-accent-success-default);
  font: var(--l3-text-label-12);
  animation: card-pop var(--app-dash-pop-duration) var(--app-dash-pop-easing) both;
}

.part[data-tone='average'] {
  background: var(--l3-surface-accent-discover-light);
  color: var(--l3-content-accent-discover-default);
}

.part[data-tone='buy'] {
  background: var(--l3-surface-accent-indicator-up-light);
  color: var(--l3-content-accent-indicator-up-default);
}

.part[data-tone='sell'] {
  background: var(--l3-surface-accent-indicator-down-light);
  color: var(--l3-content-accent-indicator-down-default);
}

/* Build steps: a spinner on the current one, a tick when done, a ring for the next ones. */
.steps {
  margin: 0;
  padding: 0;
  list-style: none;
}

.steps li {
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-08);
  font: var(--l3-text-label-12);
  color: var(--l3-content-tertiary);
}

.steps li[data-state='now'] {
  color: var(--l3-content-primary);
}

.stepMark {
  display: grid;
  place-items: center;
  flex: none;
  width: var(--l3-size-16);
  height: var(--l3-size-16);
  border: 1.5px solid var(--l3-border-light);
  border-radius: var(--l3-radius-full);
}

.steps li[data-state='done'] .stepMark {
  border-color: transparent;
  background: var(--l3-content-accent-success-default);
  color: var(--l3-static-white);
}

.steps li[data-state='now'] .stepMark {
  border-color: var(--app-dash-green) transparent var(--app-dash-green) var(--app-dash-green);
  animation: card-spin var(--l3-motion-duration-spin) linear infinite;
}

/* Notes: a warning row that folds open (wireframe "2 notes on the conversion"). */
.notes {
  border-top: 1px dashed var(--l3-border-light);
  padding-top: var(--l3-spacing-08);
}

.notesToggle {
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-08);
  width: 100%;
  min-height: var(--l3-size-32);
  padding: 0;
  border: 0;
  background: none;
  color: var(--l3-content-accent-warning-default);
  font: var(--l3-text-label-12);
  text-align: start;
  cursor: pointer;
}

.notesText {
  flex: 1;
}

.chevron {
  transition: transform var(--l3-motion-duration-short) var(--l3-motion-easing-standard);
}

.notesToggle[aria-expanded='true'] .chevron {
  transform: rotate(180deg);
}

.noteList {
  margin: 0;
  padding: 0 0 0 var(--l3-spacing-24);
  color: var(--l3-content-secondary);
  font: var(--l3-text-description-12);
  animation: card-fade var(--l3-motion-duration-medium) var(--l3-motion-easing-decelerate) both;
}

.noteList li + li {
  margin-top: var(--l3-spacing-04);
}

@keyframes card-fade {
  from {
    opacity: 0;
    transform: translateY(var(--l3-spacing-04));
  }
}

@keyframes card-pop {
  from {
    opacity: 0;
    transform: scale(0.6);
  }
}

@keyframes card-spin {
  to {
    transform: rotate(1turn);
  }
}

@media (prefers-reduced-motion: reduce) {
  .title,
  .tag,
  .part,
  .noteList,
  .steps li[data-state='now'] .stepMark {
    animation: none;
  }
}
```

### `src/flows/dash/components/IndicatorCard.tsx`

```tsx
import { useState } from 'react'
import { Button } from '@l3/components/Button'
import { Icon } from '@l3/components/Icon'
import { Stack } from '@l3/components/Stack'
import { Tag } from '@l3/components/Tag'
import { msAdd, msCheck, msError, msKeyboardArrowDown } from '@l3/icons/material'
import { t } from '../copy'
import { buildSteps, type IndicatorDraft } from '../indicatorData'
import { MiniChart } from './MiniChart'
import styles from './IndicatorCard.module.css'

/** Where the indicator is: being built (with the step reached), ready, being added, or on the chart. */
export type IndicatorCardState = { kind: 'building'; step: 0 | 1 | 2 } | { kind: 'ready' } | { kind: 'adding' } | { kind: 'onChart' }

type IndicatorCardProps = {
  draft: IndicatorDraft
  state: IndicatorCardState
  /** Its name is known (shown from step 1 of the build — "taking shape"). */
  named?: boolean
  sellTone?: 'sell' | 'orange'
  onAdd?: () => void
}

/**
 * The indicator being built, pinned above the chat (like the rule card in Trading rules) — but it shows a mini live
 * chart instead of text (wireframe "AI Indicator"):
 * - building: "Your indicator" → its name; a wave → candles with the lines drawing in; the parts appear as chips;
 *   three steps tick through;
 * - ready: Ready tag, BUY / SELL labels on the chart, where it goes, DASH's notes (fold open), Add to chart;
 * - adding: the button shows a loader; on chart: the tag and the button turn to "On chart".
 */
export function IndicatorCard({ draft, state, named = state.kind !== 'building' || state.step > 0, sellTone, onAdd }: IndicatorCardProps) {
  const [notesOpen, setNotesOpen] = useState(false)
  const building = state.kind === 'building'
  const stage = building ? (state.step === 0 ? 'sketch' : 'candles') : 'ready'

  return (
    <article className={styles.card} data-state={state.kind} aria-label={named ? draft.name : t('indicator.yourIndicator')}>
      <Stack gap="12" className={styles.body}>
        <Stack direction="horizontal" align="start" justify="between" gap="12">
          <Stack gap="04" className={styles.heading}>
            {building && <span className={styles.eyebrow}>{t('indicator.building')}</span>}
            <h2 key={named ? 'named' : 'unnamed'} className={styles.title}>{named ? draft.name : t('indicator.yourIndicator')}</h2>
            {!building && <span className={styles.meta}>{t('indicator.placement', { place: draft.placement, count: draft.testedOn })}</span>}
          </Stack>
          {!building && (
            <Tag color="success" size="sm" iconLeft={state.kind === 'onChart' ? <Icon icon={msCheck} size={14} /> : undefined} className={styles.tag}>
              {state.kind === 'onChart' ? t('indicator.onChart') : t('indicator.ready')}
            </Tag>
          )}
        </Stack>

        <div className={styles.chart}>
          <MiniChart stage={stage} sellTone={sellTone} />
        </div>

        {(named || !building) && (
          <Stack direction="horizontal" gap="08" wrap className={styles.parts}>
            {draft.parts.map((p, i) => (
              <span key={p.label} className={styles.part} data-tone={p.tone} style={{ animationDelay: `${i * 90}ms` }}>
                {p.kind === 'line' && building ? t('indicator.linePart', { name: p.label }) : p.label}
              </span>
            ))}
          </Stack>
        )}

        {building ? (
          <Stack as="ol" gap="08" className={styles.steps}>
            {buildSteps.map((label, i) => (
              <li key={label} data-state={i < state.step ? 'done' : i === state.step ? 'now' : 'next'}>
                <span className={styles.stepMark} aria-hidden="true">{i < state.step && <Icon icon={msCheck} size={12} />}</span>
                {label}
              </li>
            ))}
          </Stack>
        ) : (
          <>
            <div className={styles.notes}>
              <button type="button" className={styles.notesToggle} aria-expanded={notesOpen} onClick={() => setNotesOpen((o) => !o)}>
                <Icon icon={msError} size={16} />
                <span className={styles.notesText}>{t('indicator.notes', { count: draft.notes.length })}</span>
                <Icon icon={msKeyboardArrowDown} size={16} className={styles.chevron} />
              </button>
              {notesOpen && (
                <ul className={styles.noteList}>
                  {draft.notes.map((n) => <li key={n}>{n}</li>)}
                </ul>
              )}
            </div>
            <Stack direction="horizontal" align="center" justify="between" gap="12">
              {state.kind === 'onChart' ? (
                <Button variant="secondary" fullWidth iconLeft={<Icon icon={msCheck} />} disabled>{t('indicator.onChart')}</Button>
              ) : (
                <Button variant="primary" fullWidth iconLeft={<Icon icon={msAdd} />} loading={state.kind === 'adding'} onClick={onAdd}>
                  {state.kind === 'adding' ? t('indicator.adding') : t('indicator.addToChart')}
                </Button>
              )}
            </Stack>
          </>
        )}
      </Stack>
    </article>
  )
}
```

### `src/flows/dash/components/MiniChart.module.css`

```css
/*
 * The indicator preview. SVG geometry is artwork (viewBox units); colours are L3 tokens. Motion: candles fade up one
 * after another, then each line draws itself left to right (stroke-dashoffset 1 → 0), then the labels pop in.
 */
.chart {
  display: block;
  width: 100%;
  height: auto;
  overflow: visible;
}

/* sketch: a faint wave that keeps running while DASH reads the request */
.sketch {
  fill: none;
  stroke: var(--app-dash-glow-1);
  stroke-width: 2;
  stroke-linecap: round;
  stroke-dasharray: 0.35 0.65;
  opacity: var(--l3-opacity-60);
  animation: mini-sketch 1.4s linear infinite;
}

@keyframes mini-sketch {
  from { stroke-dashoffset: 1; }
  to { stroke-dashoffset: 0; }
}

.candles g {
  stroke: var(--l3-content-accent-indicator-down-default);
  fill: var(--l3-content-accent-indicator-down-default);
  opacity: var(--l3-opacity-60);
  animation: mini-candle var(--l3-motion-duration-medium) var(--l3-motion-easing-decelerate) both;
}

.candles g[data-up] {
  stroke: var(--l3-content-accent-indicator-up-default);
  fill: var(--l3-content-accent-indicator-up-default);
}

.candles line {
  stroke-width: 1;
}

@keyframes mini-candle {
  from {
    opacity: 0;
    transform: translateY(6px);
  }
}

.ema,
.trend {
  fill: none;
  stroke-width: 1.6;
  stroke-linecap: round;
  stroke-linejoin: round;
  stroke-dasharray: 1;
  stroke-dashoffset: 1;
  animation: mini-draw 900ms var(--l3-motion-easing-standard) 500ms forwards;
}

.ema {
  stroke: var(--l3-content-accent-discover-default);
}

.trend {
  stroke: var(--l3-content-accent-indicator-down-default);
  stroke-width: 2;
}

.trend[data-up] {
  stroke: var(--l3-content-accent-indicator-up-default);
}

@keyframes mini-draw {
  to { stroke-dashoffset: 0; }
}

/* ready: lines already drawn (no replay), labels pop in */
.chart[data-stage='ready'] .ema,
.chart[data-stage='ready'] .trend {
  animation: none;
  stroke-dashoffset: 0;
}

.chart[data-stage='ready'] .candles g {
  animation: none;
}

.signal {
  animation: mini-pop var(--app-dash-pop-duration) var(--app-dash-pop-easing) both;
}

.signal rect {
  fill: var(--l3-content-accent-indicator-up-default);
}

.signal[data-side='sell'] rect {
  fill: var(--l3-content-accent-indicator-down-default);
}

.signal[data-side='orange'] rect {
  fill: var(--l3-content-accent-orange-default);
}

.signal text {
  fill: var(--l3-static-white);
  font: var(--l3-text-heading-10);
  font-size: 7px; /* viewBox units: the chart scales to ≈ 1.1 × on a 328 card */
  text-anchor: middle;
}

@keyframes mini-pop {
  from {
    opacity: 0;
    transform: scale(0.4);
  }
}

@media (prefers-reduced-motion: reduce) {
  .sketch,
  .candles g,
  .ema,
  .trend,
  .signal {
    animation: none;
  }

  .ema,
  .trend {
    stroke-dashoffset: 0;
  }
}
```

### `src/flows/dash/components/MiniChart.tsx`

```tsx
import { useMemo } from 'react'
import { ema, sampleCandles, supertrend } from '../indicatorData'
import styles from './MiniChart.module.css'

/**
 * - `sketch`: DASH is still reading — a faint wave runs across (no candles yet);
 * - `candles`: the candles are in, the indicator lines draw themselves in;
 * - `ready`: lines drawn, BUY / SELL labels pop in where the Supertrend flips.
 */
export type MiniChartStage = 'sketch' | 'candles' | 'ready'

const W = 300 // viewBox units — the SVG scales to the card's width
const H = 128
const PAD = 8

/**
 * The live preview on the indicator card: sample candles with the indicator drawn on them — Supertrend (green while
 * up, red while down), EMA 20 (blue) and BUY / SELL labels. Decoration for the preview: the card's text says what it is.
 */
export function MiniChart({ stage, sellTone = 'sell' }: { stage: MiniChartStage; sellTone?: 'sell' | 'orange' }) {
  const data = useMemo(() => {
    const candles = sampleCandles()
    const avg = ema(candles, 20)
    const trend = supertrend(candles, 10, 3)
    const values = candles.flatMap((c) => [c.high, c.low]).concat(trend.line.slice(10))
    const min = Math.min(...values)
    const max = Math.max(...values)
    const step = (W - PAD * 2) / candles.length
    const x = (i: number) => PAD + step * (i + 0.5)
    const y = (v: number) => PAD + (1 - (v - min) / (max - min)) * (H - PAD * 2)
    const path = (vs: number[], from = 0) => vs.slice(from).map((v, i) => `${i ? 'L' : 'M'}${x(i + from).toFixed(1)},${y(v).toFixed(1)}`).join('')
    // Supertrend as runs of one direction, so each run takes its colour.
    const runs: { up: boolean; d: string }[] = []
    let start = 10
    for (let i = 11; i <= candles.length; i++) {
      if (i === candles.length || trend.up[i] !== trend.up[start]) {
        runs.push({ up: trend.up[start], d: path(trend.line.slice(0, i), start) })
        start = i
      }
    }
    return { candles, x, y, step, ema: path(avg, 5), runs, signals: trend.signals.filter((s) => s.index > 10) }
  }, [])

  return (
    <svg viewBox={`0 0 ${W} ${H}`} className={styles.chart} data-stage={stage} aria-hidden="true">
      {stage === 'sketch' ? (
        <path className={styles.sketch} d={`M${PAD},${H / 2} C 60,${H / 2 - 30} 100,${H / 2 + 30} 150,${H / 2} S 240,${H / 2 - 30} ${W - PAD},${H / 2}`} pathLength={1} />
      ) : (
        <>
          <g className={styles.candles}>
            {data.candles.map((c, i) => {
              const up = c.close >= c.open
              const bodyTop = data.y(Math.max(c.open, c.close))
              const bodyH = Math.max(1, Math.abs(data.y(c.open) - data.y(c.close)))
              return (
                <g key={i} data-up={up || undefined} style={{ animationDelay: `${i * 8}ms` }}>
                  <line x1={data.x(i)} x2={data.x(i)} y1={data.y(c.high)} y2={data.y(c.low)} />
                  <rect x={data.x(i) - data.step * 0.3} width={data.step * 0.6} y={bodyTop} height={bodyH} />
                </g>
              )
            })}
          </g>
          <path className={styles.ema} d={data.ema} pathLength={1} />
          {data.runs.map((r, i) => (
            <path key={i} className={styles.trend} data-up={r.up || undefined} d={r.d} pathLength={1} />
          ))}
          {stage === 'ready' &&
            data.signals.map((s, i) => {
              const c = data.candles[s.index]
              const buy = s.side === 'buy'
              const cx = data.x(s.index)
              const cy = buy ? data.y(c.low) + 12 : data.y(c.high) - 12
              return (
                <g key={i} className={styles.signal} data-side={buy ? 'buy' : sellTone} style={{ animationDelay: `${i * 70}ms`, transformOrigin: `${cx}px ${cy}px` }}>
                  <rect x={cx - 13} y={cy - 6} width={26} height={12} rx={3} />
                  <text x={cx} y={cy + 3}>{buy ? 'BUY' : 'SELL'}</text>
                </g>
              )
            })}
        </>
      )}
    </svg>
  )
}
```

### `src/flows/dash/components/QuestionCard.module.css`

```css
/* Figma question card: white, radius 16, 1px border-light, the DASH float shadow; 12 inside. */
.card {
  padding: var(--l3-spacing-08) var(--l3-spacing-12) var(--l3-spacing-04);
  border: 1px solid var(--l3-border-light);
  border-radius: var(--l3-radius-16);
  background: var(--l3-surface-default);
  box-shadow: var(--app-dash-float-shadow);
}

.header {
  padding-block: var(--l3-spacing-04);
}

.question {
  margin: 0;
  font: var(--l3-text-heading-14);
  color: var(--l3-content-primary);
}

.options {
  margin: 0;
  padding: 0;
  list-style: none;
}

.options li + li {
  border-top: 1px solid var(--l3-border-light);
}

/* An answer row: 44 tall (Figma), number badge · value · tick. Whole row is the button. */
.option {
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-12);
  width: 100%;
  min-height: var(--l3-size-tap-target);
  padding: var(--l3-spacing-08) 0;
  border: 0;
  background: none;
  color: var(--l3-content-primary);
  text-align: start;
  cursor: pointer;
}

.option:focus-visible {
  outline: 2px solid var(--l3-content-primary);
  outline-offset: -2px;
  border-radius: var(--l3-radius-08);
}

/* Figma: a 24 grey disc with the number (Label/12, secondary). */
.number {
  display: grid;
  place-items: center;
  flex: none;
  width: var(--l3-size-24);
  height: var(--l3-size-24);
  border-radius: var(--l3-radius-full);
  background: var(--l3-surface-secondary);
  color: var(--l3-content-secondary);
  font: var(--l3-text-label-12);
}

.label {
  flex: 1;
  font: var(--l3-text-label-14);
}

/* Picked: a filled 24 disc with a tick (content-primary, like Figma's black). */
.tick {
  display: grid;
  place-items: center;
  width: var(--l3-size-24);
  height: var(--l3-size-24);
  border-radius: var(--l3-radius-full);
  background: var(--l3-content-primary);
  color: var(--l3-content-inverted);
}

/* "Type your answer": under a dashed line, a pencil and a plain field. */
.typeRow {
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-12);
  min-height: var(--l3-size-tap-target);
  border-top: 1px dashed var(--l3-border-light);
}

.typeIcon {
  flex: none;
  color: var(--l3-content-primary);
}

.typeField {
  flex: 1;
  min-width: 0;
  border: 0;
  padding: 0;
  background: none;
  color: var(--l3-content-primary);
  font: var(--l3-text-label-12);
  outline: none;
}

.typeField::placeholder {
  color: var(--l3-content-tertiary);
}
```

### `src/flows/dash/components/QuestionCard.tsx`

```tsx
import { useId, useState } from 'react'
import { Button } from '@l3/components/Button'
import { Icon } from '@l3/components/Icon'
import { Stack } from '@l3/components/Stack'
import { msCheck, msClose, msEdit } from '@l3/icons/material'
import { t } from '../copy'
import type { RuleQuestion } from '../data'
import styles from './QuestionCard.module.css'

type QuestionCardProps = {
  question: RuleQuestion
  /** Prototype state: an answer already ticked. */
  selected?: string
  onAnswer?: (answer: string) => void
  onClose?: () => void
}

/**
 * DASH asks a follow-up, docked above the chat input (Figma 5612:40650): the question + ✕, numbered answers (the
 * picked one gets a filled tick), and a last "Type your answer" row for anything else.
 */
export function QuestionCard({ question, selected: initial, onAnswer, onClose }: QuestionCardProps) {
  const id = useId()
  const [selected, setSelected] = useState(initial)
  const [typed, setTyped] = useState('')
  const pick = (answer: string) => {
    setSelected(answer)
    onAnswer?.(answer)
  }
  return (
    <section className={styles.card} aria-labelledby={id}>
      <Stack direction="horizontal" align="center" justify="between" gap="12" className={styles.header}>
        <h2 id={id} className={styles.question}>{question.question}</h2>
        <Button variant="ghost" size="sm" iconLeft={<Icon icon={msClose} />} aria-label={t('question.close')} onClick={onClose} />
      </Stack>
      <ol className={styles.options}>
        {question.options.map((option, i) => (
          <li key={option}>
            <button type="button" className={styles.option} aria-pressed={selected === option} onClick={() => pick(option)}>
              <span className={styles.number} aria-hidden="true">{i + 1}</span>
              <span className={styles.label}>{option}</span>
              {selected === option && (
                <span className={styles.tick} aria-hidden="true">
                  <Icon icon={msCheck} size={16} />
                </span>
              )}
            </button>
          </li>
        ))}
      </ol>
      {question.freeText && (
        <form
          className={styles.typeRow}
          onSubmit={(e) => {
            e.preventDefault()
            if (typed.trim()) pick(typed.trim())
          }}
        >
          <Icon icon={msEdit} size={20} className={styles.typeIcon} />
          <input
            className={styles.typeField}
            value={typed}
            onChange={(e) => setTyped(e.target.value)}
            placeholder={t('question.typePlaceholder')}
            aria-label={t('question.typeLabel')}
            enterKeyHint="send"
          />
        </form>
      )}
    </section>
  )
}
```

### `src/flows/dash/components/RuleCard.module.css`

```css
/* Figma rule card (5612:41008): 1px border-light, radius 12, white body 12 / 12 / 16, then the backtest strip. */
.card {
  overflow: clip;
  border: 1px solid var(--l3-border-light);
  border-radius: var(--l3-radius-12);
  background: var(--l3-surface-default);
}

.body {
  padding: var(--l3-spacing-12) var(--l3-spacing-12) var(--l3-spacing-16);
}

.title {
  margin: 0;
  font: var(--l3-text-heading-14);
  color: var(--l3-content-primary);
}

/* Figma dashed line under the name (1px, border-light). */
.rule {
  width: 100%;
  margin: 0;
  border: 0;
  border-top: 1px dashed var(--l3-border-light);
}

/* The value the user picked: a grey chip (Figma 12 / 8, full radius, Label/12). */
.valueChip {
  align-self: flex-start;
  padding: var(--l3-spacing-08) var(--l3-spacing-12);
  border-radius: var(--l3-radius-full);
  background: var(--l3-surface-secondary);
  color: var(--l3-content-primary);
  font: var(--l3-text-label-12);
}

/* The description (2 lines, cut with …) + chevron: opens the read-more sheet. */
.description {
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-12);
  width: 100%;
  padding: 0;
  border: 0;
  background: none;
  text-align: start;
  cursor: pointer;
}

.descriptionText {
  display: -webkit-box;
  flex: 1;
  overflow: hidden;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  color: var(--l3-content-tertiary);
  font: var(--l3-text-label-12);
}

.chevron {
  flex: none;
  color: var(--l3-content-primary);
}

.description:focus-visible {
  outline: 2px solid var(--l3-content-primary);
  outline-offset: 2px;
  border-radius: var(--l3-radius-04);
}

/* ---- Backtest strip: Figma 12 / 8, a top-to-bottom tint from white into the result's light accent (to 135%). */
.strip {
  --tint: var(--l3-surface-secondary);
  --tone: var(--l3-content-secondary);
  padding: var(--l3-spacing-08) var(--l3-spacing-12);
  background: linear-gradient(to bottom, var(--l3-surface-default), var(--tint) 135%);
}

.strip[data-result='saved'] {
  --tint: var(--l3-surface-accent-success-light);
  --tone: var(--l3-content-accent-success-default);
}

.strip[data-result='cost'] {
  --tint: var(--l3-surface-accent-error-light);
  --tone: var(--l3-content-accent-error-default);
}

/* In the read-more sheet: a card of its own (Figma: 1px edge in the result's colour, radius 8, a left-to-right tint). */
.strip[data-shape='pill'] {
  padding: var(--l3-spacing-12);
  border: 1px solid var(--l3-border-light);
  border-radius: var(--l3-radius-08);
  background: linear-gradient(to right, var(--tint), var(--l3-surface-default) 60%);
}

.strip[data-shape='pill'][data-result='saved'] { border-color: var(--l3-border-accent-success-light); }
.strip[data-shape='pill'][data-result='cost'] { border-color: var(--l3-border-accent-error-light); }

.stripIcon {
  flex: none;
  color: var(--tone);
}

.strip[data-result='none'] .stripIcon {
  color: var(--l3-content-secondary);
}

.stripText {
  margin: 0;
  font: var(--l3-text-heading-12);
  color: var(--l3-content-secondary);
}

.amount {
  color: var(--tone);
}

/* The backtest strip unfolds (height + fade, 300 ms) when DASH's backtest comes back; the description fades in. */
.unfold {
  display: grid;
  grid-template-rows: 1fr;
  animation: rule-unfold var(--app-dash-pin-duration) var(--l3-motion-easing-decelerate) both;
}

.unfoldInner {
  min-height: 0;
  overflow: hidden;
}

@keyframes rule-unfold {
  from {
    grid-template-rows: 0fr;
    opacity: 0;
  }
}

.description {
  animation: rule-fade var(--l3-motion-duration-medium) var(--l3-motion-easing-decelerate) both;
}

@keyframes rule-fade {
  from {
    opacity: 0;
  }
}

@media (prefers-reduced-motion: reduce) {
  .unfold,
  .description {
    animation: none;
  }
}
```

### `src/flows/dash/components/RuleCard.tsx`

```tsx
import { Icon } from '@l3/components/Icon'
import { Stack } from '@l3/components/Stack'
import { msChevronRight, msError, msTrendingDown, msTrendingUp } from '@l3/icons/material'
import { t } from '../copy'
import { rupees, type Backtest, type DraftRule } from '../data'
import { DashOrb } from '../../../shared/ui/dash/DashOrb'
import styles from './RuleCard.module.css'

type RuleCardProps = {
  rule: DraftRule
  /** Preview (before the backtest): the picked value as a chip. Once tested: the description + the backtest strip. */
  backtest?: Backtest
  /** The description opens the "read more" sheet. */
  onOpen?: () => void
}

/**
 * The rule DASH is building, pinned above the chat: the DASH orb + the rule's name, a dashed line, then the value
 * the user picked (preview) or the rule's description (tap → read more) and what the backtest says, on a strip
 * tinted by the result (saved = green, cost = red, no data = grey).
 */
export function RuleCard({ rule, backtest, onOpen }: RuleCardProps) {
  return (
    <article className={styles.card} aria-label={rule.title}>
      <Stack gap="12" className={styles.body}>
        <Stack gap="08">
          <Stack direction="horizontal" align="center" gap="08">
            <DashOrb size="sm" />
            <h2 className={styles.title}>{rule.title}</h2>
          </Stack>
          <hr className={styles.rule} />
        </Stack>
        {backtest ? (
          <button type="button" className={styles.description} onClick={onOpen} aria-label={`${rule.description} ${t('rule.readMore')}`}>
            <span className={styles.descriptionText}>{rule.description}</span>
            <Icon icon={msChevronRight} size={20} className={styles.chevron} />
          </button>
        ) : (
          <span className={styles.valueChip}>{rule.value}</span>
        )}
      </Stack>
      {backtest && (
        // Unfolds under the card when the backtest comes back (same page, so it plays once).
        <div className={styles.unfold}>
          <div className={styles.unfoldInner}>
            <BacktestStrip backtest={backtest} />
          </div>
        </div>
      )}
    </article>
  )
}

/** What the backtest says, on a strip tinted by the result. Also used, rounded, in the read-more sheet. */
export function BacktestStrip({ backtest, shape = 'strip' }: { backtest: Backtest; shape?: 'strip' | 'pill' }) {
  const icon = backtest.result === 'saved' ? msTrendingUp : backtest.result === 'cost' ? msTrendingDown : msError
  return (
    <Stack direction="horizontal" align="center" gap="08" className={styles.strip} data-result={backtest.result} data-shape={shape}>
      <Icon icon={icon} size={24} className={styles.stripIcon} />
      <p className={styles.stripText}>
        {backtest.result === 'none' ? (
          t('backtest.none')
        ) : (
          <>
            {t(backtest.result === 'saved' ? 'backtest.savedBefore' : 'backtest.costBefore')}
            <strong className={styles.amount}>{rupees(backtest.amount)}</strong>
            {t(backtest.result === 'saved' ? 'backtest.savedAfter' : 'backtest.costAfter', { months: backtest.months })}
          </>
        )}
      </p>
    </Stack>
  )
}
```

### `src/flows/dash/components/RuleSummary.module.css`

```css
/* Figma: 1px border-light, radius 12, 12 inside. */
.box {
  padding-inline: var(--l3-spacing-12);
  border: 1px solid var(--l3-border-light);
  border-radius: var(--l3-radius-12);
}

.oneLine {
  display: block;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

### `src/flows/dash/components/RuleSummary.tsx`

```tsx
import { Avatar } from '@l3/components/Avatar'
import { ListCell } from '@l3/components/ListCell'
import styles from './RuleSummary.module.css'

/** A rule in one bordered row (created / delete sheets): its initial on an Avatar, the name, one line of what it does. */
export function RuleSummary({ title, description }: { title: string; description: string }) {
  return (
    <div className={styles.box}>
      <ListCell iconLeft={<Avatar initials={title.charAt(0)} size={40} />} label={title} description={<span className={styles.oneLine}>{description}</span>} />
    </div>
  )
}
```

### `src/flows/dash/components/RulesIntro.module.css`

```css
/* Figma: heading 56 below the bar; the examples sit just above the input. */
.page {
  flex: 1;
  padding-block: var(--l3-spacing-48) var(--l3-spacing-24);
}

.heading {
  text-align: center;
}

.title {
  margin: 0;
  font: var(--l3-text-heading-20);
  color: var(--l3-content-primary);
  white-space: pre-line; /* two lines: the greeting, then the question */
}

.description {
  margin: 0;
  font: var(--l3-text-label-12);
  color: var(--l3-content-tertiary);
  text-wrap: balance;
}
```

### `src/flows/dash/components/RulesIntro.tsx`

```tsx
import { Inset } from '@l3/components/Inset'
import { Stack } from '@l3/components/Stack'
import { DashOrb } from '../../../shared/ui/dash/DashOrb'
import { t } from '../copy'
import { examplePrompts, firstName } from '../data'
import { PromptChips, SparkleHeading } from './Chat'
import styles from './RulesIntro.module.css'

/**
 * The empty chat (Figma "Rule - Landing screen" 5612:40261; the indicator chat uses it too): greeting, the orb, example prompts as DASH chips. It's the
 * first state of the one chat screen — tapping a chip sends it, and this fades away as the conversation starts.
 */
type RulesIntroProps = {
  onPick?: (prompt: string, from: HTMLElement) => void
  /** Defaults: Trading rules. The indicator chat passes its own. */
  title?: string
  description?: string
  examples?: string[][]
}

export function RulesIntro({ onPick, title = t('rulesHome.title', { name: firstName }), description = t('rulesHome.description'), examples = examplePrompts }: RulesIntroProps) {
  return (
    <Stack gap="40" justify="between" className={styles.page}>
      <Inset paddingInline="16">
        <Stack gap="12" className={styles.heading}>
          <h1 className={styles.title}>{title}</h1>
          <p className={styles.description}>{description}</p>
        </Stack>
      </Inset>
      <Stack align="center">
        <DashOrb size="lg" />
      </Stack>
      <Stack as="section" gap="24">
        <SparkleHeading>{t('rulesHome.examples')}</SparkleHeading>
        <PromptChips rows={examples} onPick={onPick} />
      </Stack>
    </Stack>
  )
}
```

### `src/flows/dash/components/SuccessBurst.module.css`

```css
/*
 * "Your rule is created" — timings from the live recording (2026-10-23): the sheet is up at ~0.25 s; the disc pops
 * 0.15 → 0.55 s (slight overshoot), the tick draws 0.35 → 0.6 s, confetti bursts with the pop and falls for 1.6 s.
 * Confetti px values are artwork geometry (set per piece in SuccessBurst.tsx).
 */
.burst {
  position: relative;
  display: grid;
  place-items: center;
  width: var(--l3-size-48);
  height: var(--l3-size-48);
}

.tick {
  display: grid;
  place-items: center;
  width: var(--l3-size-48);
  height: var(--l3-size-48);
  border-radius: var(--l3-radius-full);
  background: var(--l3-content-accent-success-default);
  color: var(--l3-static-white);
  animation: tick-pop var(--app-dash-pop-duration) var(--app-dash-pop-easing) 150ms both;
}

@keyframes tick-pop {
  from {
    transform: scale(0);
  }
}

/* View Rules: the tick turns away like a coin (live: 28.5 → 28.8 s) before the list fades in. */
.tick[data-leaving] {
  animation: tick-flip var(--l3-motion-duration-short) var(--l3-motion-easing-accelerate) both;
}

@keyframes tick-flip {
  to {
    transform: scaleX(0);
  }
}

.check {
  width: var(--l3-size-28);
  height: var(--l3-size-28);
  fill: none;
  stroke: currentColor;
  stroke-width: 3; /* artwork stroke, in the 24 viewBox */
  stroke-linecap: round;
  stroke-linejoin: round;
}

.check path {
  stroke-dasharray: 1;
  stroke-dashoffset: 1;
  animation: tick-draw var(--l3-motion-duration-medium) var(--l3-motion-easing-decelerate) 350ms forwards;
}

@keyframes tick-draw {
  to {
    stroke-dashoffset: 0;
  }
}

/* Confetti: every piece starts at the tick, bursts out to (dx, dy), then drifts down and fades. */
.confetti {
  position: absolute;
  inset: 50% auto auto 50%;
  pointer-events: none;
}

.confetti span {
  position: absolute;
  width: var(--l3-size-04);
  height: var(--l3-size-04);
  border-radius: var(--l3-radius-full);
  background: var(--l3-content-accent-success-default);
  opacity: 0;
  animation: confetti var(--app-dash-confetti-duration) cubic-bezier(0.15, 0.7, 0.4, 1) calc(150ms + var(--delay)) forwards;
}

.confetti span[data-tone='1'] {
  background: var(--l3-base-hue-yellow-200);
}

.confetti span[data-tone='2'] {
  background: var(--l3-base-hue-brand-lemonn-400); /* lime in light mode … */
}

:root[data-mode='dark'] .confetti span[data-tone='2'] {
  background: var(--l3-base-neutral-white-base); /* … white in dark, as in the live app */
}

.confetti span[data-long] {
  width: var(--l3-size-08);
  height: var(--l3-spacing-02);
  border-radius: var(--l3-radius-02);
}

@keyframes confetti {
  0% {
    opacity: 0;
    transform: translate(0, 0) scale(0.4);
  }
  8% {
    opacity: 1;
  }
  30% {
    transform: translate(var(--dx), var(--dy)) rotate(calc(var(--turn) / 3)) scale(1);
  }
  80% {
    opacity: 1;
  }
  100% {
    opacity: 0;
    transform: translate(var(--dx), calc(var(--dy) + var(--fall))) rotate(var(--turn)) scale(1);
  }
}

@media (prefers-reduced-motion: reduce) {
  .tick,
  .tick[data-leaving],
  .check path,
  .confetti span {
    animation: none;
  }

  .check path {
    stroke-dashoffset: 0;
  }
}
```

### `src/flows/dash/components/SuccessBurst.tsx`

```tsx
import type { CSSProperties } from 'react'
import styles from './SuccessBurst.module.css'

/** Confetti pieces. Positions are spread with the golden angle, so they look random but render the same every time. */
const PIECES = Array.from({ length: 36 }, (_, i) => {
  const angle = (i * 137.5 * Math.PI) / 180
  const reach = 48 + ((i * 53) % 132) // 48 … 180 out from the tick
  return {
    dx: Math.round(Math.cos(angle) * reach * 1.3), // wider than tall, like the live burst
    dy: Math.round(Math.sin(angle) * reach * 0.55 - 24), // biased upwards
    fall: 90 + ((i * 37) % 70), // then drifts down 90 … 160
    turn: (i * 71) % 360,
    delay: (i % 6) * 30,
    tone: i % 3, // green · yellow · white
    long: i % 4 === 0, // a few strips among the dots
  }
})

/**
 * The success moment on "Your rule is created" (live app recording, 2026-10-23): a green disc pops in with a slight
 * overshoot, the tick draws itself, and confetti bursts out of it and drifts down for ~1.6 s.
 * `leaving` flips the tick away (a coin turn, 200 ms) before the next screen. Decoration only: the heading says it.
 */
export function SuccessBurst({ leaving = false }: { leaving?: boolean }) {
  return (
    <span className={styles.burst} aria-hidden="true">
      <span className={styles.confetti}>
        {PIECES.map((p, i) => (
          <span
            key={i}
            data-tone={p.tone}
            data-long={p.long || undefined}
            style={{ '--dx': `${p.dx}px`, '--dy': `${p.dy}px`, '--fall': `${p.fall}px`, '--turn': `${p.turn}deg`, '--delay': `${p.delay}ms` } as CSSProperties}
          />
        ))}
      </span>
      <span className={styles.tick} data-leaving={leaving || undefined}>
        <svg viewBox="0 0 24 24" className={styles.check}>
          <path d="M6 12.5l4 4 8-9" pathLength={1} />
        </svg>
      </span>
    </span>
  )
}
```

### `src/flows/dash/components/flyMessage.ts`

```ts
// The sent message flies to its place: the chip that was tapped (or the text that was typed) becomes the user's
// message at the top of the chat — like ChatGPT / Claude, the screen itself changes state, nothing is pushed in.
//
// How: a copy ("ghost") of the source is drawn on top of the screen at the source's place, then moved and reshaped
// into the message bubble's place and look; the real bubble shows when it lands. Measured, so it works for any chip,
// any text length and any screen width. If anything goes wrong, the bubble simply shows.

type Look = {
  left: string
  top: string
  width: string
  height: string
  borderRadius: string
  backgroundColor: string
  borderColor: string
  fontSize: string
  lineHeight: string
  paddingTop: string
  paddingRight: string
  paddingBottom: string
  paddingLeft: string
}

/** Where something was and how it looked, captured before the screen changes. */
export type FlyOrigin = { rect: DOMRect; text: string; borderWidth: string; snapshot: Omit<Look, 'left' | 'top' | 'width' | 'height'> }

function lookOf(s: CSSStyleDeclaration): FlyOrigin['snapshot'] {
  return {
    borderRadius: s.borderRadius,
    backgroundColor: s.backgroundColor,
    borderColor: s.borderTopColor,
    fontSize: s.fontSize,
    lineHeight: s.lineHeight,
    paddingTop: s.paddingTop,
    paddingRight: s.paddingRight,
    paddingBottom: s.paddingBottom,
    paddingLeft: s.paddingLeft,
  }
}

export function captureOrigin(el: HTMLElement, text: string): FlyOrigin {
  // Read now: the source leaves the screen as soon as the message is sent.
  const style = getComputedStyle(el)
  return { rect: el.getBoundingClientRect(), text, borderWidth: style.borderTopWidth, snapshot: lookOf(style) }
}

const reduced = () => typeof matchMedia !== 'undefined' && matchMedia('(prefers-reduced-motion: reduce)').matches

/**
 * Fly a ghost from `origin` to `target` (the real message bubble, already laid out). `stage` is the positioned element
 * the ghost is drawn in (the screen). The target is hidden until the ghost lands.
 */
export function flyMessage(origin: FlyOrigin, target: HTMLElement, stage: HTMLElement, durationMs: number) {
  if (reduced()) return
  const s = stage.getBoundingClientRect()
  const to = target.getBoundingClientRect()
  const targetStyle = getComputedStyle(target)
  const box = (r: DOMRect) => ({ left: `${r.left - s.left}px`, top: `${r.top - s.top}px`, width: `${r.width}px`, height: `${r.height}px` })
  const from: Look = { ...box(origin.rect), ...origin.snapshot }
  const end: Look = { ...box(to), ...lookOf(targetStyle) }

  const ghost = document.createElement('div')
  ghost.textContent = origin.text
  ghost.setAttribute('aria-hidden', 'true')
  ghost.dataset.flyGhost = ''
  Object.assign(ghost.style, {
    position: 'absolute',
    zIndex: '30',
    boxSizing: 'border-box',
    display: 'flex',
    alignItems: 'center',
    overflow: 'hidden',
    pointerEvents: 'none',
    borderStyle: 'solid',
    borderWidth: origin.borderWidth,
    color: targetStyle.color,
    fontFamily: targetStyle.fontFamily,
    fontWeight: targetStyle.fontWeight,
    whiteSpace: 'nowrap', // one line, like the chip and the bubble (the border would otherwise make it wrap)
    ...from,
  })

  const land = () => {
    target.style.visibility = ''
    ghost.remove()
  }
  try {
    stage.appendChild(ghost)
    target.style.visibility = 'hidden'
    const animation = ghost.animate([from, { ...end, borderColor: 'transparent' }], { duration: durationMs, easing: 'cubic-bezier(0.2, 0, 0, 1)', fill: 'forwards' })
    animation.onfinish = land
    animation.oncancel = land
    // Safety net: never leave the message hidden.
    window.setTimeout(land, durationMs + 300)
  } catch {
    land()
  }
}
```

### `src/flows/dash/copy.json`

```json
{
  "landing.menu": "Your rules",
  "landing.greeting": "Hi {name}!",
  "landing.title": "Let me know what you would like to do today?",
  "landing.choose": "Choose what to build",
  "landing.rulesTitle": "Set Trading Rules",
  "landing.rulesDescription": "Build trading habits that protect your capital.",
  "landing.strategyTitle": "Create Custom Strategy",
  "landing.strategyDescription": "Describe your strategy and let DASH build it for you.",
  "rulesHome.history": "Rule history",
  "rulesHome.title": "Hello, {name}!\nWhat rules do you want to set?",
  "rulesHome.description": "Describe your trading habits, and I'll turn them into personalised rules.",
  "rulesHome.examples": "Here are some examples",
  "examples.label": "Example rules",
  "input.placeholder": "Type here",
  "input.label": "Message DASH",
  "input.send": "Send",
  "replies.label": "Quick replies",
  "chat.title": "Trading rules",
  "chat.thinking": "Understanding your request…",
  "chat.gotIt": "Got it!",
  "chat.needDetails": "I just need a few details to set this up.",
  "chat.previewTitle": "Rule preview!",
  "chat.refine": "No, I want to refine",
  "chat.proceed": "Proceed",
  "chat.readyTitle": "Your rule is ready! 🎉",
  "chat.readyText": "Do you want to save this rule?",
  "chat.save": "Yes, save it",
  "chat.weakTitle": "Your rule looks weak to me!",
  "chat.weakText": "I would suggest you improve your rule.",
  "chat.saveAnyway": "No, save as it is",
  "chat.improve": "Yes, improve my rule",
  "question.close": "Close",
  "question.typePlaceholder": "Type your answer",
  "question.typeLabel": "Your own answer",
  "rule.readMore": "Read more",
  "backtest.savedBefore": "Would have saved you ",
  "backtest.savedAfter": " in {months} months",
  "backtest.costBefore": "Would’ve cost ",
  "backtest.costAfter": " of profit in {months} months",
  "backtest.none": "Couldn’t test this, not enough history yet",
  "readMore.okay": "Okay",
  "created.title": "Your rule is created",
  "created.description": "Trade as normal. I’ll step in only at the line you set.",
  "created.cancel": "Cancel",
  "created.viewRules": "View Rules",
  "rules.title": "Trading rules",
  "rules.active": "Active rules ({count})",
  "rules.delete": "Delete",
  "rules.deleteLabel": "Delete {name}",
  "rules.toggleLabel": "{name} on",
  "rules.createNew": "Create New",
  "delete.title": "Are you sure you want to delete this rule?",
  "delete.no": "No",
  "delete.yes": "Yes, Delete",
  "deleted.title": "Your rule has been deleted",
  "deleted.description": "DASH has stopped tracking this rule.",
  "deleted.okay": "Okay",
  "landing.indicatorTitle": "Create Custom Indicator",
  "landing.indicatorDescription": "Describe what your chart should show — DASH draws it.",
  "indicator.title": "Custom indicator",
  "indicator.history": "Your indicators",
  "indicator.emptyTitle": "What should your chart show?",
  "indicator.emptyDescription": "Describe it in plain words. DASH builds the indicator and draws it on your chart.",
  "indicator.gotIt": "Got it!",
  "indicator.fewQuestions": "Two quick questions before I build it.",
  "indicator.buildingNow": "Building it for {chart} on {timeframe} candles — watch it take shape above.",
  "indicator.readyTitle": "Your indicator is ready",
  "indicator.readyText": "Add it to your chart, or tell me what to change.",
  "indicator.changed": "Done — the preview above is updated.",
  "indicator.onChartText": "It's on your {chart} chart now. You'll see the BUY / SELL labels as new candles form.",
  "indicator.changeHint": "Ask for changes in plain words — “make the SELL labels orange”, “add a 50 EMA”.",
  "indicator.yourIndicator": "Your indicator",
  "indicator.building": "Building",
  "indicator.ready": "Ready",
  "indicator.onChart": "On chart",
  "indicator.placement": "{place} · checked on {count} candles",
  "indicator.linePart": "Line · {name}",
  "indicator.notes": "{count} notes from testing",
  "indicator.addToChart": "Add indicator on Chart",
  "indicator.adding": "Adding…"
}
```

### `src/flows/dash/copy.ts`

```ts
import { makeCopy } from '../../shared/copy'
import copy from './copy.json'

/** Every user-facing string of DASH AI is in copy.json: t('screen.title'), t('key', { count }). */
export const t = makeCopy('dash', copy)
```

### `src/flows/dash/data.ts`

```ts
// DASH AI · Trading rules — sample data and the shapes the app needs from the DASH API.
import { inr } from '../../shared/data/format'
import { user } from '../../shared/data/user'

/** First name for DASH's greeting ("Hello, Rohan!"). The fictional user every flow uses. */
export const firstName = user.name.split(' ')[0]

/** Example prompts on the rules landing, in two sideways-scrolling rows. Tapping one sends it as the message. */
export const examplePrompts: string[][] = [
  ["Don't let me overtrade", 'Max daily loss', 'Limit my number of trades'],
  ['Limit my position size', 'Cut losing trades automatically', 'Protect my profits'],
]

/** A follow-up DASH asks before it can build the rule: a few suggested answers, or the user types one. */
export type RuleQuestion = {
  question: string
  options: string[]
  /** Free-text answer allowed (the "Type your answer" row). */
  freeText: boolean
}

export const dailyLossQuestion: RuleQuestion = {
  question: 'At what daily loss should I step in?',
  options: ['₹2,000', '₹5,000', '₹10,000'],
  freeText: true,
}

/**
 * What a backtest of the rule on the user's last 3 months of trades says.
 * - saved: the rule would have stopped losses — `amount` saved.
 * - cost: the rule would have stopped trades that ended in profit — `amount` of profit missed ("weak rule").
 * - none: not enough trading history to test it.
 */
export type Backtest =
  | { result: 'saved'; amount: number; months: number }
  | { result: 'cost'; amount: number; months: number }
  | { result: 'none' }

export type DraftRule = {
  title: string
  /** The value the user picked (shown as a chip on the preview card). */
  value: string
  /** One sentence on what the rule does. */
  description: string
}

export const maxDailyLoss: DraftRule = {
  title: 'Max daily loss',
  value: '₹5,000',
  description: 'This rule will help prevent revenge trading by stopping you once your daily losses reach ₹5,000.',
}

export const backtests = {
  saved: { result: 'saved', amount: 18400, months: 3 },
  cost: { result: 'cost', amount: 22390, months: 3 },
  none: { result: 'none' },
} satisfies Record<string, Backtest>

/** A rule the user has saved (Active rules list). `on` = DASH is watching it. */
export type SavedRule = {
  id: string
  title: string
  /** One line: what it does, at what limit. */
  summary: string
  on: boolean
  /** What happened lately / when it was made — the card's footer line. `strong` is the part in bold. */
  activity: { text: string; strong?: string; after?: string }
}

export const activeRules: SavedRule[] = [
  { id: 'max-daily-loss', title: 'Max daily loss', summary: 'Warns at ₹2,000', on: true, activity: { text: 'Created on 30 Jul’26' } },
  { id: 'lot-size-cap', title: 'Lot size cap', summary: 'Max 4 lots per order', on: false, activity: { text: 'Stepped in ', strong: '3 times', after: ' this month' } },
  { id: 'daily-trade-limit', title: 'Daily trade limit', summary: 'This rule will help prevent overtrading by stopping you once your daily trade count reaches 5.', on: true, activity: { text: 'Hit the limit ', strong: '2 days', after: ' this month' } },
  { id: 'cut-losers', title: 'Cut losers', summary: 'Exits any position at −8%', on: true, activity: { text: 'Saved ', strong: '₹6,200', after: ' this month' } },
]

/** After deleting "Max daily loss". */
export const rulesAfterDelete = activeRules.filter((r) => r.id !== 'max-daily-loss')

/** ₹ with Indian grouping, no paise: 18400 → "₹18,400". */
export const rupees = (n: number) => inr(n, 0)
```

### `src/flows/dash/index.tsx`

```tsx
import type { Flow } from '../types'
import { activeRules, backtests, maxDailyLoss, rulesAfterDelete } from './data'
import { ActiveRules } from './screens/ActiveRules'
import { Landing } from './screens/Landing'
import { IndicatorChat } from './screens/IndicatorChat'
import { RuleChat } from './screens/RuleChat'
import { DeleteRuleSheet, ReadMoreSheet, RuleCreatedSheet, RuleDeletedSheet } from './sheets/RuleSheets'

const maxDailyLossRule = activeRules[0]

/**
 * DASH AI · Trading rules — DASH (the in-app AI) turns a trading habit the user describes into a rule, backtests it
 * on their last 3 months, saves it, and lists their rules. Figma: "DASH AI - main flow" (11491:7726) in the F&O dev
 * handoff file — an old design without library components, rebuilt with L3 components plus DASH's own chat style.
 */
export const dash: Flow = {
  id: 'dash',
  title: 'DASH AI · Trading rules',
  description: 'Describe a trading habit; DASH builds it into a rule, backtests it, saves it. Your active rules, on / off, delete.',
  figmaFile: 'nLs24dTJdPz9A5pu3AWMbe',
  sections: [
    { id: 'entry', title: 'Entry' },
    { id: 'create', title: 'Create a rule' },
    { id: 'result', title: 'Rule ready' },
    { id: 'rules', title: 'Your rules' },
    { id: 'indicator', title: 'Custom indicator' },
  ],
  screens: [
    // ---- Entry
    { id: 'dash-02', section: 'entry', name: 'DASH landing', figma: '5612:81961', note: 'From the DASH pill in the Stocks home top bar (stocks-01; first visit: the coach mark, stocks-08). Back returns home. Set Trading Rules opens the rules chat; Create Custom Strategy is another flow (not designed yet). ☰ opens your rules.', render: (go) => <Landing onBack={() => go('stocks-01')} onRules={() => go('dash-03')} onIndicator={() => go('dash-20')} onMenu={() => go('dash-14')} /> },
    // ---- Create a rule
    { id: 'dash-03', group: 'chat', section: 'create', name: 'Rules · what to set', figma: '5612:40261', note: 'The chat’s empty state — the same screen as the conversation. A chip (or typing + Send) sends the message: it flies up into the chat as the greeting and examples fade away. History opens your rules.', render: (go) => <RuleChat step="empty" onBack={() => go('dash-02')} onHistory={() => go('dash-14')} onSend={() => go('dash-04')} /> },
    { id: 'dash-04', group: 'chat', section: 'create', name: 'Thinking', figma: '5612:40517', note: 'Moves on by itself after 1.6 s in the prototype (the real app: when DASH replies).', render: (go) => <RuleChat step="thinking" onBack={() => go('dash-03')} onHistory={() => go('dash-14')} onThought={() => go('dash-05')} /> },
    { id: 'dash-05', group: 'chat', section: 'create', name: 'DASH asks the limit', figma: '5612:40650', note: 'The question card takes the input’s place. Any answer (or one typed) builds the preview.', render: (go) => <RuleChat step="question" onBack={() => go('dash-03')} onHistory={() => go('dash-14')} onAnswer={() => go('dash-06')} /> },
    { id: 'dash-06', group: 'chat', section: 'create', name: 'Rule preview', figma: '5612:40816', note: 'The draft is pinned in the top bar’s bottom slot. Proceed runs the backtest; refine goes back to the question.', render: (go) => <RuleChat step="preview" onBack={() => go('dash-03')} onHistory={() => go('dash-14')} onRefine={() => go('dash-05')} onProceed={() => go('dash-07')} /> },
    // ---- Rule ready (3 backtest results)
    { id: 'dash-07', group: 'chat', section: 'result', name: 'Ready · would have saved', figma: '5612:40971', note: 'Backtest: would have saved ₹18,400 in 3 months (green strip). The description opens Read more.', render: (go) => <RuleChat step="saved" onBack={() => go('dash-03')} onHistory={() => go('dash-14')} onReadMore={() => go('dash-10')} onRefine={() => go('dash-05')} onSave={() => go('dash-13')} /> },
    { id: 'dash-08', group: 'chat', section: 'result', name: 'Weak · would have cost profit', figma: '5612:41698', note: 'Backtest: the rule would have cut ₹22,390 of profit — red glow and strip; DASH suggests improving it.', render: (go) => <RuleChat step="cost" onBack={() => go('dash-03')} onHistory={() => go('dash-14')} onReadMore={() => go('dash-11')} onSave={() => go('dash-13')} onImprove={() => go('dash-05')} /> },
    { id: 'dash-09', group: 'chat', section: 'result', name: 'Ready · no backtest data', figma: '5612:41530', note: 'Not enough trading history to test: a neutral strip.', render: (go) => <RuleChat step="none" onBack={() => go('dash-03')} onHistory={() => go('dash-14')} onReadMore={() => go('dash-12')} onRefine={() => go('dash-05')} onSave={() => go('dash-13')} /> },
    { id: 'dash-10', group: 'chat', section: 'result', name: 'Read more · saved', figma: '5612:41133', render: (go) => <RuleChat step="saved" sheet={<ReadMoreSheet rule={maxDailyLoss} backtest={backtests.saved} onClose={() => go('dash-07')} />} /> },
    { id: 'dash-11', group: 'chat', section: 'result', name: 'Read more · cost', figma: '5612:41268', note: 'Figma says ₹22,300 here and ₹22,390 on the card — one number (₹22,390) used for both.', render: (go) => <RuleChat step="cost" sheet={<ReadMoreSheet rule={maxDailyLoss} backtest={backtests.cost} onClose={() => go('dash-08')} />} /> },
    { id: 'dash-12', group: 'chat', section: 'result', name: 'Read more · no data', figma: '5612:41402', render: (go) => <RuleChat step="none" sheet={<ReadMoreSheet rule={maxDailyLoss} backtest={backtests.none} onClose={() => go('dash-09')} />} /> },
    { id: 'dash-13', group: 'chat', section: 'result', name: 'Rule created', figma: '5612:41875', note: 'Over the ready screen it was saved from (Figma shows an older card behind it).', render: (go) => <RuleChat step="saved" sheet={<RuleCreatedSheet rule={maxDailyLoss} onCancel={() => go('dash-07')} onViewRules={() => go('dash-14')} />} /> },
    // ---- Your rules
    { id: 'dash-14', enter: 'fade', section: 'rules', name: 'Active rules', figma: '5612:42076', note: 'Switches turn a rule on / off in place. Delete asks first; Create New starts a new rule.', render: (go) => <ActiveRules rules={activeRules} onBack={() => go('dash-02')} onDelete={() => go('dash-15')} onCreate={() => go('dash-03')} /> },
    { id: 'dash-15', section: 'rules', name: 'Delete rule?', figma: '5612:42250', note: 'Over the current Active rules (Figma shows an older list with Active / Paused tabs behind it).', render: (go) => <ActiveRules rules={activeRules} sheet={<DeleteRuleSheet rule={maxDailyLossRule} onNo={() => go('dash-14')} onYes={() => go('dash-16')} />} /> },
    { id: 'dash-16', section: 'rules', name: 'Rule deleted', figma: '5612:42455', note: 'The list behind already has 3 rules.', render: (go) => <ActiveRules rules={rulesAfterDelete} sheet={<RuleDeletedSheet onOkay={() => go('dash-17')} />} /> },
    { id: 'dash-17', section: 'rules', name: 'Active rules · after delete', figma: '5612:42076', note: 'Not a Figma frame: the list once the sheet closes (3 rules).', render: (go) => <ActiveRules rules={rulesAfterDelete} onBack={() => go('dash-02')} onDelete={() => go('dash-15')} onCreate={() => go('dash-03')} /> },
    // ---- Custom indicator (wireframe "AI Indicator") — ONE chat screen in different states (group 'indicator')
    { id: 'dash-20', group: 'indicator', section: 'indicator', name: 'Indicator · what to show', figma: '', note: 'Third option on the DASH landing. Empty chat: example prompts; a chip or typed text flies up and DASH reads it.', render: (go) => <IndicatorChat step="empty" onBack={() => go('dash-02')} onSend={() => go('dash-21')} onChart={() => go('dash-22')} onTimeframe={() => go('dash-23')} onBuilt={() => go('dash-24')} onAdded={() => go('dash-25')} /> },
    { id: 'dash-21', group: 'indicator', section: 'indicator', name: 'Indicator · which chart', figma: '', note: 'DASH asks only what the prompt didn’t say: the chart…', render: (go) => <IndicatorChat step="chart" onBack={() => go('dash-02')} onSend={() => go('dash-21')} onChart={() => go('dash-22')} onTimeframe={() => go('dash-23')} onBuilt={() => go('dash-24')} onAdded={() => go('dash-25')} /> },
    { id: 'dash-22', group: 'indicator', section: 'indicator', name: 'Indicator · which timeframe', figma: '', note: '…and the timeframe.', render: (go) => <IndicatorChat step="timeframe" onBack={() => go('dash-02')} onSend={() => go('dash-21')} onChart={() => go('dash-22')} onTimeframe={() => go('dash-23')} onBuilt={() => go('dash-24')} onAdded={() => go('dash-25')} /> },
    { id: 'dash-23', group: 'indicator', section: 'indicator', name: 'Indicator · building', figma: '', note: 'The card pins and builds live: a wave, then candles with the lines drawing in, the parts as chips, three steps ticking (~3.3 s in the prototype).', render: (go) => <IndicatorChat step="building" onBack={() => go('dash-02')} onSend={() => go('dash-21')} onChart={() => go('dash-22')} onTimeframe={() => go('dash-23')} onBuilt={() => go('dash-24')} onAdded={() => go('dash-25')} /> },
    { id: 'dash-24', group: 'indicator', section: 'indicator', name: 'Indicator · ready', figma: '', note: 'BUY / SELL labels on the preview, notes from testing (fold open), Add to chart (loader → On chart). Ask for changes in plain words: the preview redraws (SELL labels turn orange).', render: (go) => <IndicatorChat step="ready" onBack={() => go('dash-02')} onSend={() => go('dash-21')} onChart={() => go('dash-22')} onTimeframe={() => go('dash-23')} onBuilt={() => go('dash-24')} onAdded={() => go('dash-25')} /> },
    { id: 'dash-25', group: 'indicator', section: 'indicator', name: 'Indicator · on chart', figma: '', note: 'Added: the tag and the button read On chart.', render: (go) => <IndicatorChat step="onChart" onBack={() => go('dash-02')} onSend={() => go('dash-21')} onChart={() => go('dash-22')} onTimeframe={() => go('dash-23')} onBuilt={() => go('dash-24')} onAdded={() => go('dash-25')} /> },
  ],
}
```

### `src/flows/dash/indicatorData.ts`

```ts
// DASH AI · Custom indicators — sample data, the questions DASH asks, and the maths the mini chart draws.
// The indicator values are really computed (EMA, Supertrend) on sample candles, so the preview looks like the real
// thing. In the app, the candles come from the chart's own data feed.
import type { RuleQuestion } from './data'

/** Example prompts on the empty indicator chat (simple, plain words — no formulas). */
export const indicatorPrompts: string[][] = [
  ['Supertrend with buy / sell labels and a 20 EMA', 'Show the trend with a 50-day average'],
  ['Mark the 15-minute opening range', 'Highlight candles with 2× average volume'],
]

/** What DASH asks before building (only what the prompt didn't say). */
export const chartQuestion: RuleQuestion = {
  question: 'Which chart should it go on?',
  options: ['Nifty 50', 'Bank Nifty', 'Any chart I open'],
  freeText: false,
}

export const timeframeQuestion: RuleQuestion = {
  question: 'Which timeframe?',
  options: ['5 min', '15 min', '1 hour', '1 day'],
  freeText: false,
}

/** The indicator DASH builds from the sample prompt. */
export type IndicatorDraft = {
  name: string
  /** What it draws, as chips: lines and labels. */
  parts: { kind: 'line' | 'label'; label: string; tone: 'trend' | 'average' | 'buy' | 'sell' }[]
  /** "On the price chart · checked on 500 candles". */
  placement: string
  testedOn: number
  /** DASH's notes from testing — things worth knowing before using it. */
  notes: string[]
}

export const supertrendEma: IndicatorDraft = {
  name: 'Supertrend (10, 3) with EMA 20',
  parts: [
    { kind: 'line', label: 'Supertrend', tone: 'trend' },
    { kind: 'line', label: 'EMA 20', tone: 'average' },
    { kind: 'label', label: 'BUY', tone: 'buy' },
    { kind: 'label', label: 'SELL', tone: 'sell' },
  ],
  placement: 'On the price chart',
  testedOn: 500,
  notes: [
    'It gave 14 buy and 13 sell signals on the last 500 candles — about one a day on 15 min.',
    'In sideways markets the trend flips often; the 20 EMA helps you spot those stretches.',
  ],
}

/** The steps DASH shows while it builds. */
export const buildSteps = ['Reading your request', 'Writing the logic', 'Testing it on 500 candles'] as const

// ---------------------------------------------------------------------------------------------------------------------
// Sample candles + indicator maths

export type Candle = { open: number; high: number; low: number; close: number }

/** 60 sample 15-min candles around Nifty's level — a deterministic random walk (same every time). */
export function sampleCandles(count = 60, start = 22650): Candle[] {
  let seed = 7
  const rand = () => ((seed = (seed * 16807) % 2147483647) / 2147483647)
  const candles: Candle[] = []
  let close = start
  for (let i = 0; i < count; i++) {
    const drift = Math.sin(i / 5) * 9 // gentle swings, so the trend flips a few times
    const open = close
    close = open + drift + (rand() - 0.5) * 28
    const high = Math.max(open, close) + rand() * 12
    const low = Math.min(open, close) - rand() * 12
    candles.push({ open, high, low, close })
  }
  return candles
}

/** Exponential moving average of the closes. */
export function ema(candles: Candle[], period: number): number[] {
  const k = 2 / (period + 1)
  const out: number[] = []
  candles.forEach((c, i) => out.push(i === 0 ? c.close : c.close * k + out[i - 1] * (1 - k)))
  return out
}

/** Supertrend (ATR period, multiplier): the line and its direction per candle; a flip is a buy / sell signal. */
export function supertrend(candles: Candle[], period = 10, multiplier = 3) {
  const tr = candles.map((c, i) => (i === 0 ? c.high - c.low : Math.max(c.high - c.low, Math.abs(c.high - candles[i - 1].close), Math.abs(c.low - candles[i - 1].close))))
  const atr: number[] = []
  tr.forEach((t, i) => atr.push(i === 0 ? t : (atr[i - 1] * (period - 1) + t) / period))
  const line: number[] = []
  const up: boolean[] = []
  let upper = 0
  let lower = 0
  candles.forEach((c, i) => {
    const mid = (c.high + c.low) / 2
    const bu = mid + multiplier * atr[i]
    const bl = mid - multiplier * atr[i]
    const prevClose = i ? candles[i - 1].close : c.close
    upper = i && (bu < upper || prevClose > upper) ? bu : i ? upper : bu
    lower = i && (bl > lower || prevClose < lower) ? bl : i ? lower : bl
    const wasUp = i ? up[i - 1] : true
    const isUp = wasUp ? c.close >= lower : c.close > upper
    up.push(isUp)
    line.push(isUp ? lower : upper)
  })
  const signals = up.flatMap((u, i) => (i > 0 && u !== up[i - 1] ? [{ index: i, side: u ? ('buy' as const) : ('sell' as const) }] : []))
  return { line, up, signals }
}
```

### `src/flows/dash/screens/ActiveRules.module.css`

```css
/* The list sits on a white panel with 24 top corners, like the chat. */
.panel {
  flex: 1;
  border-radius: var(--l3-radius-24) var(--l3-radius-24) 0 0;
  background: var(--l3-surface-default);
}

.heading {
  margin: 0;
  font: var(--l3-text-heading-14);
  color: var(--l3-content-primary);
}

.list {
  margin: 0;
  padding: 0;
  list-style: none;
}

/* Long summaries: two lines, then … (Figma "Daily trade limit"). */
.summary {
  display: -webkit-box;
  overflow: hidden;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
}

.activity {
  margin: 0;
  font: var(--l3-text-label-12);
  color: var(--l3-content-secondary);
}

.activity strong {
  font: var(--l3-text-heading-12);
  color: var(--l3-content-primary);
}

/* The card footer row spans the card: the activity on the left, Delete on the right. */
.footer {
  width: 100%;
}
```

### `src/flows/dash/screens/ActiveRules.tsx`

```tsx
import { useState, type ReactNode } from 'react'
import { Avatar } from '@l3/components/Avatar'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { Card } from '@l3/components/Card'
import { Icon } from '@l3/components/Icon'
import { Inset } from '@l3/components/Inset'
import { ListCell } from '@l3/components/ListCell'
import { Stack } from '@l3/components/Stack'
import { Switch } from '@l3/components/Switch'
import { msAdd } from '@l3/icons/material'
import { DashBar, DashScreen } from '../components/DashScreen'
import { t } from '../copy'
import type { SavedRule } from '../data'
import styles from './ActiveRules.module.css'

type ActiveRulesProps = {
  rules: SavedRule[]
  onBack?: () => void
  onDelete?: (rule: SavedRule) => void
  onCreate?: () => void
  sheet?: ReactNode
}

/**
 * Trading rules — the user's saved rules (Figma "Active rules" 5612:42076): one L3 card per rule — its initial, name,
 * what it does and an on / off switch; the footer says what the rule did lately, with Delete. Create New in the dock.
 */
export function ActiveRules({ rules, onBack, onDelete, onCreate, sheet }: ActiveRulesProps) {
  return (
    <DashScreen
      glow="header"
      top={<DashBar title={t('rules.title')} onBack={onBack} />}
      bottom={
        <ButtonGroup aria-label={t('rules.title')}>
          <Button iconLeft={<Icon icon={msAdd} />} onClick={onCreate}>{t('rules.createNew')}</Button>
        </ButtonGroup>
      }
      sheet={sheet}
    >
      <div className={styles.panel}>
        <Inset paddingInline="16" paddingBlock="24">
          <Stack as="section" gap="12" aria-labelledby="dash-active-rules">
            <h2 id="dash-active-rules" className={styles.heading}>{t('rules.active', { count: rules.length })}</h2>
            <Stack as="ul" gap="16" className={styles.list}>
              {rules.map((rule) => (
                <RuleItem key={rule.id} rule={rule} onDelete={() => onDelete?.(rule)} />
              ))}
            </Stack>
          </Stack>
        </Inset>
      </div>
    </DashScreen>
  )
}

function RuleItem({ rule, onDelete }: { rule: SavedRule; onDelete: () => void }) {
  const [on, setOn] = useState(rule.on)
  const { text, strong, after } = rule.activity
  return (
    <Card
      as="li"
      padding="none"
      footerFilled
      footer={
        <Stack direction="horizontal" align="center" justify="between" gap="12" className={styles.footer}>
          <p className={styles.activity}>
            {text}
            {strong && <strong>{strong}</strong>}
            {after}
          </p>
          <Button variant="tertiary" size="sm" onClick={onDelete} aria-label={t('rules.deleteLabel', { name: rule.title })}>
            {t('rules.delete')}
          </Button>
        </Stack>
      }
    >
      <Inset paddingInline="12">
        <ListCell
          iconLeft={<Avatar initials={rule.title.charAt(0)} size={40} />}
          label={rule.title}
          multiline
          description={<span className={styles.summary}>{rule.summary}</span>}
          trailing={<Switch checked={on} onChange={(e) => setOn(e.target.checked)} aria-label={t('rules.toggleLabel', { name: rule.title })} />}
        />
      </Inset>
    </Card>
  )
}
```

### `src/flows/dash/screens/IndicatorChat.module.css`

```css
/* Under the input once the indicator is ready: how to ask for changes (wireframe). */
.hint {
  margin: var(--l3-spacing-08) 0 0;
  color: var(--l3-content-tertiary);
  font: var(--l3-text-label-12);
}
```

### `src/flows/dash/screens/IndicatorChat.tsx`

```tsx
import { useEffect, useLayoutEffect, useRef, useState, type ReactNode } from 'react'
import { ActionbarAction } from '@l3/components/Actionbar'
import { Inset } from '@l3/components/Inset'
import { Stack } from '@l3/components/Stack'
import { msHistory } from '@l3/icons/material'
import { usePreview } from '../../../shared/prototype'
import { AssistantMessage, ChatInput, Thinking, UserMessage } from '../components/Chat'
import { DashBar, DashScreen } from '../components/DashScreen'
import { captureOrigin, flyMessage, type FlyOrigin } from '../components/flyMessage'
import { IndicatorCard, type IndicatorCardState } from '../components/IndicatorCard'
import { QuestionCard } from '../components/QuestionCard'
import { RulesIntro } from '../components/RulesIntro'
import { t } from '../copy'
import { chartQuestion, indicatorPrompts, supertrendEma, timeframeQuestion } from '../indicatorData'
// The same chat layout and motion as Trading rules (panel, dock, entrances, the pinned card's opening).
import styles from './RuleChat.module.css'
import own from './IndicatorChat.module.css'

/** Where the indicator chat is — ONE screen, these are its states. */
export type IndicatorStep = 'empty' | 'chart' | 'timeframe' | 'building' | 'ready' | 'onChart'

type IndicatorChatProps = {
  step: IndicatorStep
  request?: string
  onBack?: () => void
  onHistory?: () => void
  /** 'empty': a prompt sent — DASH asks which chart. */
  onSend?: (message: string) => void
  onChart?: (chart: string) => void
  onTimeframe?: (timeframe: string) => void
  /** 'building' finishes by itself (prototype): the indicator is ready. */
  onBuilt?: () => void
  /** Add to chart → (loader) → on the chart. */
  onAdded?: () => void
}

const THINKING_MS = 1200
const FLY_MS = 450
const INTRO_OUT_MS = 300
/** Each build step (Reading → Writing the logic → Testing). */
const BUILD_STEP_MS = 1100
const ADDING_MS = 1200

/**
 * DASH AI · Custom indicator — one chat screen, like Trading rules (wireframe "AI Indicator"):
 * empty state with example prompts → the prompt flies up → DASH asks which chart and which timeframe (question cards)
 * → the indicator card pins at the top and builds itself live (wave → candles → lines drawing in, steps ticking) →
 * Ready (BUY / SELL labels, notes, Add to chart) → On chart. Changes are asked for in plain words: "make the SELL
 * labels orange" redraws the preview in place.
 */
export function IndicatorChat({ step, request: initialRequest = indicatorPrompts[0][0], onBack, onHistory, onSend, onChart, onTimeframe, onBuilt, onAdded }: IndicatorChatProps) {
  const { interactive } = usePreview()
  const [request, setRequest] = useState(initialRequest)
  const [chart, setChart] = useState(chartQuestion.options[0])
  const [timeframe, setTimeframe] = useState(timeframeQuestion.options[1])
  const [pending, setPending] = useState<(() => void) | null>(null)
  const [buildStep, setBuildStep] = useState<0 | 1 | 2>(0)
  const [adding, setAdding] = useState(false)
  /** Plain-words changes asked on the ready card (prototype: SELL labels turn orange). */
  const [changes, setChanges] = useState<string[]>([])
  const [changing, setChanging] = useState(false)
  const [flying, setFlying] = useState<FlyOrigin | null>(null)
  const [introLeaving, setIntroLeaving] = useState(false)
  const stage = useRef<HTMLDivElement>(null)

  const send = (text: string, from: HTMLElement) => {
    setFlying(captureOrigin(from, text))
    setRequest(text)
    setIntroLeaving(true)
    window.setTimeout(() => setIntroLeaving(false), INTRO_OUT_MS)
    setPending(() => () => onSend?.(text)) // DASH reads the prompt, then asks
  }

  useLayoutEffect(() => {
    if (!flying) return
    const target = stage.current?.querySelector<HTMLElement>('[data-turn="request"] p')
    const screen = stage.current?.closest<HTMLElement>('[data-glow]')
    if (target && screen) flyMessage(flying, target, screen, FLY_MS)
    setFlying(null)
  }, [flying])

  // DASH thinking between turns.
  useEffect(() => {
    if (!pending) return
    const timer = window.setTimeout(() => {
      setPending(null)
      pending()
    }, THINKING_MS)
    return () => window.clearTimeout(timer)
  }, [pending])
  useEffect(() => setPending(null), [step])

  // Building: the steps tick through, then it's ready.
  useEffect(() => {
    if (step !== 'building') return
    setBuildStep(0)
    if (!interactive) return
    const timers = [window.setTimeout(() => setBuildStep(1), BUILD_STEP_MS), window.setTimeout(() => setBuildStep(2), BUILD_STEP_MS * 2), window.setTimeout(() => onBuilt?.(), BUILD_STEP_MS * 3)]
    return () => timers.forEach(window.clearTimeout)
  }, [step, interactive, onBuilt])

  const add = () => {
    setAdding(true)
    window.setTimeout(() => {
      setAdding(false)
      onAdded?.()
    }, ADDING_MS)
  }

  const askChange = (text: string) => {
    setChanges((c) => [...c, text])
    setChanging(true)
    window.setTimeout(() => setChanging(false), THINKING_MS)
  }

  // The empty state shows before sending; while DASH reads the prompt it's already the chat.
  const empty = step === 'empty' && !pending
  const chatStarted = step !== 'empty' || pending != null
  const pinned = step === 'building' || step === 'ready' || step === 'onChart'
  const cardState: IndicatorCardState =
    step === 'building' ? { kind: 'building', step: buildStep } : step === 'onChart' ? { kind: 'onChart' } : adding ? { kind: 'adding' } : { kind: 'ready' }
  const asking = !pending && (step === 'chart' || step === 'timeframe')
  const sellTone = changes.length ? 'orange' : 'sell'

  useLayoutEffect(() => {
    const body = stage.current?.parentElement
    if (body) body.scrollTo({ top: body.scrollHeight, behavior: 'smooth' })
  }, [step, pending, changes.length, changing])

  const turns: { key: string; node: ReactNode }[] = []
  if (chatStarted) turns.push({ key: 'request', node: <UserMessage>{request}</UserMessage> })
  if (step !== 'empty') turns.push({ key: 'got-it', node: <AssistantMessage title={t('indicator.gotIt')}>{t('indicator.fewQuestions')}</AssistantMessage> })
  const past = ['timeframe', 'building', 'ready', 'onChart'].includes(step) || (step === 'chart' && pending)
  if (past) {
    turns.push({ key: 'q-chart', node: <AssistantMessage>{chartQuestion.question}</AssistantMessage> })
    turns.push({ key: 'a-chart', node: <UserMessage>{chart}</UserMessage> })
  }
  if (['building', 'ready', 'onChart'].includes(step) || (step === 'timeframe' && pending)) {
    turns.push({ key: 'q-timeframe', node: <AssistantMessage>{timeframeQuestion.question}</AssistantMessage> })
    turns.push({ key: 'a-timeframe', node: <UserMessage>{timeframe}</UserMessage> })
  }
  if (step === 'building') turns.push({ key: 'building', node: <AssistantMessage>{t('indicator.buildingNow', { chart, timeframe })}</AssistantMessage> })
  if (step === 'ready' || step === 'onChart') turns.push({ key: 'ready', node: <AssistantMessage title={t('indicator.readyTitle')}>{t('indicator.readyText')}</AssistantMessage> })
  changes.forEach((c, i) => {
    turns.push({ key: `change-${i}`, node: <UserMessage>{c}</UserMessage> })
    if (!changing || i < changes.length - 1) turns.push({ key: `changed-${i}`, node: <AssistantMessage>{t('indicator.changed')}</AssistantMessage> })
  })
  if (step === 'onChart') turns.push({ key: 'on-chart', node: <AssistantMessage>{t('indicator.onChartText', { chart })}</AssistantMessage> })

  const question = step === 'chart' ? chartQuestion : timeframeQuestion

  return (
    <DashScreen
      glow={chatStarted ? 'header' : 'bottom'}
      top={
        <DashBar
          title={chatStarted ? <span className={styles.titleIn}>{t('indicator.title')}</span> : undefined}
          onBack={onBack}
          actions={<ActionbarAction icon={msHistory} label={t('indicator.history')} onClick={() => onHistory?.()} />}
          bottom={
            pinned ? (
              <div className={styles.pin}>
                <div className={styles.pinInner}>
                  <Inset paddingInline="16" paddingBlock="08">
                    <IndicatorCard draft={supertrendEma} state={cardState} sellTone={sellTone} onAdd={add} />
                  </Inset>
                </div>
              </div>
            ) : undefined
          }
        />
      }
      bottom={
        <div className={styles.dock} data-surface="primary">
          <Inset paddingInline="16" paddingBlock="16">
            {asking ? (
              <div className={styles.rise} key={step}>
                <QuestionCard
                  question={question}
                  onAnswer={(a) => {
                    if (step === 'chart') {
                      setChart(a)
                      setPending(() => () => onChart?.(a))
                    } else {
                      setTimeframe(a)
                      setPending(() => () => onTimeframe?.(a))
                    }
                  }}
                />
              </div>
            ) : (
              <ChatInput key="input" onSend={empty ? send : step === 'ready' || step === 'onChart' ? askChange : () => {}} />
            )}
            {(step === 'ready' || step === 'onChart') && <p className={own.hint}>{t('indicator.changeHint')}</p>}
          </Inset>
        </div>
      }
    >
      <div className={styles.stage} ref={stage}>
        {(empty || introLeaving) && (
          <div className={styles.intro} data-leaving={(chatStarted && introLeaving) || undefined}>
            <RulesIntro onPick={empty ? send : undefined} title={t('indicator.emptyTitle')} description={t('indicator.emptyDescription')} examples={indicatorPrompts} />
          </div>
        )}
        {chatStarted && (
          <div className={styles.panel}>
            <Inset paddingInline="16" paddingBlock="24">
              <Stack gap="24">
                {turns.map((turn) => (
                  <div key={turn.key} className={turn.key === 'request' ? undefined : styles.appear} data-turn={turn.key}>
                    {turn.node}
                  </div>
                ))}
                {(pending || changing) && (
                  <div className={step === 'empty' ? styles.appearAfterSend : styles.appear} key="thinking">
                    <Thinking>{t('chat.thinking')}</Thinking>
                  </div>
                )}
              </Stack>
            </Inset>
          </div>
        )}
      </div>
    </DashScreen>
  )
}
```

### `src/flows/dash/screens/Landing.module.css`

```css
/* Figma: the orb's top 64 below the bar; heading 20 / 28 centred. */
.hero {
  padding-top: var(--l3-spacing-48);
  text-align: center;
}

.greeting {
  margin: 0;
  font: var(--l3-text-label-14);
  color: var(--l3-content-secondary);
}

.title {
  margin: 0;
  font: var(--l3-text-heading-20);
  color: var(--l3-content-primary);
  text-wrap: balance;
}
```

### `src/flows/dash/screens/Landing.tsx`

```tsx
import { ActionbarAction } from '@l3/components/Actionbar'
import { Inset } from '@l3/components/Inset'
import { Stack } from '@l3/components/Stack'
import { msFunction, msMenu, msMonitoring, msPostAdd } from '@l3/icons/material'
import { BuildOption } from '../components/BuildOption'
import { SparkleHeading } from '../components/Chat'
import { DashBar, DashScreen } from '../components/DashScreen'
import { DashOrb } from '../../../shared/ui/dash/DashOrb'
import { t } from '../copy'
import { firstName } from '../data'
import styles from './Landing.module.css'

type LandingProps = {
  onBack?: () => void
  onRules?: () => void
  onStrategy?: () => void
  onIndicator?: () => void
  onMenu?: () => void
}

/**
 * DASH's home (Figma "Landing screen" 5612:81961): the orb, a greeting, and what DASH can build — trading rules or a
 * custom strategy. ☰ opens the user's rules.
 */
export function Landing({ onBack, onRules, onStrategy, onIndicator, onMenu }: LandingProps) {
  return (
    <DashScreen glow="landing" top={<DashBar onBack={onBack} actions={<ActionbarAction icon={msMenu} label={t('landing.menu')} onClick={() => onMenu?.()} />} />}>
      <Inset paddingInline="16" paddingBlock="24">
        <Stack gap="64" align="stretch">
          <Stack gap="24" align="center" className={styles.hero}>
            <DashOrb size="lg" />
            <Stack gap="12" align="center">
              <p className={styles.greeting}>{t('landing.greeting', { name: firstName })}</p>
              <h1 className={styles.title}>{t('landing.title')}</h1>
            </Stack>
          </Stack>
          <Stack as="section" gap="24">
            <SparkleHeading>{t('landing.choose')}</SparkleHeading>
            <Stack gap="16">
              <BuildOption icon={msPostAdd} title={t('landing.rulesTitle')} description={t('landing.rulesDescription')} onClick={onRules} />
              <BuildOption icon={msMonitoring} title={t('landing.indicatorTitle')} description={t('landing.indicatorDescription')} onClick={onIndicator} />
              <BuildOption icon={msFunction} title={t('landing.strategyTitle')} description={t('landing.strategyDescription')} onClick={onStrategy} />
            </Stack>
          </Stack>
        </Stack>
      </Inset>
    </DashScreen>
  )
}
```

### `src/flows/dash/screens/RuleChat.module.css`

```css
/*
 * The chat: a panel over the glow, 24 top corners (Figma 5612:40695), filling the space down to the dock.
 * Motion measured on the live app's recording (2026-10-23, 360 dp): every entrance decelerates (L3 easing).
 */
/* One screen, two layers: the intro (empty state) and the chat panel. The stage fills the body; while the chat starts
   the intro sits on top of the panel, fading up and away. */
.stage {
  position: relative;
  display: flex;
  flex: 1;
  flex-direction: column;
}

.intro {
  display: flex;
  flex: 1;
  flex-direction: column;
  animation: dash-appear var(--l3-motion-duration-medium) var(--l3-motion-easing-decelerate) both;
}

/* Sent: the greeting, orb and examples lift 24 and fade (300 ms) while the message flies up past them. */
.intro[data-leaving] {
  position: absolute;
  inset: 0; /* same height as before, so nothing inside moves as it fades */
  z-index: 1;
  pointer-events: none;
  animation: dash-intro-out var(--app-dash-intro-out) var(--l3-motion-easing-accelerate) both;
}

@keyframes dash-intro-out {
  to {
    opacity: 0;
    transform: translateY(calc(-1 * var(--l3-spacing-24)));
  }
}

/* The title arrives with the conversation. */
.titleIn {
  animation: dash-appear var(--l3-motion-duration-medium) var(--l3-motion-easing-decelerate) both;
}

.panel {
  flex: 1;
  border-radius: var(--l3-radius-24) var(--l3-radius-24) 0 0;
  background: var(--l3-surface-default);
  animation: dash-panel-in var(--app-dash-glow-move) var(--l3-motion-easing-decelerate) both;
}

/* The panel's surface fades in as the glow moves up behind the header (same timing). */
@keyframes dash-panel-in {
  from {
    background-color: transparent;
  }
}

/* The input / question dock continues the panel. */
.dock {
  background: var(--l3-surface-default);
}

/* A new turn: fades up 8 into place (250 ms). Turns already on the page keep their key, so they don't replay it. */
.appear {
  animation: dash-appear var(--l3-motion-duration-medium) var(--l3-motion-easing-decelerate) both;
}

.appearAfterSend {
  animation: dash-appear var(--l3-motion-duration-medium) var(--l3-motion-easing-decelerate) var(--app-dash-fly) both;
}

@keyframes dash-appear {
  from {
    opacity: 0;
    transform: translateY(var(--l3-spacing-08));
  }
}

/* DASH's question card rises out of the input's place: 24 up and in, 250 ms (live: 17.9 → 18.15 s). */
.rise {
  animation: dash-rise var(--l3-motion-duration-medium) var(--l3-motion-easing-decelerate) both;
}

@keyframes dash-rise {
  from {
    opacity: 0;
    transform: translateY(var(--l3-spacing-24));
  }
}

/* Answered: the input drops back from where the card was (live: 150 ms down from the card's top). */
.inputBack {
  animation: dash-input-back var(--l3-motion-duration-short) var(--l3-motion-easing-decelerate) both;
}

@keyframes dash-input-back {
  from {
    opacity: 0;
    transform: translateY(calc(-1 * var(--app-dash-input-return)));
  }
}

/* The pinned rule card opens in the top bar's bottom slot and pushes the chat down (live: 300 ms, 22.6 → 22.9 s). */
.pin {
  display: grid;
  grid-template-rows: 1fr;
  animation: dash-pin var(--app-dash-pin-duration) var(--l3-motion-easing-decelerate) both;
}

.pinInner {
  min-height: 0;
  overflow: hidden;
}

@keyframes dash-pin {
  from {
    grid-template-rows: 0fr;
    opacity: 0;
  }
}

@media (prefers-reduced-motion: reduce) {
  .appear,
  .appearAfterSend,
  .intro,
  .intro[data-leaving],
  .titleIn,
  .panel,
  .rise,
  .inputBack,
  .pin {
    animation: none;
  }
}
```

### `src/flows/dash/screens/RuleChat.tsx`

```tsx
import { useEffect, useLayoutEffect, useRef, useState, type ReactNode } from 'react'
import { ActionbarAction } from '@l3/components/Actionbar'
import { Inset } from '@l3/components/Inset'
import { Stack } from '@l3/components/Stack'
import { msHistory } from '@l3/icons/material'
import { usePreview } from '../../../shared/prototype'
import { AssistantMessage, ChatInput, ReplyChips, Thinking, UserMessage } from '../components/Chat'
import { DashBar, DashScreen } from '../components/DashScreen'
import { captureOrigin, flyMessage, type FlyOrigin } from '../components/flyMessage'
import { QuestionCard } from '../components/QuestionCard'
import { RuleCard } from '../components/RuleCard'
import { RulesIntro } from '../components/RulesIntro'
import { t } from '../copy'
import { backtests, dailyLossQuestion, maxDailyLoss, type Backtest } from '../data'
import styles from './RuleChat.module.css'

/** Where the conversation is. */
export type RuleChatStep =
  | 'empty' //      nothing sent yet: greeting, orb, example prompts
  | 'thinking' //   DASH reads the request
  | 'question' //   DASH asks for the limit
  | 'preview' //    the rule, before testing
  | 'saved' //      ready: the backtest says it would have saved money
  | 'cost' //       weak: it would have cut trades that ended in profit
  | 'none' //       ready, but there's not enough history to test it

type RuleChatProps = {
  step: RuleChatStep
  /** What the user asked for (the chip they tapped or what they typed). */
  request?: string
  onBack?: () => void
  onHistory?: () => void
  /** 'empty': a chip tapped or a message typed — the chat starts (on the same screen). */
  onSend?: (message: string) => void
  /** 'thinking' moves on by itself after a moment (prototype). */
  onThought?: () => void
  onAnswer?: (answer: string) => void
  onRefine?: () => void
  onProceed?: () => void
  onSave?: () => void
  onImprove?: () => void
  /** The rule card's description: opens the read-more sheet. */
  onReadMore?: () => void
  sheet?: ReactNode
}

/** How long DASH "thinks" before replying (prototype; the live app takes 1.5–3 s). */
const THINKING_MS = 1600
/** The sent message flying from the chip / input up to its place in the chat. */
const FLY_MS = 450
/** The intro (greeting, orb, examples) fading out as the chat starts. */
const INTRO_OUT_MS = 300

/**
 * Trading rules — ONE chat screen, like ChatGPT or Claude: from the empty state (Figma "Rule - Landing screen") through
 * the conversation (Figma "Creating the rule" ×3, "Rule created" ×3). Every step is the same page (the flow's
 * `group: 'chat'`), so moving on changes the screen's state in place:
 * - sending: the tapped chip (or the typed text) flies up and becomes the user's message; the greeting, orb and
 *   examples fade up and away; the chat panel fades in; the glow glides from the bottom to the header; the title fades in;
 * - the conversation builds up — every message stays, the new one fades up into place, the panel scrolls to it;
 * - after an answer (or Proceed) DASH thinks first: the three dots, then the reply;
 * - DASH's question card rises out of the input's place; once answered the input comes back;
 * - when DASH has a draft, the rule card opens in the top bar's bottom slot and pushes the chat panel down;
 *   the backtest strip then unfolds under the card.
 */
export function RuleChat({ step, request: initialRequest = maxDailyLoss.title, onBack, onHistory, onSend, onThought, onAnswer, onRefine, onProceed, onSave, onImprove, onReadMore, sheet }: RuleChatProps) {
  const { interactive } = usePreview()
  /** After an answer or Proceed: DASH thinks on this page before the next step comes (prototype). */
  const [pending, setPending] = useState<(() => void) | null>(null)
  const [answer, setAnswer] = useState(dailyLossQuestion.options[1])
  /** What the user sent — the chip's text or what they typed (kept as the screen moves on). */
  const [request, setRequest] = useState(initialRequest)
  /** Set while the sent message is on its way up; the intro stays on top, fading, until it's gone. */
  const [flying, setFlying] = useState<FlyOrigin | null>(null)
  const [introLeaving, setIntroLeaving] = useState(false)
  const stage = useRef<HTMLDivElement>(null)

  const send = (text: string, from: HTMLElement) => {
    setFlying(captureOrigin(from, text))
    setRequest(text)
    setIntroLeaving(true)
    window.setTimeout(() => setIntroLeaving(false), INTRO_OUT_MS)
    onSend?.(text)
  }

  // The chat has started: fly the message from where it was sent to its place (measured once both exist).
  useLayoutEffect(() => {
    if (!flying || step === 'empty') return
    const target = stage.current?.querySelector<HTMLElement>('[data-turn="request"] p')
    const screen = stage.current?.closest<HTMLElement>('[data-glow]')
    if (target && screen) flyMessage(flying, target, screen, FLY_MS)
    setFlying(null)
  }, [flying, step])

  useEffect(() => {
    if (step !== 'thinking' || !interactive || !onThought) return
    const timer = window.setTimeout(onThought, THINKING_MS)
    return () => window.clearTimeout(timer)
  }, [step, interactive, onThought])

  useEffect(() => {
    if (!pending) return
    const timer = window.setTimeout(() => {
      setPending(null)
      pending()
    }, THINKING_MS)
    return () => window.clearTimeout(timer)
  }, [pending])
  // A new step has arrived: DASH has replied.
  useEffect(() => setPending(null), [step])

  const thinkThen = (next?: () => void) => () => next && setPending(() => next)

  const backtest: Backtest | undefined = step === 'saved' || step === 'cost' || step === 'none' ? backtests[step] : undefined
  const empty = step === 'empty'
  const pinned = !empty && step !== 'thinking' && step !== 'question'
  const thinking = step === 'thinking' || pending != null
  const asking = step === 'question' && !pending

  // Keep the newest message in view as the conversation grows.
  useLayoutEffect(() => {
    const body = stage.current?.parentElement // the screen's scrolling body
    if (body) body.scrollTo({ top: body.scrollHeight, behavior: 'smooth' })
  }, [step, pending])

  return (
    <DashScreen
      glow={empty ? 'bottom' : 'header'}
      tone={step === 'cost' ? 'loss' : 'brand'}
      top={
        <DashBar
          title={empty ? undefined : <span className={styles.titleIn}>{t('chat.title')}</span>}
          onBack={onBack}
          actions={<ActionbarAction icon={msHistory} label={t('rulesHome.history')} onClick={() => onHistory?.()} />}
          bottom={
            pinned ? (
              <div className={styles.pin}>
                <div className={styles.pinInner}>
                  <Inset paddingInline="16" paddingBlock="08">
                    <RuleCard rule={maxDailyLoss} backtest={backtest} onOpen={onReadMore} />
                  </Inset>
                </div>
              </div>
            ) : undefined
          }
        />
      }
      bottom={
        <div className={styles.dock} data-surface="primary">
          <Inset paddingInline="16" paddingBlock="16">
            {asking ? (
              <div className={styles.rise} key="question">
                <QuestionCard
                  question={dailyLossQuestion}
                  onAnswer={(a) => {
                    setAnswer(a)
                    thinkThen(() => onAnswer?.(a))()
                  }}
                />
              </div>
            ) : (
              <div className={step === 'question' || step === 'preview' ? styles.inputBack : undefined} key="input">
                <ChatInput onSend={empty ? send : () => {}} />
              </div>
            )}
          </Inset>
        </div>
      }
      sheet={sheet}
    >
      <div className={styles.stage} ref={stage}>
        {(empty || introLeaving) && (
          <div className={styles.intro} data-leaving={(!empty && introLeaving) || undefined}>
            <RulesIntro onPick={empty ? send : undefined} />
          </div>
        )}
        {!empty && (
          <div className={styles.panel}>
            <Inset paddingInline="16" paddingBlock="24">
              <Stack gap="24">
                {conversation(step, request, step === 'question' && pending ? answer : step !== 'thinking' && step !== 'question' ? answer : null, {
                  onRefine,
                  onProceed: thinkThen(onProceed),
                  onSave,
                  onImprove,
                  busy: pending != null,
                })}
                {thinking && (
                  // Right after sending, the dots wait for the message to land (live app).
              <div className={step === 'thinking' ? styles.appearAfterSend : styles.appear} key="thinking">
                    <Thinking>{t('chat.thinking')}</Thinking>
                  </div>
                )}
              </Stack>
            </Inset>
          </div>
        )}
      </div>
    </DashScreen>
  )
}

type Replies = Pick<RuleChatProps, 'onRefine' | 'onSave' | 'onImprove'> & { onProceed: () => void; busy: boolean }

/**
 * The conversation so far — every turn stays, like the live app. Each turn has a stable key, so when the page moves on
 * only the new turns play their entrance (fade up).
 */
function conversation(step: RuleChatStep, request: string, answered: string | null, on: Replies) {
  const turns: { key: string; node: ReactNode }[] = [{ key: 'request', node: <UserMessage>{request}</UserMessage> }]
  if (step === 'empty') return []
  if (step === 'thinking') return render(turns)
  turns.push({ key: 'got-it', node: <AssistantMessage title={t('chat.gotIt')}>{t('chat.needDetails')}</AssistantMessage> })
  if (answered) {
    turns.push({ key: 'asked', node: <AssistantMessage>{dailyLossQuestion.question}</AssistantMessage> })
    turns.push({ key: 'answer', node: <UserMessage>{answered}</UserMessage> })
  }
  if (step === 'question') return render(turns)

  const tested = step === 'saved' || step === 'cost' || step === 'none'
  turns.push({
    key: 'preview',
    node: (
      <Stack gap="16">
        <AssistantMessage title={t('chat.previewTitle')}>{maxDailyLoss.description}</AssistantMessage>
        {!tested && !on.busy && (
          <ReplyChips options={[{ label: t('chat.refine'), onClick: on.onRefine }, { label: t('chat.proceed'), onClick: on.onProceed, suggested: true }]} />
        )}
      </Stack>
    ),
  })
  if (step === 'cost') {
    turns.push({
      key: 'weak',
      node: (
        <Stack gap="16">
          <AssistantMessage title={t('chat.weakTitle')}>{t('chat.weakText')}</AssistantMessage>
          <ReplyChips options={[{ label: t('chat.saveAnyway'), onClick: on.onSave }, { label: t('chat.improve'), onClick: on.onImprove, suggested: true }]} />
        </Stack>
      ),
    })
  } else if (tested) {
    turns.push({
      key: 'ready',
      node: (
        <Stack gap="16">
          <AssistantMessage title={t('chat.readyTitle')}>{t('chat.readyText')}</AssistantMessage>
          <ReplyChips options={[{ label: t('chat.refine'), onClick: on.onRefine }, { label: t('chat.save'), onClick: on.onSave, suggested: true }]} />
        </Stack>
      ),
    })
  }
  return render(turns)
}

const render = (turns: { key: string; node: ReactNode }[]) =>
  turns.map((turn) => (
    // The user's message flies into place (flyMessage) instead of fading up.
    <div key={turn.key} className={turn.key === 'request' ? undefined : styles.appear} data-turn={turn.key}>
      {turn.node}
    </div>
  ))
```

### `src/flows/dash/sheets/RuleSheets.module.css`

```css
/* DASH sheets: L3 bottom sheets; the read-more and saved sheets carry DASH's tint at the top. */
/* The L3 sheet content already brings 16 / 16 / 24; these pages add 8 on top (Figma: 24 above the art). */
.content {
  padding-top: var(--l3-spacing-08);
}

.centre {
  text-align: center;
}

.roomy {
  padding-block: var(--l3-spacing-48) var(--l3-spacing-08);
}

.title {
  margin: 0;
  font: var(--l3-text-heading-20);
  color: var(--l3-content-primary);
}

.description {
  margin: 0;
  font: var(--l3-text-label-14);
  color: var(--l3-content-tertiary);
  text-wrap: balance;
}

/* Read more: a top-down tint in the result's light accent (Figma: green for saved, pink for cost, none for no data). */
.tinted {
  --tint: transparent;
  /* edge to edge: out of the sheet's 16 content padding, then the same 16 back inside */
  margin: calc(-1 * var(--l3-spacing-16)) calc(-1 * var(--l3-spacing-16)) 0;
  padding: var(--l3-spacing-16) var(--l3-spacing-16) 0;
  background: linear-gradient(to bottom, var(--tint), var(--l3-surface-default) 45%);
}

.tinted[data-result='saved'] { --tint: var(--l3-surface-accent-success-light); }
.tinted[data-result='cost'] { --tint: var(--l3-surface-accent-error-light); }

/* The orb on a soft white disc with a thin DASH edge (Figma: 88, 1px lime at 40%). */
.orbDisc {
  display: grid;
  place-items: center;
  width: var(--l3-size-80);
  height: var(--l3-size-80);
  border: 1px solid color-mix(in srgb, var(--app-dash-green) 40%, transparent);
  border-radius: var(--l3-radius-full);
  background: radial-gradient(circle, var(--l3-surface-default) 55%, color-mix(in srgb, var(--app-dash-glow-1) 15%, var(--l3-surface-default)));
}
```

### `src/flows/dash/sheets/RuleSheets.tsx`

```tsx
import { useId, useState } from 'react'
import { BottomSheetHeader } from '@l3/components/BottomSheet'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { Stack } from '@l3/components/Stack'
import { Sheet } from '../../../shared/ui/Sheet'
import { DashOrb } from '../../../shared/ui/dash/DashOrb'
import { BacktestStrip } from '../components/RuleCard'
import { SuccessBurst } from '../components/SuccessBurst'
import { RuleSummary } from '../components/RuleSummary'
import { t } from '../copy'
import type { Backtest, DraftRule } from '../data'
import styles from './RuleSheets.module.css'

/**
 * Read more — the rule in full (Figma "Clicking on read more" ×3): the orb on a soft disc over a tint of the result,
 * the rule's name and sentence, and what the backtest found (nothing when there's no data).
 */
export function ReadMoreSheet({ rule, backtest, onClose }: { rule: DraftRule; backtest: Backtest; onClose?: () => void }) {
  const id = useId()
  return (
    <Sheet
      labelledBy={id}
      onClose={onClose}
      footer={
        <ButtonGroup aria-label={rule.title}>
          <Button onClick={onClose}>{t('readMore.okay')}</Button>
        </ButtonGroup>
      }
    >
      <div className={styles.tinted} data-result={backtest.result}>
        <Stack gap="24" align="stretch" className={styles.content}>
          <Stack gap="16" align="center" className={styles.centre}>
            <span className={styles.orbDisc}>
              <DashOrb size="md" />
            </span>
            <Stack gap="08" align="center">
              <h2 id={id} className={styles.title}>{rule.title}</h2>
              <p className={styles.description}>{rule.description}</p>
            </Stack>
          </Stack>
          {backtest.result !== 'none' && <BacktestStrip backtest={backtest} shape="pill" />}
        </Stack>
      </div>
    </Sheet>
  )
}

/** How long the tick's coin-turn runs before View Rules moves on (= motion/duration-short). */
const TICK_FLIP_MS = 150

/** Saved (Figma "Rule saved" 5612:41875): the tick pops in with confetti (live app motion), what happens now, the rule, Cancel / View Rules. */
export function RuleCreatedSheet({ rule, onCancel, onViewRules }: { rule: DraftRule; onCancel?: () => void; onViewRules?: () => void }) {
  const id = useId()
  // View Rules: the tick turns away first (150 ms), then the rules list fades in (live app).
  const [leaving, setLeaving] = useState(false)
  const viewRules = () => {
    setLeaving(true)
    window.setTimeout(() => onViewRules?.(), TICK_FLIP_MS)
  }
  return (
    <Sheet
      labelledBy={id}
      onClose={onCancel}
      footer={
        <ButtonGroup aria-label={t('created.title')} direction="horizontal">
          <Button variant="secondary" onClick={onCancel}>{t('created.cancel')}</Button>
          <Button onClick={viewRules}>{t('created.viewRules')}</Button>
        </ButtonGroup>
      }
    >
      <div>
        <Stack gap="24" align="stretch" className={styles.content}>
          <Stack gap="16" align="center" className={styles.centre}>
            <SuccessBurst leaving={leaving} />
            <Stack gap="08" align="center">
              <h2 id={id} className={styles.title}>{t('created.title')}</h2>
              <p className={styles.description}>{t('created.description')}</p>
            </Stack>
          </Stack>
          <RuleSummary title={rule.title} description={rule.description} />
        </Stack>
      </div>
    </Sheet>
  )
}

/** Delete? (Figma "Delete rule" 5612:42250): the question as the header, the rule, No / Yes, Delete. */
export function DeleteRuleSheet({ rule, onNo, onYes }: { rule: { title: string; summary: string }; onNo?: () => void; onYes?: () => void }) {
  const id = useId()
  return (
    <Sheet
      labelledBy={id}
      onClose={onNo}
      header={<BottomSheetHeader headingId={id} heading={t('delete.title')} />}
      footer={
        <ButtonGroup aria-label={t('delete.title')} direction="horizontal">
          <Button variant="secondary" onClick={onNo}>{t('delete.no')}</Button>
          <Button onClick={onYes}>{t('delete.yes')}</Button>
        </ButtonGroup>
      }
    >
      <div className={styles.content}>
        <RuleSummary title={rule.title} description={rule.summary} />
      </div>
    </Sheet>
  )
}

/** Deleted (Figma "Rule deleted" 5612:42455). */
export function RuleDeletedSheet({ onOkay }: { onOkay?: () => void }) {
  const id = useId()
  return (
    <Sheet
      labelledBy={id}
      onClose={onOkay}
      footer={
        <ButtonGroup aria-label={t('deleted.title')}>
          <Button onClick={onOkay}>{t('deleted.okay')}</Button>
        </ButtonGroup>
      }
    >
      <Stack gap="08" align="center" className={[styles.content, styles.centre, styles.roomy].join(' ')}>
        <h2 id={id} className={styles.title}>{t('deleted.title')}</h2>
        <p className={styles.description}>{t('deleted.description')}</p>
      </Stack>
    </Sheet>
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

### `src/shared/data/user.ts`

```ts
// The sample user every flow uses. FICTIONAL — the live screenshots contain a real customer's details; never copy them.
export type User = {
  name: string
  fatherName: string
  mobile: string
  email: string
  pan: string
  dob: string
  occupation: string
  maritalStatus: string
}

export const user: User = {
  name: 'Rohan Mehta',
  fatherName: 'Suresh Mehta',
  mobile: '9876543210',
  email: 'rohan.mehta@gmail.com',
  pan: 'ABCPM1234K',
  dob: '14 Aug 1996',
  occupation: 'Private sector',
  maritalStatus: 'Single',
}

export type BankAccount = { bank: string; accountNumber: string; ifsc: string; type: 'Savings' | 'Current'; branch: string }

export const bankAccount: BankAccount = {
  bank: 'HDFC Bank',
  accountNumber: '50100234567821',
  ifsc: 'HDFC0001234',
  type: 'Savings',
  branch: 'Andheri West, Mumbai',
}
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

### `src/shared/ui/dash/DashOrb.module.css`

```css
/*
 * The orb artwork is the Figma vector component: every part keeps the exact Figma box (px inside a 100 × 100 frame —
 * artwork geometry, not layout), and the whole thing scales to the slot. Sizes come from L3 size tokens.
 */
.orb {
  --orb-box: var(--l3-size-96);
  position: relative;
  display: inline-block;
  flex: none;
  width: var(--orb-box);
  height: var(--orb-box);
}

.orb[data-size='lg'] { --orb-box: var(--l3-size-96); }
.orb[data-size='md'] { --orb-box: var(--l3-size-64); }
.orb[data-size='sm'] { --orb-box: var(--l3-size-32); }

/* The art is drawn at 100 and scaled to the box. */
.scale {
  position: absolute;
  top: 0;
  left: 0;
  width: 100px;
  height: 100px;
  transform: scale(calc(var(--orb-box) / 100px));
  transform-origin: 0 0;
}

.art {
  position: absolute;
  inset: 0;
}

/* Figma: the halo copy sits 8 right / 10 down, layer blur 23.35. */
.halo {
  transform: translate(8px, 10px);
  filter: blur(var(--app-dash-orb-halo-blur));
}

.art img {
  display: block;
  max-width: none;
}

/* "Group 1000006113" (back leaf, blurred glow): box 55.5 × 57.8, bottom 49.7, centre +17.2. */
.leafBackBox,
.leafFrontBox {
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: center;
  transform: translateX(-50%);
}

.leafBackBox {
  bottom: 49.71px;
  left: calc(50% + 17.24px);
  width: 55.526px;
  height: 57.791px;
}

.leafBack,
.leafFront {
  position: relative;
  flex: none;
  /* Figma's order: rotate, then skew, then the vertical flip (Tailwind -scale-y-100 rotate-[-108.93deg] skew-x-1) */
  transform: rotate(-108.93deg) skewX(1deg) scaleY(-1);
}

.leafBack {
  width: 45.469px;
  height: 43.372px;
}

.leafBackImg {
  position: absolute;
  inset: -14.91% -14.25% -14.71% -13.92%;
  width: auto;
  height: auto;
}

/* "Group 1000006112" (front leaves). */
.leafFrontBox {
  bottom: 50.97px;
  left: calc(50% + 17.61px);
  width: 50.159px;
  height: 51.373px;
}

.leafFront {
  width: 40.069px;
  height: 39.526px;
}

.leafFront img {
  width: 100%;
  height: 100%;
}

/* "Ellipse 8": the ring's soft inner fill. */
.fill {
  position: absolute;
  top: 30.85px;
  left: 10.38px;
  width: 57.662px;
  height: 59.282px;
}

/* "circleGroup": the angular-gradient ring. */
.ringBox {
  position: absolute;
  top: 27.88px;
  left: 7.5px;
  width: 62.852px;
  height: 64.618px;
}

.ring {
  position: absolute;
  inset: -7.11% -7.31%;
  width: auto;
  height: auto;
}

/*
 * Idle motion (live app, 2026-10-23): the ring's yellow highlight goes round clockwise once a second, at a steady
 * speed — the ring and its soft fill turn on their own centres; the leaves stay still. Off for reduced motion.
 */
.ringBox,
.fill {
  animation: dash-orb-spin var(--app-dash-orb-spin) linear infinite;
}

@keyframes dash-orb-spin {
  to {
    transform: rotate(1turn);
  }
}

@media (prefers-reduced-motion: reduce) {
  .ringBox,
  .fill {
    animation: none;
  }
}
```

### `src/shared/ui/dash/DashOrb.tsx`

```tsx
import leafBack from './assets/leaf-back.svg'
import leafFront from './assets/leaf-front.svg'
import orbFill from './assets/orb-fill.svg'
import orbRing from './assets/orb-ring.svg'
import styles from './DashOrb.module.css'

export type DashOrbSize = 'sm' | 'md' | 'lg'

/**
 * DASH's mascot — the lime ring with two leaves (Figma "Logo" / "BotIcon", the 100 × 100 vector component), drawn from
 * its own vector parts in the Figma positions, with the soft halo behind it (a blurred copy).
 * Sizes: lg 96 (landing pages), md 64 (sheet headers, on a soft disc), sm 32 (the rule card's icon). The DASH pill sets its own
 * box (21.8). Shared: DASH flows and the home's entry pill use the same artwork.
 * Decorative: screen readers skip it — the text next to it says what it is.
 */
export function DashOrb({ size = 'lg', halo = true }: { size?: DashOrbSize; halo?: boolean }) {
  return (
    <span className={styles.orb} data-size={size} aria-hidden="true">
      <span className={styles.scale}>
        {halo && <OrbArt className={styles.halo} />}
        <OrbArt />
      </span>
    </span>
  )
}

/** The artwork at its Figma size (100 × 100); the parent scales it. */
function OrbArt({ className }: { className?: string }) {
  return (
    <span className={[styles.art, className].filter(Boolean).join(' ')}>
      <span className={styles.leafBackBox}>
        <span className={styles.leafBack}>
          <img src={leafBack} alt="" className={styles.leafBackImg} />
        </span>
      </span>
      <span className={styles.leafFrontBox}>
        <span className={styles.leafFront}>
          <img src={leafFront} alt="" />
        </span>
      </span>
      <img src={orbFill} alt="" className={styles.fill} />
      <span className={styles.ringBox}>
        <img src={orbRing} alt="" className={styles.ring} />
      </span>
    </span>
  )
}
```

### `src/shared/ui/dash/assets/leaf-back.svg`

```tsx
<svg preserveAspectRatio="none" overflow="visible" style="display: block;" width="58.2771" height="56.2179" viewBox="0 0 58.2771 56.2179" fill="none" xmlns="http://www.w3.org/2000/svg">
<g id="Group 1000006113" filter="url(#filter0_f_0_282)">
<path id="Big_leaf_00000052817416241482779350000003325489541389591972_" d="M11.1805 6.4948L10.2325 8.23984C7.21698 13.7241 6.07176 19.8031 6.63218 25.5588C7.5941 35.2352 13.3302 44.2732 22.6694 49.2476L23.6172 47.5025C27.4934 40.6061 28.2232 32.8573 26.4785 25.7606C24.3978 17.9116 19.1614 10.7108 11.1805 6.4948Z" fill="#8EF087" style="fill:#8EF087;fill:color(display-p3 0.5578 0.9428 0.5275);fill-opacity:1;"/>
<path id="Small_leaf_00000086657169406454258550000009755690847739837326_" d="M51.7823 38.3919C48.9397 43.4603 44.4029 47.018 39.1965 48.6543C33.6484 50.4553 27.3477 50.0009 21.8287 47.1335C24.6713 42.065 29.2933 38.5078 34.4997 36.8715C39.9626 35.07 46.2638 35.4411 51.7823 38.3919Z" fill="#8EF087" style="fill:#8EF087;fill:color(display-p3 0.5578 0.9428 0.5275);fill-opacity:1;"/>
</g>
<defs>
<filter id="filter0_f_0_282" x="-7.45058e-08" y="2.2538e-07" width="58.2771" height="56.2179" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB">
<feFlood flood-opacity="0" result="BackgroundImageFix"/>
<feBlend mode="normal" in="SourceGraphic" in2="BackgroundImageFix" result="shape"/>
<feGaussianBlur stdDeviation="3.2474" result="effect1_foregroundBlur_0_282"/>
</filter>
</defs>
</svg>
```

### `src/shared/ui/dash/assets/leaf-front.svg`

```tsx
<svg preserveAspectRatio="none" overflow="visible" style="display: block;" width="40.0692" height="39.5257" viewBox="0 0 40.0692 39.5257" fill="none" xmlns="http://www.w3.org/2000/svg">
<g id="Group 1000006112">
<path id="Small_leaf_00000086657169406454258550000009755690847739837326_" d="M40.0554 29.6165C37.5947 34.0042 33.6673 37.084 29.1603 38.5005C24.3574 40.0595 18.9031 39.6661 14.1254 37.1839C16.5862 32.7963 20.5873 29.7169 25.0943 28.3004C29.8234 26.7409 35.2782 27.0621 40.0554 29.6165Z" fill="url(#paint0_linear_0_409)"/>
<path id="Big_leaf_00000052817416241482779350000003325489541389591972_" d="M4.19434 0.0243672L3.37396 1.5349C0.764101 6.28215 -0.226587 11.5445 0.259219 16.5271C1.09303 24.9038 6.05968 32.7283 14.1449 37.0356L14.9652 35.525C18.3199 29.5554 18.9507 22.8476 17.4396 16.704C15.6376 9.90912 11.1037 3.67494 4.19434 0.0243672Z" fill="url(#paint1_linear_0_409)"/>
</g>
<defs>
<linearGradient id="paint0_linear_0_409" x1="21.3968" y1="29.3938" x2="22.0238" y2="39.1604" gradientUnits="userSpaceOnUse">
<stop stop-color="#7CE883" style="stop-color:#7CE883;stop-color:color(display-p3 0.4863 0.9098 0.5137);stop-opacity:1;"/>
<stop offset="1" stop-color="#3EBF47" style="stop-color:#3EBF47;stop-color:color(display-p3 0.2433 0.7503 0.2802);stop-opacity:1;"/>
</linearGradient>
<linearGradient id="paint1_linear_0_409" x1="5.24577" y1="6.42552" x2="13.4379" y2="33.8372" gradientUnits="userSpaceOnUse">
<stop stop-color="#7CE883" style="stop-color:#7CE883;stop-color:color(display-p3 0.4863 0.9098 0.5137);stop-opacity:1;"/>
<stop offset="1" stop-color="#3EBF47" style="stop-color:#3EBF47;stop-color:color(display-p3 0.2433 0.7503 0.2802);stop-opacity:1;"/>
</linearGradient>
</defs>
</svg>
```

### `src/shared/ui/dash/assets/orb-fill.svg`

```tsx
<svg preserveAspectRatio="none" overflow="visible" style="display: block;" width="57.6624" height="59.2824" viewBox="0 0 57.6624 59.2824" fill="none" xmlns="http://www.w3.org/2000/svg">
<mask id="stroke0_0_416" fill="white">
<path d="M57.6624 29.6412C57.6624 46.0116 44.7543 59.2824 28.8312 59.2824C12.9082 59.2824 0 46.0116 0 29.6412C0 13.2708 12.9082 0 28.8312 0C44.7543 0 57.6624 13.2708 57.6624 29.6412Z"/>
</mask>
<path id="Ellipse 8" d="M57.6624 29.6412C57.6624 46.0116 44.7543 59.2824 28.8312 59.2824C12.9082 59.2824 0 46.0116 0 29.6412C0 13.2708 12.9082 0 28.8312 0C44.7543 0 57.6624 13.2708 57.6624 29.6412Z" stroke-width="16.8671" stroke="url(#paint0_linear_0_416)" mask="url(#stroke0_0_416)"/>
<defs>
<linearGradient id="paint0_linear_0_416" x1="17.0104" y1="56.2094" x2="43.5226" y2="4.92706" gradientUnits="userSpaceOnUse">
<stop stop-color="#72E780" style="stop-color:#72E780;stop-color:color(display-p3 0.4476 0.9047 0.5009);stop-opacity:1;"/>
<stop offset="1" stop-color="#FDFFB5" style="stop-color:#FDFFB5;stop-color:color(display-p3 0.9903 1.0000 0.7091);stop-opacity:1;"/>
</linearGradient>
</defs>
</svg>
```

### `src/shared/ui/dash/assets/orb-ring.svg`

```tsx
<svg preserveAspectRatio="none" overflow="visible" style="display: block;" width="72.0446" height="73.8103" viewBox="0 0 72.0446 73.8103" fill="none" xmlns="http://www.w3.org/2000/svg">
<g id="circleGroup">
<g id="Ellipse 9" filter="url(#filter0_f_0_407)">
<mask id="stroke0_0_407" fill="white">
<path d="M67.4483 36.9052C67.4483 54.7489 53.3784 69.2141 36.0223 69.2141C18.6662 69.2141 4.59628 54.7489 4.59628 36.9052C4.59628 19.0615 18.6662 4.59628 36.0223 4.59628C53.3784 4.59628 67.4483 19.0615 67.4483 36.9052Z"/>
</mask>
<mask id="stroke1_0_407">
<path d="M67.4483 36.9052C67.4483 54.7489 53.3784 69.2141 36.0223 69.2141C18.6662 69.2141 4.59628 54.7489 4.59628 36.9052C4.59628 19.0615 18.6662 4.59628 36.0223 4.59628C53.3784 4.59628 67.4483 19.0615 67.4483 36.9052Z" stroke-width="18.3851" stroke="white" mask="url(#stroke0_0_407)"/>
</mask>
<g clip-path="url(#paint0_angular_0_407_clip_path)" data-figma-skip-parse="true" mask="url(#stroke1_0_407)"><g transform="matrix(0.01625 -0.02375 0.0230642 0.0166474 37.0963 36.713)"><foreignObject x="-3827.82" y="-3827.82" width="7655.64" height="7655.64"><div xmlns="http://www.w3.org/1999/xhtml" style="background:conic-gradient(from 90deg,rgba(121, 234, 105, 1) 0deg,rgba(246, 253, 38, 1) 124.615deg,rgba(52, 177, 33, 1) 238.846deg,rgba(122, 235, 105, 1) 360deg);height:100%;width:100%;opacity:1"></div></foreignObject></g></g><path d="M67.4483 36.9052C67.4483 54.7489 53.3784 69.2141 36.0223 69.2141C18.6662 69.2141 4.59628 54.7489 4.59628 36.9052C4.59628 19.0615 18.6662 4.59628 36.0223 4.59628C53.3784 4.59628 67.4483 19.0615 67.4483 36.9052Z" stroke-width="18.3851" stroke="black" stroke-opacity="0" data-figma-gradient-stroke="{&#34;type&#34;:&#34;GRADIENT_ANGULAR&#34;,&#34;stops&#34;:[{&#34;color&#34;:{&#34;r&#34;:0.47711750864982605,&#34;g&#34;:0.92151820659637451,&#34;b&#34;:0.41342675685882568,&#34;a&#34;:1.0},&#34;position&#34;:0.0},{&#34;color&#34;:{&#34;r&#34;:0.96524602174758911,&#34;g&#34;:0.99325793981552124,&#34;b&#34;:0.15290218591690063,&#34;a&#34;:1.0},&#34;position&#34;:0.34615385532379150},{&#34;color&#34;:{&#34;r&#34;:0.20681096613407135,&#34;g&#34;:0.69540637731552124,&#34;b&#34;:0.13164243102073669,&#34;a&#34;:1.0},&#34;position&#34;:0.66346156597137451},{&#34;color&#34;:{&#34;r&#34;:0.47843137383460999,&#34;g&#34;:0.92156863212585449,&#34;b&#34;:0.41176471114158630,&#34;a&#34;:1.0},&#34;position&#34;:1.0}],&#34;transform&#34;:{&#34;m00&#34;:32.500003814697266,&#34;m01&#34;:46.128456115722656,&#34;m02&#34;:-2.2179441452026367,&#34;m10&#34;:-47.500007629394531,&#34;m11&#34;:33.294700622558594,&#34;m12&#34;:43.815639495849609},&#34;opacity&#34;:1.0,&#34;blendMode&#34;:&#34;NORMAL&#34;,&#34;stopsVar&#34;:[{&#34;color&#34;:{&#34;r&#34;:0.47711750864982605,&#34;g&#34;:0.92151820659637451,&#34;b&#34;:0.41342675685882568,&#34;a&#34;:1.0},&#34;position&#34;:0.0},{&#34;color&#34;:{&#34;r&#34;:0.96524602174758911,&#34;g&#34;:0.99325793981552124,&#34;b&#34;:0.15290218591690063,&#34;a&#34;:1.0},&#34;position&#34;:0.34615385532379150},{&#34;color&#34;:{&#34;r&#34;:0.20681096613407135,&#34;g&#34;:0.69540637731552124,&#34;b&#34;:0.13164243102073669,&#34;a&#34;:1.0},&#34;position&#34;:0.66346156597137451},{&#34;color&#34;:{&#34;r&#34;:0.47843137383460999,&#34;g&#34;:0.92156863212585449,&#34;b&#34;:0.41176471114158630,&#34;a&#34;:1.0},&#34;position&#34;:1.0}],&#34;visible&#34;:true}" mask="url(#stroke0_0_407)"/>
</g>
</g>
<defs>
<filter id="filter0_f_0_407" x="0" y="0" width="72.0446" height="73.8103" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB">
<feFlood flood-opacity="0" result="BackgroundImageFix"/>
<feBlend mode="normal" in="SourceGraphic" in2="BackgroundImageFix" result="shape"/>
<feGaussianBlur stdDeviation="2.29814" result="effect1_foregroundBlur_0_407"/>
</filter>
<clipPath id="paint0_angular_0_407_clip_path"><rect x="-41.3666" y="-41.3666" width="154.778" height="156.543"/></clipPath></defs>
</svg>
```
