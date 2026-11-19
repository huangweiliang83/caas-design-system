# CAAS Design System: Tokens

Every value below is copied from the CAAS Design System Figma Make file. The sources are the applied stylesheets, the Foundation pages (Typography, Colour, Icons, Layout, Logo, Border) and the Installation Guidelines.

**Which stylesheets are applied.** `src/main.tsx` imports `styles/index.css`, which loads, in order: `fonts.css` (Inter from Google Fonts) → `tailwindcss` → `tw-animate-css` → `default_theme.css` → `globals.css`. Values in `globals.css` take precedence. The file also contains a `styles/theme.css`, but nothing imports it, so its rules do not apply.

There are two token layers, and both are valid:

| Layer | Where it lives | How you reference it |
|---|---|---|
| **SGDS / brand CSS variables** | `:root` in `_brand-colors.scss` / `globals.scss` (see "Setup" at the end) | `var(--sgds-…)`, `var(--brand-…)` |
| **App theme (Tailwind v4)** | `theme.css` → `@theme inline` | Tailwind classes such as `bg-primary`, `text-muted-foreground`, `rounded-lg` |

If a value has no token, it is not part of the system. Don't invent one.

---

## 1. Colour

### 1.1 Brand colours

| Name | Hex | RGB (as stated) | Print | Use |
|---|---|---|---|---|
| Jet Blue | `#193055` | 25, 47, 85 | Pantone® 540 C · 100C 80M 20Y 30K | Primary brand colour for headers and key elements |
| Sky Blue | `#0087BF` | 0, 135, 191 | Pantone® 2925 C/U · 85C 25M 0Y 0K | Secondary brand colour for accents and links |
| Crimson Red | `#A80061` | 168, 0, 96 | Pantone® 227 C/U · 0C 100M 10Y 20K | Use sparingly for emphasis. **No tints.** |

### 1.2 Primitive scales (`--brand-*`)

These come from the canonical token file (Installation Guidelines, Step 2). There is no Tailwind utility for them, so reference them with `var(--brand-…)`.

**Jet Blue.** Use for primary buttons, headers and navigation.

| Token | Hex | Contrast (from Foundation page) |
|---|---|---|
| `--brand-jet-blue-900` | `#061d33` | 16.84 on white, AAA |
| `--brand-jet-blue-800` | `#0a2847` | 14.26 on white, AAA |
| `--brand-jet-blue-700` | `#0c3763` | (see note A) |
| `--brand-jet-blue-600` | `#193055` | (see note A) |
| `--brand-jet-blue-500` | `#3d5f82` | 6.65 on white, AA |
| `--brand-jet-blue-400` | `#5c7996` | 4.54 on white, AA / 4.63 with dark text, AA |
| `--brand-jet-blue-300` | `#8fa3b7` | 8.09 with dark text, AAA |
| `--brand-jet-blue-200` | `#b4c1cf` | 11.47 with dark text, AAA |
| `--brand-jet-blue-100` | `#dae1e8` | 15.21 with dark text, AAA |
| `--brand-jet-blue-50` | `#e7ebef` | 17.53 with dark text, AAA |

**Sky Blue.** Use for links, focus states and info alerts.

| Token | Hex | Contrast |
|---|---|---|
| `--brand-sky-blue-900` | `#003d5c` | — |
| `--brand-sky-blue-800` | `#005a82` | — |
| `--brand-sky-blue-700` | `#0073a8` | — |
| `--brand-sky-blue-600` | `#0087bf` | Sky Blue base |
| `--brand-sky-blue-500` | `#0095d4` | 6.25 with dark text |
| `--brand-sky-blue-400` | `#33aadd` | 7.93 with dark text, AAA |
| `--brand-sky-blue-300` | `#54b8e2` | 9.33 with dark text, AAA |
| `--brand-sky-blue-200` | `#8aceeb` | 12.11 with dark text, AAA |
| `--brand-sky-blue-100` | `#b0def2` | 14.58 with dark text, AAA |
| `--brand-sky-blue-50` | `#e6f4fb` | 18.70 with dark text, AAA |

