# rushenn/css-modern-features

A collection of agent skills for Claude Code, Cursor, Windsurf, Codex, and other AI coding agents.

## Install all skills

```bash
npx skills add rushenn/css-modern-features
```

## Install a specific skill

```bash
npx skills add rushenn/css-modern-features@css-modern-features
```

---

## Skills

| Skill | Description |
|-------|-------------|
| [`css-modern-features`](#css-modern-features) | Use modern CSS features based on your project's browser targets |

---

## `css-modern-features`

> Use modern CSS features wherever browser support allows — `clamp()`, `:has()`, container queries, OKLCH colors, scroll-driven animations, anchor positioning, `@starting-style`, `light-dark()`, `text-wrap: balance`, and 50+ more.

The skill reads your `browserslist` config and maps it to a **feature tier**, then runs a checklist of ~57 modern CSS features before finalizing any CSS output. It covers plain CSS, CSS Modules, styled-components, Tailwind arbitrary values, and inline styles in React/Vue/Svelte.

### What it prevents

| Legacy pattern | Modern replacement |
|---|---|
| `100vh` on mobile | `100dvh` / `100svh` |
| Breakpoint-stepped font sizes | `clamp(1.5rem, 4vw, 3rem)` |
| JS scroll listeners for animation | Scroll-driven animations |
| Two `@media prefers-color-scheme` blocks | `light-dark()` |
| JS `getBoundingClientRect` for tooltips | Anchor positioning |
| `::-webkit-scrollbar` hacks | `scrollbar-color` / `scrollbar-width` |
| `display:none` that can't animate | `@starting-style` + `transition-behavior: allow-discrete` |
| `height: auto` can't transition | `interpolate-size: allow-keywords` |
| Padding-top ratio hack | `aspect-ratio` |
| JS class toggling for parent styles | `:has()` selector |
| BEM / data attributes for scoping | `@scope` |
| Specificity hacks | `@layer` |

### Browser tier system

The skill detects your `browserslist` config (from `package.json`, `.browserslistrc`, or framework config) and maps it to a tier:

| Tier | Typical target | Strategy |
|------|---------------|----------|
| **Tier 0** | Latest Chrome/Edge only | Experimental features with `@supports` |
| **Tier 1** | Last 1–2 Chrome/Edge/Firefox/Safari | Everything stable |
| **Tier 2** | Last 2 versions / > 0.5% | Modern baseline |
| **Tier 3** | > 1% / Safari 15- | Selective + `@supports` guards |
| **Tier 4** | IE 11 / legacy Android | Fundamentals only |

### Example output

**Input prompt**: *"Make a responsive card component with dark mode"*

Without this skill, an agent typically writes:
```css
.card {
  width: 100%;
  max-width: 400px;
  background: #ffffff;
  padding: 24px;
}

@media (max-width: 768px) {
  .card { padding: 16px; }
}

@media (prefers-color-scheme: dark) {
  .card { background: #1a1a1a; }
}
```

With `css-modern-features` skill (Tier 2 target):
```css
.card {
  width: min(100%, 400px);
  background: light-dark(oklch(0.98 0 0), oklch(0.15 0 0));
  padding: clamp(1rem, 3cqw, 1.5rem);
  container-type: inline-size;
  border-radius: 0.75rem;
  text-wrap: balance; /* headings inside */
}

/* Component-level, not viewport-level */
@container (min-width: 500px) {
  .card { display: grid; grid-template-columns: 1fr 2fr; }
}
```

### Features covered

**Color & Theming** — OKLCH, `color-mix()`, relative color syntax, `light-dark()`, `accent-color`

**Layout & Sizing** — `clamp()`, `min()`, `max()`, `round()`, dynamic viewport units (`dvh/svh/lvh`), container queries, subgrid, `field-sizing: content`, `margin-trim`

**Selectors & Logic** — `:has()`, `:is()`, `:where()`, `@layer`, `@scope`, CSS nesting

**Animation** — Scroll-driven animations, `@starting-style`, `transition-behavior: allow-discrete`, view transitions, `offset-path`, `interpolate-size`

**Typography** — `text-wrap: balance/pretty`, `cap`/`lh` units, `::marker`, `counter()`, `font-size-adjust`

**Positioning** — Anchor positioning, logical properties, `inset`, `scroll-margin/padding`

**Visual Effects** — `backdrop-filter`, `mix-blend-mode`, `clip-path`, gradient borders, `border-image`

**Component Patterns** — `env(safe-area-inset-*)`, `scrollbar-color/width`, `overscroll-behavior`, media features `(hover: hover)`, `(pointer: coarse)`, `(scripting: none)`

**Experimental (watch list)** — `interpolate-size`, `masonry`, `if()`, `@function`, `reading-flow`

**Houdini** — Paint Worklet, `CSS.registerProperty()`

### Tailwind support

Modern CSS features map directly to Tailwind arbitrary values:

```html
<!-- Fluid type -->
<h1 class="text-[clamp(1.5rem,4vw,3rem)] text-wrap-balance">

<!-- OKLCH color -->
<div class="bg-[oklch(0.7_0.15_200)]">

<!-- Dynamic viewport -->
<div class="h-[100dvh]">

<!-- Safe area (iOS) -->
<nav class="pt-[env(safe-area-inset-top)]">

<!-- Accent color -->
<input type="checkbox" class="accent-[oklch(0.6_0.2_260)]">
```

---

## Contributing

PRs welcome — especially new skills. Each skill lives in its own folder with a `SKILL.md` and optional `references/` directory.

```
rushenn/css-modern-features
├── css-modern-features/
│   ├── SKILL.md
│   └── references/
│       ├── color.md
│       ├── layout.md
│       └── ...
└── your-new-skill/
    └── SKILL.md
```

## License

MIT
