# CAAS Design System: Patterns

These patterns come from the screens and templates in the CAAS Design System Figma Make file: the Overview, Foundation and Installation pages, the Layout page, and the seven page templates. Token values are in `tokens.md` and component APIs are in `components.md`.

The design system covers **digital services and digital applications only**.

---

## 1. Page layouts

### 1.1 Standard page shell (from the Make app)

```
┌ Header (sticky, white, border-b, shadow-sm) ────────────────┐
│ [Logo h-12]      Nav (ghost buttons + dropdowns)   [Search ⌘K] │
└──────────────────────────────────────────────────────────────┘
  Hero: centred H1 + muted intro (max-w-3xl) + optional CTA
  Content: Cards (p-6) stacked with space-y-8
```

```tsx
<div className="min-h-screen bg-white">
  <header className="sticky top-0 z-50 bg-white border-b border-border shadow-sm">
    <div className="max-w-7xl mx-auto px-3 md:px-6 py-4">…</div>
  </header>
  <main className="max-w-[1400px] mx-auto px-3 md:px-6 pt-6 md:pt-12 pb-14 md:pb-20">
    <div className="text-center mb-8 md:mb-12">
      <h1 className="mb-4 text-[56px] font-bold">FOUNDATION</h1>
      <p className="text-muted-foreground max-w-3xl mx-auto mb-6">Core design principles…</p>
    </div>
    <div className="space-y-8">
      <Card className="p-6">…</Card>
    </div>
  </main>
</div>
```

- Landing/overview pages use `max-w-7xl … pt-8 md:pt-16`, with the hero block at `mb-12 md:mb-16`.
- Section headings inside cards: `text-[32px] font-bold` (H3 level). Sub-sections: `text-[24px] font-semibold` or `text-[20px] font-semibold`.
- A tinted intro panel looks like `bg-[#edf9ff] rounded-[16px] p-[24px]` in `max-w-4xl mx-auto`.
- Card grid on the Overview: `grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-5 gap-6`. Square image tiles; on hover, a `bg-black/40 backdrop-blur-md` overlay shows the title (`text-[28px] font-bold`) and description.

### 1.2 SGDS grid page (production apps)

```html
<div class="sgds-container">
  <div class="sgds-grid">
    <div class="sgds-col-4 sgds-col-lg-6">Column 1</div>
    <div class="sgds-col-4 sgds-col-lg-6">Column 2</div>
  </div>
</div>
```

| Range | Columns |
|---|---|
| Mobile 0–511px | 4 |
| Small 512–767px | 8 |
| Medium 768–1023px | 8 |
| Large 1024–1279px | 12 |
| X-Large 1280–1439px | 12 |
| 2X-Large 1440px+ | 12 |

- Centring: `sgds-col-center-2`, `sgds-col-sm-center-4`, `sgds-col-lg-center-6`, `sgds-col-lg-center-8`, `sgds-col-xl-center-10`.
- Hiding at a breakpoint: `sgds-col-none` (mobile), `sgds-col-sm-none`, `sgds-col-md-none`, `sgds-col-lg-none`, `sgds-col-xl-none`, `sgds-col-2-xl-none`.
- Full-bleed container: `<div class="sgds-container" style="max-width: 100%;">`.
- Custom gap: `style="gap: var(--sgds-gap-sm);"` gives 12px.

### 1.3 Documentation / template detail layout (sidebar + content)

This is used on every Template page and on the Dark Site template.

```tsx
<Card className="p-3 md:p-6 bg-[rgba(250,250,250,1)]">
  {/* Mobile: section switcher as a Select (md:hidden) */}
  <div className="flex flex-col sm:flex-row sm:items-center justify-between gap-3">
    <h2 className="text-4xl font-bold">Dark Site</h2>
    <Button variant="ghost" size="sm" className="border border-border"><Download className="h-4 w-4 mr-2" />Download template</Button>
  </div>
  <div className="border-t border-gray-300" />
  <div className="grid grid-cols-1 md:grid-cols-[200px_1fr] gap-6">
    <nav className="hidden md:block space-y-1">{/* side nav buttons */}</nav>
    <Card className="p-6 md:p-8 bg-white rounded-xl">
      <div className="grid grid-cols-1 md:grid-cols-12 gap-8">
        <div className="md:col-span-7">{/* title (text-[20px] font-bold, border-b) + preview image */}</div>
        <div className="md:col-span-5 space-y-4">{/* text-sm paragraphs; sub-headings text-base font-bold */}</div>
      </div>
    </Card>
  </div>
</Card>
```