**Neutral Grey.** Use for text, borders and backgrounds. These are also the primary surface colours for the dark site.

| Token | Hex | Contrast |
|---|---|---|
| `--brand-grey-900` | `#343940` | 11.63 on white, AAA |
| `--brand-grey-800` | `#444a54` | 8.92 on white, AAA |
| `--brand-grey-700` | `#57606c` | 6.37 on white, AA |
| `--brand-grey-600` | `#707b8a` | 4.89 with dark text, AA |
| `--brand-grey-500` | `#7b8798` | 5.76 with dark text, AA |
| `--brand-grey-400` | `#959fad` | 7.84 with dark text, AAA |
| `--brand-grey-300` | `#a7afba` | 9.48 with dark text, AAA |
| `--brand-grey-200` | `#c2c8d0` | 12.47 with dark text, AAA |
| `--brand-grey-100` | `#d6dadf` | 14.95 with dark text, AAA |
| `--brand-grey-50` | `#f2f3f5` | 18.91 with dark text, AAA |

There is no Crimson Red scale. Only the base `#A80061` exists. The Button spec also names a darker hover, `#8a0051`.

### 1.3 Semantic SGDS tokens (use these in components)

| Token | Maps to | Resolved | Purpose |
|---|---|---|---|
| `--sgds-primary-surface-default` | `--brand-jet-blue-600` | `#193055` | Button background |
| `--sgds-primary-surface-emphasis` | `--brand-jet-blue-900` | `#061d33` | Hover / emphasis |
| `--sgds-primary-surface-muted` | `--brand-jet-blue-50` | `#e7ebef` | Subtle background |
| `--sgds-primary-color-default` | `--brand-jet-blue-600` | `#193055` | Primary text/icon |
| `--sgds-primary-border-color-default` | `--brand-jet-blue-600` | `#193055` | Primary border |
| `--sgds-accent-surface-default` | `--brand-sky-blue-600` | `#0087bf` | Secondary button, accents |
| `--sgds-accent-color-default` | `--brand-sky-blue-600` | `#0087bf` | Accent text |
| `--sgds-link-color-default` | `--brand-sky-blue-600` | `#0087bf` | Link |
| `--sgds-link-color-emphasis` | `--brand-sky-blue-800` | `#005a82` | Link hover |
| `--sgds-body-color-default` | `--brand-grey-900` | `#343940` | Body text |
| `--sgds-body-color-subtle` | `--brand-grey-600` | `#707b8a` | Secondary text |
| `--sgds-heading-color-default` | `--brand-grey-800` | `#444a54` | Headings |
| `--sgds-color-muted` | `--brand-grey-400` | `#959fad` | Placeholder |
| `--sgds-bg-default` | — | `#ffffff` | Page background |
| `--sgds-bg-alternate` | `--brand-grey-50` | `#f2f3f5` | Alternate sections |
| `--sgds-surface-default` | (SGDS default) | — | Cards |
| `--sgds-border-color-default` | `--brand-grey-500` | `#7b8798` | Standard border |
| `--sgds-border-color-muted` | `--brand-grey-200` | `#c2c8d0` | Subtle border |
| `--sgds-outline-focus` | — | `2px solid var(--brand-sky-blue-400)` | Focus ring |
| `--sgds-outline-offset-focus` | (SGDS default) | — | Focus ring offset |
| `--sgds-color-inverse` | (SGDS default) | — | Text on primary surfaces |
| `--sgds-success-color-default` / `--sgds-danger-color-default` / `--sgds-warning-color-default` | (SGDS default) | — | Status. **Don't override.** |

**Dark Site / Emergency overrides.** Apply these only on the dark site:

