# UX Quality Checklist

Essential UX rules for interfaces built with shadcn/ui. Read this when creating new pages or reviewing existing UI for quality. Condensed from industry standards (WCAG, Apple HIG, Material Design).

See [design.md](./design.md) for aesthetic guidance (color, typography, layout, animation).

## Contents

- Accessibility (critical)
- Touch & interaction (critical)
- Performance (high)
- Forms & feedback
- Navigation patterns
- Chart color theming
- Responsive component selection

---

## Accessibility (Critical)

These are hard requirements, not suggestions. Failing these makes the product unusable for some users.

- **Color contrast ≥ 4.5:1** for normal text, ≥ 3:1 for large text. Test both light and dark modes. shadcn's semantic tokens are designed for this — don't override them with low-contrast custom values.
- **Visible focus rings** on all interactive elements. shadcn components include `--ring` styling. Never remove focus outlines (`outline-none` without a replacement).
- **Keyboard navigation** must work for every interaction. Tab order follows visual order. Overlay components (Dialog, Sheet, Drawer) trap focus correctly — don't break this with custom wrappers.
- **`aria-label`** on icon-only buttons. A `Button` with just an icon and no visible text needs `aria-label="Close"` or similar.
- **Alt text** on meaningful images. Decorative images get `alt=""`.
- **Heading hierarchy** — sequential `h1` → `h2` → `h3`, no skipping levels. One `h1` per page.
- **Don't convey meaning by color alone** — always pair color with an icon, text, or pattern. A red `Badge` is fine; a red-only status indicator without text is not.
- **Respect `prefers-reduced-motion`** — wrap animations in `motion-safe:` or `@media (prefers-reduced-motion: no-preference)`.

## Touch & Interaction (Critical)

- **Touch targets ≥ 44×44px** (≥ 48×48dp on Android). If the visual element is smaller, extend the hit area with padding.
- **Spacing ≥ 8px between touch targets** to prevent mis-taps.
- **Don't rely on hover alone** — any hover-revealed content must also be accessible via click/tap/focus (Tooltip, HoverCard).
- **Loading feedback** — disable buttons during async operations, show `Spinner` via `data-icon`. Use `Skeleton` for content loading > 300ms.
- **Press feedback** — interactive elements should respond immediately (visual state change within 100ms).

## Performance (High)

- **Lazy load below-the-fold content** — use `loading="lazy"` for images, dynamic imports for heavy components.
- **Reserve space for async content** (images, charts, embeds) to prevent layout shift. Use `aspect-ratio` or explicit `width`/`height`.
- **Use `Skeleton`** for loading states — not blank space or full-page spinners.
- **Virtualize long lists** (50+ items) — use `@tanstack/react-virtual` or similar.
- **Debounce search/filter inputs** — 200–300ms delay before triggering queries.

## Forms & Feedback

- **Visible labels on every input** — never placeholder-only. Use shadcn's `Field` + `FieldLabel` (see [forms.md](./forms.md)).
- **Error messages below the field**, not just at the top of the form. Use `Field data-invalid` + `FieldDescription`.
- **Validate on blur** (not on keystroke) to avoid premature error messages.
- **Required field indicators** — mark required fields with an asterisk or "(required)" text.
- **Destructive actions need confirmation** — use `AlertDialog` before delete/remove/disconnect.
- **Success feedback** — confirm completed actions with `toast.success()` or brief visual flash.
- **Progressive disclosure** — reveal advanced options only when needed. Don't overwhelm users with every field upfront.

## Navigation Patterns

- **Predictable back behavior** — back button restores previous scroll position and state.
- **Bottom navigation ≤ 5 items** on mobile, with both icon and text label.
- **Active state on current nav item** — highlight with color/weight/indicator so users know where they are.
- **Modals are not navigation** — don't use Dialog/Sheet for primary flow steps. They're for focused tasks and confirmations.
- **Search must be easily reachable** — top of the page or in a persistent nav element.
- **Breadcrumbs for deep hierarchies** (3+ levels) on web.

## Chart Color Theming

shadcn provides `--chart-1` through `--chart-5` as CSS variables for data visualization. The user's preset sets initial values for these. Only override them in the `tailwindCssFile` if the default chart colors don't fit the data visualization needs. See [design.md](./design.md#reference-palettes) for reference palettes that include chart color examples.

**Rules for chart colors:**

- Space colors across the hue wheel — not adjacent shades of the same color.
- Test for colorblind accessibility. Pair hue differences with lightness differences so series remain distinct without full color vision.
- Data-ink contrast ≥ 3:1 against the chart background.
- Legends should be visible near the chart, not below a scroll fold. Use `Tooltip` for exact values on hover/tap.
- Don't rely on color alone — supplement with patterns, direct labels, or shape differences where possible.

## Responsive Component Selection

These are design decisions, not just API choices. The right component depends on viewport size:

| Desktop | Mobile | When |
|---|---|---|
| `Dialog` | `Drawer` | Focused tasks, forms, confirmations |
| `Sidebar` | Sheet or bottom nav | App-level navigation |
| Horizontal `Tabs` | `Select` or stacked `Accordion` | When 5+ tabs overflow on small screens |
| `Popover` | Full-screen `Sheet` | Complex pickers (date, color, filter) |
| Multi-column `Card` grid | Single-column stack | Dashboard/feature layouts |
| `Table` with all columns | `Card` list with key fields | Data display |
| `HoverCard` | `Popover` (tap-triggered) | Supplementary info |

Use viewport detection (`useMediaQuery` or CSS `@media`) to swap components rather than hiding/forcing desktop patterns onto mobile. Check the `framework` field from project context for routing conventions that affect layout decisions.
