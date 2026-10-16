# Flow: Login (`login`)

> One self-contained file for converting this flow to another platform. Read https://kirtanyak-pe.github.io/lemonn-app-prototype/CONVERT.md first. Design system: Lemonnade V3 @ `7739bca` (components, tokens and rules: https://kirtanyak-pe.github.io/lemonnade-v3-docs/llms.txt).

Welcome, mobile number + OTP, fingerprint, PIN and email — where the app starts.

Prototype: https://kirtanyak-pe.github.io/lemonn-app-prototype/#/flow/login · machine-readable: https://kirtanyak-pe.github.io/lemonn-app-prototype/flows/login.json

## Screens

| Id | Screen | Figma | Taps go to | Note |
|---|---|---|---|---|
| `login-01` | Welcome | [5436:7914](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7914) | `login-02`, `login-06` | Slides auto-advance every 3.5 s (dots jump). Continue with CoinSwitch signs in via CoinSwitch and skips the OTP. |
| `login-02` | Mobile number | [5436:7915](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7915) | `login-01`, `login-04` | Get OTP is off until 10 digits are typed. |
| `login-03` | Mobile number · typed | [5436:7915](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7915) | `login-01`, `login-04` |  |
| `login-04` | OTP | [5436:7916](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7916) | `login-03`, `login-06`, `login-05` | The 6th digit verifies on its own. In the prototype 000000 is a wrong code. |
| `login-05` | OTP · incorrect | [5436:7916](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7916) | `login-03`, `login-06`, `login-05` | Not in the live screenshots — the error state the screen needs. |
| `login-06` | Fingerprint unlock | [5436:7917](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7917) | `login-07` |  |
| `login-07` | Set PIN | [5436:7918](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7918) | `login-09` | New users set the PIN, then confirm it (the live app only had "Enter your PIN"). |
| `login-08` | Enter PIN · returning user | [5436:7918](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7918) | `shell-01`, `login-04` | Opening the app again on this phone. Goes straight into the app. |
| `login-09` | Add email | [5436:7919](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7919) | `kyc-01` | All three choices lead to Account setup (typing the email isn’t drawn yet). |

## Spec (README.md)


> The spec for this flow. A developer should be able to build it on any platform (Flutter, iOS, web) from this file,
> the Figma links and the screens in this folder — without reading the rest of the repo.

- **Prototype:** https://kirtanyak-pe.github.io/lemonn-app-prototype/#/flow/login · one screen: `#/frame/login-01`
- **Figma:** [User flow – auth, account setup](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=2103-86)
  (screenshots of the live app, old design system — this flow redesigns them in L3)
- **Status:** ✅ · last updated 2026-10-16
- **Entry:** app launch (not signed in) · **Exit:** new user → Account setup (`kyc-01`); returning user → the app
  (Stocks)

### Screens

| Id | Screen | Figma | What it shows / when |
|---|---|---|---|
| `login-01` | Welcome | 5436:7914 | Brand hero, 3 auto-advancing slides (3.5 s), "Log in or sign up", "Continue with CoinSwitch", SEBI reg. no. |
| `login-02` | Mobile number | 5436:7915 | Empty field; Get OTP disabled |
| `login-03` | Mobile number · typed | 5436:7915 | 10 digits typed; Get OTP enabled |
| `login-04` | OTP | 5436:7916 | 6 boxes, "Sent to +91 …", Edit number, Resend after 45 s; 6th digit verifies automatically |
| `login-05` | OTP · incorrect | — | Error under the boxes ("Incorrect OTP. You have 2 attempts left."), Resend available |
| `login-06` | Fingerprint unlock | 5436:7917 | Offer biometric unlock — Turn on / Not now |
| `login-07` | Set PIN | 5436:7918 | New 4-digit PIN, then the same screen asks to confirm it |
| `login-08` | Enter PIN · returning user | 5436:7918 | Welcome back + name, PIN, Forgot PIN?, Use fingerprint |
| `login-09` | Add email | 5436:7919 | Continue with Google / Type my email instead / Skip |

### Navigation

```
login-01 ─Log in or sign up─▶ login-02/03 ─Get OTP─▶ login-04 ─6 digits ok─▶ login-06 ─either─▶ login-07 ─PIN confirmed─▶ login-09 ─any─▶ kyc-01
         └Continue with CoinSwitch (SSO, no OTP)──────────────────────────▶ login-06
                                          login-04 ─wrong code─▶ login-05 · Edit number ─▶ login-02
returning user on this phone: app launch ─▶ login-08 ─PIN / fingerprint─▶ app · Forgot PIN? ─▶ OTP (login-04)
```

### Data