Side nav items look like `w-full text-left px-3 py-2 rounded-md text-sm flex items-center gap-2`. Active: `bg-accent text-accent-foreground`. Inactive: `hover:bg-accent/50 text-muted-foreground`.

---

## 2. Page templates

Each template documents a purpose, its use cases and the components it uses.

### Homepage
The primary gateway to the CAAS website, acting as both a communication hub and a navigation hub. Sections run in this order:

1. **Hero banner** with CAAS core values and mission, plus a CTA to About Us
2. **News**: latest updates and announcements
3. **Highlights**: key content areas (replaces the old carousel, for better visibility and accessibility)
4. **Promoted Content Banner**: up to **two** key initiatives
5. **Quick Links**: up to **six** links (legislation, regulatory information, e-services)

Components: Hero, Infobar, Infopic, InfoCards, InfoCols, KeyStatistics, Collection Card, Blockquote, CollectionBlock.

### Index
An auto-generated structured directory for a folder's contents (pages, subfolders, collections). It is mandatory and created automatically for every new folder. It helps navigation and SEO (internal links, topic-hub relevance). There are **no editable components**. Editors can only adjust the order and thumbnails of child pages.

### Article
For blog posts, news, case studies and long-form content, with readable typography. Components: Accordion, Blockquote, Callout, Image, ImageGallery, Paragraph.

### Content
The default layout for detailed information pages. It supports headings with titles and subtitles, embedded CTA buttons, in-page navigation (TableOfContents), images and video. Components: Accordion, Blockquote, Callout, ContentPic, Image, ImageGallery, InfoCards, InfoCols, Infobar, KeyStatistics, Paragraph.

### Collection
For browsing a large set of related items (repositories, libraries, archives). Built-in **search**, **category filters** and **year-of-publication filters**. Each item is a Collection Card.

### Dark Site (emergency)
A pre-built site that stays inactive until a critical aviation incident (aircraft accident, major airport disruption, significant safety incident). At that point it replaces the standard homepage.

- **Design approach:** deliberate restraint. A **gradient hero banner in muted tones**, no imagery, and no decorative elements.
- **Information hierarchy:** critical updates and hotline contacts come first. Social channels and media releases come later.
- **Sections:** Latest Update → Key Contacts (emergency hotlines, prominent) → Frequently Asked Questions → Media Release.
- **Colours:** apply the Dark Site primary overrides (`--sgds-primary-*` mapped to the grey scale, see `tokens.md` §1.3).
- **Components:** Hero, InfoCards, InfoCols, Infobar, CollectionBlock.

### E-services
For CAAS e-services where applicants submit information across one or more steps, for example CAPELS, ESOMS and ELMERS. See §3.

---

## 3. Forms (E-service)

### 3.1 When to use this pattern
Use it when the user must complete an application in one or more stages, provide several types of information, upload supporting documents, review before submitting, or understand which step they are on. For a single-step application, drop the Stepper.

### 3.2 Page structure

```
[Stepper: all steps visible]
┌ Card section (padding --sgds-padding-2xl = 48px) ┐
│ H5 section title (24px desktop / 18px mobile)     │
│ Instructional text / Callout (important reminders)│
│ Field rows (gap --sgds-gap-xl = 24px)             │
└───────────────────────────────────────────────────┘
[Previous]                                   [Next / Submit]
```

### 3.3 Form container

```html
<div class="sgds-container">
  <div class="sgds-grid">
    <form class="sgds-col-4 sgds-col-sm-8 sgds-col-md-8 sgds-col-lg-center-8 sgds-col-xl-center-8 sgds-col-2-xl-center-8">
      <!-- E-service form content goes here -->
    </form>
  </div>
</div>
```

The form spans the full grid on mobile, sm and md, and is centred at 8 of 12 columns from lg upwards.

### 3.4 Two-column row (fields stack until lg)

