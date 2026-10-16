# Flow: Account setup (`kyc`)

> One self-contained file for converting this flow to another platform. Read https://kirtanyak-pe.github.io/lemonn-app-prototype/CONVERT.md first. Design system: Lemonnade V3 @ `7739bca` (components, tokens and rules: https://kirtanyak-pe.github.io/lemonnade-v3-docs/llms.txt).

KYC after login: PAN, details and declarations, nominee, bank account and F&O activation.

Prototype: https://kirtanyak-pe.github.io/lemonn-app-prototype/#/flow/kyc · machine-readable: https://kirtanyak-pe.github.io/lemonn-app-prototype/flows/kyc.json

## Screens

| Id | Screen | Figma | Taps go to | Note |
|---|---|---|---|---|
| `kyc-01` | Start | [5436:7920](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7920) | `kyc-02` | Arrives with the "Email verified" toast from Login. |
| `kyc-02` | PAN found | [5436:7921](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7921) | `kyc-04`, `kyc-03`, `kyc-01` | The PAN linked to the mobile number, masked, for confirmation. |
| `kyc-03` | PAN · typed | [5436:7922](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7922) | `kyc-01`, `kyc-04` |  |
| `kyc-04` | Getting your details | [5436:7923](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7923) | — | Moves on by itself when the three come back. |
| `kyc-05` | Getting your details · slow network | [5436:7924](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7924) | — |  |
| `kyc-06` | Confirm details | [5436:7925](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7925) | `kyc-03`, `kyc-07`, `kyc-08`, `kyc-13` | Confirm turns on when the declarations are agreed. |
| `kyc-07` | F&O risk disclosure | [5436:7932](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7932) | `kyc-06` | Shown when F&O is ticked. |
| `kyc-08` | Add a nominee | [5436:7926](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7926) | `kyc-06`, `kyc-09` |  |
| `kyc-09` | Nominee · date of birth | [5436:7927](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7927) | `kyc-10`, `kyc-08` |  |
| `kyc-10` | Nominees | [5436:7928](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7928) | `kyc-08`, `kyc-11` |  |
| `kyc-11` | Confirm details · nominee added | [5436:7933](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7933) | `kyc-03`, `kyc-07`, `kyc-12`, `kyc-13` |  |
| `kyc-12` | Nominee details | [5436:7931](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7931) | `kyc-11`, `kyc-08` |  |
| `kyc-13` | Bank account found | [5436:7934](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7934) | `kyc-15`, `kyc-14` |  |
| `kyc-14` | Bank account · typed | [5436:7934](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7934) | `kyc-11`, `kyc-15` |  |
| `kyc-15` | Verifying bank (₹1) | [5436:7935](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7935) | — | Penny drop. Moves on by itself. |
| `kyc-16` | Bank verified | [5436:7936](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7936) | `kyc-17`, `kyc-20` |  |
| `kyc-17` | Share bank statement | [5436:7937](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7937) | `kyc-16`, `kyc-18`, `kyc-20` |  |
| `kyc-18` | CAMSfinserv (external) | [5436:7938](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7938) | `kyc-17` | Handoff placeholder: CAMSfinserv’s own OTP, account and consent pages (frames 5436:7939, 5436:7913, 5436:7940) are not built by Lemonn. |
| `kyc-19` | Statement received | [5436:7941](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7941) | `kyc-20` |  |
| `kyc-20` | You’re all set | [5436:7951](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=5436-7951) | `deposit-01`, `shell-01` | Replaces the live "KYC already completed" web page. Add money → First deposit. |

## Spec (README.md)


> The spec for this flow. A developer should be able to build it on any platform (Flutter, iOS, web) from this file,
> the Figma links and the screens in this folder — without reading the rest of the repo.