From `src/shared/data/user.ts` (fictional sample user) and `data.ts`:
- `user.mobile` (10 digits, shown as `+91 98765 43210`), `user.name` (first name on the returning-user screen).
- `slides[]`: `{ icon, title, description }` — slide 1 is the live app's copy; slides 2–3 are placeholder copy.
- Rules: `OTP_LENGTH` 6, `PIN_LENGTH` 4, `RESEND_AFTER_S` 45, `SEBI_REG`.
- API: send OTP (mobile) → verify OTP (mobile, code) returns new / existing user; set PIN; register biometrics;
  save email (Google ID token or typed + verified).

### Components (L3 → any platform)

| Where | L3 component (Figma name) | Props / variant |
|---|---|---|
| Every screen | SystemStatusbar | (OS bar mock) |
| Welcome hero | BrandLogo ("L3 → Brand logo") | `brand="lemonn"`, 32 |
| Welcome slide art, biometrics, PIN, email | **Illustration slot** (`Spot`, src/shared/ui) | lg 96 disc + halo / md 64; replace with artwork |
| Welcome dock, biometrics, email | ButtonGroup ("L3: Button Dock") | vertical: primary on top, secondary below |
| Continue with CoinSwitch | Button | `variant="secondary"`, BrandLogo coinswitch icon left |
| Phone / OTP back + Help | Actionbar + ActionbarAction | `onBack`; Help = tertiary icon button (`help`) |
| Mobile number | TextField ("L3: input field & text Box") | label "Mobile number", value `+91 …`, numeric |
| OTP | OtpInput | `length={6}`, `edit` (Edit number), `action` (Resend in 0:45 → Resend OTP), `status="error"` |
| PIN | OtpInput | `length={4}`, `mask`, `action` = Forgot PIN? (returning) |
| Use fingerprint | Button | `variant="tertiary"`, Large, icon `fingerprint` |
| Email Skip | Button in Actionbar actions | `variant="ghost"`, `size="sm"` |
| Keypad | — | the phone's system number keyboard (not an app component) |

Headlines are Heading/24 (`--l3-text-heading-24`), supporting lines Description/14, secondary.

### Rules & behaviour

- Mobile: exactly 10 digits, India (+91) only. Get OTP is disabled until then.
- OTP: verifies on the 6th digit (no Verify button) and shows "Verifying…" with a small loader; wrong code → error
  state + attempts left; Resend unlocks after 45 s and restarts the timer; 3 wrong codes → back to the number (server
  rule).
- PIN: set, then confirm; mismatch clears both and shows "The PINs didn't match. Set it again." Weak PINs (1234,
  birth year) → hint, not a block.
- Biometrics: "Not now" just moves on; can be turned on later in Settings.
- Email: Google returns a verified email; typed emails need a verification link (not drawn).
- Returning user: PIN or fingerprint opens the app; Forgot PIN re-verifies by OTP, then Set PIN.

### Differences from Figma

- **Welcome:** the old blue gradient became a lime L3 brand panel. The carousel now has 3 slides with dots.
  The buttons are an L3 dock: the primary "Log in or sign up" on top, the secondary CoinSwitch button below.
- **Phone:** the title says what to do ("Enter your mobile number"), and the old raw number becomes a labelled field.
  A Terms / Privacy line was added.
- **OTP:** the Verify OTP button was removed, because the code auto-verifies. The wrong-code state is new.
- **PIN:** the live app only had "Enter your PIN". New users now set and confirm one. Returning users get the welcome back
  screen with fingerprint.
- **Email:** the abstract blob artwork became an illustration slot, and "Type my email instead" was added. Skip moved
  into the top bar.
- Toasts and other states follow L3 rules: toasts float at the bottom.

### L3 gaps

- **Carousel indicator:** the dots are local (`components/Dots`), because L3 has no carousel or pagination component.
- **Illustrations:** L3 has none for onboarding, so `Spot` slots stand in.
- **Google sign-in mark:** the official "G" mark is needed for the Google button; a mail icon stands in.

### Changelog

- 2026-10-16 — first version (9 screens).


## Source — this flow

### `src/flows/login/components/Dots.module.css`

```css
/* Carousel position: small dots, the current one a wider pill (L3 gap: no carousel indicator yet). */
.dots {
  display: flex;
  justify-content: center;
  gap: var(--l3-spacing-06);
}

.dot {
  position: relative;
  width: var(--l3-size-08);
  height: var(--l3-size-08);
  padding: 0;
  border: 0;
  border-radius: var(--l3-radius-full);
  background: var(--l3-border-intense);
  cursor: pointer;
  transition: width var(--l3-motion-duration-short) var(--l3-motion-easing-standard);
}

/* 48 × 48 touch area, invisible. */
.dot::after {
  content: '';
  position: absolute;
  inset: calc((var(--l3-size-08) - var(--l3-size-tap-target)) / 2) calc(-1 * var(--l3-spacing-04));
}

.dot[aria-current='true'] {
  width: var(--l3-size-24);
  background: var(--l3-content-primary);
}

@media (prefers-reduced-motion: reduce) {
  .dot {
    transition: none;
  }
}
```

