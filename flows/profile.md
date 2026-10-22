# Flow: Profile & settings (`profile`)

> One self-contained file for converting this flow to another platform. Read https://kirtanyak-pe.github.io/lemonn-app-prototype/CONVERT.md first. Design system: Lemonnade V3 @ `cd84910` (components, tokens and rules: https://kirtanyak-pe.github.io/lemonnade-v3-docs/llms.txt).

Your account, app settings (biometric, dark mode, accessibility), help, and logout.

Prototype: https://kirtanyak-pe.github.io/lemonn-app-prototype/#/profile · machine-readable: https://kirtanyak-pe.github.io/lemonn-app-prototype/flows/profile.json

## Screens

| Id | Screen | Figma | Taps go to | Note |
|---|---|---|---|---|
| `profile-01` | Profile | — | `stocks-01`, `profile-02`, `login-01` | The live page 1:1 (scrolls: account, app settings, help, Logout). From the profile icon on Stocks home. |
| `profile-02` | Settings | — | `profile-01`, `profile-03` | Biometric, Dark Mode (live: switches the app theme), and the PM’s Accessibility row after Dark Mode. |
| `profile-03` | Accessibility | — | `profile-02` | PM 5a · first visit: Low vision mode off. |
| `profile-04` | Accessibility · low vision on | — | `profile-02` | PM 5b · on, phone text size 100 %: the note says how to make text bigger. ♿ Accessible colours. |
| `profile-05` | Accessibility · turning on failed | — | `profile-02` | PM 7a · the switch is back off; error toast with Retry at the bottom. |
| `profile-06` | Accessibility · turning off failed | — | `profile-02` | PM 7b · the switch stays on, low-vision colours stay; error toast with Retry at the bottom. |

## Spec (README.md)


> The spec for this flow. A developer should be able to build it on any platform (Flutter, iOS, web) from this file,
> the screens in this folder and the live app — without reading the rest of the repo.

- **Prototype:** https://kirtanyak-pe.github.io/lemonn-app-prototype/#/profile · one screen: `#/profile-01`
- **Source:** screenshots of the live app's Profile and Settings pages (dark theme, shared 2026-10-22) and the PM's
  Accessibility proposal (frames 9, 5a, 5b, 7a, 7b). No Figma yet. Redesigned in L3.
- **Status:** ✅ · last updated 2026-10-22
- **Entry:** the profile icon in the Stocks home's top bar (`stocks-01 … 07`). **Exit:** back to Stocks; Logout →
  the welcome screen (`login-01`).

### Screens

| Id | Screen | What it shows / when |
|---|---|---|
| `profile-01` | Profile | The live page 1:1. Grey header (back, name, View Details ›, avatar) that stays put; the list scrolls under its rounded top: **Account Information** (Bank Details · Verified, Balance, Get ₹149 instant balance, Tax and P&L Reports) · **App Settings** (Settings, Kill Switch, Auto TP/SL) · **How can we help?** (Help & Support, Follow us on X, Subscribe on YouTube, Follow us on Instagram, Rate Us, About 7.2.38 (3)) · Logout · the broker's line with Read More |
| `profile-02` | Settings | Biometric (switch) · Dark Mode (switch) · Accessibility › (new, PM proposal: after Dark Mode) |
| `profile-03` | Accessibility | PM 5a · first visit: Low vision mode off |
| `profile-04` | Accessibility · low vision on | PM 5b · switch on; note "Your phone's text size is 100% — raise it in your phone's settings and Lemonn will follow, up to 2×"; the screen in ♿ Accessible colours |
| `profile-05` | Accessibility · turning on failed | PM 7a · the save failed: switch back off; error toast "Couldn't turn on low vision mode" + Retry |
| `profile-06` | Accessibility · turning off failed | PM 7b · the save failed: switch still on, colours still low-vision; toast "Couldn't turn off low vision mode" + Retry |

### Navigation

```
stocks-0x ─profile icon─▶ profile-01 ─Settings─▶ profile-02 ─Accessibility─▶ profile-03/04 ─back─▶ profile-02
                          profile-01 ─Logout─▶ login-01 (welcome) · back ─▶ stocks-01
Every other row (Bank Details, Balance, instant balance, reports, Kill Switch, Auto TP/SL, Help & Support, the social
links, Rate Us, About, View Details, the accessibility statement) is tappable but goes nowhere in the prototype yet.
```

### Data

