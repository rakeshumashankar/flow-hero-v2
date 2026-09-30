# Flow — Header + Hero, Version 2 (blue)

Static, framework-free reference implementation of the Flow landing page header and hero, v2 layout:
centered headline and CTAs over a light-blue canvas, floating tool icons connected by lines, and a cluster of product UI cards.
Open `index.html` in a browser, or serve the folder (`python -m http.server`) — no build step, no dependencies.

Version 1 (green, sky panel + scanner card): https://github.com/rakeshumashankar/flow-hero

```
flow-hero-v2/
├── index.html        markup (semantic, accessible)
├── css/styles.css    all styles; design tokens at the top in :root
├── js/main.js        mobile menu toggle only
└── assets/flow-mark.png   Flow swirl (transparent PNG, used as a CSS mask)
```

## Design tokens (`:root` in `css/styles.css`)

| Token | Value | Use |
|---|---|---|
| `--color-primary` | `#1E5BD6` | Buttons, logo circle, eyebrow, chart, accents. White on it = 6.1:1 |
| `--color-primary-hover` | `#174AB3` | Button hover |
| `--color-primary-soft` | `#E6EEFC` | Icon tiles, secondary-button hover |
| `--color-bg` | `#F4F7FC` | Page background (light blue) |
| `--color-surface` | `#FFFFFF` | Cards, icon circles |
| `--color-fg` | `#111827` | Headline, primary text (16.9:1 on bg) |
| `--color-fg-soft` | `#374151` | Nav links (9.6:1) |
| `--color-muted` | `#5B6475` | Secondary text (5.6:1) |
| `--color-line` | `#D5E0F0` | Connector lines |
| `--font` | Plus Jakarta Sans 400/500/600 | Google Fonts, `display=swap` |

**Type scale:** 11 (eyebrow) · 13 (nav, header buttons) · 14 (hero buttons, card titles) · 16 (wordmark) · headline `clamp(32px, 3.6vw, 52px)`, weight 500, `-0.035em`.
**Spacing:** 8pt grid (`--s-1` 4px … `--s-9` 64px).
**Layout frame:** `--page-w: 1440px`, `--gutter: 24px` (16px ≤ 860px) — identical to v1, nav unchanged.
**Controls:** header buttons 36px, hero buttons 40px tall, fully rounded.

## Breakpoints

| Width | Changes |
|---|---|
| > 1180px | Full composition: floating tool icons + connector lines |
| ≤ 1180px | Icons and lines hidden (they would collide with the cards) |
| ≤ 860px | Nav collapses into a menu button; cards become 2 columns (timeline + team, chart); right column hidden |
| ≤ 560px | CTAs stack full-width; single column showing the chart, then the team card |

## Components & behaviour

- **Header** — same as v1 (sticky, translucent, blur), blue logo circle (`.mark`, CSS mask of `flow-mark.png`).
- **Hero** — eyebrow → H1 → two CTAs (solid blue + blue-outlined).
- **Connector lines** — one inline SVG, `viewBox="0 0 100 100"` with `preserveAspectRatio="none"` so coordinates are percentages of the hero; `vector-effect: non-scaling-stroke` keeps them 1px. Each line runs from the product centre (50%, 72%) out through an icon to the edge.
- **Tool icons** — positioned with `--x` / `--y` custom properties (percent of the hero) so they stay on their lines. Generic icons (calendar, chat, docs, mail, tasks, video) — swap in official partner logos only for real integrations.
- **Product cards** — decorative illustration: the `.cluster` wrapper has `role="img"` and a descriptive `aria-label`; the cards themselves are `aria-hidden`. Content is static demo copy.
- **Motion** — staggered fade/rise on load, bars grow once (`scaleY`), icons float gently. All disabled under `prefers-reduced-motion`.

## Accessibility (WCAG 2.2 AA)

- All text/background pairs ≥ 4.5:1 (annotated in CSS).
- Skip link, visible `:focus-visible` ring, `aria-expanded` menu button, Esc closes the menu.
- Targets ≥ 24×24px (buttons 36–40px; mobile menu rows 48px). Decorative SVGs are `aria-hidden`.

## To wire up

- Replace placeholder `href`s: `#login`, `#get-started`, `#product`, `#how-it-works`, `#pricing`.
- No sub-headline by design; add one under the H1 if copy is supplied.

## Porting to React / Next.js / Vue

Suggested split: `<SiteHeader />` (shared with v1), `<Hero />`, `<ConnectorLines />`, `<ToolIcon x y />`, `<ProductCluster />` with `<TimelineCard />`, `<TeamCard />`, `<ProgressChart />`, `<MeetingCard />`, `<ProjectCard />`.