### `src/flows/login/components/Dots.tsx`

```tsx
import styles from './Dots.module.css'

/** Carousel position dots (local: L3 has no carousel indicator — listed as an L3 gap). */
export function Dots({ count, index, onPick }: { count: number; index: number; onPick?: (i: number) => void }) {
  return (
    <div className={styles.dots} role="group" aria-label="Slides">
      {Array.from({ length: count }, (_, i) => (
        <button key={i} type="button" className={styles.dot} aria-current={i === index} aria-label={`Slide ${i + 1} of ${count}`} onClick={() => onPick?.(i)} />
      ))}
    </div>
  )
}
```

### `src/flows/login/data.ts`

```ts
import { msCandlestickChart, msQueryStats, msShield } from '@l3/icons/material'

/** Welcome carousel. Slide 1 is the live app's; 2–3 are placeholder copy until product confirms the messages. */
export type Slide = { icon: string; title: string; description: string }

export const slides: Slide[] = [
  { icon: msQueryStats, title: 'Analyst ratings', description: 'Expert ratings on which stocks to buy and sell' },
  { icon: msCandlestickChart, title: 'Stocks, F&O and mutual funds', description: 'Everything you invest in, in one app' },
  { icon: msShield, title: 'Trade with guardrails', description: 'Stop-loss, take-profit and P&L protection built in' },
]

/** Login rules. */
export const OTP_LENGTH = 6
export const PIN_LENGTH = 4
export const RESEND_AFTER_S = 45
/** Any other code is "incorrect" in the prototype; the real check is the server's. */
export const PROTOTYPE_WRONG_OTP = '000000'
export const SEBI_REG = 'INZ000304837'
```

### `src/flows/login/index.tsx`

```tsx
import type { Flow } from '../types'
import { Biometrics } from './screens/Biometrics'
import { Email } from './screens/Email'
import { Otp } from './screens/Otp'
import { Phone } from './screens/Phone'
import { CreatePin, EnterPin } from './screens/Pin'
import { Welcome } from './screens/Welcome'

/**
 * Login — the app's first screens: welcome → mobile number → OTP → fingerprint → PIN → email, then Account setup.
 * Returning users on a known phone go straight to their PIN (login-08). Figma: live screenshots, section 2103:86.
 */
export const login: Flow = {
  id: 'login',
  title: 'Login',
  description: 'Welcome, mobile number + OTP, fingerprint, PIN and email — where the app starts.',
  figmaFile: 'sG24GPizTdwbMBxLm0IpTA',
  screens: [
    {
      id: 'login-01', name: 'Welcome', figma: '5436:7914',
      note: 'Slides auto-advance every 3.5 s (dots jump). Continue with CoinSwitch signs in via CoinSwitch and skips the OTP.',
      render: (go) => <Welcome onLogin={() => go('login-02')} onCoinSwitch={() => go('login-06')} />,
    },
    {
      id: 'login-02', name: 'Mobile number', figma: '5436:7915',
      note: 'Get OTP is off until 10 digits are typed.',
      render: (go) => <Phone onBack={() => go('login-01')} onSubmit={() => go('login-04')} />,
    },
    {
      id: 'login-03', name: 'Mobile number · typed', figma: '5436:7915',
      render: (go) => <Phone initial="9876543210" onBack={() => go('login-01')} onSubmit={() => go('login-04')} />,
    },
    {
      id: 'login-04', name: 'OTP', figma: '5436:7916',
      note: 'The 6th digit verifies on its own. In the prototype 000000 is a wrong code.',
      render: (go) => <Otp initial="48" onBack={() => go('login-03')} onEdit={() => go('login-03')} onVerified={() => go('login-06')} onWrong={() => go('login-05')} />,
    },
    {
      id: 'login-05', name: 'OTP · incorrect', figma: '5436:7916',
      note: 'Not in the live screenshots — the error state the screen needs.',
      render: (go) => <Otp initial="000000" wrong resendIn={0} onBack={() => go('login-03')} onEdit={() => go('login-03')} onVerified={() => go('login-06')} onWrong={() => go('login-05')} />,
    },
    {
      id: 'login-06', name: 'Fingerprint unlock', figma: '5436:7917',
      render: (go) => <Biometrics onAllow={() => go('login-07')} onSkip={() => go('login-07')} />,
    },
    {
      id: 'login-07', name: 'Set PIN', figma: '5436:7918',
      note: 'New users set the PIN, then confirm it (the live app only had "Enter your PIN").',
      render: (go) => <CreatePin initial="12" onDone={() => go('login-09')} />,
    },
    {
      id: 'login-08', name: 'Enter PIN · returning user', figma: '5436:7918',
      note: 'Opening the app again on this phone. Goes straight into the app.',
      render: (go) => <EnterPin initial="1" onUnlocked={() => go('shell-01')} onForgot={() => go('login-04')} />,
    },
    {
      id: 'login-09', name: 'Add email', figma: '5436:7919',
      note: 'All three choices lead to Account setup (typing the email isn’t drawn yet).',
      render: (go) => <Email onGoogle={() => go('kyc-01')} onManual={() => go('kyc-01')} onSkip={() => go('kyc-01')} />,
    },
  ],
}
```