- **Prototype:** https://kirtanyak-pe.github.io/lemonn-app-prototype/#/flow/kyc · one screen: `#/frame/kyc-01`
- **Figma:** [User flow – auth, account setup](https://www.figma.com/design/sG24GPizTdwbMBxLm0IpTA/?node-id=2103-86)
  (screenshots of the live app, old design system — this flow redesigns them in L3)
- **Status:** ✅ · last updated 2026-10-16
- **Entry:** Login, after the email step (`login-09`), arriving with an "Email verified" toast
- **Exit:** "Add money" → First deposit (`deposit-01`); "Explore stocks first" → the app

Three stages, shown up front and tracked in a progress bar under the top bar: **1 Verify your identity** →
**2 Add your bank account** → **3 Activate F&O (optional)**.

### Screens

| Id | Screen | Figma | What it shows / when |
|---|---|---|---|
| `kyc-01` | Start | 5436:7920 | "Let's open your account": the 3 stages with times, a trust line, Start. "Email verified" toast. |
| `kyc-02` | PAN found | 5436:7921 | Sheet over the PAN screen: name + masked PAN linked to the mobile. Use it / Enter a different PAN. |
| `kyc-03` | PAN · typed | 5436:7922 | PAN field (format hint, error if the pattern is wrong). Continue turns on when it's valid. |
| `kyc-04` | Getting your details | 5436:7923 | Fetch from the government database: PAN ✓, Personal (loading), Bank (to do). Moves on by itself. |
| `kyc-05` | Getting your details · slow network | 5436:7924 | Same, with a "Slow connection" warning toast. |
| `kyc-06` | Confirm details | 5436:7925 | Read-only personal details, segments, nominee, unused-funds setting, declarations + one agree tick. |
| `kyc-07` | F&O risk disclosure | 5436:7932 | Sheet when F&O is ticked: SEBI's 4-point risk summary + source. "I understand". |
| `kyc-08` | Add a nominee | 5436:7926 | Relationship (pills), name, date of birth, last 4 of Aadhaar, "Same contact details as mine". |
| `kyc-09` | Nominee · date of birth | 5436:7927 | Date picker sheet over the filled form. |
| `kyc-10` | Nominees | 5436:7928 | Saved nominee(s) with their share. Add another / Done. |
| `kyc-11` | Confirm details · nominee added | 5436:7933 | kyc-06 with the nominee row (100%), declarations agreed, "Nominee added" toast. |
| `kyc-12` | Nominee details | 5436:7931 | Sheet from the nominee row: their details. Edit / Done. |
| `kyc-13` | Bank account found | 5436:7934 | Sheet over the bank screen: bank, masked account, IFSC, type. Use it / Add a different account. |
| `kyc-14` | Bank account · typed | 5436:7934 | Account number twice + IFSC; a valid IFSC shows the bank and branch. |
| `kyc-15` | Verifying bank (₹1) | 5436:7935 | Penny-drop sheet (can't be dismissed). Moves on by itself. |
| `kyc-16` | Bank verified | 5436:7936 | Sheet: verified + progress (identity ✓, bank ✓, F&O next). Activate F&O / Skip for now. |
| `kyc-17` | Share bank statement | 5436:7937 | Why F&O needs income proof and how it's fetched (CAMSfinserv, read-only, ~2 min). |
| `kyc-18` | CAMSfinserv (external) | 5436:7938 | **Handoff placeholder** — CAMSfinserv's own pages (5436:7939, 5436:7913, 5436:7940) are not Lemonn's. |
| `kyc-19` | Statement received | 5436:7941 | F&O under review, 24–48 hours, we'll notify you. |
| `kyc-20` | You're all set | 5436:7951 | "You're all set, Rohan": identity ✓, bank ✓, F&O under review. Add money / Explore stocks first. |

### Navigation

```
kyc-01 ─Start─▶ kyc-02 (PAN found sheet) ─Use this PAN─▶ kyc-04 ─auto─▶ kyc-06
                  └Enter a different PAN─▶ kyc-03 ─Continue─▶ kyc-04          (slow network: kyc-05)
kyc-06 ─tick F&O─▶ kyc-07 (risk sheet) ─I understand─▶ kyc-06
       ─Add a nominee─▶ kyc-08 ─date field─▶ kyc-09 (date sheet) ─Confirm─▶ kyc-10 ─Done─▶ kyc-11
                         └Skip─▶ kyc-06                                     └Add another─▶ kyc-08
kyc-11 ─nominee row─▶ kyc-12 (sheet) ─Edit─▶ kyc-08 · Done─▶ kyc-11
kyc-06 / kyc-11 ─Confirm and continue─▶ kyc-13 (bank found sheet) ─Use this account─▶ kyc-15
                                           └Add a different account─▶ kyc-14 ─Verify account─▶ kyc-15
kyc-15 ─auto (₹1 sent)─▶ kyc-16 ─Activate F&O─▶ kyc-17 ─Continue to CAMSfinserv─▶ kyc-18 ─approved─▶ kyc-19 ─▶ kyc-20
                           └Skip for now────────────▶ kyc-20         └Do this later─▶ kyc-20
kyc-20 ─Add money─▶ deposit-01 · Explore stocks first─▶ the app
```

### Data

From `src/shared/data/user.ts` (fictional sample user — never real customer data) and `data.ts`:
- `user`: name, father's name, date of birth, occupation, marital status, PAN (`ABCPM1234K`, shown masked
  `ABCPM••••K`).
- `bankAccount` / `foundBank`: bank, account number (shown `••••••••••7821`), IFSC, type, branch.
- `stages[]`, `fetchSteps[]`, `segments[]` (`cash` always on; `fno` optional), `declarations[]` (8 regulatory
  statements), `settlementOptions` (30 / 90 days), `relationships[]`, `nominee`, `PENNY_DROP` (₹1),
  `riskDisclosure[]`.
- API: PAN lookup by mobile → fetch PAN / personal / bank details from the KRA (three results, each can be slow) →
  save segments, settlement, declarations (one consent with timestamp) and nominees → bank lookup by PAN, or IFSC
  lookup + penny drop → account-aggregator consent (CAMSfinserv) → F&O review status.

### Components (L3 → any platform)

| Where | L3 component (Figma name) | Props / variant |
|---|---|---|
| Top bar | Actionbar | title + `onBack`; kyc-01 has an empty bar |
| Stage progress under the bar | ProgressBar (`components/StageBar`) | "Step 1 of 3 · Verify your identity", value by stage |
| Headlines | `Intro` (src/shared/ui) | Heading/24 title + Description/14 secondary; `align="center"` on result screens |
| Stage list, fetch list, progress | `Steps` (src/shared/ui) | done ✓ / current / loading / to do, with a connector line |
| Illustration | **Illustration slot** (`Spot`, src/shared/ui) | md 64 in sheet headers and kyc-17, lg 96 + halo on result screens |
| Sheets | BottomSheet + BottomSheetHeader | `size="lg"` + `icon` for confirmations; small header for details / date |
| Sheet and screen buttons | ButtonGroup ("L3: Button Dock") | vertical: primary over secondary; horizontal for Edit / Done, Skip / Save, Add another / Done |
| Facts (PAN, bank, personal, nominee) | KeyValueList + KeyValueRow | `variant="card"` |
| Segments, agree tick, same-contact tick | ListCell | `variant="card"`, `as="label"`, `multiline`, Checkbox in `trailing` |
| Nominee row, Add a nominee | ListCell | `variant="card"`, `as="button"`, chevron `iconRight`, share in a neutral Tag |
| Unused-funds setting | ListCell + Select | Select `size="sm"` (opens a 30 / 90 days picker) |
| Declarations | Card | bulleted Description/12 list |
| Relationship | Tabs | `appearance="pill"`, `emphasis="secondary"`, `behavior="toggle"` |
| Fields | TextField | PAN, name, Aadhaar last 4, account number ×2, IFSC (`status="success"` + bank/branch helper) |
| Date of birth | TextField (read-only, calendar icon) → DatePicker in a sheet | `max` = today |
| Toasts | Aerobar | `floating`, `emphasis="primary"`: success (Email verified, Nominee added), warning (Slow connection) |
| Loaders | Loader | `size="lg"` in the penny-drop sheet and the handoff screen |
| CAMSfinserv handoff | `External` (src/shared/ui) | filled Card + "External" Tag + the provider's steps |

### Rules & behaviour

- **PAN:** 5 letters, 4 digits, 1 letter (`/^[A-Z]{5}\d{4}[A-Z]$/`), auto-capitalised. If a PAN is linked to the
  mobile, it's offered first (masked); otherwise the field.
- **Fetching:** three calls (PAN, personal, bank); each row turns ✓ as it returns; after ~5 s without a result show
  the "Slow connection" toast; moves on when all three are back. Personal details are read-only — corrections go
  through support.
- **Segments:** Stocks & mutual funds always on (disabled tick). Ticking F&O shows the risk disclosure sheet
  (SEBI-mandated) every time; un-ticking needs no confirmation.
- **Declarations:** listed in full, agreed with one tick; "Confirm and continue" stays off until it's ticked. Store
  the consent with a timestamp.
- **Nominee:** optional ("Recommended · you can skip this"). Relationship, full name, date of birth (not in the
  future), last 4 of Aadhaar (not verified). One nominee = 100%; with several, shares must add up to 100% (up to 10).
- **Unused funds:** running-account settlement every 30 or 90 days (default 90).
- **Bank:** account number typed twice and must match; IFSC is 11 characters and looked up to show the bank and
  branch. Penny drop sends ₹1 shown as "Lemonn" on the statement; the name at the bank must match the PAN name (on a
  mismatch: an error sheet and "Add a different account" — not drawn).
- **F&O:** optional; "Skip for now" / "Do this later" both go to kyc-20 with F&O not started. Approved on CAMSfinserv
  → review, usually 24–48 hours, with a notification.
- **You're all set** shows what's done and what's pending (F&O under review only if it was started).

### Differences from Figma

- **One flow for everything after login.** The live app jumps between native screens and a web page ("KYC already
  completed"); kyc-20 replaces that page with a native "You're all set" that leads into the first deposit.
- **Stage progress** (`Step x of 3`) under every top bar, so people always know how far they are.
- **PAN found** became a confirmation sheet with the PAN masked, instead of a plain screen.
- **Declarations:** the separate toggles became one readable list + one "I agree" tick.
- **F&O risk disclosure** opens as a sheet when F&O is ticked, instead of a separate page.
- **Nominee:** relationship as pill choices, the address/email/phone question as one tick, a nominee summary sheet
  from Confirm details.
- **Bank:** the penny drop and the verified state are sheets over the bank screen; the verified sheet shows the
  overall progress and offers F&O.
- **CAMSfinserv** (third party) is a handoff placeholder (kyc-18), not a redraw of their pages.
- **Artwork:** old illustrations became illustration slots; all customer data in the screenshots was replaced with the
  fictional sample user.

### L3 gaps

- **Illustrations:** L3 has none for onboarding / results; `Spot` slots stand in (marked `data-illustration-slot`).
- **Steps / timeline:** L3 has no vertical step list; `Steps` (src/shared/ui) is built from L3 tokens and icons.
- **Stage progress:** made from ProgressBar + a label (`components/StageBar`); an L3 "stepper header" would replace it.

### Changelog

- 2026-10-16 — first version (20 screens).


## Source — this flow

### `src/flows/kyc/components/StageBar.module.css`

```css
/* Under the Actionbar on the setup forms: "Step 1 of 3 · Verify your identity" over a thin progress bar. */
.bar {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-08);
  padding: var(--l3-spacing-12) var(--l3-spacing-16) 0;
}

.label {
  font: var(--l3-text-label-12);
  color: var(--l3-content-secondary);
}
```

### `src/flows/kyc/components/StageBar.tsx`

```tsx
import { ProgressBar } from '@l3/components/ProgressBar'
import { stages } from '../data'
import styles from './StageBar.module.css'

/** Where the user is in account setup (stage 1–3). */
export function StageBar({ stage }: { stage: 1 | 2 | 3 }) {
  const name = stages[stage - 1].label
  return (
    <div className={styles.bar}>
      <span className={styles.label}>Step {stage} of {stages.length} · {name}</span>
      <ProgressBar size="sm" value={(stage / stages.length) * 100 - 15} label={`Account setup: step ${stage} of ${stages.length}`} />
    </div>
  )
}
```

### `src/flows/kyc/data.ts`

```ts
import { bankAccount } from '../../shared/data/user'

/** The three stages shown at the start and the end of account setup. */
export const stages = [
  { label: 'Verify your identity', description: 'PAN and personal details · about 2 min' },
  { label: 'Add your bank account', description: 'To add money and withdraw · about 1 min' },
  { label: 'Activate F&O (optional)', description: 'Needs a bank statement · about 2 min' },
]

/** What's fetched from the government (NSDL / KRA) database after PAN confirmation. */
export const fetchSteps = ['PAN details', 'Personal details', 'Bank details']

/** Segments the user can trade. Stocks & mutual funds are always on; F&O needs income proof. */
export type Segment = { id: 'cash' | 'fno'; label: string; description: string; required?: boolean }
export const segments: Segment[] = [
  { id: 'cash', label: 'Stocks & mutual funds', description: 'Always included', required: true },
  { id: 'fno', label: 'Futures & options (F&O)', description: 'Needs a bank statement — you’ll add it after the bank' },
]

/** Regulatory declarations (the live app's list, reworded). The user agrees to all of them once. */
export const declarations = [
  'I am not a politically exposed person.',
  'I am an Indian resident — not a US person, US citizen / green card holder or tax resident of any other country.',
  'I accept the DDPI (Demat Debit and Pledge Instruction).',
  'I accept the BSE StAR MF terms.',
  'Send contract notes (ECN) and other communications to my email.',
  'I am capable of operating my account on my own.',
  'I have no past actions from SEBI or an exchange in the last 3 years.',
  'Print my nominee details in my statements of holdings.',
]

/** Running-account settlement: how often unused funds go back to the bank. */
export const settlementOptions = ['30 days', '90 days']

export type Relationship = 'Mother' | 'Father' | 'Spouse' | 'Son' | 'Daughter' | 'Other'
export const relationships: Relationship[] = ['Father', 'Mother', 'Spouse', 'Son', 'Daughter', 'Other']

export type Nominee = { name: string; relationship: Relationship; dob: string; aadhaarLast4: string; share: number }
export const nominee: Nominee = { name: 'Suresh Mehta', relationship: 'Father', dob: '27 Apr 1967', aadhaarLast4: '2390', share: 100 }

/** Bank found through the PAN / mobile (shown for confirmation) and the ₹1 penny-drop check. */
export const foundBank = bankAccount
export const PENNY_DROP = 1

/** SEBI's mandated F&O risk disclosure (summary). */
export const riskDisclosure = [
  '9 out of 10 individual traders in equity F&O made net losses.',
  'On average, loss makers lost close to ₹50,000.',
  'Loss makers spent an extra 28% of their net trading losses on transaction costs.',
  'Traders who made a profit spent 15% to 50% of it on transaction costs.',
]
```

### `src/flows/kyc/index.tsx`

```tsx
import { Aerobar } from '@l3/components/Aerobar'
import { Icon } from '@l3/components/Icon'
import { msCheckCircle } from '@l3/icons/material'
import { user } from '../../shared/data/user'
import type { Flow } from '../types'
import { Bank } from './screens/Bank'
import { Details } from './screens/Details'
import { Done } from './screens/Done'
import { Fetching } from './screens/Fetching'
import { FnoConsent, FnoHandoff, FnoReceived } from './screens/Fno'
import { NomineeForm, NomineeList } from './screens/Nominee'
import { Pan } from './screens/Pan'
import { Start } from './screens/Start'
import { BankFoundSheet, BankVerifiedSheet, VerifyingBankSheet } from './sheets/BankSheets'
import { DobSheet } from './sheets/DobSheet'
import { NomineeSheet } from './sheets/NomineeSheet'
import { PanFoundSheet } from './sheets/PanFoundSheet'
import { RiskSheet } from './sheets/RiskSheet'

const nomineeAdded = <Aerobar floating type="success" emphasis="primary" icon={<Icon icon={msCheckCircle} />} heading="Nominee added" />

/**
 * Account setup (KYC) — after Login: identity (PAN + government fetch), details & declarations, nominee, bank
 * (penny drop), F&O activation via the CAMSfinserv account aggregator, then "You're all set" → First deposit.
 */
export const kyc: Flow = {
  id: 'kyc',
  title: 'Account setup',
  description: 'KYC after login: PAN, details and declarations, nominee, bank account and F&O activation.',
  figmaFile: 'sG24GPizTdwbMBxLm0IpTA',
  sections: [
    { id: 'identity', title: 'Identity' },
    { id: 'details', title: 'Details & nominee' },
    { id: 'bank', title: 'Bank account' },
    { id: 'fno', title: 'Activate F&O' },
    { id: 'done', title: 'Done' },
  ],
  screens: [
    { id: 'kyc-01', section: 'identity', name: 'Start', figma: '5436:7920', note: 'Arrives with the "Email verified" toast from Login.', render: (go) => <Start onStart={() => go('kyc-02')} /> },
    {
      id: 'kyc-02', section: 'identity', name: 'PAN found', figma: '5436:7921',
      note: 'The PAN linked to the mobile number, masked, for confirmation.',
      render: (go) => <Pan sheet={<PanFoundSheet onUse={() => go('kyc-04')} onOther={() => go('kyc-03')} />} onBack={() => go('kyc-01')} />,
    },
    { id: 'kyc-03', section: 'identity', name: 'PAN · typed', figma: '5436:7922', render: (go) => <Pan value={user.pan} onBack={() => go('kyc-01')} onSubmit={() => go('kyc-04')} /> },
    { id: 'kyc-04', section: 'identity', name: 'Getting your details', figma: '5436:7923', note: 'Moves on by itself when the three come back.', render: () => <Fetching done={1} next="kyc-06" /> },
    { id: 'kyc-05', section: 'identity', name: 'Getting your details · slow network', figma: '5436:7924', render: () => <Fetching done={3} slow next="kyc-06" /> },

    { id: 'kyc-06', section: 'details', name: 'Confirm details', figma: '5436:7925', note: 'Confirm turns on when the declarations are agreed.', render: (go) => <Details onBack={() => go('kyc-03')} onFno={() => go('kyc-07')} onAddNominee={() => go('kyc-08')} onConfirm={() => go('kyc-13')} /> },
    { id: 'kyc-07', section: 'details', name: 'F&O risk disclosure', figma: '5436:7932', note: 'Shown when F&O is ticked.', render: (go) => <Details sheet={<RiskSheet onAccept={() => go('kyc-06')} onClose={() => go('kyc-06')} />} /> },
    { id: 'kyc-08', section: 'details', name: 'Add a nominee', figma: '5436:7926', render: (go) => <NomineeForm onBack={() => go('kyc-06')} onDob={() => go('kyc-09')} onSkip={() => go('kyc-06')} /> },
    { id: 'kyc-09', section: 'details', name: 'Nominee · date of birth', figma: '5436:7927', render: (go) => <NomineeForm filled sheet={<DobSheet onConfirm={() => go('kyc-10')} onClose={() => go('kyc-08')} />} /> },
    { id: 'kyc-10', section: 'details', name: 'Nominees', figma: '5436:7928', render: (go) => <NomineeList onBack={() => go('kyc-08')} onAdd={() => go('kyc-08')} onDone={() => go('kyc-11')} /> },
    { id: 'kyc-11', section: 'details', name: 'Confirm details · nominee added', figma: '5436:7933', render: (go) => <Details withNominee agreed toast={nomineeAdded} onBack={() => go('kyc-03')} onFno={() => go('kyc-07')} onNominee={() => go('kyc-12')} onConfirm={() => go('kyc-13')} /> },
    { id: 'kyc-12', section: 'details', name: 'Nominee details', figma: '5436:7931', render: (go) => <Details withNominee agreed sheet={<NomineeSheet onClose={() => go('kyc-11')} onEdit={() => go('kyc-08')} />} /> },

    { id: 'kyc-13', section: 'bank', name: 'Bank account found', figma: '5436:7934', render: (go) => <Bank sheet={<BankFoundSheet onUse={() => go('kyc-15')} onOther={() => go('kyc-14')} />} /> },
    { id: 'kyc-14', section: 'bank', name: 'Bank account · typed', figma: '5436:7934', render: (go) => <Bank filled onBack={() => go('kyc-11')} onVerify={() => go('kyc-15')} /> },
    { id: 'kyc-15', section: 'bank', name: 'Verifying bank (₹1)', figma: '5436:7935', note: 'Penny drop. Moves on by itself.', render: () => <Bank filled sheet={<VerifyingBankSheet next="kyc-16" />} /> },
    { id: 'kyc-16', section: 'bank', name: 'Bank verified', figma: '5436:7936', render: (go) => <Bank filled sheet={<BankVerifiedSheet onFno={() => go('kyc-17')} onSkip={() => go('kyc-20')} />} /> },

    { id: 'kyc-17', section: 'fno', name: 'Share bank statement', figma: '5436:7937', render: (go) => <FnoConsent onBack={() => go('kyc-16')} onContinue={() => go('kyc-18')} onLater={() => go('kyc-20')} /> },
    { id: 'kyc-18', section: 'fno', name: 'CAMSfinserv (external)', figma: '5436:7938', note: 'Handoff placeholder: CAMSfinserv’s own OTP, account and consent pages (frames 5436:7939, 5436:7913, 5436:7940) are not built by Lemonn.', render: (go) => <FnoHandoff next="kyc-19" onBack={() => go('kyc-17')} /> },
    { id: 'kyc-19', section: 'fno', name: 'Statement received', figma: '5436:7941', render: (go) => <FnoReceived onContinue={() => go('kyc-20')} /> },

    { id: 'kyc-20', section: 'done', name: 'You’re all set', figma: '5436:7951', note: 'Replaces the live "KYC already completed" web page. Add money → First deposit.', render: (go) => <Done onAddMoney={() => go('deposit-01')} onExplore={() => go('shell-01')} /> },
  ],
}
```

### `src/flows/kyc/screens/Bank.tsx`

```tsx
import type { ReactNode } from 'react'
import { Actionbar } from '@l3/components/Actionbar'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { TextField } from '@l3/components/TextField'
import { Intro } from '../../../shared/ui/Intro'
import { Screen } from '../../../shared/ui/Screen'
import { StageBar } from '../components/StageBar'
import { foundBank } from '../data'
import styles from './Kyc.module.css'

/**
 * kyc-13 / 14 · Bank account: number twice + IFSC (the IFSC shows the bank and branch once valid). The account found
 * for the PAN opens as a sheet first (kyc-13).
 */
export function Bank({ filled = false, sheet, onBack, onVerify }: { filled?: boolean; sheet?: ReactNode; onBack?: () => void; onVerify?: () => void }) {
  return (
    <Screen
      top={<><Actionbar title="Add bank account" onBack={onBack} /><StageBar stage={2} /></>}
      sheet={sheet}
      bottom={<ButtonGroup aria-label="Verify"><Button disabled={!filled} onClick={onVerify}>Verify account</Button></ButtonGroup>}
    >
      <Intro title="Add your bank account" description="Money you add or withdraw only moves between this account and Lemonn" />
      <div className={styles.content}>
        <TextField label="Account number" inputMode="numeric" readOnly value={filled ? foundBank.accountNumber : ''} placeholder="Your account number" />
        <TextField label="Confirm account number" inputMode="numeric" readOnly value={filled ? foundBank.accountNumber : ''} placeholder="Type it again" />
        <TextField
          label="IFSC"
          readOnly
          value={filled ? foundBank.ifsc : ''}
          placeholder="e.g. HDFC0001234"
          status={filled ? 'success' : undefined}
          helperText={filled ? `${foundBank.bank} · ${foundBank.branch}` : '11 characters, on your cheque or passbook'}
        />
      </div>
    </Screen>
  )
}
```

### `src/flows/kyc/screens/Details.tsx`

```tsx
import { useState, type ReactNode } from 'react'
import { Actionbar } from '@l3/components/Actionbar'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { Card } from '@l3/components/Card'
import { Checkbox } from '@l3/components/Checkbox'
import { Icon } from '@l3/components/Icon'
import { KeyValueList, KeyValueRow } from '@l3/components/KeyValueList'
import { ListCell } from '@l3/components/ListCell'
import { SectionHeader } from '@l3/components/SectionHeader'
import { Select } from '@l3/components/Select'
import { Tag } from '@l3/components/Tag'
import { msChevronRight, msPersonAdd } from '@l3/icons/material'
import { user } from '../../../shared/data/user'
import { Screen } from '../../../shared/ui/Screen'
import { StageBar } from '../components/StageBar'
import { declarations, nominee, segments } from '../data'
import styles from './Kyc.module.css'

type Props = {
  /** A nominee has been added (shows their row instead of "Add a nominee"). */
  withNominee?: boolean
  /** Declarations already agreed (board states). */
  agreed?: boolean
  toast?: ReactNode
  sheet?: ReactNode
  onBack?: () => void
  onFno?: () => void
  onAddNominee?: () => void
  onNominee?: () => void
  onConfirm?: () => void
}

/**
 * kyc-06 / 07 / 11 / 12 · Confirm details: what came back from the government database (read-only), trading
 * segments, nominee, settlement and the declarations — agreed once with one checkbox. Confirm needs that tick.
 */
export function Details({ withNominee = false, agreed: initialAgreed = false, toast, sheet, onBack, onFno, onAddNominee, onNominee, onConfirm }: Props) {
  const [agreed, setAgreed] = useState(initialAgreed)
  return (
    <Screen
      top={<><Actionbar title="Confirm your details" onBack={onBack} /><StageBar stage={1} /></>}
      toast={toast}
      sheet={sheet}
      bottom={<ButtonGroup aria-label="Confirm"><Button disabled={!agreed} onClick={onConfirm}>Confirm and continue</Button></ButtonGroup>}
    >
      <div className={styles.content}>
        <section className={styles.section}>
          <SectionHeader title="Personal details" description="From the government database. Something wrong? Contact support." />
          <KeyValueList variant="card" aria-label="Personal details">
            <KeyValueRow label="Full name" value={user.name} />
            <KeyValueRow label="Father’s name" value={user.fatherName} />
            <KeyValueRow label="Date of birth" value={user.dob} />
            <KeyValueRow label="Occupation" value={user.occupation} />
            <KeyValueRow label="Marital status" value={user.maritalStatus} />
          </KeyValueList>
        </section>

        <section className={styles.section}>
          <SectionHeader title="What you’ll trade" />
          <div className={styles.stack}>
            {segments.map((s) => (
              <ListCell
                key={s.id}
                variant="card"
                as="label"
                label={s.label}
                description={s.description}
                multiline
                trailing={<Checkbox defaultChecked disabled={s.required} onChange={s.id === 'fno' ? (e) => e.target.checked && onFno?.() : undefined} />}
              />
            ))}
          </div>
        </section>

        <section className={styles.section}>
          <SectionHeader title="Nominee" description="Who receives your holdings if something happens to you" />
          {withNominee ? (
            <ListCell variant="card" as="button" label={nominee.name} description={`${nominee.relationship} · born ${nominee.dob}`} trailing={<Tag color="neutral">{nominee.share}%</Tag>} iconRight={<Icon icon={msChevronRight} />} onClick={onNominee} />
          ) : (
            <ListCell variant="card" as="button" iconLeft={<Icon icon={msPersonAdd} />} label="Add a nominee" description="Recommended · you can skip this" iconRight={<Icon icon={msChevronRight} />} onClick={onAddNominee} />
          )}
        </section>

        <section className={styles.section}>
          <SectionHeader title="Unused funds" />
          <ListCell variant="card" label="Return unused funds to my bank every" multiline trailing={<Select size="sm" onClick={() => {}}>90 days</Select>} />
        </section>

        <section className={styles.section}>
          <SectionHeader title="Declarations" />
          <Card>
            <ul className={styles.bullets}>{declarations.map((d) => <li key={d} className={styles.fine}>{d}</li>)}</ul>
          </Card>
          <ListCell variant="card" as="label" label="I agree to all the declarations above" multiline trailing={<Checkbox checked={agreed} onChange={(e) => setAgreed(e.target.checked)} />} />
        </section>
      </div>
    </Screen>
  )
}
```

### `src/flows/kyc/screens/Done.tsx`

```tsx
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { Card } from '@l3/components/Card'
import { msCelebration } from '@l3/icons/material'
import { user } from '../../../shared/data/user'
import { Intro } from '../../../shared/ui/Intro'
import { Screen } from '../../../shared/ui/Screen'
import { Spot } from '../../../shared/ui/Spot'
import { Steps } from '../../../shared/ui/Steps'
import styles from './Kyc.module.css'

/** kyc-20 · Account ready. Stocks and mutual funds work now; F&O follows its review. Next: add money (or look around). */
export function Done({ fnoUnderReview = true, onAddMoney, onExplore }: { fnoUnderReview?: boolean; onAddMoney?: () => void; onExplore?: () => void }) {
  return (
    <Screen
      bottom={
        <ButtonGroup aria-label="What next">
          <Button onClick={onAddMoney}>Add money</Button>
          <Button variant="secondary" onClick={onExplore}>Explore stocks first</Button>
        </ButtonGroup>
      }
    >
      <div className={styles.centre}>
        <Intro align="center" art={<Spot icon={msCelebration} tone="brand" />} title={`You’re all set, ${user.name.split(' ')[0]}`} description="Start investing in stocks and mutual funds today" />
        <Card>
          <Steps
            label="Account status"
            steps={[
              { label: 'Identity verified', state: 'done' },
              { label: 'Bank account added', state: 'done' },
              fnoUnderReview
                ? { label: 'F&O under review', description: 'Usually 24–48 hours', state: 'loading' }
                : { label: 'F&O not activated', description: 'Activate it anytime from Profile', state: 'todo' },
            ]}
          />
        </Card>
      </div>
    </Screen>
  )
}
```

### `src/flows/kyc/screens/Fetching.tsx`

```tsx
import { Aerobar } from '@l3/components/Aerobar'
import { Card } from '@l3/components/Card'
import { Icon } from '@l3/components/Icon'
import { msDatabase, msWifiOff } from '@l3/icons/material'
import { useAutoAdvance } from '../../../shared/prototype'
import { Intro } from '../../../shared/ui/Intro'
import { Screen } from '../../../shared/ui/Screen'
import { Spot } from '../../../shared/ui/Spot'
import { Steps, type StepState } from '../../../shared/ui/Steps'
import { fetchSteps } from '../data'
import styles from './Kyc.module.css'

/**
 * kyc-04 / 05 · Fetching PAN, personal and bank details from the government (KRA / NSDL) database.
 * `done` = how many of the three are back. A slow connection shows a warning toast (kyc-05) — the wait continues.
 */
export function Fetching({ done = 1, slow = false, next }: { done?: number; slow?: boolean; next?: string }) {
  useAutoAdvance(next, 2600)
  const state = (i: number): StepState => (i < done ? 'done' : i === done ? 'loading' : 'todo')
  return (
    <Screen
      toast={slow && <Aerobar floating type="warning" icon={<Icon icon={msWifiOff} />} heading="Slow connection" paragraph="This may take a little longer. Please keep the app open." />}
    >
      <div className={styles.centre}>
        <Intro
          align="center"
          art={<Spot icon={msDatabase} tone="discover" />}
          title="Getting your details"
          description="From the government database, so you don’t have to type them"
        />
        <Card variant="filled">
          <Steps label="Fetching details" steps={fetchSteps.map((label, i) => ({ label, state: state(i) }))} />
        </Card>
      </div>
    </Screen>
  )
}
```

### `src/flows/kyc/screens/Fno.tsx`

```tsx
import { Actionbar } from '@l3/components/Actionbar'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { Card } from '@l3/components/Card'
import { Icon } from '@l3/components/Icon'
import { ListCell } from '@l3/components/ListCell'
import { Loader } from '@l3/components/Loader'
import { msCheckCircle, msLock, msReceiptLong, msSchedule, msVerifiedUser } from '@l3/icons/material'
import { useAutoAdvance } from '../../../shared/prototype'
import { External } from '../../../shared/ui/External'
import { Intro } from '../../../shared/ui/Intro'
import { Screen } from '../../../shared/ui/Screen'
import { Spot } from '../../../shared/ui/Spot'
import { StageBar } from '../components/StageBar'
import styles from './Kyc.module.css'

const why = [
  { icon: msVerifiedUser, label: 'Through CAMSfinserv', description: 'An RBI-regulated account aggregator — Lemonn never sees your bank login' },
  { icon: msLock, label: 'Read-only, one time', description: 'The last 6 months of statements, only to check your income' },
  { icon: msSchedule, label: 'About 2 minutes', description: 'Approve it with an OTP on CAMSfinserv' },
]

/** kyc-17 · Why a bank statement is needed for F&O and how it's fetched, before handing off to CAMSfinserv. */
export function FnoConsent({ onBack, onContinue, onLater }: { onBack?: () => void; onContinue?: () => void; onLater?: () => void }) {
  return (
    <Screen
      top={<><Actionbar title="Activate F&O" onBack={onBack} /><StageBar stage={3} /></>}
      bottom={
        <ButtonGroup aria-label="Bank statement">
          <Button onClick={onContinue}>Continue to CAMSfinserv</Button>
          <Button variant="secondary" onClick={onLater}>Do this later</Button>
        </ButtonGroup>
      }
    >
      <Intro art={<Spot icon={msReceiptLong} tone="discover" size="md" />} title="Share your bank statement" description="SEBI needs proof of income before you can trade futures and options" />
      <div className={styles.content}>
        <Card variant="filled">
          {why.map((w) => <ListCell key={w.label} iconLeft={<Icon icon={w.icon} />} label={w.label} description={w.description} multiline />)}
        </Card>
      </div>
    </Screen>
  )
}

/** kyc-18 · HANDOFF: the account aggregator's own pages (not built by Lemonn). Returns to kyc-19 when approved. */
export function FnoHandoff({ next, onBack }: { next?: string; onBack?: () => void }) {
  useAutoAdvance(next, 3000)
  return (
    <Screen top={<Actionbar title="Activate F&O" onBack={onBack} />}>
      <div className={styles.centre}>
        <Intro align="center" art={<Loader size="lg" label={null} />} title="Opening CAMSfinserv" description="You’ll come back here once you approve" />
        <External provider="CAMSfinserv · account aggregator" steps={['Verify the mobile number with a 6-digit OTP', 'Pick the bank account to share', 'Approve sharing the statement']} />
      </div>
    </Screen>
  )
}

/** kyc-19 · Back from CAMSfinserv: statement received, F&O under review. */
export function FnoReceived({ onContinue }: { onContinue?: () => void }) {
  return (
    <Screen bottom={<ButtonGroup aria-label="Continue"><Button onClick={onContinue}>Continue</Button></ButtonGroup>}>
      <div className={styles.centre}>
        <Intro
          align="center"
          art={<Spot icon={msCheckCircle} tone="success" />}
          title="Bank statement received"
          description="We’re reviewing it. F&O usually activates within 24–48 hours — we’ll notify you."
        />
      </div>
    </Screen>
  )
}
```

### `src/flows/kyc/screens/Kyc.module.css`

```css
/* Shared layout for the account-setup screens. */
.content {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-24);
  padding: var(--l3-spacing-24) var(--l3-spacing-16);
}

.section {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-16);
}

.stack {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-08);
}

.centre {
  display: flex;
  flex: 1;
  flex-direction: column;
  justify-content: center;
  gap: var(--l3-spacing-24);
  padding: var(--l3-spacing-24) var(--l3-spacing-16);
}

.fine {
  margin: 0;
  font: var(--l3-text-description-12);
  color: var(--l3-content-secondary);
}

.fineCentre {
  text-align: center;
}

.bullets {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-08);
  margin: 0;
  padding-left: var(--l3-spacing-16);
  font: var(--l3-text-description-14);
  color: var(--l3-content-primary);
}

.sheetBody {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-16);
  /* The L3 sheet's content slot already brings 16 on every side (24 below). */
}
```

### `src/flows/kyc/screens/Nominee.tsx`

```tsx
import { useState, type ReactNode } from 'react'
import { Actionbar } from '@l3/components/Actionbar'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { Checkbox } from '@l3/components/Checkbox'
import { Icon } from '@l3/components/Icon'
import { ListCell } from '@l3/components/ListCell'
import { SectionHeader } from '@l3/components/SectionHeader'
import { Tabs } from '@l3/components/Tabs'
import { Tag } from '@l3/components/Tag'
import { TextField } from '@l3/components/TextField'
import { msCalendarMonth } from '@l3/icons/material'
import { Screen } from '../../../shared/ui/Screen'
import { nominee, relationships, type Relationship } from '../data'
import styles from './Kyc.module.css'

type FormProps = {
  filled?: boolean
  sheet?: ReactNode
  onBack?: () => void
  onDob?: () => void
  onSave?: () => void
  onSkip?: () => void
}

/**
 * kyc-08 / 09 · Add a nominee: relationship (one choice), name, date of birth (sheet), last 4 of Aadhaar (not
 * validated), and "same address, email and phone as mine". Save turns on when everything's filled.
 */
export function NomineeForm({ filled = false, sheet, onBack, onDob, onSave, onSkip }: FormProps) {
  const [relationship, setRelationship] = useState<Relationship | ''>(filled ? nominee.relationship : '')
  const ready = filled && relationship !== ''
  return (
    <Screen
      top={<Actionbar title="Add a nominee" onBack={onBack} />}
      sheet={sheet}
      bottom={
        <ButtonGroup aria-label="Nominee" direction="horizontal">
          <Button variant="secondary" onClick={onSkip}>Skip</Button>
          <Button disabled={!ready} onClick={onSave}>Save nominee</Button>
        </ButtonGroup>
      }
    >
      <div className={styles.content}>
        <section className={styles.section}>
          <SectionHeader title="Who is your nominee?" />
          <Tabs
            aria-label="Relationship"
            appearance="pill"
            emphasis="secondary"
            behavior="toggle"
            items={relationships.map((r) => ({ value: r, label: r }))}
            value={relationship}
            onChange={(v) => setRelationship(v as Relationship)}
          />
        </section>
        <section className={styles.section}>
          <TextField label="Full name" placeholder="As on their ID" defaultValue={filled ? nominee.name : ''} />
          <TextField label="Date of birth" placeholder="DD MMM YYYY" value={filled ? nominee.dob : ''} readOnly iconRight={<Icon icon={msCalendarMonth} />} onClick={onDob} />
          <TextField label="Last 4 digits of Aadhaar" placeholder="1234" inputMode="numeric" maxLength={4} defaultValue={filled ? nominee.aadhaarLast4 : ''} helperText="Only for identification — we don’t verify it" />
          <ListCell variant="card" as="label" multiline label="Same contact details as mine" description="Address, email and phone number" trailing={<Checkbox defaultChecked />} />
        </section>
      </div>
    </Screen>
  )
}

/** kyc-10 · Nominees saved: each with their share (100% for one). Add another or finish. */
export function NomineeList({ onBack, onAdd, onDone }: { onBack?: () => void; onAdd?: () => void; onDone?: () => void }) {
  return (
    <Screen
      top={<Actionbar title="Nominees" onBack={onBack} />}
      bottom={
        <ButtonGroup aria-label="Nominees" direction="horizontal">
          <Button variant="secondary" onClick={onAdd}>Add another</Button>
          <Button onClick={onDone}>Done</Button>
        </ButtonGroup>
      }
    >
      <div className={styles.content}>
        <ListCell variant="card" label={nominee.name} description={`${nominee.relationship} · born ${nominee.dob}`} trailing={<Tag color="neutral">{nominee.share}%</Tag>} />
        <p className={styles.fine}>You can add up to 10 nominees and split your holdings between them. Change this anytime in Profile.</p>
      </div>
    </Screen>
  )
}
```

### `src/flows/kyc/screens/Pan.tsx`

```tsx
import type { ReactNode } from 'react'
import { Actionbar } from '@l3/components/Actionbar'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { TextField } from '@l3/components/TextField'
import { Intro } from '../../../shared/ui/Intro'
import { Screen } from '../../../shared/ui/Screen'
import { StageBar } from '../components/StageBar'
import styles from './Kyc.module.css'

/** kyc-02 / 03 · PAN. A PAN found for the mobile number opens as a sheet first (kyc-02); otherwise type it (kyc-03). */
export function Pan({ value = '', sheet, onBack, onSubmit }: { value?: string; sheet?: ReactNode; onBack?: () => void; onSubmit?: () => void }) {
  const valid = /^[A-Z]{5}\d{4}[A-Z]$/.test(value)
  return (
    <Screen
      top={<><Actionbar title="Verify identity" onBack={onBack} /><StageBar stage={1} /></>}
      bottom={<ButtonGroup aria-label="Continue"><Button disabled={!valid} onClick={onSubmit}>Continue</Button></ButtonGroup>}
      sheet={sheet}
    >
      <Intro title="Enter your PAN" description="It links your trading account to your tax records" />
      <div className={styles.content}>
        <TextField
          label="PAN"
          value={value}
          placeholder="ABCDE1234F"
          readOnly
          autoCapitalize="characters"
          helperText={value && !valid ? 'A PAN has 5 letters, 4 digits and a letter' : '10 characters, as printed on your PAN card'}
          status={value && !valid ? 'error' : undefined}
        />
      </div>
    </Screen>
  )
}
```

### `src/flows/kyc/screens/Start.tsx`

```tsx
import { Actionbar } from '@l3/components/Actionbar'
import { Aerobar } from '@l3/components/Aerobar'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { Card } from '@l3/components/Card'
import { Icon } from '@l3/components/Icon'
import { msCheckCircle } from '@l3/icons/material'
import { Intro } from '../../../shared/ui/Intro'
import { Screen } from '../../../shared/ui/Screen'
import { Steps } from '../../../shared/ui/Steps'
import { stages } from '../data'
import styles from './Kyc.module.css'

/** kyc-01 · Account setup overview: three stages and how long they take. Arrives with an "Email verified" toast. */
export function Start({ emailVerified = true, onStart }: { emailVerified?: boolean; onStart?: () => void }) {
  return (
    <Screen
      top={<Actionbar />}
      toast={emailVerified && <Aerobar floating type="success" emphasis="primary" icon={<Icon icon={msCheckCircle} />} heading="Email verified" />}
      bottom={<ButtonGroup aria-label="Start"><Button onClick={onStart}>Start</Button></ButtonGroup>}
    >
      <Intro title="Let’s open your account" description="3 quick steps, about 5 minutes. Keep your PAN card handy." />
      <div className={styles.content}>
        <Card>
          <Steps label="Account setup" steps={stages.map((s, i) => ({ ...s, state: i === 0 ? 'current' : 'todo' }))} />
        </Card>
        <p className={styles.fine}>Your details are verified with SEBI-registered KRAs and stay encrypted.</p>
      </div>
    </Screen>
  )
}
```

### `src/flows/kyc/sheets/BankSheets.tsx`

```tsx
import { useId } from 'react'
import { BottomSheetHeader } from '@l3/components/BottomSheet'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { KeyValueList, KeyValueRow } from '@l3/components/KeyValueList'
import { Loader } from '@l3/components/Loader'
import { msAccountBalance, msCheckCircle } from '@l3/icons/material'
import { useAutoAdvance } from '../../../shared/prototype'
import { inr, mask } from '../../../shared/data/format'
import { Sheet } from '../../../shared/ui/Sheet'
import { Spot } from '../../../shared/ui/Spot'
import { Steps } from '../../../shared/ui/Steps'
import { foundBank, PENNY_DROP } from '../data'
import styles from '../screens/Kyc.module.css'

/** kyc-13 · A bank account linked to the PAN was found: use it, or add another. */
export function BankFoundSheet({ onUse, onOther }: { onUse?: () => void; onOther?: () => void }) {
  const id = useId()
  return (
    <Sheet
      labelledBy={id}
      onClose={onOther}
      header={<BottomSheetHeader size="lg" headingId={id} icon={<Spot icon={msAccountBalance} tone="discover" size="md" />} heading="Use this bank account?" description="We found it linked to your PAN" />}
      footer={
        <ButtonGroup aria-label="Bank account">
          <Button onClick={onUse}>Use this account</Button>
          <Button variant="secondary" onClick={onOther}>Add a different account</Button>
        </ButtonGroup>
      }
    >
      <div className={styles.sheetBody}>
        <KeyValueList variant="card" aria-label="Bank account found">
          <KeyValueRow label="Bank" value={foundBank.bank} />
          <KeyValueRow label="Account number" value={mask(foundBank.accountNumber, 0, 4)} />
          <KeyValueRow label="IFSC" value={foundBank.ifsc} />
          <KeyValueRow label="Type" value={foundBank.type} />
        </KeyValueList>
      </div>
    </Sheet>
  )
}

/** kyc-15 · Penny drop: ₹1 is sent to the account to prove it's the user's. Can't be dismissed. */
export function VerifyingBankSheet({ next }: { next?: string }) {
  useAutoAdvance(next, 2200)
  const id = useId()
  return (
    <Sheet
      labelledBy={id}
      header={<BottomSheetHeader size="lg" headingId={id} icon={<Loader size="lg" label={null} />} heading="Verifying your bank account" description={`We’re sending ${inr(PENNY_DROP, 0)} to it. It shows as “Lemonn” in your statement.`} />}
    >
      <p className={`${styles.fine} ${styles.fineCentre}`}>Takes a few seconds — please keep the app open</p>
    </Sheet>
  )
}

/** kyc-16 · Bank verified. F&O is the last (optional) step — it needs a bank statement. */
export function BankVerifiedSheet({ onFno, onSkip }: { onFno?: () => void; onSkip?: () => void }) {
  const id = useId()
  return (
    <Sheet
      labelledBy={id}
      header={<BottomSheetHeader size="lg" headingId={id} icon={<Spot icon={msCheckCircle} tone="success" size="md" />} heading="Bank account verified" description={`${foundBank.bank} ${mask(foundBank.accountNumber, 0, 4)}`} />}
      footer={
        <ButtonGroup aria-label="Next">
          <Button onClick={onFno}>Activate F&O</Button>
          <Button variant="secondary" onClick={onSkip}>Skip for now</Button>
        </ButtonGroup>
      }
    >
      <div className={styles.sheetBody}>
        <Steps
          label="Account setup"
          steps={[
            { label: 'Identity verified', state: 'done' },
            { label: 'Bank account verified', state: 'done' },
            { label: 'Activate F&O', description: 'Share a bank statement · about 2 min', state: 'current' },
          ]}
        />
      </div>
    </Sheet>
  )
}
```

### `src/flows/kyc/sheets/DobSheet.tsx`

```tsx
import { useId, useState } from 'react'
import { BottomSheetHeader } from '@l3/components/BottomSheet'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { DatePicker } from '@l3/components/DatePicker'
import { Sheet } from '../../../shared/ui/Sheet'
import styles from '../screens/Kyc.module.css'

/** kyc-09 · Nominee's date of birth (L3 DatePicker in a sheet; no future dates). */
export function DobSheet({ onConfirm, onClose }: { onConfirm?: () => void; onClose?: () => void }) {
  const id = useId()
  const [date, setDate] = useState<Date | null>(new Date(1967, 3, 27))
  return (
    <Sheet
      labelledBy={id}
      onClose={onClose}
      header={<BottomSheetHeader headingId={id} heading="Nominee’s date of birth" />}
      footer={<ButtonGroup aria-label="Date of birth"><Button disabled={!date} onClick={onConfirm}>Confirm</Button></ButtonGroup>}
    >
      <div className={styles.sheetBody}>
        <DatePicker label="Date of birth" value={date} onChange={setDate} max={new Date()} initialMonth={new Date(1967, 3, 1)} />
      </div>
    </Sheet>
  )
}
```

### `src/flows/kyc/sheets/NomineeSheet.tsx`

```tsx
import { useId } from 'react'
import { BottomSheetHeader } from '@l3/components/BottomSheet'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { KeyValueList, KeyValueRow } from '@l3/components/KeyValueList'
import { Sheet } from '../../../shared/ui/Sheet'
import { nominee } from '../data'
import styles from '../screens/Kyc.module.css'

/** kyc-12 · Tapping the nominee on Confirm details: their details, to check before confirming. */
export function NomineeSheet({ onClose, onEdit }: { onClose?: () => void; onEdit?: () => void }) {
  const id = useId()
  return (
    <Sheet
      labelledBy={id}
      onClose={onClose}
      header={<BottomSheetHeader headingId={id} heading="Nominee details" />}
      footer={
        <ButtonGroup aria-label="Nominee" direction="horizontal">
          <Button variant="secondary" onClick={onEdit}>Edit</Button>
          <Button onClick={onClose}>Done</Button>
        </ButtonGroup>
      }
    >
      <div className={styles.sheetBody}>
        <KeyValueList variant="card" aria-label="Nominee">
          <KeyValueRow label="Name" value={nominee.name} />
          <KeyValueRow label="Relationship" value={nominee.relationship} />
          <KeyValueRow label="Date of birth" value={nominee.dob} />
          <KeyValueRow label="Aadhaar (last 4)" value={nominee.aadhaarLast4} />
          <KeyValueRow label="Share" value={`${nominee.share}%`} />
        </KeyValueList>
      </div>
    </Sheet>
  )
}
```

### `src/flows/kyc/sheets/PanFoundSheet.tsx`

```tsx
import { useId } from 'react'
import { BottomSheetHeader } from '@l3/components/BottomSheet'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { KeyValueList, KeyValueRow } from '@l3/components/KeyValueList'
import { msBadge } from '@l3/icons/material'
import { mask } from '../../../shared/data/format'
import { user } from '../../../shared/data/user'
import { Sheet } from '../../../shared/ui/Sheet'
import { Spot } from '../../../shared/ui/Spot'
import styles from '../screens/Kyc.module.css'

/** kyc-02 · A PAN is linked to this mobile number: confirm it (masked) or type another. */
export function PanFoundSheet({ onUse, onOther }: { onUse?: () => void; onOther?: () => void }) {
  const id = useId()
  return (
    <Sheet
      labelledBy={id}
      onClose={onOther}
      header={<BottomSheetHeader size="lg" headingId={id} icon={<Spot icon={msBadge} tone="discover" size="md" />} heading="Is this your PAN?" description="We found a PAN linked to your mobile number" />}
      footer={
        <ButtonGroup aria-label="Confirm PAN">
          <Button onClick={onUse}>Yes, use this PAN</Button>
          <Button variant="secondary" onClick={onOther}>Enter a different PAN</Button>
        </ButtonGroup>
      }
    >
      <div className={styles.sheetBody}>
        <KeyValueList variant="card" aria-label="PAN found">
          <KeyValueRow label="Name" value={user.name} />
          <KeyValueRow label="PAN" value={mask(user.pan, 5, 1)} />
        </KeyValueList>
      </div>
    </Sheet>
  )
}
```

### `src/flows/kyc/sheets/RiskSheet.tsx`

```tsx
import { useId } from 'react'
import { BottomSheetHeader } from '@l3/components/BottomSheet'
import { Button } from '@l3/components/Button'
import { ButtonGroup } from '@l3/components/ButtonGroup'
import { msWarning } from '@l3/icons/material'
import { Sheet } from '../../../shared/ui/Sheet'
import { Spot } from '../../../shared/ui/Spot'
import { riskDisclosure } from '../data'
import styles from '../screens/Kyc.module.css'

/** kyc-07 · SEBI's risk disclosure, shown when F&O is switched on. Must be acknowledged; the source links to SEBI. */
export function RiskSheet({ onAccept, onClose }: { onAccept?: () => void; onClose?: () => void }) {
  const id = useId()
  return (
    <Sheet
      labelledBy={id}
      onClose={onClose}
      header={<BottomSheetHeader size="lg" headingId={id} icon={<Spot icon={msWarning} tone="warning" size="md" />} heading="Before you trade F&O" description="Risk disclosure on derivatives (SEBI)" />}
      footer={<ButtonGroup aria-label="Acknowledge"><Button onClick={onAccept}>I understand</Button></ButtonGroup>}
    >
      <div className={styles.sheetBody}>
        <ul className={styles.bullets}>{riskDisclosure.map((r) => <li key={r}>{r}</li>)}</ul>
        <p className={styles.fine}>Source: SEBI study, FY 2022–24.</p>
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

### `src/shared/ui/External.module.css`

```css
/* A step that happens outside Lemonn (account aggregator, UPI app): one labelled card, not a rebuilt page. */
.external {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-08);
}

.head {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--l3-spacing-08);
}