From `data.ts` (fictional) and `src/shared/data/user.ts` (the sample user):
- `user.name` (header; avatar initials), `balance` (₹, may be negative: shown `Balance : −₹245.30`),
  `INSTANT_BALANCE` (₹149), `APP_VERSION` (`7.2.38 (3)`).
- Accessibility: `MAX_TEXT_SCALE` (2×) and `PHONE_TEXT_SIZE` (the OS text size, %).
- API: get profile (name, avatar, bank verified, balance); save preferences (biometric, theme, low vision mode) — the
  save can fail (profile-05 / 06).
- The broker's regulatory line: `src/shared/data/broker.ts` (also on the Stocks home).

### Copy

Every user-facing string is in `copy.json` (flat keys → text; `{name}` is filled in by the code). Port it as the
platform's string table (Flutter ARB, iOS Localizable.strings).

### Components (L3 → any platform)

| Where | L3 component (Figma name) | Props / variant |
|---|---|---|
| Profile top | Actionbar | `onBack`, no title; the header (name, View Details, avatar, rounded lip) is its `bottom` slot so it doesn't scroll; bar + header on `surface-secondary`, no divider |
| Name | text | Heading/24 |
| View Details › | Button | `variant="ghost"`, `iconRight` chevron |
| Avatar | Avatar | `initials`, 48, filled `surface-accent-discover-default` (live app's blue) |
| Section headings | SectionHeader | `title` + `description`; cards 16 below, 12 apart; sections 32 apart |
| Rows | ListCell `variant="card"`, tappable | `iconLeft` (24), `label`, `description` (`multiline`), `iconRight` chevron |
| Verified | Tag | `size="sm" variant="secondary" color="success"`, in the row's `trailing` |
| About | BrandLogo | `lemonn`, `variant="icon"`, 24, decorative |
| Kill Switch / Auto TP/SL icons | Lemonnade custom icons | `lmKillSwitch`, `lmAutoTpSl` |
| Logout | Button | `variant="tertiary"`, `lg`, full width |
| Disclosure | text + Button | Description/12 tertiary, 5 lines; `ghost` `sm` Read More / Read less |
| Settings rows | ListCell `variant="card"` `as="label"` + Switch | the whole card toggles; Accessibility is a tappable card row with a chevron |
| Accessibility rows | ListCell (plain) + Switch; SectionHeader "Vision" | `as="label"`, `multiline`; statement row with open-in-new |
| Low vision note | Aerobar | `floating`, `placement="inline"`, `type="discover"`, `emphasis="secondary"`, icon info |
| Failed save | Aerobar (toast) | `floating`, `type="danger"`, `emphasis="primary"`, icon error, `action` Retry; auto-dismisses (6 s) |

### Rules & behaviour

- **Profile and Settings copy the live app** (layout, order, copy) — only the parts are L3. No redesign yet.
- **Logout** goes straight to the welcome screen, as in the live app.
- **Settings apply at once** (switches, no Save). Dark Mode: on = dark, off = light (CS PRO has no light theme → the
  switch is disabled there). The Accessibility row says "Low vision mode is on" when it's on.
- **Low vision mode** (stored on this phone):
  - **Text:** normally the app keeps text at 1× whatever the phone's text size. With low vision mode on, it follows
    the phone's text size up to 2× (Flutter: `MediaQuery.textScaler` clamped to 1.0 normally, to 2.0 in low vision
    mode). Layouts must reflow at 2× (no clipped labels; rows grow).
  - **Colours:** the app switches to L3's ♿ Accessible theme (`data-contrast="accessible"`: stronger secondary /
    tertiary text, borders and accents). Every screen uses the same semantic tokens, so nothing else changes.
  - **Landscape:** the app may rotate to landscape (it's portrait-only otherwise).
  - **The note** shows while it's on and the phone's text size is 100 % (nothing can grow yet): how to raise it.
  - **Saving fails** (profile-05 / 06): the switch shows what's really stored (turning on failed → off; turning off
    failed → on, colours stay low-vision), and an error toast at the bottom offers Retry.
- Prototype only: Dark Mode and Low vision mode change the prototype's real theme (and stay on this browser), so the
  rest of the app follows. Each Accessibility state also draws its own colours, so the states always look right.

### Differences from the live app and the PM proposal