### `src/flows/login/screens/Biometrics.tsx`

```tsx
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { msFingerprint } from '@l3/icons/material'
import { Intro } from '../../../shared/ui/Intro'
import { Screen } from '../../../shared/ui/Screen'
import { Spot } from '../../../shared/ui/Spot'
import styles from './Form.module.css'

/** login-06 · Offer fingerprint / face unlock right after the number is verified. Either choice moves on to the PIN. */
export function Biometrics({ onAllow, onSkip }: { onAllow?: () => void; onSkip?: () => void }) {
  return (
    <Screen
      bottom={
        <ButtonGroup aria-label="Biometric unlock">
          <Button onClick={onAllow}>Turn on fingerprint unlock</Button>
          <Button variant="secondary" onClick={onSkip}>Not now</Button>
        </ButtonGroup>
      }
    >
      <div className={`${styles.centre} ${styles.fill}`}>
        <Intro
          align="center"
          art={<Spot icon={msFingerprint} tone="brand" />}
          title="Unlock Lemonn with your fingerprint"
          description="Faster than typing your PIN, and only you can open the app. You can change this anytime in Settings."
        />
      </div>
    </Screen>
  )
}
```

### `src/flows/login/screens/Email.tsx`

```tsx
import { Actionbar } from '@l3/components/Actionbar'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { Icon } from '@l3/components/Icon'
import { msMail } from '@l3/icons/material'
import { Intro } from '../../../shared/ui/Intro'
import { Screen } from '../../../shared/ui/Screen'
import { Spot } from '../../../shared/ui/Spot'
import styles from './Form.module.css'

/**
 * login-09 · Email (new users). Google fills and verifies it in one tap; typing it is the fallback.
 * Skippable — but KYC needs it later, so the next flow asks again if skipped.
 * Artwork gap: the Google button needs the official Google "G" mark (brand rules) — a mail icon stands in.
 */
export function Email({ onGoogle, onManual, onSkip }: { onGoogle?: () => void; onManual?: () => void; onSkip?: () => void }) {
  return (
    <Screen
      top={<Actionbar actions={<Button variant="ghost" size="sm" onClick={onSkip}>Skip</Button>} />}
      bottom={
        <ButtonGroup aria-label="Add email">
          <Button iconLeft={<Icon icon={msMail} />} onClick={onGoogle}>Continue with Google</Button>
          <Button variant="secondary" onClick={onManual}>Type my email instead</Button>
        </ButtonGroup>
      }
    >
      <div className={`${styles.centre} ${styles.fill}`}>
        <Intro
          align="center"
          art={<Spot icon={msMail} tone="discover" />}
          title="Add your email"
          description="We’ll send your contract notes, statements and account alerts here. It also speeds up your KYC."
        />
      </div>
    </Screen>
  )
}
```

### `src/flows/login/screens/Form.module.css`

```css
/* Shared by the login form screens: the field area under the intro and the small print. */
.fields {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-16);
  padding: var(--l3-spacing-24) var(--l3-spacing-16);
}

.fine {
  margin: 0;
  font: var(--l3-text-description-12);
  color: var(--l3-content-secondary);
}

.link {
  color: var(--l3-content-accent-discover-default);
  font: var(--l3-text-label-12);
  text-decoration: none;
}

.centre {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--l3-spacing-16);
  padding: var(--l3-spacing-24) var(--l3-spacing-16);
}

/* Fills the body and centres its content (moments: biometrics, welcome back). */
.fill {
  flex: 1;
  justify-content: center;
}
```

### `src/flows/login/screens/Otp.tsx`