.title {
  font: var(--l3-text-label-14);
}

.list {
  margin: 0;
  padding-left: var(--l3-spacing-16);
  font: var(--l3-text-description-12);
  color: var(--l3-content-secondary);
}

.list li + li {
  margin-top: var(--l3-spacing-04);
}
```

### `src/shared/ui/External.tsx`

```tsx
import { Card } from '@l3/components/Card'
import { Tag } from '@l3/components/Tag'
import styles from './External.module.css'

/**
 * HANDOFF PLACEHOLDER — marks a step that runs outside Lemonn (CAMSfinserv's account-aggregator pages, the UPI app).
 * Developers link out to the provider; they don't build these pages. Lists what the user does there.
 */
export function External({ provider, steps }: { provider: string; steps: string[] }) {
  return (
    <Card variant="filled" className={styles.external}>
      <div className={styles.head}>
        <span className={styles.title}>{provider}</span>
        <Tag color="neutral">External</Tag>
      </div>
      <ol className={styles.list}>
        {steps.map((s) => <li key={s}>{s}</li>)}
      </ol>
    </Card>
  )
}
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
import { SystemStatusbar } from '@l3/components/SystemStatusbar'
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
 * The frame of every screen: the OS status bar (L3 SystemStatusbar), then top · body · bottom, a toast above the
 * bottom, and a sheet over it all. The screen is the sheet's container, so sheets and their scrim stay in the phone;
 * its SheetHost keeps one sheet open while the screen's sheet changes (no scrim flash between two sheets).
 */