```css
--sgds-primary-surface-default: var(--brand-grey-900);   /* #343940 */
--sgds-primary-surface-emphasis: var(--brand-grey-800);  /* #444a54 */
--sgds-primary-surface-muted: var(--brand-grey-100);     /* #d6dadf */
--sgds-primary-color-default: var(--brand-grey-50);      /* #f2f3f5 */
--sgds-primary-border-color-default: var(--brand-grey-700); /* #57606c */
```

### 1.4 Brand base colours and utilities (`globals.css`)

| CSS variable | Value | Tailwind colour | Utility classes (defined with `!important`) |
|---|---|---|---|
| `--brand-jet-blue` | `#193055` | `--color-brand-jet-blue` | `bg-brand-jet-blue`, `text-brand-jet-blue`, `border-brand-jet-blue` |
| `--brand-sky-blue` | `#0087BF` | `--color-brand-sky-blue` | `bg-brand-sky-blue`, `text-brand-sky-blue`, `border-brand-sky-blue` |
| `--brand-crimson-red` | `#A80061` | `--color-brand-crimson-red` | `bg-brand-crimson-red`, `text-brand-crimson-red`, `border-brand-crimson-red` |
| `--brand-jet-blue-rgb` | `25, 47, 85` | — | for `rgba(var(--brand-jet-blue-rgb), a)` |
| `--brand-sky-blue-rgb` | `0, 135, 191` | — | |
| `--brand-crimson-red-rgb` | `168, 0, 96` | — | |

Use these when you need a brand colour in a Tailwind class. For anything with a semantic meaning (buttons, links, text), prefer the semantic tokens in §1.3 or §1.6.

### 1.5 App theme colours (`default_theme.css` + `globals.css`, light / `:root`)

| CSS variable | Value | Tailwind class stem |
|---|---|---|
| `--background` | `#ffffff` | `bg-background` |
| `--foreground` | `oklch(0.145 0 0)` | `text-foreground` |
| `--card` | `#ffffff` | `bg-card` |
| `--card-foreground` | `oklch(0.145 0 0)` | `text-card-foreground` |
| `--popover` | `oklch(1 0 0)` | `bg-popover` |
| `--popover-foreground` | `oklch(0.145 0 0)` | `text-popover-foreground` |
| `--primary` | `#193055` | `bg-primary` / `text-primary` |
| `--primary-foreground` | `oklch(1 0 0)` | `text-primary-foreground` |
| `--primary-hover` | `#0c3763` | `bg-primary-hover` |
| `--primary-active` | `#061d33` | `bg-primary-active` |
| `--primary-disabled` | `#8fa3b7` | `bg-primary-disabled` |
| `--secondary` | `oklch(0.95 0.0058 264.53)` | `bg-secondary` |
| `--secondary-foreground` | `#030213` | `text-secondary-foreground` |
| `--muted` | `#ececf0` | `bg-muted` |
| `--muted-foreground` | `#717182` | `text-muted-foreground` |
| `--accent` | `#e9ebef` | `bg-accent` |
| `--accent-foreground` | `#030213` | `text-accent-foreground` |
| `--destructive` | `#d4183d` | `bg-destructive` / `text-destructive` |
| `--destructive-foreground` | `#ffffff` | `text-destructive-foreground` |
| `--border` | `rgba(0, 0, 0, 0.1)` | `border-border` (applied to `*` by default) |
| `--input` | `transparent` | `border-input` |
| `--input-background` | `#f3f3f5` | `bg-input-background` |
| `--switch-background` | `#cbced4` | `bg-switch-background` |
| `--ring` | `oklch(0.708 0 0)` | `ring-ring` (default outline is `outline-ring/50`) |
| `--chart-1` … `--chart-5` | `oklch(0.646 0.222 41.116)`, `oklch(0.6 0.118 184.704)`, `oklch(0.398 0.07 227.392)`, `oklch(0.828 0.189 84.429)`, `oklch(0.769 0.188 70.08)` | `bg-chart-1` … `bg-chart-5` |
| `--sidebar` | `oklch(0.985 0 0)` | `bg-sidebar` |
| `--sidebar-foreground` | `oklch(0.145 0 0)` | `text-sidebar-foreground` |
| `--sidebar-primary` | `#193055` | `bg-sidebar-primary` |
| `--sidebar-primary-foreground` | `oklch(0.985 0 0)` | `text-sidebar-primary-foreground` |
| `--sidebar-accent` | `oklch(0.97 0 0)` | `bg-sidebar-accent` |
| `--sidebar-accent-foreground` | `oklch(0.205 0 0)` | `text-sidebar-accent-foreground` |
| `--sidebar-border` | `oklch(0.922 0 0)` | `border-sidebar-border` |
| `--sidebar-ring` | `oklch(0.708 0 0)` | `ring-sidebar-ring` |