```html
<div class="sgds-grid">
  <div class="sgds-col-4 sgds-col-sm-8 sgds-col-md-8 sgds-col-lg-6 sgds-col-xl-6 sgds-col-2-xl-6"><!-- field 1 --></div>
  <div class="sgds-col-4 sgds-col-sm-8 sgds-col-md-8 sgds-col-lg-6 sgds-col-xl-6 sgds-col-2-xl-6"><!-- field 2 --></div>
</div>
```

### 3.5 Full-width row

```html
<div class="sgds-grid">
  <div class="sgds-col-4 sgds-col-sm-8 sgds-col-md-8 sgds-col-lg-12 sgds-col-xl-12 sgds-col-2-xl-12"><!-- field --></div>
</div>
```

### 3.6 Field anatomy and typography

| Part | Size | Token |
|---|---|---|
| Section title | H5: 24px desktop / 18px mobile | `--sgds-font-size-4` |
| Label, description, placeholder, input text | Body: 16px / 14px mobile | `--sgds-font-size-2` |
| Helper / hint text, validation text | Small: 14px / 13px mobile | `--sgds-font-size-1` |
| Gap between fields (horizontal) and rows (vertical) | 24px | `--sgds-gap-xl` |
| Section padding | 48px | `--sgds-padding-2xl` |

- Put the label **above** the field and keep it visible.
- Required fields get a red `*`. Write `(optional)` when most fields are required.
- Show hint text below the label/field **before** validation runs.
- Show validation text directly under the field it refers to.
- Order: Stepper, then sections, then a footer with `Previous` (outline) on the left and `Next` / `Submit` (primary) on the right, above a top border (`pt-6 border-t`).

### 3.7 Wizard behaviour
- Show every step up front with descriptive labels.
- Disable `Next` / `Submit` until the current step is valid. Disable `Previous` on step 1.
- Only completed steps are clickable.
- The last step is **Review & Submit**: a read-only summary on a muted surface (`bg-gray-50 p-4 rounded-lg`), an optional message textarea, required terms/privacy checkboxes and a verification (captcha) block.
- Long processes should auto-save.

---

## 4. Tables

### 4.1 Data table
- Use the Table component with a **caption** describing what the table holds.
- Headers should be clear and concise. Right-align numeric columns. Keep date and number formats consistent within a column.
- Use zebra striping (`bg-white` / `bg-gray-50` alternating) or row hover.
- Use radius 0 for a rigid, structured look.
- On mobile, scroll horizontally (`overflow-x-auto` wrapper).

### 4.2 Searchable table (10+ rows)

```
[H2 title text-lg font-semibold]
[🔍 Enter a search term……………………]        ← full width, resets to page 1 on change
┌ Header row (primary surface, white text) ┐
│ rows striped, numbers right-aligned       │
└───────────────────────────────────────────┘
                       [Previous] 1 2 3 4 5 … [Next]
```

- 5 rows per page. The page buttons are `px-3 py-1 border text-sm`, and the current page is filled.
- Previous and Next are disabled at the ends (`disabled:opacity-50 disabled:cursor-not-allowed`).

### 4.3 Documentation tables (Layout / Foundation pages)
`table.w-full.text-sm.border`, header row `bg-gray-50 border-b`, cells `py-2 px-4`, tokens shown as `<code className="bg-gray-100 px-2 py-1 rounded text-xs">`.

---

## 5. Navigation

### 5.1 Primary header
- Sticky, white, `border-b border-border shadow-sm`, `z-50`.
- **Logo** on the left. It is clickable and goes home. Minimum height is 40px on desktop and 32px on mobile (the Make header uses `h-12`), with a clickable area of at least 48×48px.
- **Nav** in the centre: ghost buttons. Dropdowns open on hover/click (`DropdownMenuContent align="start" className="w-56"`). The active section gets `bg-accent`.
- **Search** on the right: a read-only Input with a Search icon (`pl-10`, `bg-neutral-50`) and a `⌘ K` hint. Cmd/Ctrl + K opens the global search dialog.
- Keep it to 5–7 top-level items.

