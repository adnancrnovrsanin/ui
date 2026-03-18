# Design & Aesthetics

Guidance for making design decisions within shadcn/ui's component system. Read this when creating new pages, layouts, or full-featured components — not for every bug fix or single-component addition.

See [customization.md](../customization.md) for the mechanics of CSS variables, theming, and adding custom colors. This file covers *what* to choose; customization.md covers *how* to implement it.

## Contents

- Design thinking
- Color systems (with example palettes)
- Typography (with curated font pairings)
- Visual hierarchy & layout
- Animation & motion
- Anti-patterns

---

## Design Thinking

Before selecting components, make three deliberate decisions:

**1. Product context** — The aesthetic should match what users expect from this type of product:

| Product Type | Typical Direction | Key Traits |
|---|---|---|
| SaaS / Dashboard | Professional, data-oriented | Muted backgrounds, clear data hierarchy, restrained accents |
| E-commerce | Warm, conversion-focused | Strong CTAs, product-first imagery, trust signals |
| Portfolio / Creative | Expressive, distinctive | Bold typography, generous whitespace, signature animations |
| Developer tool | Technical, information-dense | Monospace accents, compact spacing, dark mode default |
| Healthcare / Finance | Trust-forward, conservative | Blue/green families, high contrast, minimal decoration |

**2. Aesthetic tone** — Pick a direction and commit to it across the whole project. Mixing tones (playful header + corporate body + brutalist footer) looks incoherent. Options include:

- Refined minimal — lots of whitespace, subtle type, restrained palette
- Warm professional — rounded corners, soft shadows, approachable colors
- Bold editorial — large type, dramatic contrast, asymmetric layout
- Technical/utilitarian — dense information, monospace accents, functional color
- Expressive/creative — distinctive fonts, unique color combinations, signature motion

**3. One memorable choice** — Pick one element that gives the design its identity: a distinctive font pairing, an unexpected accent color, a signature animation pattern, or an unconventional layout. This prevents the "competent but forgettable" trap.

---

## Color Systems

shadcn uses CSS variables in OKLCH format defined in the `tailwindCssFile` (usually `globals.css`). The user's preset or theme choice from `ui.shadcn.com/create` or `npx shadcn init` already sets these variables — **don't override them unless the user explicitly asks for a different palette.** See [customization.md](../customization.md) for the mechanics.

### Working With the Existing Palette