- **Profile / Settings:** same layout and copy; parts swapped to L3. The Verified tag sits in the card's trailing slot
  (right side, before the chevron) instead of next to the title — L3's list cell has no tag-after-label slot. The
  avatar is 48 (L3's largest) with initials instead of the large smiley picture. Settings' rounded sticky header
  became the L3 Actionbar.
- **Accessibility (PM):** the "I have low vision / I don't have low vision" radio pair → one L3 Switch, "Low vision
  mode" (an on/off setting that applies at once); first visit = off. The red banner at the top → the L3 error toast at
  the bottom with Retry. "Read our accessibility statement" → a row "Accessibility statement" with open-in-new. The
  note keeps the PM's copy.

### L3 gaps

- **Social brand marks:** no X / YouTube / Instagram logos in L3 (BrandLogo has Lemonn, Zing, CoinSwitch); Material
  stand-ins (campaign, smart display, camera) are used.
- **Avatar colour / size:** no colour option (its default fill disappears on `surface-secondary`, so the profile
  sets the discover fill locally) and nothing above 48.
- **Tag next to a list cell's label:** no slot; the Verified tag goes in `trailing`.
- **Text scale:** L3 has no text-scale token or "follow OS text size up to 2×" setting; the prototype can't show the
  bigger text (a browser can't read the phone's text size).

### Changelog

- 2026-10-22 — first version (6 screens): Profile and Settings replicated from the live app with L3 parts;
  Accessibility (4 states) from the PM proposal.


## Source — this flow

### `src/flows/profile/components/CardRow.tsx`

```tsx
import type { ReactNode } from 'react'
import { Icon, type IconSource } from '@l3/components/Icon'
import { ListCell } from '@l3/components/ListCell'
import { msChevronRight } from '@l3/icons/material'

type Props = {
  icon?: IconSource
  /** Anything that isn't a Material / Lemonnade icon (the Lemonn mark). */
  leading?: ReactNode
  label: string
  description?: string
  /** A Tag, or a control (then the row is a label: tapping it toggles the control). */
  trailing?: ReactNode
  /** The row holds a Switch: `as="label"`, no chevron. */
  toggle?: boolean
  onClick?: () => void
}

/**
 * One row of the live app's profile / settings lists, as an L3 card row (ListCell `variant="card"`): icon · label +
 * description (wraps) · tag / switch · chevron. Tappable rows are clickable cards; switch rows toggle on a tap anywhere.
 */
export function CardRow({ icon, leading, label, description, trailing, toggle = false, onClick }: Props) {
  const iconLeft = leading ?? (icon && <Icon icon={icon} />)
  if (toggle) return <ListCell variant="card" as="label" iconLeft={iconLeft} label={label} description={description} multiline trailing={trailing} />
  return (
    <ListCell
      variant="card"
      onClick={onClick ?? (() => {})}
      iconLeft={iconLeft}
      label={label}
      description={description}
      multiline
      trailing={trailing}
      iconRight={<Icon icon={msChevronRight} />}
    />
  )
}
```

### `src/flows/profile/components/Group.tsx`

```tsx
import type { ReactNode } from 'react'
import { Icon, type IconSource } from '@l3/components/Icon'
import { Inset } from '@l3/components/Inset'
import { ListCell } from '@l3/components/ListCell'
import { SectionHeader } from '@l3/components/SectionHeader'
import { Stack } from '@l3/components/Stack'
import { msChevronRight, msOpenInNew } from '@l3/icons/material'

/**
 * A group of settings rows under a heading (L3 "settings" archetype: SectionHeader + flat ListCell rows).
 * The heading sits on the 16 page margin; the rows are flat (plain) and run edge to edge with their own 16 padding.
 * 8 between heading and rows: the rows bring 8 above their content, so the content sits 16 under the heading.
 */
export function Group({ title, description, children }: { title?: string; description?: string; children: ReactNode }) {
  return (
    <Stack gap="08" as="section" aria-label={title}>
      {title && (
        <Inset paddingInline="16" paddingBlock="00">
          <SectionHeader title={title} description={description} />
        </Inset>
      )}
      <Stack>{children}</Stack>
    </Stack>
  )
}

type RowProps = {
  icon?: IconSource
  /** Anything that isn't a Material / Lemonnade icon (a brand mark). */
  leading?: ReactNode
  label: string
  description?: string
  /** A Tag or a value, before the chevron. */
  trailing?: ReactNode
  /** A line under a value in `trailing` ("Available"). */
  caption?: ReactNode
  /** Opens another app or a web page: the open-in-new icon instead of the chevron. */
  external?: boolean
  onClick?: () => void
}

/** One navigation row: icon · label + description · (tag / value) · chevron. The whole row is the tap target. */
export function Row({ icon, leading, label, description, trailing, caption, external = false, onClick }: RowProps) {
  return (
    <ListCell
      onClick={onClick ?? (() => {})}
      iconLeft={leading ?? (icon && <Icon icon={icon} />)}
      label={label}
      description={description}
      trailing={trailing}
      caption={caption}
      iconRight={<Icon icon={external ? msOpenInNew : msChevronRight} />}
    />
  )
}
```

### `src/flows/profile/copy.json`

```json
{
  "profile.viewDetails": "View Details",
  "profile.avatarLabel": "Profile picture",
  "account.title": "Account Information",
  "account.description": "Find all your profile-related information",
  "account.bank": "Bank Details",
  "account.bankVerified": "Verified",
  "account.bankHint": "This account is used to facilitate all your deposits and withdrawal",
  "account.balance": "Balance : {amount}",
  "account.balanceHint": "Check your account balance",
  "account.instant": "Get {amount} instant balance",
  "account.instantHint": "For intraday, MTF, F&O",
  "account.reports": "Tax and P&L Reports",
  "account.reportsHint": "For Stocks and F&O",
  "appSettings.title": "App Settings",
  "appSettings.description": "Get the best out of your Lemonn profile",
  "appSettings.settings": "Settings",
  "appSettings.settingsHint": "Edit security or display settings",
  "appSettings.killSwitch": "Kill Switch",
  "appSettings.killSwitchHint": "Pause new trading for today",
  "appSettings.autoTpSl": "Auto TP/SL",
  "appSettings.autoTpSlHint": "Automate TP/SL limits",
  "help.title": "How can we help?",
  "help.description": "We're here for you 24x7",
  "help.support": "Help & Support",
  "help.supportHint": "Contact Us",
  "help.x": "Follow us on X",
  "help.xHint": "Stay updated with the latest news",
  "help.youtube": "Subscribe on YouTube",
  "help.youtubeHint": "Check out our latest videos",
  "help.instagram": "Follow us on Instagram",
  "help.instagramHint": "Get to know our Insta community",
  "help.rate": "Rate Us",
  "help.rateHint": "Tell us what you think",
  "help.about": "About",
  "logout.button": "Logout",
  "disclosure.more": "Read More",
  "disclosure.less": "Read less",
  "settings.title": "Settings",
  "settings.biometric": "Biometric",
  "settings.biometricHint": "Login with your fingerprint",
  "settings.darkMode": "Dark Mode",
  "settings.darkModeHint": "Switch between dark and light mode",
  "settings.accessibility": "Accessibility",
  "settings.accessibilityHint": "Make text and screens easier to see",
  "settings.accessibilityOn": "Low vision mode is on",
  "a11y.title": "Accessibility",
  "a11y.intro": "Make Lemonn easier to read if you have low vision. Your choice stays on this phone.",
  "a11y.vision": "Vision",
  "a11y.lowVision": "Low vision mode",
  "a11y.lowVisionHint": "Bigger text, darker hint text and landscape",
  "a11y.textSizeTitle": "Your phone’s text size is {size}%",
  "a11y.textSizeText": "Raise it in your phone’s settings and Lemonn will follow, up to {max}×.",
  "a11y.more": "More",
  "a11y.statement": "Accessibility statement",
  "a11y.statementHint": "How we make Lemonn work for everyone",
  "a11y.onFailed": "Couldn’t turn on low vision mode",
  "a11y.offFailed": "Couldn’t turn off low vision mode",
  "a11y.retry": "Retry"
}
```

### `src/flows/profile/copy.ts`

```ts
import { makeCopy } from '../../shared/copy'
import copy from './copy.json'

/** Every user-facing string of Profile & settings is in copy.json: t('settings.title'), t('profile.clientId', { id }). */
export const t = makeCopy('profile', copy)
```

### `src/flows/profile/data.ts`

```ts
// Sample data for Profile & settings. FICTIONAL — the live screenshots show a real customer's name and balance; never
// copy them. The person is the app's sample user (src/shared/data/user.ts).
import { bankAccount, user } from '../../shared/data/user'

export { bankAccount, user }

/** The trading balance (₹). Can be negative when charges are due — shown as "−₹…". */
export const balance = -245.3

/** The instant-balance offer (₹, for intraday, MTF and F&O). */
export const INSTANT_BALANCE = 149

/** The app version shown under About. */
export const APP_VERSION = '7.2.38 (3)'

/** Low vision mode: Lemonn follows the phone's text size up to this scale (normally it stays at 1×). */
export const MAX_TEXT_SCALE = 2

/** The phone's own text size (from the OS), in %. At 100 % low vision mode can't make text bigger — the screen says so. */
export const PHONE_TEXT_SIZE = 100

/** Initials for the avatar: "Rohan Mehta" → "RM". */
export const initials = (name = user.name) =>
  name.split(/\s+/).filter(Boolean).slice(0, 2).map((w) => w[0]!.toUpperCase()).join('')
```

### `src/flows/profile/index.tsx`

```tsx
import type { Flow } from '../types'
import { Accessibility } from './screens/Accessibility'
import { Profile } from './screens/Profile'
import { Settings } from './screens/Settings'

/**
 * Profile & settings — opened from the profile icon on the Stocks home. The live app's Profile and Settings pages
 * (screenshots, 2026-10-22) rebuilt 1:1 with L3 parts, plus the PM's Accessibility screens (redrawn in L3). No Figma yet.
 */
export const profile: Flow = {
  id: 'profile',
  title: 'Profile & settings',
  description: 'Your account, app settings (biometric, dark mode, accessibility), help, and logout.',
  figmaFile: '',
  sections: [
    { id: 'profile', title: 'Profile' },
    { id: 'settings', title: 'Settings' },
    { id: 'accessibility', title: 'Accessibility' },
  ],
  screens: [
    {
      id: 'profile-01', section: 'profile', name: 'Profile', figma: '',
      note: 'The live page 1:1 (scrolls: account, app settings, help, Logout). From the profile icon on Stocks home.',
      render: (go) => <Profile onBack={() => go('stocks-01')} onSettings={() => go('profile-02')} onLogout={() => go('login-01')} />,
    },
    {
      id: 'profile-02', section: 'settings', name: 'Settings', figma: '',
      note: 'Biometric, Dark Mode (live: switches the app theme), and the PM’s Accessibility row after Dark Mode.',
      render: (go) => <Settings onBack={() => go('profile-01')} onAccessibility={() => go('profile-03')} />,
    },
    {
      id: 'profile-03', section: 'accessibility', name: 'Accessibility', figma: '',
      note: 'PM 5a · first visit: Low vision mode off.',
      render: (go) => <Accessibility onBack={() => go('profile-02')} />,
    },
    {
      id: 'profile-04', section: 'accessibility', name: 'Accessibility · low vision on', figma: '',
      note: 'PM 5b · on, phone text size 100 %: the note says how to make text bigger. ♿ Accessible colours.',
      render: (go) => <Accessibility lowVision onBack={() => go('profile-02')} />,
    },
    {
      id: 'profile-05', section: 'accessibility', name: 'Accessibility · turning on failed', figma: '',
      note: 'PM 7a · the switch is back off; error toast with Retry at the bottom.',
      render: (go) => <Accessibility failed="on" onBack={() => go('profile-02')} />,
    },
    {
      id: 'profile-06', section: 'accessibility', name: 'Accessibility · turning off failed', figma: '',
      note: 'PM 7b · the switch stays on, low-vision colours stay; error toast with Retry at the bottom.',
      render: (go) => <Accessibility lowVision failed="off" onBack={() => go('profile-02')} />,
    },
  ],
}
```

### `src/flows/profile/screens/Accessibility.tsx`

```tsx
import { useState } from 'react'
import { Actionbar } from '@l3/components/Actionbar'
import { Aerobar } from '@l3/components/Aerobar'
import { Icon } from '@l3/components/Icon'
import { Inset } from '@l3/components/Inset'
import { ListCell } from '@l3/components/ListCell'
import { Stack } from '@l3/components/Stack'
import { Switch } from '@l3/components/Switch'
import { msError, msInfo } from '@l3/icons/material'
import { useTheme } from '@l3/theme'
import { usePreview } from '../../../shared/prototype'
import { Screen } from '../../../shared/ui/Screen'
import { Group, Row } from '../components/Group'
import { t } from '../copy'
import { MAX_TEXT_SCALE, PHONE_TEXT_SIZE } from '../data'
import styles from './Profile.module.css'

type Props = {
  /** Low vision mode when the screen opens. */
  lowVision?: boolean
  /** A change that just failed to save: 'on' = turning it on failed (switch back off), 'off' = turning it off failed. */
  failed?: 'on' | 'off'
  onBack?: () => void
}

/**
 * profile-06 … 09 · Accessibility (PM proposal, redrawn in L3). One setting: Low vision mode — an L3 Switch row,
 * because it's on/off and applies at once (the proposal's "I have / don't have low vision" radio pair said the same
 * thing in two rows and could be left unanswered). On, it does two things:
 *  - text follows the phone's text size up to 2× (normally the app stays at 1×) — so at 100 % nothing grows, and an
 *    inline note says how to raise it;
 *  - colours switch to L3's ♿ Accessible theme (stronger hint text, borders and accents).
 * A failed save shows the L3 error toast at the bottom with Retry (not a banner at the top), and the switch shows
 * what's really saved: turning on failed → off; turning off failed → still on, still in low-vision colours.
 * Prototype: the switch sets the app's real contrast (L3 ThemeProvider), so every screen follows; this screen also
 * carries the theme attributes itself, so each state shows its own colours.
 */
export function Accessibility({ lowVision = false, failed, onBack }: Props) {
  const theme = useTheme()
  const { interactive } = usePreview()
  const [on, setOn] = useState(lowVision)
  const [failure, setFailure] = useState(failed)

  const apply = (next: boolean) => {
    setOn(next)
    setFailure(undefined)
    if (interactive) theme.setContrast(next ? 'accessible' : 'default')
  }

  return (
    <div className={styles.scope} data-product={theme.product} data-mode={theme.mode} data-contrast={on ? 'accessible' : undefined}>
      <Screen
        top={<Actionbar title={t('a11y.title')} onBack={onBack} />}
        toast={
          failure && (
            <Aerobar
              floating
              type="danger"
              emphasis="primary"
              icon={<Icon icon={msError} />}
              heading={failure === 'on' ? t('a11y.onFailed') : t('a11y.offFailed')}
              action={{ label: t('a11y.retry'), onClick: () => apply(failure === 'on') }}
              onDismiss={() => setFailure(undefined)}
            />
          )
        }
      >
        <Inset paddingBlock="16" paddingInline="00">
          <Stack gap="24">
            <Inset paddingInline="16" paddingBlock="00">
              <p className={styles.text}>{t('a11y.intro')}</p>
            </Inset>
            <Group title={t('a11y.vision')}>
              <ListCell
                as="label"
                label={t('a11y.lowVision')}
                description={t('a11y.lowVisionHint')}
                multiline
                trailing={<Switch checked={on} onChange={(e) => apply(e.target.checked)} aria-label={t('a11y.lowVision')} />}
              />
              {on && (
                <Inset paddingInline="16" paddingBlock="08">
                  <Aerobar
                    floating
                    placement="inline"
                    type="discover"
                    emphasis="secondary"
                    icon={<Icon icon={msInfo} />}
                    heading={t('a11y.textSizeTitle', { size: PHONE_TEXT_SIZE })}
                    paragraph={t('a11y.textSizeText', { max: MAX_TEXT_SCALE })}
                  />
                </Inset>
              )}
            </Group>
            <Row label={t('a11y.statement')} description={t('a11y.statementHint')} external />
          </Stack>
        </Inset>
      </Screen>
    </div>
  )
}
```

### `src/flows/profile/screens/Profile.module.css`

```css
/* Profile & settings — text styles and the profile's grey header; layout comes from the L3 layout blocks. */

.grow {
  flex: 1;
  min-width: 0;
}

/* Live app: the profile's top bar sits on the same grey as its header, with no divider (the lip separates them).
   Doubled class: wins over the Actionbar's own surface + border. */
.headerBar.headerBar {
  background: var(--l3-surface-secondary);
  border-bottom-color: transparent;
  box-shadow: none;
}

.header {
  padding-top: var(--l3-spacing-08);
  background: var(--l3-surface-secondary);
}

/* The rounded top of the list: the page scrolls under it, so it stays put like the live app's sheet. */
.lip {
  height: var(--l3-spacing-24);
  margin-top: var(--l3-spacing-32);
  border-radius: var(--l3-radius-32) var(--l3-radius-32) 0 0;
  background: var(--l3-surface-default);
}

.sheet {
  padding-bottom: var(--l3-spacing-32);
}

.name {
  font: var(--l3-text-heading-24);
  color: var(--l3-content-primary);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.text {
  margin: 0;
  font: var(--l3-text-description-14);
  color: var(--l3-content-secondary);
}

.fine {
  margin: 0;
  font: var(--l3-text-description-12);
  color: var(--l3-content-tertiary);
}

/* Disclosure: 5 lines, then "Read More". */
.clamp {
  display: -webkit-box;
  overflow: hidden;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 5;
  line-clamp: 5;
}

/* Accessibility: the screen carries its own theme attributes (data-product / data-mode / data-contrast), so low
   vision mode shows in ♿ Accessible colours on the screen itself. The wrapper adds no box. */
.scope {
  display: contents;
}

/* Live app: a blue avatar on the grey header (L3 Avatar has no colour option and its default fill disappears on the
   grey — L3 gap). Doubled class: wins over the Avatar's own fill. */
.avatar.avatar {
  background: var(--l3-surface-accent-discover-default);
  color: var(--l3-content-inverted);
}
```

### `src/flows/profile/screens/Profile.tsx`

```tsx
import { useState, type ReactNode } from 'react'
import { Actionbar } from '@l3/components/Actionbar'
import { Avatar } from '@l3/components/Avatar'
import { BrandLogo } from '@l3/components/BrandLogo'
import { Button } from '@l3/components/Button'
import { Icon } from '@l3/components/Icon'
import { Inset } from '@l3/components/Inset'
import { SectionHeader } from '@l3/components/SectionHeader'
import { Stack } from '@l3/components/Stack'
import { Tag } from '@l3/components/Tag'
import { lmAutoTpSl, lmKillSwitch } from '@l3/icons/lemonnade'
import {
  msAccountBalance, msAccountBalanceWallet, msBolt, msCampaign, msChevronRight, msPhotoCamera, msReceiptLong, msReviews,
  msSettings, msSmartDisplay, msSupportAgent,
} from '@l3/icons/material'
import { disclosure } from '../../../shared/data/broker'
import { inr } from '../../../shared/data/format'
import { Screen } from '../../../shared/ui/Screen'
import { CardRow } from '../components/CardRow'
import { t } from '../copy'
import { APP_VERSION, INSTANT_BALANCE, balance, initials, user } from '../data'
import styles from './Profile.module.css'

type Props = {
  onBack?: () => void
  onSettings?: () => void
  onLogout?: () => void
}

/** "−₹245.30" for a negative amount (a real minus sign, before the ₹). */
const signedInr = (n: number) => (n < 0 ? `−${inr(-n)}` : inr(n))

/** A section of the list: heading + description, then its card rows 12 apart. */
function Group({ title, description, children }: { title: string; description?: string; children: ReactNode }) {
  return (
    <Stack gap="16" as="section" aria-label={title}>
      <SectionHeader title={title} description={description} />
      <Stack gap="12">{children}</Stack>
    </Stack>
  )
}

/**
 * profile-01 · Profile — the live app's page, rebuilt 1:1 with L3 parts. Opened from the profile icon on the Stocks
 * home. A grey header (back, name, View Details ›, avatar) with the list on a rounded sheet over it: Account
 * Information, App Settings, How can we help? — every row a card — then Logout and the broker's line (Read More).
 */
export function Profile({ onBack, onSettings, onLogout }: Props) {
  const [more, setMore] = useState(false)
  return (
    <Screen
      top={
        // The header doesn't scroll (live app): it's the Actionbar's content-bottom slot, ending in the rounded lip
        // the list scrolls under.
        <Actionbar
          onBack={onBack}
          className={styles.headerBar}
          bottom={
            <div className={styles.header}>
              <Inset paddingInline="16" paddingBlock="00">
                <Stack direction="horizontal" gap="16" align="center" justify="between">
                  <Stack gap="04" align="start" className={styles.grow}>
                    <span className={styles.name}>{user.name}</span>
                    <Button variant="ghost" iconRight={<Icon icon={msChevronRight} />} onClick={() => {}}>{t('profile.viewDetails')}</Button>
                  </Stack>
                  <Avatar initials={initials()} size={48} label={t('profile.avatarLabel')} className={styles.avatar} />
                </Stack>
              </Inset>
              <div className={styles.lip} aria-hidden="true" />
            </div>
          }
        />
      }
    >
      <div className={styles.sheet}>
        <Inset paddingInline="16" paddingBlock="00">
          <Stack gap="32">
            <Group title={t('account.title')} description={t('account.description')}>
              <CardRow
                icon={msAccountBalance}
                label={t('account.bank')}
                description={t('account.bankHint')}
                trailing={<Tag size="sm" variant="secondary" color="success">{t('account.bankVerified')}</Tag>}
              />
              <CardRow icon={msAccountBalanceWallet} label={t('account.balance', { amount: signedInr(balance) })} description={t('account.balanceHint')} />
              <CardRow icon={msBolt} label={t('account.instant', { amount: inr(INSTANT_BALANCE, 0) })} description={t('account.instantHint')} />
              <CardRow icon={msReceiptLong} label={t('account.reports')} description={t('account.reportsHint')} />
            </Group>

            <Group title={t('appSettings.title')} description={t('appSettings.description')}>
              <CardRow icon={msSettings} label={t('appSettings.settings')} description={t('appSettings.settingsHint')} onClick={onSettings} />
              <CardRow icon={lmKillSwitch} label={t('appSettings.killSwitch')} description={t('appSettings.killSwitchHint')} />
              <CardRow icon={lmAutoTpSl} label={t('appSettings.autoTpSl')} description={t('appSettings.autoTpSlHint')} />
            </Group>

            <Group title={t('help.title')} description={t('help.description')}>
              <CardRow icon={msSupportAgent} label={t('help.support')} description={t('help.supportHint')} />
              <CardRow icon={msCampaign} label={t('help.x')} description={t('help.xHint')} />
              <CardRow icon={msSmartDisplay} label={t('help.youtube')} description={t('help.youtubeHint')} />
              <CardRow icon={msPhotoCamera} label={t('help.instagram')} description={t('help.instagramHint')} />
              <CardRow icon={msReviews} label={t('help.rate')} description={t('help.rateHint')} />
              <CardRow leading={<BrandLogo brand="lemonn" variant="icon" size={24} decorative />} label={t('help.about')} description={APP_VERSION} />
            </Group>

            <Button variant="tertiary" fullWidth onClick={onLogout}>{t('logout.button')}</Button>

            <Stack gap="04" align="start">
              <p className={more ? styles.fine : `${styles.fine} ${styles.clamp}`}>{disclosure}</p>
              <Button variant="ghost" size="sm" onClick={() => setMore((m) => !m)}>{more ? t('disclosure.less') : t('disclosure.more')}</Button>
            </Stack>
          </Stack>
        </Inset>
      </div>
    </Screen>
  )
}
```

### `src/flows/profile/screens/Settings.tsx`

```tsx
import { useState } from 'react'
import { Actionbar } from '@l3/components/Actionbar'
import { Inset } from '@l3/components/Inset'
import { Stack } from '@l3/components/Stack'
import { Switch } from '@l3/components/Switch'
import { msAccessibilityNew, msDarkMode, msFingerprint } from '@l3/icons/material'
import { useTheme } from '@l3/theme'
import { Screen } from '../../../shared/ui/Screen'
import { CardRow } from '../components/CardRow'
import { t } from '../copy'

/**
 * profile-05 · Settings — the live app's page in L3 card rows: Biometric and Dark Mode (switches that apply at once;
 * the whole row toggles), then Accessibility (PM proposal: a row into its own screen; "Low vision mode is on" when on).
 * Prototype: Dark mode and the Accessibility row are live — they read and set the app's real theme (L3 ThemeProvider),
 * like the app's own setting would.
 */
export function Settings({ onBack, onAccessibility }: { onBack?: () => void; onAccessibility?: () => void }) {
  const theme = useTheme()
  const [biometric, setBiometric] = useState(true)
  const dark = theme.mode === 'dark'
  const lowVision = theme.contrast === 'accessible'

  return (
    <Screen top={<Actionbar title={t('settings.title')} onBack={onBack} />}>
      <Inset padding="16">
        <Stack gap="12">
        <CardRow
          toggle
          icon={msFingerprint}
          label={t('settings.biometric')}
          description={t('settings.biometricHint')}
          trailing={<Switch checked={biometric} onChange={(e) => setBiometric(e.target.checked)} aria-label={t('settings.biometric')} />}
        />
        <CardRow
          toggle
          icon={msDarkMode}
          label={t('settings.darkMode')}
          description={t('settings.darkModeHint')}
          trailing={
            <Switch
              checked={dark}
              disabled={theme.availableModes.length < 2}
              onChange={(e) => theme.setModePreference(e.target.checked ? 'dark' : 'light')}
              aria-label={t('settings.darkMode')}
            />
          }
        />
        <CardRow
          icon={msAccessibilityNew}
          label={t('settings.accessibility')}
          description={lowVision ? t('settings.accessibilityOn') : t('settings.accessibilityHint')}
          onClick={onAccessibility}
        />
        </Stack>
      </Inset>
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

### `src/shared/data/broker.ts`

```ts
// The broker behind the Lemonn app — shown wherever the app needs its regulatory line (Stocks home, Profile).

/** The broker's regulatory line, in full. */
export const disclosure =
  'The online broking and mutual fund execution services are provided by NU Investors Technologies Pvt. Ltd. | CIN U67200MH2021PTC364704 | SEBI Registration No. INZ000304837 | SEBI DP Registration No. IN-DP-712-2022 | SEBI Research Analyst Registration No. INH000016764 | NSE member code 90251.'
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
