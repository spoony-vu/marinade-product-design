---
name: marinade-product-design
description: Build Marinade Finance dashboards, internal tools, and product UI. Use when building with Marinade's design system, implementing teal-accent themed interfaces, creating Solana/DeFi dashboards, adding charts with Recharts, styling Shadcn components for Marinade, formatting SOL/USD/address data, or scaffolding new Marinade tools. Covers design tokens (light/dark), Geist typography, chart interactivity, animation timing, accessibility, and auto-fix audit patterns.
---

You are helping build an internal tool/dashboard for Marinade Finance.

## DESIGN SYSTEM RULES — follow these strictly

### Stack
- React + TypeScript
- Shadcn UI (https://ui.shadcn.com) as the base component library
- Tailwind CSS for utility styling
- Lucide React for all icons (16px inline, 20px standalone)
- Geist as the primary typeface (import via `next/font/google` or CDN)
- Recharts for all charts

### Gotchas — Common Mistakes to Avoid

These are the top failure patterns. Check every one before shipping:

1. **`dark:` Tailwind modifiers for colors** — Never. All color switching happens through CSS variables only.
2. **Hardcoded hex/rgb colors** — Always use token CSS variables (`var(--primary)`, etc.), never raw hex values.
3. **Default Recharts tooltip** — Always use a custom `<CustomTooltip />` styled with Marinade tokens.
4. **Active dot inside `preserveAspectRatio="none"` SVG** — Distorts circles into ellipses. Render dots as HTML `<div>` outside the SVG.
5. **`primary` (teal) for tab/range selectors** — Use `secondary` (grey). Teal competes with chart colors.
6. **Serif or non-Geist fonts** — Always Geist (sans) or Geist Mono (data). Sans-serif fallback stack only.
7. **Dynamic numbers without `tabular-nums`** — Causes layout shift on counters, prices, APY, TVL. Always set `font-variant-numeric: tabular-nums`.
8. **`transition: all`** — Specify exact properties (`transition: opacity 150ms ease-out, transform 150ms ease-out`).
9. **Missing `prefers-reduced-motion`** — Every animation needs a `prefers-reduced-motion: reduce` fallback. Mandatory.
10. **`ease-in` on entering elements** — Use `ease-out` for elements appearing, `ease-in-out` for movement. Never `ease-in` or `linear` for UI.
11. **Static chart tooltips/crosshairs** — Must be hidden by default, shown only on hover. Never render statically.
12. **Hover effects without `@media (hover: hover)` guard** — Hover is progressive enhancement, not baseline.
13. **Full Solana addresses** — Always truncate: `AB1c...Xz9f` (4 prefix + 4 suffix).
14. **`border` for subtle dividers** — Use `box-shadow: 0 0 0 1px rgba(0,0,0,0.08)` or `border-grid` token instead.

### Color Tokens — THE SOURCE OF TRUTH

The full token definitions are embedded below as JSON. When scaffolding a project, generate CSS variables + Tailwind config from these tokens.

**Every color in the system has a light and dark variant.** Light mode is the default. Use CSS custom properties that switch based on a `.dark` class on `<html>` or via `prefers-color-scheme`.

#### tokens.json (embedded)

```json
{
  "colors": {
    "background":           { "light": "#FFFFFF", "dark": "#030707" },
    "background-page":      { "light": "#F3F7F7", "dark": "#050D0C" },
    "foreground":           { "light": "#182120", "dark": "#F6F9F9" },
    "card":                 { "light": "#FFFFFF", "dark": "#050D0C" },
    "card-foreground":      { "light": "#081211", "dark": "#F6F9F9" },
    "popover":              { "light": "#FFFFFF", "dark": "#182120" },
    "popover-foreground":   { "light": "#042F2E", "dark": "#F6F9F9" },
    "primary":              { "light": "#0C9790", "dark": "#179F99" },
    "primary-20":           { "light": "#0C979026", "dark": "#179F9933" },
    "primary-90":           { "light": "#0C9790E6", "dark": "#179F99E6" },
    "primary-foreground":   { "light": "#F6F9F9", "dark": "#042F2E" },
    "secondary":            { "light": "#E7EEEF", "dark": "#212C2B" },
    "secondary-foreground": { "light": "#3A4E4D", "dark": "#F6F9F9" },
    "tertiary":             { "light": "#DDE7E8", "dark": "#3A4E4D" },
    "tertiary-foreground":  { "light": "#6F8383", "dark": "#9AB1B2" },
    "muted":                { "light": "#F3F7F7", "dark": "#111a19" },
    "muted-foreground":     { "light": "#6C8383", "dark": "#6C8383" },
    "accent":               { "light": "#E7EEEF", "dark": "#212C2B" },
    "accent-foreground":    { "light": "#081211", "dark": "#F0FDFA" },
    "destructive":          { "light": "#DC2626", "dark": "#F87171" },
    "destructive-20":       { "light": "#DC262633", "dark": "#F871714D" },
    "info":                 { "light": "#6366F1", "dark": "#818CF8" },
    "info-20":              { "light": "#6366F133", "dark": "#818CF84D" },
    "warning":              { "light": "#E59606", "dark": "#FB923C" },
    "warning-20":           { "light": "#E5960633", "dark": "#FB923C33" },
    "warning-10":           { "light": "#E596061A", "dark": "#FB923C33" },
    "border":               { "light": "#DDE7E8", "dark": "#FFFFFF26" },
    "border-grid":          { "light": "#E7EEEF", "dark": "#FFFFFF0F" },
    "input":                { "light": "#E7EEEF", "dark": "#FFFFFF26" },
    "ring":                 { "light": "#9AB1B24D", "dark": "#6F838380" },
    "sidebar-background":          { "light": "#FFFFFF", "dark": "#081211" },
    "sidebar-foreground":          { "light": "#042F2E", "dark": "#F6F9F9" },
    "sidebar-primary":             { "light": "#182120", "dark": "#1D4ED8" },
    "sidebar-primary-foreground":  { "light": "#F6F9F9", "dark": "#F6F9F9" },
    "sidebar-accent":              { "light": "#F3F7F7", "dark": "#212C2B" },
    "sidebar-accent-foreground":   { "light": "#182120", "dark": "#F6F9F9" },
    "sidebar-border":              { "light": "#F3F7F7", "dark": "#FFFFFF1A" },
    "sidebar-ring":                { "light": "#9AB1B2", "dark": "#6F8383" },
    "chart-1":  { "light": "#0C9790", "dark": "#3AC3BC" },
    "chart-2":  { "light": "#818CF8", "dark": "#6366F1" },
    "chart-3":  { "light": "#FBBF24", "dark": "#F59E0B" },
    "chart-4":  { "light": "#C084FC", "dark": "#A855F7" },
    "chart-5":  { "light": "#FB7185", "dark": "#F43F5E" },
    "tag-1":    { "light": "#CA8A04", "dark": "#FACC15" },
    "tag-1-20": { "light": "#EAB30826", "dark": "#FACC1526" },
    "tag-2":    { "light": "#EA580C", "dark": "#FB923C" },
    "tag-2-20": { "light": "#EA580C26", "dark": "#FB923C26" },
    "tag-3":    { "light": "#64748B", "dark": "#CBD5E1" },
    "tag-3-20": { "light": "#64748B26", "dark": "#CBD5E126" },
    "tag-4":    { "light": "#3B82F6", "dark": "#60A5FA" },
    "tag-4-20": { "light": "#3B82F626", "dark": "#60A5FA26" }
  },
  "fonts": {
    "sans":  "Geist",
    "mono":  "Geist Mono"
  },
  "shadow": { "offset-x": 0, "offset-y": 2, "blur-radius": 10, "spread-radius": 0, "color": "#0000000D" }
}
```

#### Semantic Color Tokens

| Token | Light | Dark | Usage |
|-------|-------|------|-------|
| `background` | `#FFFFFF` | `#030707` | Page body background |
| `background-page` | `#F3F7F7` | `#050D0C` | Page-level surface behind cards |
| `foreground` | `#182120` | `#F6F9F9` | Primary text color |
| `card` | `#FFFFFF` | `#050D0C` | Card surface |
| `card-foreground` | `#081211` | `#F6F9F9` | Text on cards |
| `popover` | `#FFFFFF` | `#182120` | Dropdown/popover surface |
| `popover-foreground` | `#042F2E` | `#F6F9F9` | Text in popovers |
| `primary` | `#0C9790` | `#179F99` | Primary actions, brand accent (teal) |
| `primary-foreground` | `#F6F9F9` | `#042F2E` | Text on primary buttons |
| `secondary` | `#E7EEEF` | `#212C2B` | Secondary surfaces |
| `secondary-foreground` | `#3A4E4D` | `#F6F9F9` | Text on secondary |
| `tertiary` | `#DDE7E8` | `#3A4E4D` | Tertiary surfaces |
| `tertiary-foreground` | `#6F8383` | `#9AB1B2` | Text on tertiary |
| `muted` | `#F3F7F7` | `#111a19` | Muted backgrounds (stat cards, subtle surfaces) |
| `muted-foreground` | `#6C8383` | `#6C8383` | Muted text (same in both modes) |
| `accent` | `#E7EEEF` | `#212C2B` | Accent surfaces |
| `accent-foreground` | `#081211` | `#F0FDFA` | Text on accent |
| `destructive` | `#DC2626` | `#F87171` | Error, delete, danger |
| `info` | `#6366F1` | `#818CF8` | Informational (indigo) |
| `warning` | `#E59606` | `#FB923C` | Warning states (amber/orange) |
| `border` | `#DDE7E8` | `rgba(255,255,255,0.15)` | Default borders |
| `border-grid` | `#E7EEEF` | `rgba(255,255,255,0.06)` | Subtle grid/divider lines |
| `input` | `#E7EEEF` | `rgba(255,255,255,0.15)` | Input field borders |
| `ring` | `rgba(154,177,178,0.3)` | `rgba(111,131,131,0.5)` | Focus ring |

#### Alpha Variants
Tokens with `-20`, `-10`, `-90` suffixes are the base color at that opacity percentage. Use them for:
- `primary-20` — subtle primary tint backgrounds
- `destructive-20` — error badge backgrounds
- `warning-10` / `warning-20` — warning badge backgrounds
- `primary-90` — slightly transparent primary for overlays

#### Chart Colors (5-color palette)
| Token | Light | Dark |
|-------|-------|------|
| `chart-1` | `#0C9790` | `#3AC3BC` |
| `chart-2` | `#818CF8` | `#6366F1` |
| `chart-3` | `#FBBF24` | `#F59E0B` |
| `chart-4` | `#C084FC` | `#A855F7` |
| `chart-5` | `#FB7185` | `#F43F5E` |

#### Tag Colors (4-color palette)
| Token | Light | Dark |
|-------|-------|------|
| `tag-1` | `#CA8A04` (yellow) | `#FACC15` |
| `tag-2` | `#EA580C` (orange) | `#FB923C` |
| `tag-3` | `#64748B` (slate) | `#CBD5E1` |
| `tag-4` | `#3B82F6` (blue) | `#60A5FA` |

Each tag also has a `-20` alpha variant for badge backgrounds.

#### Sidebar Tokens
| Token | Light | Dark |
|-------|-------|------|
| `sidebar-background` | `#FFFFFF` | `#081211` |
| `sidebar-foreground` | `#042F2E` | `#F6F9F9` |
| `sidebar-primary` | `#182120` | `#1D4ED8` |
| `sidebar-primary-foreground` | `#F6F9F9` | `#F6F9F9` |
| `sidebar-accent` | `#F3F7F7` | `#212C2B` |
| `sidebar-accent-foreground` | `#182120` | `#F6F9F9` |
| `sidebar-border` | `#F3F7F7` | `rgba(255,255,255,0.1)` |
| `sidebar-ring` | `#9AB1B2` | `#6F8383` |

#### CSS Variable Implementation

When scaffolding, generate this in `globals.css`:

```css
:root {
  --background: #FFFFFF;
  --background-page: #F3F7F7;
  --foreground: #182120;
  --card: #FFFFFF;
  --card-foreground: #081211;
  --primary: #0C9790;
  --primary-foreground: #F6F9F9;
  --secondary: #E7EEEF;
  --secondary-foreground: #3A4E4D;
  --muted: #F3F7F7;
  --muted-foreground: #6C8383;
  --accent: #E7EEEF;
  --accent-foreground: #081211;
  --destructive: #DC2626;
  --info: #6366F1;
  --warning: #E59606;
  --border: #DDE7E8;
  --input: #E7EEEF;
  --ring: rgba(154,177,178,0.3);
}

.dark {
  --background: #030707;
  --background-page: #050D0C;
  --foreground: #F6F9F9;
  --card: #050D0C;
  --card-foreground: #F6F9F9;
  --primary: #179F99;
  --primary-foreground: #042F2E;
  --secondary: #212C2B;
  --secondary-foreground: #F6F9F9;
  --muted: #111a19;
  --muted-foreground: #6C8383;
  --accent: #212C2B;
  --accent-foreground: #F0FDFA;
  --destructive: #F87171;
  --info: #818CF8;
  --warning: #FB923C;
  --border: rgba(255,255,255,0.15);
  --input: rgba(255,255,255,0.15);
  --ring: rgba(111,131,131,0.5);
}
```

Wire into `tailwind.config.ts`:
```ts
colors: {
  background: "var(--background)",
  "background-page": "var(--background-page)",
  foreground: "var(--foreground)",
  card: { DEFAULT: "var(--card)", foreground: "var(--card-foreground)" },
  primary: { DEFAULT: "var(--primary)", foreground: "var(--primary-foreground)" },
  secondary: { DEFAULT: "var(--secondary)", foreground: "var(--secondary-foreground)" },
  muted: { DEFAULT: "var(--muted)", foreground: "var(--muted-foreground)" },
  accent: { DEFAULT: "var(--accent)", foreground: "var(--accent-foreground)" },
  destructive: "var(--destructive)",
  info: "var(--info)",
  warning: "var(--warning)",
  border: "var(--border)",
  input: "var(--input)",
  ring: "var(--ring)",
}
```

### Fonts
- **Sans:** Geist — all UI text, labels, headings, body copy
- **Mono:** Geist Mono — data, code, addresses, numbers
- **Never use serif fonts.** Fallback stack: `ui-sans-serif, system-ui, -apple-system, sans-serif`

### Typography
- `font-variant-numeric: tabular-nums` on all dynamic numbers (timers, counters, prices, TVL, APY)
- `-webkit-font-smoothing: antialiased` on body/root
- `text-wrap: balance` on headings

### Visual Language
- Teal primary accent — always use the `primary` token, never hardcode
- Rounded corners: `rounded-lg` (8px) for cards, `rounded-md` (6px) for buttons, `rounded-sm` (4px) for inputs
- Subtle shadows and backdrop-blur on overlays
- Eased gradients over linear ones
- Hairline borders: `border-grid` token or `box-shadow: 0 0 0 1px rgba(0,0,0,0.08)`
- `pointer-events: none` on decorative elements, `user-select: none` on decorative art
- Prefer `mask-image` over gradients for fades (but not on scrollable lists)
- Z-index scale: `--z-dropdown: 100; --z-modal: 200; --z-tooltip: 300; --z-toast: 400`. Prefer `isolation: isolate` when possible.

### Dark / Light Mode
- Light mode is the **default**
- Both modes must work using the token system
- Toggle via `.dark` class on `<html>` or `prefers-color-scheme`
- Never use Tailwind `dark:` modifier for colors — all color switching happens through CSS variables
- Test both modes before shipping

### Animation & Motion

#### Decision: Should I Animate This?
```
Will users see this 100+ times daily?
├── Yes → Don't animate (e.g. sidebar toggle, frequent actions)
└── No
    ├── Is this user-initiated? → Animate with ease-out (150-250ms)
    └── Is this a page transition? → Animate (300-400ms max)
```

#### Decision: What Easing?
```
Is the element entering or exiting?
├── Yes → ease-out
└── No
    ├── Is it moving on screen? → ease-in-out
    └── Is it a hover/color change? → ease
```

#### Easing Tokens
```css
--ease-out-quint: cubic-bezier(0.23, 1, 0.32, 1);     /* Strong ease-out for modals, drawers */
--ease-out-cubic: cubic-bezier(0.215, 0.61, 0.355, 1); /* Standard ease-out for tooltips, dropdowns */
--ease-in-out-quart: cubic-bezier(0.77, 0, 0.175, 1);  /* Movement on screen */
```

#### Duration
| Element Type | Duration |
|---|---|
| Micro-interactions (button press, toggle) | 100-150ms |
| Tooltips, dropdowns, popovers | 150-250ms |
| Modals, drawers, sheets | 200-300ms |
| Page transitions | 300-400ms max |

#### Rules
- Only animate `transform` and `opacity` — never `height`, `width`, `padding`, `margin`
- **Paired elements rule**: Elements that animate together (modal + overlay, tooltip + arrow) must share the same easing and duration
- Exit animations can be faster than entrances
- Avoid `blur` filters above 20px (expensive, especially Safari)
- `will-change: transform` only when needed — remove when not animating

#### Reduced Motion — MANDATORY
```css
@media (prefers-reduced-motion: reduce) {
  .animated { animation: none; transition: none; }
}
```

#### Theme Transitions
Switching dark/light mode must NOT trigger transitions on elements. Disable transitions during theme changes.

### Interactivity Patterns

#### Touch-First, Hover-Enhanced
- Design for touch first, add hover as progressive enhancement
- **Never rely on hover for core functionality**
- Guard all hover effects:
```css
@media (hover: hover) and (pointer: fine) {
  .element:hover { background: var(--accent); }
}
```
- `touch-action: manipulation` on buttons/links to prevent double-tap zoom

#### Tap Targets
- **44px minimum** on all interactive elements
- Small icon buttons: expand hit area with `::before { content: ''; position: absolute; inset: -10px; }`

#### Button Press Feel
- `transform: scale(0.97)` on `:active` for tactile feedback

#### No Layout Shift
- `tabular-nums` on all changing numbers
- Never change font weight on hover/selected states
- Hardcoded dimensions for skeleton loaders and image placeholders

#### Tooltips
- **200ms delay** before showing to prevent accidental activation
- **Sequential ("warm") tooltips**: Once one tooltip is open, subsequent ones open instantly. Clear warm state after 300ms of no tooltip.
- Tooltip content is supplementary only — never required for functionality

#### Focus & Keyboard
- Tab through visible elements only
- `scrollIntoView({ behavior: 'smooth', block: 'nearest' })` on keyboard navigation
- Focus moves to modal on open, returns to trigger on close
- Icon buttons always need `aria-label`
- Focus outlines: grey, black, or white only — never colored

#### Forms & Inputs
- Wrap inputs with `<form>` for Enter-to-submit
- `Cmd+Enter` / `Ctrl+Enter` for textarea submission
- Input font size: **16px minimum** (prevents iOS auto-zoom)
- Label clicks must focus the associated input
- Disable button after submission to prevent duplicates
- Colocate errors near the field that caused them

### Data Display Patterns
- Use Geist Mono for all numerical data (balances, APY, TVL, addresses)
- Truncate Solana addresses: `AB1c...Xz9f` (4 prefix + 4 suffix)
- Format large numbers with locale-aware grouping (1,234,567.89)
- SOL values: 2-4 decimal places depending on context
- USD values: always 2 decimal places with `$` prefix
- Percentage values: 2 decimal places with `%` suffix
- Use `primary` token for positive changes, `destructive` for negative

### Charts & Data Visualization

#### Chart Styling Rules
- **Always** use `chart-1` through `chart-5` CSS variables for series colors — never hardcode
- Background: transparent or `var(--card)`
- Grid lines: `var(--border-grid)` (very subtle)
- Axis labels: `var(--muted-foreground)`, Geist Mono, 11-12px
- Axis ticks: `var(--border-grid)` or hidden
- No chart borders — let the card handle containment

#### Interactivity Patterns — CRITICAL

All chart interactive elements (tooltip, crosshair, active dot) must be **hidden by default** and only appear on hover. Never show them statically.

- **Tooltips**: Hidden by default. On hover, show with `transition: opacity 150ms ease-out`. Style: `var(--popover)` bg, `var(--popover-foreground)` text, `var(--border)` border, `rounded-lg`, `shadow-lg`, `pointer-events: none`. Geist Mono for values. Clamp position to chart bounds.
- **Crosshair line**: Vertical dashed line at nearest data point X. `var(--border)` stroke, `strokeDasharray="4 4"`, full chart height.
- **Active dot**: `var(--background)` fill, 2.5px border in series color, radius 5px. Snaps to nearest data point. **CRITICAL: Never place inside `preserveAspectRatio="none"` SVG** — render as HTML `<div>` with `border-radius: 50%` outside the SVG, converting SVG coords to pixels.
- **Hover on series**: Dim non-hovered series to 30% opacity. `opacity 150ms ease-out`.
- **Cursor**: `cursor: crosshair` on chart container.
- **Legend**: Clickable to toggle series. `var(--muted-foreground)` labels, 8px chart-color dots.
- **Brush/zoom**: For time-series > 30 data points, use `var(--secondary)` handle.

#### Recharts Implementation
```tsx
<Tooltip
  content={<CustomTooltip />}  // Never use default Recharts tooltip
  cursor={{ stroke: 'var(--border)', strokeDasharray: '4 4' }}
  isAnimationActive={false}
/>
<Area
  activeDot={{ r: 5, fill: 'var(--background)', stroke: 'var(--chart-1)', strokeWidth: 2.5 }}
  dot={false}  // Only active dot on hover
/>
```

**CustomTooltip must:**
- Use `var(--popover)` bg, `var(--border)` border, `rounded-lg`, `shadow-lg`
- Show date in muted-foreground, value in Geist Mono bold, change % in primary/destructive
- Return `null` when not active (`if (!active || !payload) return null`)

#### Chart Types & When to Use
| Type | Use For | Notes |
|------|---------|-------|
| Area chart | TVL over time, staking trends | Fill with `chart-1` at 10% opacity, stroke at full |
| Line chart | APY history, price trends | 2px stroke, smooth curve (`type="monotone"`) |
| Bar chart | Validator comparison, distribution | `rounded-t-sm` on bars, 4px gap between |
| Stacked bar | Composition breakdown | Use chart-1 through chart-5 in order |
| Pie/donut | Portfolio allocation, vote share | Donut preferred (60% inner radius), max 5 slices |
| Sparkline | Inline mini-charts in tables | 40px tall, no axes, single color, `strokeWidth={1.5}` |

#### Time-Series Conventions
- X-axis: 5-7 date labels max. Relative format ("Mar 1") for < 3 months, month format ("Jan") for > 3 months
- Y-axis: Auto-scale with 20% padding above max. Compact notation (1.2M, 450K)
- Range selector: 7d / 30d / 90d / 1y / All as tab pills. **Selected**: `bg-secondary text-foreground`. **Unselected**: `bg-transparent text-muted-foreground`. Never use `primary` (teal) for tab selection.
- Loading: Skeleton shimmer with `var(--muted)` pulse

#### Formatting in Charts
- SOL: `1,234.56 SOL` (Geist Mono)
- USD: `$1,234.56`
- Percentages: `7.12%`
- Epoch: `#567` (mono, hash prefix)

## UI Quality Audit (Auto-Fix Mode)

When building or reviewing any Marinade UI, automatically check for and fix these issues without being asked.

### Color & Theme — Auto-Fix
- Hardcoded hex/rgb values → Replace with CSS variable tokens
- Missing dark mode support → Wire through CSS variables
- Low contrast text → Swap to correct foreground token
- Inconsistent opacity → Use token alpha variants (primary-20, etc.)
- `dark:` Tailwind modifiers → Remove, use CSS variable switching

### Typography — Auto-Fix
- Non-Geist fonts → Replace with Geist (sans) or Geist Mono (data)
- Dynamic numbers without `tabular-nums` → Add it
- Missing `-webkit-font-smoothing: antialiased` → Add to body/root
- Headings without `text-wrap: balance` → Add it
- Font weight change on hover → Remove (causes layout shift)
- Inputs under 16px → Set 16px minimum

### Layout & Spacing — Auto-Fix
- Inconsistent border-radius → Apply scale (lg/md/sm)
- Arbitrary z-index values → Use fixed scale or `isolation: isolate`
- `border` for subtle dividers → Switch to `border-grid` token or box-shadow
- Missing `isolation: isolate` on stacking contexts → Add it
- Inconsistent card padding → Standardize to 24px (16px for compact)

### Animation — Auto-Fix
- `transition: all` → Specify explicit properties
- Animations > 300ms for standard UI → Reduce to token timing
- Missing `prefers-reduced-motion` → Add media query fallback
- Animating layout properties → Refactor to `transform`/`opacity`
- `ease-in` on UI elements → Switch to `ease-out` or `ease-in-out`

### Interactivity — Auto-Fix
- Hover effects without `@media (hover: hover)` → Add guard
- Buttons without `:active` scale → Add `transform: scale(0.97)`
- Tooltips without delay → Add 200ms delay + warm state
- Paired elements with different timing → Synchronize
- Animations on frequent actions → Remove or reduce
- Missing `touch-action: manipulation` on buttons → Add it
- Forms without Enter-to-submit → Wrap in `<form>`

### Accessibility — Auto-Fix
- Touch targets < 44px → Expand with padding or `::before`
- Icon buttons without `aria-label` → Add label
- Missing focus-visible styles → Add using `ring` token
- Color-only indicators → Add icon or text supplement
- Missing hover states → Add with `@media (hover: hover)` guard
- Focus doesn't move to modal → Add focus management
- Colored focus outlines → Change to grey/black/white

### Charts — Auto-Fix
- No tooltips → Add custom styled tooltip
- Static interactive elements → Add hover handlers
- Hardcoded chart colors → Use chart-1 through chart-5
- Missing loading states → Add skeleton shimmer
- Unformatted axes → Apply Geist Mono + compact formatting

### Marinade-Specific — Auto-Fix
- Full Solana addresses → Truncate to `XXXX...XXXX` (4+4)
- Unformatted SOL amounts → Locale grouping + 2-4 decimals
- Missing `$` on USD → Add prefix + 2 decimals
- Missing `%` suffix → Add it
- Positive changes not green → Apply `primary`
- Negative changes not red → Apply `destructive`

## Starting a New Tool

When the user starts a new internal tool project, ask these questions before scaffolding:

1. **What is this tool for?** (2 sentence description)
2. **What are the 3 main data entities or concepts this tool manages?**
3. **Should this have a sidebar nav or top nav?**
4. **Are there any specific pages or views I need first?**

Then scaffold the project with:
- Layout shell (light mode default, Geist font loaded, dark mode toggle)
- Navigation (sidebar or top, based on answer)
- Placeholder home dashboard with stat cards and an empty state
- 3 starter components relevant to the tool's purpose
- Token-based Tailwind config wired to CSS variables (from tokens.json above)
- `cn()` utility and base Shadcn setup
- Theme provider with dark/light mode toggle