**Dark theme (`.dark`)**

| Variable | Value | Variable | Value |
|---|---|---|---|
| `--background` | `oklch(0.145 0 0)` | `--foreground` | `oklch(0.985 0 0)` |
| `--card` | `oklch(0.145 0 0)` | `--card-foreground` | `oklch(0.985 0 0)` |
| `--popover` | `oklch(0.145 0 0)` | `--popover-foreground` | `oklch(0.985 0 0)` |
| `--primary` | `oklch(0.985 0 0)` | `--primary-foreground` | `oklch(0.205 0 0)` |
| `--secondary` | `oklch(0.269 0 0)` | `--secondary-foreground` | `oklch(0.985 0 0)` |
| `--muted` | `oklch(0.269 0 0)` | `--muted-foreground` | `oklch(0.708 0 0)` |
| `--accent` | `oklch(0.269 0 0)` | `--accent-foreground` | `oklch(0.985 0 0)` |
| `--destructive` | `oklch(0.396 0.141 25.723)` | `--destructive-foreground` | `oklch(0.637 0.237 25.331)` |
| `--border` | `oklch(0.269 0 0)` | `--input` | `oklch(0.269 0 0)` |
| `--ring` | `oklch(0.439 0 0)` | `--chart-1…5` | `oklch(0.488 0.243 264.376)`, `oklch(0.696 0.17 162.48)`, `oklch(0.769 0.188 70.08)`, `oklch(0.627 0.265 303.9)`, `oklch(0.645 0.246 16.439)` |
| `--sidebar` | `oklch(0.205 0 0)` | `--sidebar-primary` | `oklch(0.488 0.243 264.376)` |
| `--sidebar-accent` | `oklch(0.269 0 0)` | `--sidebar-border` | `oklch(0.269 0 0)` |
| `--sidebar-ring` | `oklch(0.439 0 0)` | `--sidebar-*-foreground` | `oklch(0.985 0 0)` |

### 1.6 Button state colours

| Button | State | Hex | Token |
|---|---|---|---|
| Primary | Default | `#193055` | `var(--primary)` / `--sgds-primary-surface-default` · `bg-primary` |
| Primary | Hover | `#0c3763` | `var(--primary-hover)` · `hover:bg-primary-hover` |
| Primary | Active / Focus | `#061d33` | `var(--primary-active)` / `--brand-jet-blue-900` · `active:bg-primary-active` |
| Primary | Disabled | `#8fa3b7` | `var(--primary-disabled)` / `--brand-jet-blue-300` · `disabled:bg-primary-disabled` |
| Secondary (accent) | Default | `#0087BF` | `var(--sgds-accent-surface-default)` |
| Secondary | Hover | `#0095d4` | `var(--brand-sky-blue-500)` |
| Secondary | Active / Focus | `#006d99` | (no token, Button spec only) |
| Danger | Default | `#A80061` | Crimson Red |
| Danger | Hover | `#8a0051` | (no token, Button spec only) |

Text on every filled button is `#ffffff`.

### 1.7 Other colour values used by components in the Make file