```tsx
import { useEffect, useState } from 'react'
import { Actionbar } from '@l3/components/Actionbar'
import { Loader } from '@l3/components/Loader'
import { OtpInput } from '@l3/components/OtpInput'
import { usePreview } from '../../../shared/prototype'
import { user } from '../../../shared/data/user'
import { phone } from '../../../shared/data/format'
import { Intro } from '../../../shared/ui/Intro'
import { Keyboard, typeKey, useKeypadFocus } from '../../../shared/ui/Keyboard'
import { Screen } from '../../../shared/ui/Screen'
import { OTP_LENGTH, PROTOTYPE_WRONG_OTP, RESEND_AFTER_S } from '../data'
import styles from './Form.module.css'

type Props = {
  initial?: string
  /** login-05: the code was wrong. */
  wrong?: boolean
  /** Seconds left before "Resend" turns on. */
  resendIn?: number
  onBack?: () => void
  onEdit?: () => void
  onVerified?: () => void
  onWrong?: () => void
}

/**
 * login-04 / 05 · OTP. No Verify button: the 6th digit verifies on its own ("Verifying…" under the boxes).
 * Wrong code → error under the boxes, Resend available. Resend unlocks after 45 s. "Edit" goes back to the number.
 */
export function Otp({ initial = '', wrong = false, resendIn = RESEND_AFTER_S, onBack, onEdit, onVerified, onWrong }: Props) {
  const { interactive } = usePreview()
  const [code, setCode] = useState(initial)
  const [left, setLeft] = useState(resendIn)
  const [verifying, setVerifying] = useState(false)
  const boxes = useKeypadFocus(code, OTP_LENGTH, interactive && !verifying)

  useEffect(() => {
    if (!interactive || left <= 0) return
    const t = window.setTimeout(() => setLeft((s) => s - 1), 1000)
    return () => window.clearTimeout(t)
  }, [interactive, left])

  const complete = (value: string) => {
    if (!interactive || verifying) return
    setVerifying(true)
    window.setTimeout(() => (value === PROTOTYPE_WRONG_OTP ? onWrong : onVerified)?.(), 900)
  }
  const onKey = (k: string) => {
    if (verifying) return
    const next = typeKey(code, k, OTP_LENGTH)
    setCode(next)
    if (next.length === OTP_LENGTH && next !== code) complete(next)
  }

  const helper = verifying
    ? <span className={styles.fine}><Loader size="sm" label={null} /> Verifying…</span>
    : wrong ? 'Incorrect OTP. You have 2 attempts left.' : `Sent to +91 ${phone(user.mobile)}`

  return (
    <Screen top={<Actionbar onBack={onBack} />} bottom={<Keyboard onKey={onKey} />}>
      <Intro title="Verify your number" description="Enter the 6-digit code we sent you by SMS" />
      <div ref={boxes} className={styles.fields}>
        <OtpInput
          length={OTP_LENGTH}
          label="OTP"
          value={code}
          onChange={setCode}
          onComplete={complete}
          status={wrong && !verifying ? 'error' : undefined}
          helperText={helper}
          edit={{ label: 'Edit number', onClick: () => onEdit?.() }}
          action={{ label: left > 0 ? `Resend in 0:${String(left).padStart(2, '0')}` : 'Resend OTP', disabled: left > 0, onClick: () => setLeft(RESEND_AFTER_S) }}
          disabled={verifying}
          autoFocus={interactive}
        />
      </div>
    </Screen>
  )
}
```

### `src/flows/login/screens/Phone.tsx`

```tsx
import { useState } from 'react'
import { Actionbar, ActionbarAction } from '@l3/components/Actionbar'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { TextField } from '@l3/components/TextField'
import { msHelp } from '@l3/icons/material'
import { phone } from '../../../shared/data/format'
import { usePreview } from '../../../shared/prototype'
import { Intro } from '../../../shared/ui/Intro'
import { Keyboard, typeKey } from '../../../shared/ui/Keyboard'
import { Screen } from '../../../shared/ui/Screen'
import styles from './Form.module.css'

/**
 * login-02 / 03 · Mobile number. The number keypad types into the field; "Get OTP" turns on at 10 digits.
 * Indian numbers only (+91 is fixed). The field is focused (Typing state) while the keypad is up.
 * Real app: a number input with the system keypad.
 */
export function Phone({ initial = '', onBack, onSubmit }: { initial?: string; onBack?: () => void; onSubmit?: (mobile: string) => void }) {
  const { interactive } = usePreview()
  const [digits, setDigits] = useState(initial)
  const ready = digits.length === 10
  return (
    <Screen
      top={<Actionbar onBack={onBack} actions={<ActionbarAction icon={msHelp} label="Help" onClick={() => {}} />} />}
      bottom={
        <>
          <ButtonGroup aria-label="Continue">
            <Button disabled={!ready} onClick={() => onSubmit?.(digits)}>Get OTP</Button>
          </ButtonGroup>
          <Keyboard onKey={(k) => setDigits((d) => typeKey(d, k, 10))} />
        </>
      }
    >
      <Intro title="Enter your mobile number" description="We’ll send a 6-digit code to verify it" />
      <div className={styles.fields}>
        <TextField
          label="Mobile number"
          value={digits ? `+91 ${phone(digits)}` : ''}
          placeholder="+91 10-digit number"
          // The keypad is open, so the field is focused: L3's Typing state (dark border + caret). Real app: inputMode
          // "numeric" opens the system number keyboard; here the drawn keypad is that keyboard, so no second one.
          inputMode="none"
          autoComplete="tel-national"
          autoFocus={interactive}
          onChange={(e) => setDigits(e.target.value.replace(/^\+91/, '').replace(/\D/g, '').slice(0, 10))}
        />
        <p className={styles.fine}>
          By continuing, you agree to our <a className={styles.link} href="#terms">Terms</a> and{' '}
          <a className={styles.link} href="#privacy">Privacy policy</a>.
        </p>
      </div>
    </Screen>
  )
}
```

