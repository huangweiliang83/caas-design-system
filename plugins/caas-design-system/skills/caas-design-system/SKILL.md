---
name: caas-design-system
description: Use whenever building or modifying any UI, pages, screens or components. Applies the CAAS Design System (tokens, components, patterns).
---

# CAAS Design System

The CAAS Design System covers CAAS digital services and applications. It is built on the Singapore Government Design System (SGDS) and uses the CAAS brand: Jet Blue `#193055`, Sky Blue `#0087BF`, Crimson Red `#A80061`, and the Inter typeface. The stack is React + Tailwind v4 + Radix UI primitives, plus `@govtechsg/sgds-react` / `@govtechsg/sgds-web-component`. Everything you need is in the reference files below. No Figma access is required.

## Core rules (non-negotiable)

1. **Use only tokens.** Every colour, font size, weight, line height, spacing, radius, border and shadow must come from `references/tokens.md`, as a CSS variable (`var(--sgds-…)`, `var(--brand-…)`, `var(--primary)`) or a mapped Tailwind class (`bg-primary`, `text-muted-foreground`, `rounded-md`).
2. **Never hardcode** hex/rgb colours, px spacing, font families or font sizes in components. If you need a value that has no token, stop and say so. Don't invent one.
3. **Prefer semantic tokens over primitives.** Use `--sgds-link-color-default` for links, not `--brand-sky-blue-600`.
4. **Reuse components.** Check `references/components.md` before writing markup. Compose existing components and don't recreate them with raw HTML or inline styles.
5. **Inter** is the only typeface (`--sgds-font-family-brand`). Body text is 16px with 1.5 line height, and never below 14px on mobile.
6. **Crimson Red** is for rare emphasis and destructive actions only. It has no tints.
7. **Don't override** SGDS status colours (success, danger, warning).
8. **Accessibility:** WCAG 2.1 AA contrast (4.5:1 text, 3:1 large text), a visible focus ring (`--sgds-outline-focus`), keyboard reach for everything, `aria-label` on icon-only controls, touch targets of at least 44px.
9. **Icons:** Lucide (`lucide-react`) only, sized with `h-4 w-4` / `h-5 w-5` / `h-6 w-6` / `h-8 w-8`, and inheriting text colour.
10. **Don't copy hardcoded demo colours** from the Make file (`#1e3a5f`, `blue-600` and similar). See `tokens.md` §11 for known source inconsistencies and how to resolve them.

## Workflow for building a screen

1. **Identify the page type.** Match it to a template in `references/patterns.md` §2: Homepage, Index, Article, Content, Collection, Dark Site or E-services.
2. **Lay out with the grid.** Use `sgds-container` + `sgds-grid` with responsive `sgds-col-*` classes (4 / 8 / 8 / 12 / 12 / 12 columns). E-service forms use the centred 8-column form container (`patterns.md` §3).
3. **Pick components.** List the template's components and take each from `references/components.md`, with its props, variants and states.
4. **Apply tokens.** Wire up colour, type scale (desktop and mobile), spacing (`--sgds-padding-*`, `--sgds-gap-*`, `--sgds-margin-*`), radius and borders from `references/tokens.md`.
5. **Add every state.** Hover, active/focus, disabled, invalid/error, empty (for example "No results found") and loading/toast, as defined in `patterns.md` §6.
6. **Check accessibility.** Contrast, focus order, labels above fields, hint text before validation, error text beside the field.
7. **Self-review.** Search your diff for `#`, `rgb(`, `px` and `font-family`. Every hit must be a token or a documented exception.

## Quick reference

| Need | Use |
|---|---|
| Primary action | `<Button>` → `bg-primary` / hover `bg-primary-hover` / active `bg-primary-active` / disabled `bg-primary-disabled` |
| Link | `color: var(--sgds-link-color-default)`, hover `var(--sgds-link-color-emphasis)` |
| Brand colour as a Tailwind class | `bg-brand-jet-blue`, `text-brand-sky-blue`, `border-brand-crimson-red` (defined in `globals.css`) |
| Body / heading text | `var(--sgds-body-color-default)` / `var(--sgds-heading-color-default)` |
| Page / alternate background | `var(--sgds-bg-default)` / `var(--sgds-bg-alternate)` |
| Card | `<Card>` (`rounded-xl`, border, `p-6`) |
| Input / select radius | `rounded-md` (8px). Buttons `rounded` (4px). Dialogs `rounded-lg` (10px). |
| E-service section | padding `--sgds-padding-2xl` (48px), field gap `--sgds-gap-xl` (24px), section title H5 `--sgds-font-size-4` |

## Setup snippet (new project)

```scss
@import "@govtechsg/sgds-web-component/themes/day.css";
@import "./tokens/brand-colors";   // brand tokens from references/tokens.md, MUST follow day.css
@import "@govtechsg/sgds-web-component/css/reboot.css";
@import "@govtechsg/sgds-web-component/css/grid.css";
```

## Reference files

- [references/tokens.md](references/tokens.md): every colour, type size, weight, line height, spacing, grid, radius, border, shadow, focus and icon token, with exact values and the CSS variable or Tailwind name. Also covers setup and known source inconsistencies.
- [references/components.md](references/components.md): all components (UI primitives, e-service controls, content blocks, navbar/footer) with props, variants, states, do's and don'ts, and a copy-paste snippet for each.
- [references/patterns.md](references/patterns.md): page shells, the seven page templates, e-service form layouts, tables, navigation, and empty/error/feedback states.