| Value | Where | Token equivalent |
|---|---|---|
| `#d1d5db` | Global style for native `input[type=checkbox]` / `input[type=radio]` (`appearance: none`, 1px border, the check and dot are drawn in the same grey). Set in `globals.css` | none |
| `#1F1F1F` | Footer background | none |
| `neutral-50` / `neutral-800` | Infobar background / Infobar buttons | none |
| `neutral-700/80`, `neutral-800` | ImageGallery arrows and caption bar / active thumbnail border | none |
| `#edf9ff` | Overview "Who is this for?" panel background | none |
| `#e7f1ff` + `border-blue-200` | "Important Note" box | none |
| `#FAFAFA` / `rgba(250,250,250,1)` | Tabs list background, template shell card | none |
| `#F2F2F2` | Code example background (Layout page) | none |
| `#7B8798` → `#344054` on hover | Close button icon | `--brand-grey-500` at rest |
| `#D6DADF` | Close button / date picker / file list borders | `--brand-grey-100` |
| `#C2C8D0` | Stepper inactive border, quantity toggle disabled background | `--brand-grey-200` |
| `#707B8A` | Stepper inactive text, file meta | `--brand-grey-600` |
| `#343940` | Radio label text | `--brand-grey-900` |
| `#EBF1F8` | File upload drag-over background | none |
| `rgba(0,0,0,0.25)` | Radio unchecked border | none |
| `rgb(26,86,229)` | Table of contents link | none |

---

## 2. Typography

- **Font family:** Inter (Google Fonts). Token: `--sgds-font-family-brand: "Inter", system-ui, sans-serif;`. Fallback is `system-ui, sans-serif`. Override it only if you are using a custom brand font.
- **Loading:** `@import url("https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap");` (`fonts.css`)
- **Applied body stack (`globals.css`):** `font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, Ubuntu, Cantarell, "Noto Sans", sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";`
- **Base size:** 16px (1rem). `html { font-size: var(--font-size); }` with `--font-size: 16px`
- **Line height:** 1.5
- **App weights:** `--font-weight-normal: 400`, `--font-weight-medium: 500`

### 2.1 Type scale (desktop / mobile)

| Role | SGDS token | Desktop | Desktop class | Mobile | Mobile class | Usage |
|---|---|---|---|---|---|---|
| Display | `--sgds-font-size-9` | 56px / 3.5rem | `text-[3.5rem]` | 40px / 2.5rem | `text-[2.5rem]` | Extra large hero headings, marketing banners. Once per page. |
| H1 | `--sgds-font-size-8` | 48px / 3rem | `text-[3rem]` | 32px / 2rem | `text-[2rem]` | Main page titles. Once per page. |
| H2 | `--sgds-font-size-7` | 40px / 2.5rem | `text-[2.5rem]` | 28px / 1.75rem | `text-[1.75rem]` | Major section headers (2–4 per page) |
| H3 | `--sgds-font-size-6` | 32px / 2rem | `text-[2rem]` | 24px / 1.5rem | `text-[1.5rem]` | Subsection titles, component headers |
| H4 | `--sgds-font-size-5` | 28px / 1.75rem | `text-[1.75rem]` | 20px / 1.25rem | `text-[1.25rem]` | Card titles, sidebar headings, form sections, dialog titles |
| H5 | `--sgds-font-size-4` | 24px / 1.5rem | `text-[1.5rem]` | 18px / 1.125rem | `text-[1.125rem]` | Small card titles, list headers, **E-service section titles** |
| Large | `--sgds-font-size-3` | 20px / 1.25rem | `text-[1.25rem]` | 16px / 1rem | `text-[1rem]` | Lead paragraphs, introductions |
| Body | `--sgds-font-size-2` | 16px / 1rem | `text-[1rem]` | 14px / 0.875rem | `text-[0.875rem]` | All body text. **E-service forms:** labels, description, placeholder and input text. |
| Small | `--sgds-font-size-1` | 14px / 0.875rem | `text-[0.875rem]` | 13px / 0.8125rem | `text-[0.8125rem]` | Metadata, helper text. **E-service forms:** helper and validation text. |
| Caption | `--sgds-font-size-0` | 12px / 0.75rem | `text-[0.75rem]` | 12px / 0.75rem | `text-[0.75rem]` | Captions, timestamps, footnotes. Use sparingly. |