### 5.2 Compact / mobile navigation
- When the header is narrower than 900px, hide the centre nav and show a hamburger `Button variant="ghost" size="icon"` (`Menu` / `X`, `h-5 w-5`) with `aria-label="Toggle navigation menu"`.
- The panel sits below the header: `border-t border-border bg-white shadow-md`. Items are `px-3 py-3 text-sm font-medium rounded-md hover:bg-accent`. Sections with children collapse (ChevronDown/ChevronUp). Sub-items are indented: `ml-4 border-l border-border pl-3`, `text-muted-foreground hover:text-foreground`.
- Selecting an item closes the panel.

### 5.3 Secondary navigation
- **Breadcrumb**: below the main nav, for 3 or more levels. The current page is not a link.
- **TableOfContents** ("On this page"): for long content pages, 5–10 items, sticky if helpful.
- **Side nav** (template detail pages): a 200px column on md and up. On mobile it becomes a full-width `Select` switcher.
- **Tabs**: for sibling views inside a page (Foundation sections, Desktop/Mobile toggles).

### 5.4 Site Navbar component (public pages)
For public CAAS sites, use the `Navbar` component rather than the docs-app header. It has a 128px logo, top-level items with an underline on hover, and a search icon button. Clicking an item opens a full-width mega-menu panel (`shadow-md`). The panel shows the item title and description, a close button, and a 3-column grid of links (Jet Blue, Sky Blue on hover, each with an optional description). Only one panel is open at a time. See `components.md` → Navbar.

### 5.5 Footer
Use the dark `Footer` component (`bg-[#1F1F1F]`). It has a title, two link columns and a "Reach us" column with labelled social icons, contact and feedback links. The bottom bar (`border-t border-gray-700`) holds the copyright, last-updated date, legal links (Report Vulnerability, Privacy Statement, Terms of Use) and the "Made with / Built by" attribution. Columns stack on mobile (`grid-cols-1 md:grid-cols-3`).

---

## 6. Empty, error and feedback states

The Make file defines the following. Don't invent other states.

| Situation | Pattern from the Make file |
|---|---|
| **Search returns nothing** (SearchableTable) | Keep the table and header. Show one row with `colSpan` across every column containing **"No results found"** (`text-center text-muted-foreground`). Hide pagination when there is only 1 page. |
| **Section not built yet** | Centred muted heading: `<h2 className="text-muted-foreground text-[48px]">{Name} page coming soon...</h2>` inside the standard `main` shell, `py-20`. |
| **Template section has no components** (Index) | Body text: "No editable component is available for index template. For more information, please visit this link", followed by a link. |
| **Invalid field** | `aria-invalid="true"` on the control gives `border-destructive` plus `ring-destructive/20`. SGDS components take `invalid`, `hasFeedback` and `invalidFeedback`. Put the message directly below the field in Small text. In the date picker, an invalid format triggers validation (v3.20.0). |
| **Required field empty** | Red `*` on the label. Block progress by disabling `Next` / `Submit` until valid. On submit, show a validation message for radio groups with nothing selected. |
| **Disabled controls** | Buttons `bg-primary-disabled` (#8fa3b7). Inputs and checkboxes `opacity-50 cursor-not-allowed`. Quantity toggle at its limit `#C2C8D0` / `#A7AFBA`. |
| **File constraints** | State accepted types and size up front ("Accepted: PDF, JPG, PNG. Max 5 MB per file."), not only after a failure. |
| **Important reminders** | Callout near the related content. In e-services, at the start of a step or next to specific fields. |
| **Critical errors** | Use an alert dialog, **not** a Callout. |
| **Toast notifications** | `sonner` `<Toaster position="top-center" richColors />`. Use `toast.success('Copied to clipboard!')` and `toast.error('Failed to copy')`. |
| **Notes / warnings in docs** | Info: `bg-blue-50 border-l-4 border-blue-500 p-4 rounded` or `bg-blue-50 border border-blue-200 p-4 rounded-lg`. Warning: `bg-amber-50 border border-amber-200` / `bg-yellow-50 border-yellow-200`. |

---

## 7. Accessibility baseline (applies to every pattern)
- Follow WCAG 2.1 AA: 4.5:1 for body text and 3:1 for large text (18px+, or 14px+ bold).
- Every interactive element must be keyboard reachable and show a visible focus state (`--sgds-outline-focus`).
- Icon-only controls need an `aria-label`. Decorative icons get `aria-hidden`.
- Escape closes overlays, and focus returns to the trigger.
- Touch targets are at least 44px.
