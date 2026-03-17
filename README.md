# Marinade Product Design

A Claude Code skill for building internal tools and dashboards with Marinade Finance's design system. Enforces the full token system, component conventions, and visual language — so every UI ships on-brand without manual review.

## Install

```bash
npx skills add spoony-vu/marinade-product-design
```

## What it covers

- **Design tokens** — Full light/dark color system with CSS variables and Tailwind config, including semantic tokens, alpha variants, chart palette, tag palette, and sidebar tokens
- **Typography** — Geist (sans) + Geist Mono (data), tabular-nums enforcement, anti-aliased rendering, iOS zoom prevention
- **Component conventions** — Shadcn UI primitives, `cn()` utility, Lucide React icons, composition patterns
- **Charts & data viz** — Recharts with custom tooltips, crosshair, active dots, series dimming, time-range selectors, and proper number formatting (SOL, USD, %, addresses)
- **Animation & motion** — Duration tokens, easing curves, decision flowcharts, `prefers-reduced-motion` enforcement, paired element rules
- **Interactivity** — Touch-first with hover enhancement, 44px tap targets, button press feel, warm tooltips, keyboard/focus management
- **Visual polish** — Anti-slop checklist, hairline borders, shadow-for-border patterns, z-index scale, mask-over-gradient fades
- **Auto-fix audit** — Automatically detects and fixes 40+ common issues across color, typography, layout, animation, accessibility, charts, and Marinade-specific formatting

## Stack

| Layer | Choice |
|-------|--------|
| Framework | React + TypeScript |
| Components | Shadcn UI |
| Styling | Tailwind CSS (CSS variable tokens) |
| Icons | Lucide React |
| Typeface | Geist / Geist Mono |
| Charts | Recharts |
| Theme | Light default, dark supported |

## Usage

The skill activates automatically when building Marinade Finance dashboards or internal tools.

Manual invocation:

```
/marinade-product-design
```

## Color system

Teal-first palette with full dark/light mode support. All colors defined as CSS custom properties that switch via `.dark` class — no Tailwind `dark:` modifiers.

| Token | Light | Dark | Usage |
|-------|-------|------|-------|
| `primary` | `#0C9790` | `#179F99` | Brand accent, primary actions |
| `background` | `#FFFFFF` | `#030707` | Page body |
| `card` | `#FFFFFF` | `#050D0C` | Card surfaces |
| `destructive` | `#DC2626` | `#F87171` | Errors, danger |
| `info` | `#6366F1` | `#818CF8` | Informational |
| `warning` | `#E59606` | `#FB923C` | Warning states |

Plus 5 chart colors, 4 tag colors (with alpha variants), sidebar tokens, and semantic foreground/muted/accent tokens. Full spec in [SKILL.md](./SKILL.md).

## Data formatting

| Type | Format | Font |
|------|--------|------|
| SOL amounts | `1,234.56 SOL` | Geist Mono |
| USD values | `$1,234.56` | Geist Mono |
| Percentages | `7.12%` | Geist Mono |
| Addresses | `AB1c...Xz9f` (4+4) | Geist Mono |
| Epoch numbers | `#567` | Geist Mono |

## New project scaffolding

When starting a new tool, the skill asks 4 questions then generates:

- Layout shell with Geist font, theme toggle, light mode default
- Navigation (sidebar or top)
- Dashboard with stat cards and empty state
- Token-based Tailwind config wired to CSS variables
- `cn()` utility and Shadcn setup
- Theme provider with dark/light toggle

## License

MIT