export function Screen({ top, bottom, toast, sheet, children }: ScreenProps) {
  const [el, setEl] = useState<HTMLDivElement | null>(null)
  const { screenId } = usePreview()
  return (
    <div ref={setEl} className={styles.screen}>
      <SystemStatusbar />
      <SheetHost container={el}>
        <div className={styles.page}>
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

### `src/shared/ui/Steps.module.css`

```css
/* A vertical checklist joined by a hairline: done ✓ · current (bold, maybe a loader) · to do ○. */
.steps {
  display: flex;
  flex-direction: column;
  margin: 0;
  padding: 0;
  list-style: none;
}

.step {
  position: relative;
  display: grid;
  grid-template-columns: var(--l3-icon-size-24) 1fr;
  column-gap: var(--l3-spacing-12);
  padding-bottom: var(--l3-spacing-16);
}

.step:last-child {
  padding-bottom: 0;
}

/* The line to the next step, centred under the marker. */
.step:not(:last-child)::before {
  content: '';
  position: absolute;
  top: var(--l3-size-28);
  bottom: var(--l3-spacing-04);
  left: calc(var(--l3-icon-size-24) / 2);
  border-left: 1px solid var(--l3-border-light);
}

.step[data-state='done']:not(:last-child)::before {
  border-left-color: var(--l3-content-accent-success-default);
}

.marker {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: var(--l3-icon-size-24);
  height: var(--l3-icon-size-24);
  color: var(--l3-content-tertiary);
}

.step[data-state='done'] .marker {
  color: var(--l3-content-accent-success-default);
}

.step[data-state='current'] .marker,
.step[data-state='loading'] .marker {
  color: var(--l3-content-primary);
}

.text {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-02);
  padding-top: var(--l3-spacing-02);
}

.label {
  font: var(--l3-text-label-14);
  color: var(--l3-content-secondary);
}

.step:is([data-state='current'], [data-state='loading'], [data-state='done']) .label {
  color: var(--l3-content-primary);
}

.description {
  font: var(--l3-text-description-12);
  color: var(--l3-content-secondary);
}
```

### `src/shared/ui/Steps.tsx`

```tsx
import type { ReactNode } from 'react'
import { Icon } from '@l3/components/Icon'
import { Loader } from '@l3/components/Loader'
import { msCheckCircleFill, msRadioButtonUnchecked, msRadioButtonChecked } from '@l3/icons/material'
import styles from './Steps.module.css'

export type StepState = 'done' | 'current' | 'loading' | 'todo'
export type Step = { label: string; description?: ReactNode; state: StepState }

const marker = (state: StepState) =>
  state === 'loading' ? <Loader size="sm" label={null} />
    : <Icon icon={state === 'done' ? msCheckCircleFill : state === 'current' ? msRadioButtonChecked : msRadioButtonUnchecked} />

/** Progress through a few steps (KYC stages, fetching details, bank → F&O). Not tappable. */
export function Steps({ steps, label }: { steps: Step[]; label: string }) {
  return (
    <ol className={styles.steps} aria-label={label}>
      {steps.map((s) => (
        <li key={s.label} className={styles.step} data-state={s.state} aria-current={s.state === 'current' || s.state === 'loading' ? 'step' : undefined}>
          <span className={styles.marker}>{marker(s.state)}</span>
          <span className={styles.text}>
            <span className={styles.label}>
              {s.label}
              <span className="sr-only">{s.state === 'done' ? ' — done' : s.state === 'loading' ? ' — in progress' : s.state === 'todo' ? ' — to do' : ''}</span>
            </span>
            {s.description && <span className={styles.description}>{s.description}</span>}
          </span>
        </li>
      ))}
    </ol>
  )
}
```
