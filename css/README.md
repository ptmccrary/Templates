# CSS Template

A [CUBE CSS](https://cube.fyi/) starter organized into cascade layers.

## Usage

`main.css` is the single entry point — it declares the layer order and imports everything.

### Vite

Import once in your JS/TS entry file:

```js
import "./css/main.css";
```

### PostCSS

With [`postcss-import`](https://github.com/postcss/postcss-import) configured, import in your root stylesheet:

```css
@import "css/main.css";
```

> `@layer` declarations must come before any `@import` statements in standard CSS. `postcss-import` handles this correctly by inlining imports while respecting the layer order declared at the top of `main.css`.

## Layer order

Defined in `main.css`:

```css
@layer theme, reset, base, compositions, utilities, blocks;
```

| Layer | Purpose |
| --- | --- |
| `theme` | Design tokens as CSS custom properties |
| `reset` | Opinionated browser normalization |
| `base` | Low-specificity global element styles |
| `compositions` | Layout primitives |
| `utilities` | Single-purpose helper classes |
| `blocks` | Component-level styles |

## Directory structure

```text
css/
├── main.css               # Layer order declaration — import this first
├── global/
│   ├── variables.css      # theme layer: all design tokens
│   ├── reset.css          # reset layer: modern CSS reset
│   ├── global.css         # base layer: element defaults
│   └── print.css          # base layer: print styles (stub)
├── compositions/          # Layout primitives
├── utilities/             # Utility classes
└── blocks/                # Component styles
```

## Global

### `variables.css` — design tokens

All tokens live in `:root` under the `theme` layer.

| Group | Variables |
| --- | --- |
| Font families | `--font-sans-base`, `--font-sans`, `--font-display` |
| Font weights | `--font-light` through `--font-black` |
| Line heights | `--leading-micro` (0.85) → `--leading-loose` (1.7) |
| Colors | Base, brand, status, grays |
| Semantic colors | `--color-background/foreground/accent`, `--color-error/pending/success` |
| Shadows | `--shadow-elevation-low/medium/high` |
| Font sizes | `--font-size--3` → `--font-size-9` (fluid via `clamp`) |
| Spacing | `--space-2xs` → `--space-4xl` + one-up pairs like `--space-s-m` |
| Easing | `--ease-in/out/in-out-sine`, `--ease-in/out/in-out-cubic` |
| Measures | `--radius-s/m/l`, `--stroke`, `--flow-space`, `--site-margin`, `--wrapper-max-width` |
| Z-index | `--z-base`, `--z-sidebar`, `--z-toast`, `--z-popup`, `--z-overlay` |

Font sizes and spacing use [Utopia](https://utopia.fyi/) fluid scale (400px → 1440px viewport).

### `reset.css`

Based on [Andy Bell's modern reset](https://piccalil.li/blog/a-more-modern-css-reset/). Key rules: border-box sizing, removed margin on block elements, text-wrap balance on headings, font inheritance for inputs.

### `global.css`

Low-specificity defaults for all HTML elements: typography scale applied to headings, link styles with `text-decoration-skip-ink`, focus-visible outlines, form element defaults, details/summary accordion with animated `::details-content`, SVG sizing helpers.

## Compositions

Layout primitives, mostly from [Every Layout](https://every-layout.dev/). All scoped to `@layer compositions`.

| Class | Description | Key custom properties |
| --- | --- | --- |
| `.wrapper` | Page container with inline padding | `--wrapper-max-width` (1360px), `--site-margin` |
| `.center` | Horizontally centers an element | `--measure` |
| `.flow` | Vertical spacing between siblings (`* + *`) | `--flow-space` (1em) |
| `.cluster` | Flex row that wraps, consistent gap | `--space-cluster`, `--cluster-horizontal-alignment`, `--cluster-vertical-alignment` |
| `.repel` | Pushes two items apart (`space-between`), stacks on small viewports | `--repel-gutter`, `--repel-vertical-alignment` |
| `.sidebar` | Fixed-width sidebar + flexible main content, stacks when cramped | `--sidebar-target-width` (20rem), `--sidebar-content-min-width` (50%), `--sidebar-gutter` |
| `.switcher` | Two items inline until container is too narrow, then stacks | `--switcher-target-container-width` (40rem), `--switcher-gutter` |
| `.grid` | Auto-fill/fit grid | `--grid-min-item-size` (16rem), `--grid-gutter`, `--grid-placement` |
| `.cover` | Full-screen section with vertically centered child | `--cover-height` (100vh) |
| `.frame` | Aspect-ratio box (default 16/9) for images/video | `--n`, `--d` |
| `.reel` | Horizontal scrolling container, hidden scrollbar | — |
| `.pile` | Stacks children on top of each other (`grid-area: 1/1`) | — |
| `.icon` / `.with-icon` | Inline icon sizing and alignment | `--icon-width`, `--icon-height`, `--icon-vertical-alignment` |

### Wrapper variants

```html
<div class="wrapper" data-full-width>               <!-- removes max-width -->
<div class="wrapper" data-wrapper-padding="none">   <!-- removes inline padding -->
<div class="wrapper" data-wrapper-padding="left">   <!-- removes right padding only -->
<div class="wrapper" data-unset-position>           <!-- position: static -->
```

### Grid variants

```html
<div class="grid" data-layout="50-50">     <!-- two equal columns -->
<div class="grid" data-layout="thirds">    <!-- three columns -->
<div class="grid" data-layout="quarters">  <!-- four columns -->
```

## Utilities

Scoped to `@layer utilities`.

| File | Classes |
| --- | --- |
| `layout.css` | Text alignment (`.align-left/center/right`), flex helpers (`.flex`, `.flex-col`, `.items-center`, `.justify-center/between/end`), gap/margin/padding scale (`.gap-1/2/4`, `.mt/mb-2/4/6`, `.p-2/4/6`), `.w-100` |
| `visually-hidden.css` | `.visually-hidden` — accessible hide (still read by screen readers) |

## Blocks

Scoped to `@layer blocks`.

### `.btn` (`blocks/buttons.css`)

Base button reset with transition on `background-color`, `color`, `border-color`, `transform`. Full-width on viewports < 400px.

```html
<!-- Sizes -->
<button class="btn" data-btn-size="sm">Small</button>

<!-- Shapes -->
<button class="btn" data-btn-shape="pill">Pill</button>
<button class="btn" data-btn-shape="circle">○</button>

<!-- Themes -->
<button class="btn" data-btn-theme="success">Success</button>
```