The minimum body text on mobile is `--sgds-font-size-1` (14px).

### 2.2 Weights, line heights, letter spacing (SGDS)

```css
--sgds-font-weight-light: 300;
--sgds-font-weight-regular: 400;   /* body */
--sgds-font-weight-medium: 500;
--sgds-font-weight-semibold: 600;  /* emphasis */
--sgds-font-weight-bold: 700;      /* emphasis */

--sgds-line-height-16: 1rem;    --sgds-line-height-20: 1.25rem;
--sgds-line-height-24: 1.5rem;  /* body: 1.5 ratio on 16px */
--sgds-line-height-32: 2rem;    --sgds-line-height-36: 2.25rem;
--sgds-line-height-40: 2.5rem;  --sgds-line-height-48: 3rem;
--sgds-line-height-56: 3.5rem;  --sgds-line-height-64: 4rem;
/* Headings use tighter ratios, e.g. 48px on 40px, 56px on 48px */

--sgds-letter-spacing-tight: -0.025em;
--sgds-letter-spacing-normal: 0;
--sgds-letter-spacing-wide: 0.025em;
```

### 2.3 Base element defaults

The applied stylesheets set **no** default size or weight for headings. `globals.css` only sets `html { font-size: var(--font-size) }` (16px) and the body font stack. The h1–h4 / label / button / input rules in `theme.css` are not imported. So always give headings an explicit size and weight from the type scale above. The Make screens do this, for example `text-[56px] font-bold` (page H1), `text-[32px] font-bold` (card section titles) and `text-[36px] font-semibold` (content block titles).

---

## 3. Spacing

### 3.1 SGDS spacing tokens

| Token | Value | Note |
|---|---|---|
| `--sgds-padding-xs` | 8px | |
| `--sgds-padding-sm` | 12px | |
| `--sgds-padding-md` | 16px | default |
| `--sgds-padding-lg` | 20px | |
| `--sgds-padding-xl` | 24px | |
| `--sgds-padding-2xl` | 48px | E-service: section padding |
| `--sgds-gap-xs` | 8px | |
| `--sgds-gap-sm` | 12px | |
| `--sgds-gap-md` | 16px | default. Grid gap on mobile. |
| `--sgds-gap-xl` | 24px | E-service: horizontal gap between fields in a two-column row and vertical gap between form rows. Grid gap at md+. |
| `--sgds-gap-2-xl` | 32px | Grid gap at xl+ |
| `--sgds-margin-3-xs` | 8px | |
| `--sgds-margin-2-xs` | 16px | |
| `--sgds-margin-md` | 32px | |

### 3.2 Button padding (SGDS / Bootstrap)

| Size | Class | Horizontal | Vertical | SCSS |
|---|---|---|---|---|
| Small | `btn-sm` | 0.5rem (8px) | 0.25rem (4px) | `$btn-padding-x-sm` / `$btn-padding-y-sm` |
| Medium | `btn` | 0.75rem (12px) | 0.375rem (6px) | `$btn-padding-x` / `$btn-padding-y` |
| Large | `btn-lg` | 1rem (16px) | 0.5rem (8px) | `$btn-padding-x-lg` / `$btn-padding-y-lg` |

Keep horizontal padding at 2× vertical, and use rem units, never pixel overrides.

### 3.3 Spacing conventions used across the Make screens (Tailwind)

