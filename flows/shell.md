# Flow: App shell (`shell`)

> One self-contained file for converting this flow to another platform. Read https://kirtanyak-pe.github.io/lemonn-app-prototype/CONVERT.md first. Design system: Lemonnade V3 @ `7739bca` (components, tokens and rules: https://kirtanyak-pe.github.io/lemonnade-v3-docs/llms.txt).

The frame every flow builds on — proves the design system is wired up. Remove once Login and Stocks exist.

Prototype: https://kirtanyak-pe.github.io/lemonn-app-prototype/#/flow/shell · machine-readable: https://kirtanyak-pe.github.io/lemonn-app-prototype/flows/shell.json

## Screens

| Id | Screen | Figma | Taps go to | Note |
|---|---|---|---|---|
| `shell-01` | Empty L1 screen | — | — |  |

## Spec (README.md)


> Temporary: proves the design system is wired up. Removed once Login and Stocks exist.

- **Status:** setup only · last updated 2026-10-16

### Screens

| Id | Screen | Figma | What it shows / when |
|---|---|---|---|
| `shell-01` | Empty L1 screen | — | Actionbar title, an empty state, the main bottom navbar |

### Components (L3 → any platform)

| Where | L3 component (Figma name) | Props / variant |
|---|---|---|
| Top | Actionbar ("L3: Actionbar") | `title="Stocks"` (L1 screen: no back) |
| Body | EmptyState ("L3 → Empty state") | `title`, `description` |
| Bottom | BottomNavbar ("L3: Bottom Navbar") via `src/shared/ui/MainNav` | `value="stocks"` |

### Changelog

- 2026-10-16 — first version


## Source — this flow

### `src/flows/shell/index.tsx`

```tsx
import { Actionbar } from '@l3/components/Actionbar'
import { EmptyState } from '@l3/components/EmptyState'
import { MainNav } from '../../shared/ui/MainNav'
import { Screen } from '../../shared/ui/Screen'
import type { Flow } from '../types'

/** Setup check: the app frame every flow builds on (status + Actionbar, body, main navbar). Replace once Login and Stocks exist. */
export const shell: Flow = {
  id: 'shell',
  title: 'App shell',
  description: 'The frame every flow builds on — proves the design system is wired up. Remove once Login and Stocks exist.',
  figmaFile: '',
  screens: [
    {
      id: 'shell-01', name: 'Empty L1 screen', figma: '',
      render: () => (
        <Screen top={<Actionbar title="Stocks" />} bottom={<MainNav value="stocks" />}>
          <EmptyState title="No flows yet" description="Run /convert-flow Login in a new chat to start." />
        </Screen>
      ),
    },
  ],
}
```

## Source — shared app parts it uses

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
```

### `src/shared/ui/Screen.module.css`

```css
/* A screen fills the phone: top bar, a scrolling body that fills the rest, then the bottom bar / dock. */
.screen {
  display: flex;
  flex-direction: column;
  height: 100%;
  background: var(--l3-surface-default);
}

.body {
  flex: 1 1 auto;
  min-height: 0;
  overflow-y: auto;
}
```

### `src/shared/ui/Screen.tsx`

```tsx
import type { ReactNode } from 'react'
import styles from './Screen.module.css'

/** Screen layout: `top` (Actionbar), the body that fills the space between (DESIGN_SYSTEM 5), `bottom` (navbar / dock). */
export function Screen({ top, bottom, children }: { top?: ReactNode; bottom?: ReactNode; children: ReactNode }) {
  return (
    <div className={styles.screen}>
      {top}
      <div className={styles.body}>{children}</div>
      {bottom}
    </div>
  )
}
```