### `src/flows/login/screens/Pin.tsx`

```tsx
import { useState } from 'react'
import { BrandLogo } from '@l3/components/BrandLogo'
import { Button } from '@l3/components/Button'
import { Icon } from '@l3/components/Icon'
import { OtpInput } from '@l3/components/OtpInput'
import { msFingerprint, msPassword } from '@l3/icons/material'
import { usePreview } from '../../../shared/prototype'
import { user } from '../../../shared/data/user'
import { Intro } from '../../../shared/ui/Intro'
import { Keyboard, typeKey, useKeypadFocus } from '../../../shared/ui/Keyboard'
import { Screen } from '../../../shared/ui/Screen'
import { Spot } from '../../../shared/ui/Spot'
import { PIN_LENGTH } from '../data'
import styles from './Form.module.css'

/**
 * login-07 · New user: set a 4-digit PIN, then type it again to confirm (a mismatch clears it and says so).
 * The PIN unlocks the app and approves orders.
 */
export function CreatePin({ initial = '', onDone }: { initial?: string; onDone?: () => void }) {
  const { interactive } = usePreview()
  const [first, setFirst] = useState<string | null>(null)
  const [pin, setPin] = useState(initial)
  const [mismatch, setMismatch] = useState(false)
  const boxes = useKeypadFocus(pin, PIN_LENGTH, interactive)

  const onKey = (k: string) => {
    const next = typeKey(pin, k, PIN_LENGTH)
    setPin(next)
    if (!interactive || next.length < PIN_LENGTH || next === pin) return
    window.setTimeout(() => {
      if (first === null) { setFirst(next); setPin(''); setMismatch(false) }
      else if (next === first) onDone?.()
      else { setFirst(null); setPin(''); setMismatch(true) }
    }, 250)
  }

  const confirming = first !== null
  return (
    <Screen bottom={<Keyboard onKey={onKey} />}>
      <Intro
        art={<Spot icon={msPassword} tone="brand" size="md" />}
        title={confirming ? 'Confirm your PIN' : 'Set a 4-digit PIN'}
        description={confirming ? 'Type the same PIN again' : 'You’ll use it to open the app and approve orders'}
      />
      <div ref={boxes} className={styles.fields}>
        <OtpInput
          length={PIN_LENGTH}
          mask
          label={confirming ? 'Confirm PIN' : 'New PIN'}
          value={pin}
          onChange={setPin}
          status={mismatch ? 'error' : undefined}
          helperText={mismatch ? 'The PINs didn’t match. Set it again.' : 'Avoid 1234 or your birth year'}
        />
      </div>
    </Screen>
  )
}

/**
 * login-08 · Returning user (already registered on this phone): unlock with the PIN or fingerprint. 
 * "Forgot PIN?" re-verifies by OTP (not drawn here).
 */
export function EnterPin({ initial = '', onUnlocked, onForgot }: { initial?: string; onUnlocked?: () => void; onForgot?: () => void }) {
  const { interactive } = usePreview()
  const [pin, setPin] = useState(initial)
  const boxes = useKeypadFocus(pin, PIN_LENGTH, interactive)
  const onKey = (k: string) => {
    const next = typeKey(pin, k, PIN_LENGTH)
    setPin(next)
    if (interactive && next.length === PIN_LENGTH && next !== pin) window.setTimeout(() => onUnlocked?.(), 300)
  }
  return (
    <Screen bottom={<Keyboard onKey={onKey} />}>
      <div ref={boxes} className={`${styles.centre} ${styles.fill}`}>
        <BrandLogo brand="lemonn" variant="icon" size={48} decorative />
        <Intro align="center" title={`Welcome back, ${user.name.split(' ')[0]}`} description="Enter your 4-digit PIN" />
        <OtpInput
          length={PIN_LENGTH}
          mask
          label="PIN"
          value={pin}
          onChange={setPin}
          action={{ label: 'Forgot PIN?', onClick: () => onForgot?.() }}
        />
        <Button variant="tertiary" iconLeft={<Icon icon={msFingerprint} />} onClick={onUnlocked}>Use fingerprint</Button>
      </div>
    </Screen>
  )
}
```

