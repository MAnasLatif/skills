# Tailwind CSS v4 — Core Reference

Condensed reference of Tailwind CSS v4 core concepts, sourced from the official
docs at [tailwindcss.com/docs](https://tailwindcss.com/docs) (v4.x). Use this when
you need exact directive/function syntax, theme namespaces, or variant behavior.

## Functions and Directives

### Directives

| Directive         | Purpose                                                                                   |
| ----------------- | ----------------------------------------------------------------------------------------- |
| `@import`         | Inline-import CSS, including Tailwind: `@import "tailwindcss";`                           |
| `@theme`          | Define design tokens (colors, fonts, breakpoints) that also generate utilities            |
| `@source`         | Explicitly register source files not picked up by automatic content detection             |
| `@utility`        | Add a custom utility that works with variants (`hover:`, `focus:`, `lg:`)                 |
| `@variant`        | Apply a Tailwind variant inside custom CSS                                                |
| `@custom-variant` | Define a new custom variant for your project                                              |
| `@apply`          | Inline existing utility classes into custom CSS                                           |
| `@reference`      | Import theme/utilities/variants for reference (no output) in Vue/Svelte/CSS-module blocks |

```css
/* @source — register a UI library outside content detection */
@source "../node_modules/@my-company/ui-lib";

/* @variant inside custom CSS */
.my-element {
  background: white;
  @variant dark {
    background: black;
  }
}

/* @reference — use @apply in a Vue/Svelte <style> block without duplicating CSS */
/* <style>
  @reference "../../app.css";
  h1 { @apply text-2xl font-bold text-red-500; }
</style> */
```

### Functions

```css
/* --alpha() — adjust a color's opacity */
.my-element {
  color: --alpha(var(--color-lime-300) / 50%);
  /* compiles to color-mix(in oklab, var(--color-lime-300) 50%, transparent) */
}

/* --spacing() — generate a spacing value from your theme scale */
.my-element {
  margin: --spacing(4); /* calc(var(--spacing) * 4) */
}
```

```html
<!-- --spacing() is useful in arbitrary values with calc() -->
<div class="py-[calc(--spacing(4)-1px)]"><!-- ... --></div>
```

### v3 compatibility directives

```css
@config "../../tailwind.config.js"; /* load a legacy JS config */
@plugin "@tailwindcss/typography"; /* load a legacy JS plugin */
/* theme() function is deprecated — prefer CSS theme variables */
```

> `corePlugins`, `safelist`, and `separator` from JS configs are **not** supported
> in v4. To safelist utilities, use `@source inline(...)`.

## Theme Variable Namespaces

Each namespace maps to one or more utility/variant APIs. Defining a variable in a
namespace creates the corresponding utility automatically.

| Namespace          | Generates                                                     |
| ------------------ | ------------------------------------------------------------- |
| `--color-*`        | Color utilities: `bg-*`, `text-*`, `fill-*`, `border-*`       |
| `--font-*`         | Font family utilities: `font-sans`                            |
| `--text-*`         | Font size utilities: `text-xl` (with `--text-*--line-height`) |
| `--font-weight-*`  | `font-bold`, `font-medium`                                    |
| `--tracking-*`     | Letter spacing: `tracking-wide`                               |
| `--leading-*`      | Line height: `leading-tight`                                  |
| `--breakpoint-*`   | Responsive variants: `sm:*`, `lg:*`                           |
| `--container-*`    | Container query variants `@sm:*` and `max-w-*` sizes          |
| `--spacing-*`      | Spacing/sizing: `px-4`, `max-h-16`                            |
| `--radius-*`       | Border radius: `rounded-sm`                                   |
| `--shadow-*`       | Box shadow: `shadow-md`                                       |
| `--inset-shadow-*` | Inset shadow: `inset-shadow-xs`                               |
| `--drop-shadow-*`  | Drop shadow filter: `drop-shadow-md`                          |
| `--blur-*`         | Blur filter: `blur-md`                                        |
| `--perspective-*`  | `perspective-near`                                            |
| `--aspect-*`       | `aspect-video`                                                |
| `--ease-*`         | Timing functions: `ease-out`                                  |
| `--animate-*`      | Animation utilities: `animate-spin`                           |

### `@theme` vs `:root`

Use `@theme` when a design token should map to a utility class. Use `:root` for
regular CSS variables that should **not** generate utilities. Theme variables must
be top-level (not nested under selectors or media queries).

### Customizing the theme

```css
/* Extend — add a new token / utility */
@theme {
  --font-script: "Great Vibes", cursive; /* enables font-script */
}

/* Override — redefine an existing token */
@theme {
  --breakpoint-sm: 30rem; /* sm:* now triggers at 30rem */
}

/* Override an entire namespace — clear then redefine */
@theme {
  --color-*: initial;
  --color-white: #fff;
  --color-midnight: #121063;
}

/* Custom theme — disable ALL defaults */
@theme {
  --*: initial;
  --spacing: 4px;
  --font-body: Inter, sans-serif;
  --color-lagoon: oklch(0.72 0.11 221.19);
}
```

### Theme modifiers

```css
/* inline — utility uses the value directly, not a var() reference.
   Required when a token references another variable. */
@theme inline {
  --font-sans: var(--font-inter);
}

/* static — always emit CSS variables even if unused */
@theme static {
  --color-primary: var(--color-red-500);
  --color-secondary: var(--color-blue-500);
}
```

### Animation keyframes

Put `@keyframes` **inside** `@theme` to emit them alongside an `--animate-*` token.
Define them **outside** `@theme` if they must always be included.

```css
@theme {
  --animate-fade-in-scale: fade-in-scale 0.3s ease-out;
  @keyframes fade-in-scale {
    0% {
      opacity: 0;
      transform: scale(0.95);
    }
    100% {
      opacity: 1;
      transform: scale(1);
    }
  }
}
```

### Sharing tokens across projects

```css
/* packages/brand/theme.css */
@theme {
  --*: initial;
  --color-lagoon: oklch(0.72 0.11 221.19);
}
```

```css
/* packages/admin/app.css */
@import "tailwindcss";
@import "../brand/theme.css";
```

## Colors

- Default palette: 22 color scales, each with 11 steps (`50`–`950`), defined in
  OKLCH for better perceptual uniformity.
- Color utilities: `bg-*`, `text-*`, `border-*`, `outline-*`, `shadow-*`, `ring-*`,
  `accent-*`, `caret-*`, `fill-*`, `stroke-*`, `decoration-*`, and more.

```html
<!-- Opacity modifier: /<number> sets the alpha channel -->
<div class="bg-sky-500/50"></div>
<div class="bg-pink-500/[71.37%]"></div>
<div class="bg-cyan-400/(--my-alpha-value)"></div>
```

```css
/* Reference colors as CSS variables in the --color-* namespace */
@layer components {
  .typography a {
    color: var(--color-blue-500);
  }
}

/* --alpha() adjusts opacity when referencing a color variable */
.hit {
  background-color: --alpha(var(--color-gray-950) / 10%);
}
```

```css
/* Custom / overridden colors */
@theme {
  --color-midnight: #121063; /* enables bg-midnight, text-midnight, ... */
}

/* Disable specific default colors (removes their CSS variables) */
@theme {
  --color-lime-*: initial;
  --color-fuchsia-*: initial;
}

/* Reference another color — requires @theme inline */
:root {
  --acme-canvas-color: oklch(0.967 0.003 264.542);
}
[data-theme="dark"] {
  --acme-canvas-color: oklch(0.21 0.034 264.665);
}
@theme inline {
  --color-canvas: var(--acme-canvas-color);
}
```

## Dark Mode

```css
/* Default: driven by prefers-color-scheme. Override the dark variant to toggle manually. */

/* Class-based */
@import "tailwindcss";
@custom-variant dark (&:where(.dark, .dark *));

/* Data-attribute-based */
@custom-variant dark (&:where([data-theme=dark], [data-theme=dark] *));
```

```js
// Three-way toggle (light / dark / system) — inline in <head> to avoid FOUC
document.documentElement.classList.toggle(
  "dark",
  localStorage.theme === "dark" ||
    (!("theme" in localStorage) &&
      window.matchMedia("(prefers-color-scheme: dark)").matches),
);

localStorage.theme = "light"; // explicit light
localStorage.theme = "dark"; // explicit dark
localStorage.removeItem("theme"); // follow OS preference
```

## Responsive Design

Mobile-first: unprefixed utilities apply at all sizes; prefixed ones apply at that
breakpoint **and up**. To target mobile, use the unprefixed utility — not `sm:`.

| Breakpoint | Min width      | Media query               |
| ---------- | -------------- | ------------------------- |
| `sm`       | 40rem (640px)  | `@media (width >= 40rem)` |
| `md`       | 48rem (768px)  | `@media (width >= 48rem)` |
| `lg`       | 64rem (1024px) | `@media (width >= 64rem)` |
| `xl`       | 80rem (1280px) | `@media (width >= 80rem)` |
| `2xl`      | 96rem (1536px) | `@media (width >= 96rem)` |

```html
<!-- Mobile-first: center on mobile, left-align from sm up -->
<div class="text-center sm:text-left"></div>

<!-- Breakpoint range: only between md and xl -->
<div class="md:max-xl:flex"></div>

<!-- Single breakpoint only: md but not lg+ -->
<div class="md:max-lg:flex"></div>

<!-- Arbitrary one-off breakpoints -->
<div class="max-[600px]:bg-sky-300 min-[320px]:text-center"></div>
```

```css
/* Custom breakpoints */
@theme {
  --breakpoint-xs: 30rem; /* enables xs:* */
  --breakpoint-2xl: 100rem; /* overrides 2xl */
  --breakpoint-2xl: initial; /* removes a default breakpoint */
}
```

> Always use the same unit (`rem`) for breakpoints so generated utilities sort
> correctly.

## Container Queries

Mark a parent `@container`, then style children by the container's size.

```html
<div class="@container">
  <div class="flex flex-col @md:flex-row"><!-- ... --></div>
</div>

<!-- Max-width container query -->
<div class="@container"><div class="@max-md:flex-col"></div></div>

<!-- Range -->
<div class="@container"><div class="@sm:@max-md:flex-col"></div></div>

<!-- Named containers -->
<div class="@container/main">
  <div class="@sm/main:flex-col"><!-- ... --></div>
</div>

<!-- Container query length units -->
<div class="@container"><div class="w-[50cqw]"></div></div>
```

Built-in container sizes: `@3xs` (16rem) → `@7xl` (80rem). Customize with
`--container-*`:

```css
@theme {
  --container-8xl: 96rem; /* enables @8xl:* */
}
```

## Adding Custom Styles

### Arbitrary values, properties, variants

```html
<!-- Arbitrary value -->
<div class="top-[117px] lg:top-[344px]"></div>

<!-- Arbitrary property (like inline style, but supports modifiers) -->
<div class="[mask-type:luminance] hover:[mask-type:alpha]"></div>

<!-- Arbitrary CSS variables -->
<div class="[--scroll-offset:56px] lg:[--scroll-offset:44px]"></div>

<!-- Arbitrary variant (on-the-fly selector) -->
<li class="lg:[&:nth-child(-n+3)]:hover:underline"></li>

<!-- Whitespace: use _ for spaces in arbitrary values -->
<div class="grid grid-cols-[1fr_500px_2fr]"></div>

<!-- Type hints resolve ambiguity with CSS variables -->
<div class="text-(length:--my-var)"></div>
<div class="text-(color:--my-var)"></div>

<!-- CSS variable shorthand: fill-(--x) == fill-[var(--x)] -->
<div class="fill-(--my-brand-color)"></div>
```

### Custom CSS via layers

```css
/* Base element defaults */
@layer base {
  h1 {
    font-size: var(--text-2xl);
  }
}

/* Component classes (overridable by utilities) */
@layer components {
  .card {
    background-color: var(--color-white);
    border-radius: var(--radius-lg);
    padding: --spacing(6);
    box-shadow: var(--shadow-xl);
  }
}
```

### Custom utilities with `@utility`

```css
/* Simple */
@utility content-auto {
  content-visibility: auto;
}

/* Complex (nesting) */
@utility scrollbar-hidden {
  &::-webkit-scrollbar {
    display: none;
  }
}

/* Functional — resolve against theme keys */
@theme {
  --tab-size-2: 2;
  --tab-size-4: 4;
  --tab-size-github: 8;
}
@utility tab-* {
  tab-size: --value(--tab-size-*);
}

/* Functional — bare values (number | integer | ratio | percentage) */
@utility tab-* {
  tab-size: --value(integer);
}

/* Functional — literal values */
@utility tab-* {
  tab-size: --value("inherit", "initial", "unset");
}

/* Functional — arbitrary values */
@utility tab-* {
  tab-size: --value([integer]);
}

/* Theme + bare + arbitrary together (failed resolutions are dropped) */
@utility tab-* {
  tab-size: --value(--tab-size-*, integer, [integer]);
}

/* Default value when used with no argument */
@utility tab-* {
  tab-size: --value(integer, --default(4));
}

/* Modifiers via --modifier() (e.g. text size with line-height) */
@utility text-* {
  font-size: --value(--text-*, [length]);
  line-height: --modifier(--leading-*, [length], [*]);
}

/* Negative variants are registered separately */
@utility -inset-* {
  inset: --spacing(--value(integer) * -1);
}

/* Fractions use the ratio data type */
@utility aspect-* {
  aspect-ratio: --value(--aspect-ratio-*, ratio, [ratio]);
}
```

### Custom variants with `@custom-variant`

```css
/* Shorthand */
@custom-variant theme-midnight (&:where([data-theme="midnight"] *));

/* Nested form (use @slot for the styled rule) */
@custom-variant any-hover {
  @media (any-hover: hover) {
    &:hover {
      @slot;
    }
  }
}
```

```html
<button class="theme-midnight:bg-black"></button>
```
