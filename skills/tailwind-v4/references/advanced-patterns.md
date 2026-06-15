# Tailwind CSS v4: Advanced Patterns

Advanced Tailwind CSS v4 patterns including native CSS animations, custom utilities, theme modifiers, namespace overrides, semi-transparent color variants, container queries, and the v3-to-v4 migration checklist.

## Native CSS Animations (v4)

```css
/* Animation tokens — keyframes inside @theme are emitted with the --animate-* var */
@theme {
  --animate-dialog-in: dialog-fade-in 0.2s ease-out;
  --animate-dialog-out: dialog-fade-out 0.15s ease-in;
}

@keyframes dialog-fade-in {
  from {
    opacity: 0;
    transform: scale(0.95) translateY(-0.5rem);
  }
  to {
    opacity: 1;
    transform: scale(1) translateY(0);
  }
}

@keyframes dialog-fade-out {
  from {
    opacity: 1;
    transform: scale(1) translateY(0);
  }
  to {
    opacity: 0;
    transform: scale(0.95) translateY(-0.5rem);
  }
}

/* Native popover entry/exit animations using @starting-style */
[popover] {
  transition:
    opacity 0.2s,
    transform 0.2s,
    display 0.2s allow-discrete;
  opacity: 0;
  transform: scale(0.95);
}

[popover]:popover-open {
  opacity: 1;
  transform: scale(1);
}

@starting-style {
  [popover]:popover-open {
    opacity: 0;
    transform: scale(0.95);
  }
}
```

## Dark Mode (v4)

```css
/* Class-based dark mode variant */
@import "tailwindcss";
@custom-variant dark (&:where(.dark, .dark *));

/* Data-attribute-based dark mode variant */
@custom-variant dark (&:where([data-theme="dark"], [data-theme="dark"] *));
```

```css
/* Dark theme token overrides applied when .dark is present up the tree */
.dark {
  --color-background: oklch(14.5% 0.025 264);
  --color-foreground: oklch(98% 0.01 264);
  --color-primary: oklch(98% 0.01 264);
  --color-border: oklch(22% 0.02 264);
}
```

## Custom Utilities with `@utility`

```css
/* Custom utility for decorative lines */
@utility line-t {
  @apply relative before:absolute before:top-0 before:-left-[100vw] before:h-px before:w-[200vw] before:bg-gray-950/5 dark:before:bg-white/10;
}

/* Custom utility for text gradients */
@utility text-gradient {
  @apply bg-gradient-to-r from-primary to-accent bg-clip-text text-transparent;
}
```

## Theme Modifiers

```css
/* Use @theme inline when referencing other CSS variables */
@theme inline {
  --font-sans: var(--font-inter), system-ui;
}

/* Use @theme static to always generate CSS variables (even when unused) */
@theme static {
  --color-brand: oklch(65% 0.15 240);
}
```

## Namespace Overrides

```css
@theme {
  /* Clear all default colors and define your own */
  --color-*: initial;
  --color-white: #fff;
  --color-black: #000;
  --color-primary: oklch(45% 0.2 260);
  --color-secondary: oklch(65% 0.15 200);

  /* Clear ALL defaults for a minimal setup */
  /* --*: initial; */
}
```

## Semi-transparent Color Variants

```css
@theme {
  /* Use color-mix() for alpha variants */
  --color-primary-50: color-mix(in oklab, var(--color-primary) 5%, transparent);
  --color-primary-100: color-mix(
    in oklab,
    var(--color-primary) 10%,
    transparent
  );
  --color-primary-200: color-mix(
    in oklab,
    var(--color-primary) 20%,
    transparent
  );
}
```

## Container Queries

```css
@theme {
  --container-xs: 20rem;
  --container-sm: 24rem;
  --container-md: 28rem;
  --container-lg: 32rem;
}
```

## v3 to v4 Migration Checklist

- [ ] Replace `tailwind.config.ts` with CSS `@theme` block
- [ ] Change `@tailwind base/components/utilities` to `@import "tailwindcss"`
- [ ] Move color definitions to `@theme { --color-*: value }`
- [ ] Replace `darkMode: "class"` with `@custom-variant dark`
- [ ] Move `@keyframes` inside `@theme` blocks (ensures keyframes output with theme)
- [ ] Replace `require("tailwindcss-animate")` with native CSS animations
- [ ] Update `h-10 w-10` to `size-10` (new utility)
- [ ] Consider OKLCH colors for better color perception
- [ ] Replace custom plugins with `@utility` directives

## Best Practices

### Do's

- **Use `@theme` blocks** - CSS-first configuration is v4's core pattern
- **Use OKLCH colors** - Better perceptual uniformity than HSL
- **Use semantic tokens** - `bg-primary` not `bg-blue-500`
- **Use `size-*`** - New shorthand for `w-* h-*`
- **Add accessibility** - ARIA attributes, focus states

### Don'ts

- **Don't use `tailwind.config.ts`** - Use CSS `@theme` instead
- **Don't use `@tailwind` directives** - Use `@import "tailwindcss"`
- **Don't use arbitrary values** - Extend `@theme` instead
- **Don't hardcode colors** - Use semantic tokens
- **Don't forget dark mode** - Test both themes
