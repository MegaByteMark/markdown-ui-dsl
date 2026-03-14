---
framework: Next.js + TailwindCSS
theme: ./design-systems/web-tailwind.md
component: src/components/ProductPage.tsx
---

# Responsive Layout Example

This spec demonstrates `@breakpoint` responsive directives (Option C).
Each directive scopes design tokens to a viewport size using the blockquote
hint syntax already supported by the DSL. Directives are mobile-first and
additive — `@sm` sets the base, larger breakpoints layer on top.

***

## Page Header

::: HEADER :::
> @sm layout: stacked, gap: compact
> @md layout: row

[ IMG: Brand Logo ]

|[ Products ]| Pricing | About |

[ Login ](#login)
--- END ---

***

## Hero Section

||| COLUMN |||
> @sm padding: compact, text: base
> @lg padding: spacious, text: large

# Build something great.
Responsive design, defined in plain text.

=== ROW ===
> @sm layout: stacked, gap: compact
> @md layout: row, gap: default

[ Get Started ](#cta)
[ View Docs ](/docs)
--- END ---

--- END ---

***

## Product Cards

> @sm layout: stacked, gap: compact
> @md layout: grid-2, gap: default
> @lg layout: grid-3, gap: spacious

- ::: CARD :::
  > @sm padding: compact
  > @lg padding: default

  [ IMG: Feature Icon A ]

  ### Fast
  Optimised for performance at every breakpoint.
  --- END ---

- ::: CARD :::
  > @sm padding: compact
  > @lg padding: default

  [ IMG: Feature Icon B ]

  ### Flexible
  Token-driven layout adapts to any screen size.
  --- END ---

- ::: CARD :::
  > @sm padding: compact
  > @lg padding: default

  [ IMG: Feature Icon C ]

  ### Simple
  Write once in plain Markdown, generate anywhere.
  --- END ---

***

## Side-by-Side Content Block

::: CARD :::
> @sm layout: stacked, padding: compact
> @lg layout: row, padding: default

[ IMG: Product Screenshot (rounded) ]

||| COLUMN |||
## Why Responsive DSL?
Design-system tokens map cleanly to Tailwind responsive prefixes,
Flutter LayoutBuilder constraints, or Bootstrap infix classes —
depending on your target framework.

[ Learn More ](/docs/responsive)
--- END ---

--- END ---

***

::: FOOTER :::
> @sm layout: stacked, gap: compact, text: small
> @md layout: row, gap: default

© 2026 Markdown-UI DSL

[ GitHub ](https://github.com/MegaByteMark/markdown-ui-dsl)
[ License ](/license)
--- END ---