### `src/flows/login/screens/Welcome.module.css`

```css
/* Welcome: a lime hero panel (logo + promise + the slide's art), the slide's message, dots; the dock below. */
.page {
  display: flex;
  flex-direction: column;
  flex: 1;
}

.hero {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--l3-spacing-08);
  padding: var(--l3-spacing-32) var(--l3-spacing-16) var(--l3-spacing-40);
  border-radius: 0 0 var(--l3-radius-32) var(--l3-radius-32);
  background: var(--l3-surface-accent-brand-light);
}

.tagline {
  margin: 0;
  font: var(--l3-text-label-14);
  color: var(--l3-content-secondary);
}

.art {
  margin-top: var(--l3-spacing-40);
}

.slide {
  display: flex;
  flex: 1;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: var(--l3-spacing-08);
  padding: var(--l3-spacing-24);
  text-align: center;
}

.title {
  margin: 0;
  font: var(--l3-text-heading-24);
}

.description {
  margin: 0;
  font: var(--l3-text-description-14);
  color: var(--l3-content-secondary);
}

.dots {
  margin-top: var(--l3-spacing-16);
}

.legal {
  margin: 0;
  padding: 0 var(--l3-spacing-16) var(--l3-spacing-16);
  font: var(--l3-text-description-10);
  color: var(--l3-content-tertiary);
  text-align: center;
  background: var(--l3-surface-primary);
}
```

### `src/flows/login/screens/Welcome.tsx`

```tsx
import { useEffect, useState } from 'react'
import { BrandLogo } from '@l3/components/BrandLogo'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { usePreview } from '../../../shared/prototype'
import { Screen } from '../../../shared/ui/Screen'
import { Spot } from '../../../shared/ui/Spot'
import { Dots } from '../components/Dots'
import { SEBI_REG, slides } from '../data'
import styles from './Welcome.module.css'

/** login-01 · First screen of the app: what Lemonn is (3 slides, auto-advancing), then sign in. */
export function Welcome({ onLogin, onCoinSwitch }: { onLogin?: () => void; onCoinSwitch?: () => void }) {
  const { interactive } = usePreview()
  const [index, setIndex] = useState(0)
  useEffect(() => {
    if (!interactive) return
    const t = window.setInterval(() => setIndex((i) => (i + 1) % slides.length), 3500)
    return () => window.clearInterval(t)
  }, [interactive])
  const slide = slides[index]

  return (
    <Screen
      bottom={
        <>
          <ButtonGroup aria-label="Get started">
            <Button onClick={onLogin}>Log in or sign up</Button>
            <Button variant="secondary" iconLeft={<BrandLogo brand="coinswitch" variant="icon" size={24} decorative />} onClick={onCoinSwitch}>
              Continue with CoinSwitch
            </Button>
          </ButtonGroup>
          <p className={styles.legal} data-surface="primary">SEBI Reg. No. {SEBI_REG}</p>
        </>
      }
    >
      <div className={styles.page}>
        <div className={styles.hero} data-status-surface="brand">
          <BrandLogo brand="lemonn" size={32} />
          <p className={styles.tagline}>Stock trading made simple</p>
          <div className={styles.art}><Spot icon={slide.icon} tone="plain" /></div>
        </div>
        <div className={styles.slide}>
          <div aria-live="polite">
            <h1 className={styles.title}>{slide.title}</h1>
            <p className={styles.description}>{slide.description}</p>
          </div>
          <div className={styles.dots}><Dots count={slides.length} index={index} onPick={setIndex} /></div>
        </div>
      </div>
    </Screen>
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
// Prototype behaviour flows may use — the ONLY way a flow touches the preview. In a real app these are: interactive =
// always true; useAutoAdvance = "when the server answers, go on" (a loader / polling step).
export { LOADING_MS, usePreview, useAutoAdvance } from '../preview/context'
```

### `src/shared/ui/Intro.module.css`

```css
/* A page's opening: optional illustration, headline (Heading/24), supporting line. Left-aligned for forms, centred
 * for moments (welcome, results, loaders). */
.intro {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-08);
  padding: var(--l3-spacing-24) var(--l3-spacing-16) 0;
}

.intro[data-align='center'] {
  align-items: center;
  text-align: center;
}

.art {
  margin-bottom: var(--l3-spacing-24);
}

.title {
  margin: 0;
  font: var(--l3-text-heading-24);
}

.description {
  margin: 0;
  font: var(--l3-text-description-14);
  color: var(--l3-content-secondary);
}
```

### `src/shared/ui/Intro.tsx`