- Page container: `max-w-7xl mx-auto px-3 md:px-6 pt-8 md:pt-16 pb-14 md:pb-20` (Overview). Detail pages use `max-w-[1400px] … pt-6 md:pt-12`.
- Section cards: `Card className="p-6"`, stacked with `space-y-8`.
- Hero block bottom margin: `mb-8 md:mb-12` (detail pages) or `mb-12 md:mb-16` (Overview).
- Button spacing: at least 8–12px around buttons. Checkbox and radio options: at least 16px apart.

---

## 4. Layout grid (SGDS CSS Grid)

| Breakpoint | Min width | Columns | Gap | Container max-width |
|---|---|---|---|---|
| Default (mobile) | 0px | 4 | 16px | 100% − 40px (20px margin each side) |
| `sm` | 512px | 8 | 16px | 100% − 48px (24px each side) |
| `md` | 768px | 8 | 24px | 100% − 56px (28px each side) |
| `lg` | 1024px | 12 | 24px | 888px |
| `xl` | 1280px | 12 | 32px | 1168px |
| `2-xl` | 1440px | 12 | 32px | 1312px |

Classes: `sgds-container`, `sgds-grid`, `sgds-col-{1-4}`, `sgds-col-sm-{1-8}`, `sgds-col-md-{1-8}`, `sgds-col-lg-{1-12}`, `sgds-col-xl-{1-12}`, `sgds-col-2-xl-{1-12}`, `sgds-col-*-center-{n}`, `sgds-col-*-none`. You get them with `@import "@govtechsg/sgds-web-component/css/grid.css"`. See `patterns.md` for page and form layouts.

---

## 5. Radius

Base token: `--radius: 0.625rem` (10px). Every step derives from it, so changing this one value rescales every component.

| Tailwind class | Value | Definition | Used by |
|---|---|---|---|
| `rounded` | 4px (fixed) | Tailwind default | Buttons |
| `rounded-sm` | 6px | `calc(var(--radius) - 4px)` | Select and menu items |
| `rounded-md` | 8px | `calc(var(--radius) - 2px)` | Inputs, selects, textareas, badges, tooltips, dropdowns |
| `rounded-lg` | 10px (base) | `var(--radius)` | Modals (dialogs), alerts |
| `rounded-xl` | 14px | `calc(var(--radius) + 4px)` | Cards, tabs |
| `rounded-[4px]` | 4px | — | Checkbox |

SGDS equivalents: `--sgds-border-radius-sm` 4px (buttons), `--sgds-border-radius-md` 8px (inputs, selects, badges), `--sgds-border-radius-full` 999px (pills).

Guidance: None/minimal (0px, 2px) suits sharp, grid-based layouts. Use 0px for data tables and 2px for dividers or separators. Standard (10px) balances smoothness and sharpness.

## 6. Border width

| Name | Width | Token | Use |
|---|---|---|---|
| Default | 1px | `--sgds-border-width-1` | Component boundaries, content dividers |
| Medium | 2px | — | Emphasis on elements that rely on borders for hover interaction, e.g. input field |

Default border colour is `border-border` (`rgba(0,0,0,0.1)`), applied globally through `* { @apply border-border outline-ring/50 }`.

## 7. Shadows

The Make file defines **no custom shadow tokens**. Only Tailwind's default shadow utilities appear:

| Class | Where |
|---|---|
| `shadow-xs` | Checkbox |
| `shadow-sm` | Sticky header, active tab trigger |
| `shadow-md` | Select / dropdown content, mobile nav panel, hover on template component cards |
| `shadow-lg` | Card hover (Overview cards), date picker popup |

## 8. Focus ring

- SGDS: `outline: var(--sgds-outline-focus)` (2px solid Sky Blue-400 `#33aadd`) plus `outline-offset: var(--sgds-outline-offset-focus)`
- App UI primitives: `focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:ring-[3px]`
- Invalid: `aria-invalid:border-destructive aria-invalid:ring-destructive/20`

## 9. Icons