- **Start from the preset.** The user chose their base color, theme, and style during initialization. Respect those choices. Build on them rather than replacing them.
- **Extend, don't replace.** If the project needs a color the preset doesn't cover (e.g. a success/warning semantic), add custom variables (see [customization.md](../customization.md#adding-custom-colors)) rather than modifying `--primary` or `--accent`.
- **Dark mode as a pair**: Design light and dark variants together. Dark mode isn't just "invert everything" — desaturate and reduce chroma while maintaining contrast. Test both modes.
- **Semantic colors carry meaning**: `--destructive` = danger/delete. Don't repurpose semantic tokens for decoration.

### Reference Palettes

These are **inspiration only** — use them when the user requests a custom palette or wants to move beyond their preset. Do not apply these unless the user explicitly asks for a color change. Each set defines only the brand-differentiating variables — structural variables (background, card, border, muted) should stay as the preset configured them.

**SaaS / Professional** — Trust blue with warm accent:

```css
:root {
  --primary: oklch(0.55 0.17 250);
  --primary-foreground: oklch(0.98 0.005 250);
  --accent: oklch(0.68 0.16 45);
  --accent-foreground: oklch(0.25 0.03 45);
  --chart-1: oklch(0.55 0.17 250);
  --chart-2: oklch(0.68 0.16 45);
  --chart-3: oklch(0.60 0.12 165);
  --chart-4: oklch(0.65 0.14 300);
  --chart-5: oklch(0.70 0.10 80);
}
```

**E-commerce / Retail** — Warm terracotta with deep forest:

```css
:root {
  --primary: oklch(0.58 0.14 35);
  --primary-foreground: oklch(0.98 0.005 35);
  --accent: oklch(0.45 0.08 160);
  --accent-foreground: oklch(0.97 0.01 160);
  --chart-1: oklch(0.58 0.14 35);
  --chart-2: oklch(0.45 0.08 160);
  --chart-3: oklch(0.70 0.12 85);
  --chart-4: oklch(0.55 0.10 280);
  --chart-5: oklch(0.62 0.16 15);
}
```

**Editorial / Creative** — Rich indigo with coral pop:

```css
:root {
  --primary: oklch(0.40 0.18 280);
  --primary-foreground: oklch(0.97 0.005 280);
  --accent: oklch(0.68 0.18 25);
  --accent-foreground: oklch(0.20 0.03 25);
  --chart-1: oklch(0.40 0.18 280);
  --chart-2: oklch(0.68 0.18 25);
  --chart-3: oklch(0.55 0.14 150);
  --chart-4: oklch(0.60 0.12 60);
  --chart-5: oklch(0.50 0.16 330);
}
```

**Developer / Technical** — Cool slate with electric green:

```css
:root {
  --primary: oklch(0.75 0.18 150);
  --primary-foreground: oklch(0.15 0.02 150);
  --accent: oklch(0.55 0.15 260);
  --accent-foreground: oklch(0.97 0.005 260);
  --chart-1: oklch(0.75 0.18 150);
  --chart-2: oklch(0.55 0.15 260);
  --chart-3: oklch(0.68 0.12 45);
  --chart-4: oklch(0.60 0.16 330);
  --chart-5: oklch(0.70 0.08 200);
}
```

---

## Typography

The user's font choice from initialization is the project's default. **Don't swap fonts unless the user explicitly requests it.** If the user wants more typographic personality, the most common enhancement is adding a display/heading font to complement their existing body font.

Import additional fonts via Google Fonts in the HTML `<head>` or `@import` in the global CSS file. Configure in `tailwind.config.js` (v3) or `@theme inline` (v4).

### Font Pairing Principles

- **Respect the chosen body font.** The user selected it during init — keep it for body text. Add a heading font for contrast if the design calls for it.
- **Contrast roles**: Display/heading font provides personality; body font provides readability. Don't use the same font for both unless it has strong weight contrast.
- **Limit to two families**: One for headings, one for body. A third (e.g. monospace for code) is fine, but more creates visual noise.
- **Match the tone**: Serif headings feel editorial/premium; geometric sans feels modern/tech; rounded sans feels friendly/approachable.
- **Test at real sizes**: A heading font at 14px might be unreadable. A body font at 48px might look bland.

### Reference Pairings

These are **inspiration only** — for when the user explicitly wants a different typographic direction. Each pairing includes a Google Fonts import. Don't apply these unless asked.

**Editorial / Magazine** — Serif authority + clean sans body:
`Playfair Display` (headings) + `Source Sans 3` (body)

```
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700&family=Source+Sans+3:wght@400;600&display=swap');
```

**SaaS / Product** — Geometric sans with strong weight range:
`Plus Jakarta Sans` (headings) + `DM Sans` (body)

```
@import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@500;700&family=DM+Sans:wght@400;500&display=swap');
```

**Creative / Agency** — Distinctive display + workhorse body:
`Syne` (headings) + `Work Sans` (body)

```
@import url('https://fonts.googleapis.com/css2?family=Syne:wght@600;700&family=Work+Sans:wght@400;500&display=swap');
```

**E-commerce / Retail** — Elegant serif + neutral sans:
`DM Serif Display` (headings) + `Nunito Sans` (body)

```
@import url('https://fonts.googleapis.com/css2?family=DM+Serif+Display&family=Nunito+Sans:wght@400;600&display=swap');
```

**Developer / Technical** — Mono accents + humanist sans:
`JetBrains Mono` (code/headings) + `IBM Plex Sans` (body)

```
@import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700&family=IBM+Plex+Sans:wght@400;500&display=swap');
```

**Minimal / Modern** — Clean geometric with personality:
`Outfit` (headings) + `Inter` (body)

```
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@500;700&family=Inter:wght@400;500&display=swap');
```

### Type Scale

Use a consistent scale. shadcn components handle their own internal text sizing — this is for page-level content:

| Element | Size | Weight | Tailwind |
|---|---|---|---|
| Page title | 2.25–3rem | 700 | `text-4xl font-bold` or `text-5xl` |
| Section heading | 1.5–1.875rem | 600–700 | `text-2xl font-semibold` or `text-3xl` |
| Card title | Component-managed | — | Use `CardTitle` |
| Body text | 1rem (16px) | 400 | `text-base` (default) |
| Small/Caption | 0.875rem | 400–500 | `text-sm` |
| Label | 0.875rem | 500 | `text-sm font-medium` |

---

## Visual Hierarchy & Layout

### Composition Principles

- **Lead with the primary action.** Each page/section should have one clear focal point — usually the main CTA or the most important data. Make it the largest, highest-contrast element.
- **Group by proximity.** Related elements close together, unrelated elements apart. Use `gap-*` utilities (not margins) to control rhythm. Cards naturally group content; use `Separator` to divide within a surface.
- **Vary density intentionally.** Dashboards benefit from higher density (compact cards, tables). Marketing pages benefit from generous whitespace. Don't apply the same spacing everywhere.
- **Break the grid occasionally.** Full-bleed sections, overlapping elements, asymmetric layouts — one deliberate break per page makes the design feel crafted rather than templated.
- **Control line length.** Body text reads best at 60–75 characters. Use `max-w-prose` (65ch) or `max-w-2xl` for text blocks. Don't let paragraphs span the full container width on desktop.

### Component Layout Patterns

| Layout Need | Pattern |
|---|---|
| Dashboard | `Sidebar` + main area with `Card` grid (2–3 cols), `Chart` in featured card, `Table` below |
| Settings page | `Tabs` for categories + `Card` per section, `FieldGroup` for form content |
| Landing page | Full-width hero → feature grid → testimonials → CTA. Use `Separator` between major sections |
| Data-heavy page | `Table` with `Badge` for status, `Popover` for details, `Pagination` at bottom |
| Empty state | Center `Empty` component vertically — keep it as the only element in the content area |

---

## Animation & Motion

Use animation to communicate state changes and spatial relationships, not as decoration. Every animation should answer "what is this telling the user?"

### Timing

| Interaction | Duration | Easing |
|---|---|---|
| Hover/focus feedback | 100–150ms | `ease-out` |
| Button press, toggle | 150–200ms | `ease-out` |
| Panel open (Sheet, Dialog) | 200–300ms | `ease-out` (enter), `ease-in` (exit) |
| Page section reveal | 300–400ms | `ease-out` with stagger |
| Skeleton → content | 200ms | `ease-out` (crossfade) |

### Patterns

- **Staggered reveals**: When multiple cards or list items enter the viewport, stagger by 30–50ms per item using `animation-delay`. Creates a natural cascade effect.
- **Exit faster than enter**: Exit animations should be ~60–70% of the enter duration to feel responsive.
- **Prefer `transform` + `opacity`**: These properties are GPU-composited. Animating `width`, `height`, `top`, `left` causes layout reflow and jank.
- **Respect `prefers-reduced-motion`**: Use Tailwind's `motion-safe:` prefix or `@media (prefers-reduced-motion: no-preference)`. Reduce to opacity-only transitions when reduced motion is active.
- **Motion library** (when available): For React projects using `motion` (formerly Framer Motion), use `<motion.div>` for entrance animations and layout transitions. It handles interruptibility and spring physics well.

### Tailwind Animation Example

```tsx
// Staggered card entrance (CSS-only)
{items.map((item, i) => (
  <Card
    key={item.id}
    className="motion-safe:animate-in motion-safe:fade-in motion-safe:slide-in-from-bottom-2"
    style={{ animationDelay: `${i * 40}ms`, animationFillMode: 'backwards' }}
  >
    ...
  </Card>
))}
```

---

## Anti-Patterns

These patterns signal low design quality. Each includes what to do instead.

| Anti-Pattern | Why It's Bad | Instead |
|---|---|---|
| Purple gradient on white with Inter/system font | The #1 "AI-generated" fingerprint — instantly generic | Work with the user's chosen palette; suggest a distinctive heading font if they want more personality |
| Every section: Card → centered text → even spacing | Monotonous, no visual hierarchy | Vary section layouts: full-bleed hero, asymmetric feature grid, dense data view, spacious CTA |
| Timid, even color distribution | No focal point, nothing draws the eye | Dominant neutral + one bold accent used sparingly at high-impact points (CTA, key stat, active state) |
| Decorative animations everywhere | Distracting, slows perceived performance | Animate only state changes and entrances; keep total animated elements to 1–2 per viewport |
| Same `gap-6` on every container | Flat spatial rhythm, no grouping | Vary gaps: tight within groups (`gap-2`, `gap-3`), generous between sections (`gap-8`, `gap-12`) |
| Raw Tailwind color values for branding | Breaks theming, won't adapt to dark mode | Define as CSS variables, use semantic tokens (see [styling.md](./styling.md)) |
