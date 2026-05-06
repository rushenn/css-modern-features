# rushenn/css-modern-features

An agent skill that enforces modern CSS practices based on your project's browser targets. Covers 57+ CSS features across color, layout, selectors, animation, typography, positioning, and component patterns. Works with Claude Code, Cursor, Windsurf, Codex, Cline, GitHub Copilot, and other AI coding agents.

## Installation

```bash
npx skills add rushenn/css-modern-features
```

## How the skill works

It operates in three stages:

### 1. Browser target detection
Before writing any CSS, the skill reads your project's browser configuration - package.json browserslist, .browserslistrc, or framework-specific config files (Vite, Next.js, Astro). This is the foundation. Every feature recommendation is scoped to what your users' browsers actually support.

### 2. Tier mapping

The skill detects your `browserslist` config (from `package.json`, `.browserslistrc`, or framework config) and maps it to a tier:

| Tier | Typical target | Strategy |
|------|---------------|----------|
| **Tier 0** | Latest Chrome/Edge only | Experimental features with `@supports` |
| **Tier 1** | Last 1–2 Chrome/Edge/Firefox/Safari | Everything stable |
| **Tier 2** | Last 2 versions / > 0.5% | Modern baseline |
| **Tier 3** | > 1% / Safari 15- | Selective + `@supports` guards |
| **Tier 4** | IE 11 / legacy Android | Fundamentals only |

### 3. Feature checklist

Before finalizing any CSS output, the agent runs a 57-item checklist across eight categories: color and theming, layout and sizing, selectors and logic, animation, typography, positioning, visual effects, and component patterns. Features are applied based on the project's tier. Tier 3 targets get @supports guards and fallbacks automatically.

## Example output

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
}
```

## Features covered

| Category | Features |
|----------|----------|
| Color & Theming | OKLCH, color-mix(), relative color syntax, light-dark(), accent-color |
| Layout & Sizing | clamp(), min(), max(), round(), dynamic viewport units (dvh/svh/lvh), container queries, subgrid, field-sizing: content, margin-trim |
| Selectors & Logic | :has(), :is(), :where(), @layer, @scope, CSS nesting |
| Animation | Scroll-driven animations, @starting-style, transition-behavior: allow-discrete, view transitions, offset-path, interpolate-size |
| Typography | text-wrap: balance/pretty, cap/lh units, ::marker, counter(), font-size-adjust |
| Positioning | Anchor positioning, logical properties, inset, scroll-margin/padding |
| Visual Effects | backdrop-filter, mix-blend-mode, clip-path, gradient borders, border-image |
| Component Patterns | env(safe-area-inset-*), scrollbar-color/width, overscroll-behavior, media features (hover: hover), (pointer: coarse), (scripting: none) |
| Experimental (watch list) | interpolate-size, masonry, if(), @function, reading-flow |
| Houdini | Paint Worklet, CSS.registerProperty() |

## Tailwind support

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