| Size | px | Class | Use |
|---|---|---|---|
| Small | 16px | `h-4 w-4` | Inline text, buttons |
| Medium (default) | 20px | `h-5 w-5` | Default UI elements |
| Large | 24px | `h-6 w-6` | Headers, emphasis |
| XLarge | 32px | `h-8 w-8` | Feature icons |

The library is Lucide (`lucide-react`): outline style, 24px grid, 2px stroke.

---

## 10. Setup (from Installation Guidelines)

```bash
npm install @govtechsg/sgds-web-component
npm install sass --save-dev        # Node.js >= 18
```

`globals.scss`. Keep this import order:

```scss
@import "@govtechsg/sgds-web-component/themes/day.css"; // 1. SGDS base theme
@import "./tokens/brand-colors";                         // 2. Brand overrides (MUST follow day.css)
@import "@govtechsg/sgds-web-component/css/reboot.css";  // 3. Base styles
@import "@govtechsg/sgds-web-component/css/grid.css";
.sr-only { position:absolute; width:1px; height:1px; padding:0; margin:-1px; overflow:hidden; clip:rect(0,0,0,0); white-space:nowrap; border:0; }
.focus-ring { &:focus-visible { outline: var(--sgds-outline-focus); outline-offset: var(--sgds-outline-offset-focus); } }
```

Dark mode: `import '@govtechsg/sgds-web-component/css/day.css'; import '@govtechsg/sgds-web-component/css/dark.css';`, then toggle with `document.documentElement.setAttribute('data-theme', 'dark' | 'day')`.

---

## 11. Known inconsistencies in the Make file

These come from the source. Resolve them as described here, and don't guess.

- **A. Jet Blue 700/600.** The Foundation colour page lists `jet-blue-700 #0e3358` and `jet-blue-600 #193d6a`. The Installation Guidelines token file lists `#0c3763` and `#193055`. **Use the Installation values.** They match `theme.css` (`--primary: #193055`, `--primary-hover: #0c3763`) and every button spec.
- **B. Button spec labels.** The spec calls `#0c3763` "Jet Blue-500" and `#8fa3b7` "Jet Blue-200", and its CSS uses `var(--brand-jet-blue-500)` / `var(--brand-jet-blue-200)` for hover/disabled. Those variables actually resolve to `#3d5f82` / `#b4c1cf`. **Use `var(--primary-hover)` and `var(--primary-disabled)`** (or `--brand-jet-blue-700` / `--brand-jet-blue-300`) to get the intended hex.
- **C.** The stated RGB for Jet Blue (25, 47, 85) and Crimson Red (168, 0, 96) differs by 1 from their hex values. The hex value is authoritative.
- **D.** Several demo components hardcode off-palette colours: `#1e3a5f` / `#2a4d7a` (ButtonComponent, SearchableTable header), `#1f3a5f` (Infopic button), Tailwind `blue-600` (Hero CTA, FormDemo submit, Blockquote/Callout accents, Paragraph/ContactInformation links), `neutral-800` (Infobar buttons), `#1F1F1F` (Footer) and grey utilities. Don't copy these into new work. Use the tokens above.
- **E. Legacy guide.** `src/app/CAAS_COLOR_CUSTOMIZATION_GUIDE.md` (SGDS `_variables.scss` overrides) gives different Jet Blue, Sky Blue and Crimson scales, for example `$purple-100: #e6eaf0` and `$blue-600: #006d99`. It even suggests a Crimson tint scale, which contradicts "no Crimson tints". The page doesn't display it. **Don't use it.** The `--brand-*` scales in §1.2 are canonical.
- **F. Older exports.** An earlier export of this file (zip dated 18 Nov 2026) still has Poppins as the brand font, FontAwesome Classic Light as the icon library, an 8px "standard" radius, `--primary: #030213` and no hover/active/disabled tokens. The current file replaced all of these with the values documented here. The 19 Nov 2026 re-export matches the current file. If you see those older values in existing code, they are out of date.