```tsx
import type { ReactNode } from 'react'
import styles from './Intro.module.css'

/**
 * The top of an onboarding / result page: an optional illustration (<Spot>), the page's headline (Heading/24 — the
 * screen's h1, the Actionbar has no title on these pages) and a supporting line. `children` go right under it.
 */
export function Intro({ title, description, art, align = 'start', children }: {
  title: ReactNode
  description?: ReactNode
  art?: ReactNode
  align?: 'start' | 'center'
  children?: ReactNode
}) {
  return (
    <header className={styles.intro} data-align={align}>
      {art && <div className={styles.art}>{art}</div>}
      <h1 className={styles.title}>{title}</h1>
      {description && <p className={styles.description}>{description}</p>}
      {children}
    </header>
  )
}
```

### `src/shared/ui/Keyboard.module.css`

```css
/* OS number keypad mock: 3 × 4 keys on a grey tray. */
.keyboard {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--app-space-key);
  padding: var(--l3-spacing-12) var(--l3-spacing-12) var(--l3-spacing-24);
  /* On a real phone, clear the home indicator (like the L3 button dock does). */
  padding-bottom: calc(var(--l3-spacing-24) + env(safe-area-inset-bottom, var(--l3-spacing-00)));
  background: var(--l3-surface-secondary);
}

.key {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--l3-spacing-04);
  height: var(--app-size-key);
  border: 0;
  border-radius: var(--l3-radius-08);
  background: var(--l3-surface-primary);
  color: var(--l3-content-primary);
  font: var(--l3-text-description-18);
  cursor: pointer;
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
}

.key[data-kind='fn'] {
  background: var(--l3-surface-tertiary);
}


.key:active {
  transform: scale(var(--l3-motion-scale-press-default));
}
```

### `src/shared/ui/Keyboard.tsx`

```tsx
import { useEffect, useRef, type PointerEvent } from 'react'
import { Icon } from '@l3/components/Icon'
import { msBackspace } from '@l3/icons/material'
import styles from './Keyboard.module.css'

const KEYS = ['1', '2', '3', '4', '5', '6', '7', '8', '9', '', '0', 'Backspace']

/**
 * The phone's own number keypad (OS chrome, NOT part of the app or L3 — real apps get the system keyboard by setting
 * the field's input type to number). Drawn so the phone / OTP / PIN screens read like the phone. In the prototype the
 * keys type into the screen. `data-surface` tells the OS navigation bar below it to take the keypad's grey.
 */
export function Keyboard({ onKey }: { onKey?: (key: string) => void }) {
  return (
    <div className={styles.keyboard} role="group" aria-label="Number keypad" data-surface="secondary">
      {KEYS.map((k) =>
        k === '' ? <span key="blank" /> : (
          <button key={k} type="button" className={styles.key} data-kind={k === 'Backspace' ? 'fn' : 'digit'} aria-label={k === 'Backspace' ? 'Delete' : undefined} onPointerDown={keepFocus} onClick={() => onKey?.(k)}>
            {k === 'Backspace' ? <Icon icon={msBackspace} /> : k}
          </button>
        ),
      )}
    </div>
  )
}

/** Like the OS keyboard, a key press doesn't take focus: the field being typed into keeps its Typing state. */
const keepFocus = (e: PointerEvent) => e.preventDefault()

/** Apply a keypad key to a digit string (max length). */
export const typeKey = (value: string, key: string, max: number) =>
  key === 'Backspace' ? value.slice(0, -1) : /^\d$/.test(key) && value.length < max ? value + key : value

/**
 * Prototype glue: the drawn keypad types into an L3 OtpInput, so focus follows the box being typed (the next empty
 * one) — the Typing state moves along as it would with the phone's keyboard. Put the ref on an element around the
 * OtpInput. Real apps don't need this: the system keyboard types into the focused box.
 */
export function useKeypadFocus(value: string, length: number, enabled: boolean) {
  const ref = useRef<HTMLDivElement>(null)
  useEffect(() => {
    if (!enabled) return
    const boxes = ref.current?.querySelectorAll('input')
    boxes?.[Math.min(value.length, length - 1)]?.focus({ preventScroll: true })
  }, [value, length, enabled])
  return ref
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
}

/**
 * The frame of every screen: top · body · bottom, a toast above the bottom, and a sheet over it all. It fills the
 * space between the OS status bar and the OS navigation bar (the preview draws those — real apps get them from the
 * OS, edge-to-edge insets), so nothing sits under the gesture handle. The screen is the sheet's container, so sheets
 * and their scrim stay in the app; its SheetHost keeps one sheet open while the screen's sheet changes (no scrim flash
 * between two sheets). Mark a bottom part with data-surface="primary" | "secondary" if the navigation bar should take
 * its colour (the button dock and the bottom navbar are recognised already).
 */
export function Screen({ top, bottom, toast, sheet, children }: ScreenProps) {
  const [el, setEl] = useState<HTMLDivElement | null>(null)
  const { screenId } = usePreview()
  return (
    <div ref={setEl} className={styles.screen}>
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
