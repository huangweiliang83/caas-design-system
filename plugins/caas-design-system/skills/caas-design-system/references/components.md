# CAAS Design System: Components

All entries come from the Components page of the CAAS Design System Figma Make file (React 18 + Tailwind v4 + Radix UI, built on SGDS). Colour and size names refer to `tokens.md`.

**How components are delivered**

| Layer | Package / path | Import |
|---|---|---|
| SGDS React | `npm install @govtechsg/sgds-react` + styles `npm install @govtechsg/sgds` → `import '@govtechsg/sgds/css/sgds.css'` | `import { Button } from '@govtechsg/sgds-react/Button';` (cherry-pick; preferred) or `import { ComponentName } from '@govtechsg/sgds-react';` |
| SGDS Web Components | `@govtechsg/sgds-web-component` | `<sgds-datepicker>`, `<sgds-file-upload>` … |
| CAAS UI primitives (token-bound) | `components/ui/*` in the Make file | `import { Button } from "./ui/button"` |

Some SGDS components, such as Form, use Bootstrap icons but don't ship them. Install `bootstrap-icons` or use the CDN.

**Rule:** reuse these components. Don't rebuild them from raw markup. If a component's demo in the Make file hardcodes a colour, use the matching token instead (`tokens.md` §11 D).

## Index

- **UI primitives:** [Button](#button) · [Input](#input) · [Textarea](#textarea) · [Select / Dropdown](#dropdown) · [Checkbox](#checkbox) · [Radio](#radio) · [Card](#card) · [Tabs](#tabs) · [Accordion](#accordion) · [Breadcrumb](#breadcrumb) · [Close button](#close-button)
- **E-service / form:** [Form](#form) · [Stepper](#stepper) · [Date picker](#date-picker) · [File upload](#file-upload) · [Quantity toggle](#quantity-toggle)
- **Content blocks:** [Hero](#hero) · [Infobar](#infobar) · [Infopic](#infopic) · [InfoCards](#infocards) · [InfoCols](#infocols) · [KeyStatistics](#keystatistics) · [Contentpic](#contentpic) · [CollectionBlock](#collectionblock) · [Collection Card](#collection-card) · [Blog Card](#blog-card) · [Blockquote](#blockquote) · [Callout](#callout) · [ContactInformation](#contactinformation) · [Image](#image) · [ImageGallery](#imagegallery) · [Paragraph](#paragraph) · [OrderedList](#orderedlist) · [UnorderedList](#unorderedlist) · [Divider](#divider) · [Table](#table) · [SearchableTable](#searchabletable) · [TableOfContents](#tableofcontents)
- **Site chrome:** [Navbar](#navbar) · [Footer](#footer)
- **Tools:** [Contrast checker](#contrast-checker)

---

## UI primitives

### Button
A button that triggers an action. The Make file documents three sizes: small, medium and large.

- **Props (`ui/button`):** `variant`: `default | destructive | outline | secondary | ghost | link` · `size`: `default | sm | lg | icon` · `asChild?: boolean` · plus native button props.
- **Sizes:** `default` `h-9 px-4 py-2` · `sm` `h-8 px-3 gap-1.5` · `lg` `h-10 px-6` · `icon` `size-9`. All use `rounded` (4px), `text-sm font-medium` and `gap-2`, with icons at `size-4`.
- **States (default variant):** rest `bg-primary` #193055 → hover `bg-primary-hover` #0c3763 → active/focus `bg-primary-active` #061d33 → disabled `bg-primary-disabled` #8fa3b7 (opacity stays 100). Focus ring: `ring-ring/50 ring-[3px]`. Invalid: `aria-invalid:border-destructive`.
- **Other variants:** `destructive` `bg-destructive` (#d4183d). `outline` is bordered `bg-background` with `hover:bg-accent`. `secondary` is `bg-secondary`. `ghost` shows `hover:bg-accent` only (nav items; the active nav item adds `bg-accent`). `link` is `text-primary` with hover underline.
- **Brand button colours (SGDS spec):** Primary (Jet Blue), Secondary (Sky Blue `#0087BF` → hover `#0095d4` → active `#006d99`), Danger (Crimson `#A80061` → hover `#8a0051`). Use Danger sparingly, for destructive actions.
- **Do:** one primary button per section. Use 2–4 word, verb-led labels ("Submit", "Download"). Use `sm` for secondary actions, the default size for most buttons, and `lg` for hero/primary CTAs. Right-align buttons in forms. Leave 8–12px around buttons. Use the SGDS size classes `btn-sm` / `btn` / `btn-lg`.
- **Don't:** use a button for inline text links. Put several primary buttons on one page. Override padding with px values, or create sizes outside the scale.

```tsx
import { Button } from "./ui/button";

<Button>Submit</Button>
<Button variant="outline" size="sm">Cancel</Button>
<Button size="lg">Apply now</Button>
<Button disabled>Submit</Button>
```

### Input
A single-line text field for short, direct answers: names, identifiers, search terms, references.

- **Props:** native `input` props (`type`, `placeholder`, `disabled`, `aria-invalid` …).
- **Style:** `h-9 rounded-md border border-input bg-input-background px-3 py-1 text-base md:text-sm`, placeholder `text-muted-foreground`.
- **States:** focus `border-ring ring-ring/50 ring-[3px]` · invalid `aria-invalid:border-destructive ring-destructive/20` · disabled `opacity-50 cursor-not-allowed`.
- **Do:** put a visible label **above** the field. Use hint text for format rules. Put validation messages next to the field. Match `type` to the data (email, password, number). Use `type="text"` for NRIC, phone and postal codes so leading zeros survive. Make touch targets at least 44px high.
- **Don't:** use placeholder text instead of a label. Use an Input for multi-line answers (use Textarea).

```tsx
import { Input } from "./ui/input";

<label htmlFor="first-name">First name</label>
<Input id="first-name" placeholder="Enter your first name" />
```

### Textarea
A multi-line field for longer freeform answers: feedback, descriptions, notes.

- **Style:** `min-h-16 rounded-md border border-input bg-input-background px-3 py-2 text-base md:text-sm field-sizing-content`. Default is `resize-none`. Focus and invalid states match Input.
- **Do:** show a character count when there is a limit (the preview shows `0/1000`). Let the field grow with its content (`resize=auto` in SGDS). Always show a visible label. Use hint text, not placeholder text, for limits.
- **Don't:** use it for short structured values. Lock the height on open-ended fields. Leave the prompt vague.

```tsx
import { Textarea } from "./ui/textarea";

<label htmlFor="notes">Internal notes</label>
<Textarea id="notes" maxLength={1000} />
<p className="text-sm text-muted-foreground">0/1000</p>
```

### Dropdown
A select menu for choosing one option from a list (`ui/select`, built on Radix Select).

- **Parts:** `Select`, `SelectTrigger` (`size?: "sm" | "default"` → `h-8` / `h-9`), `SelectValue` (placeholder), `SelectContent` (`position="popper"`), `SelectGroup`, `SelectLabel`, `SelectItem`, `SelectSeparator`.
- **Style:** trigger `rounded-md border border-gray-300 bg-white px-3 py-2 text-sm` with a muted chevron. Content `rounded-md border shadow-md`. Items `rounded-sm` with `focus:bg-accent` and a check icon when selected.
- **Label conventions:** `*` (red asterisk) marks a required field. Write `(optional)` when most fields are required. Use no indicator when every field is optional.
- **Do:** use it for 4 or more options. Sort logically. Group long lists. Give a placeholder.
- **Don't:** use it for 2–3 options (use Radio). Use it for multi-select (use Checkbox). Use it to trigger critical actions.

```tsx
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "./ui/select";

<label className="block">Select an option <span className="text-destructive">*</span></label>
<Select>
  <SelectTrigger className="w-full"><SelectValue placeholder="Choose an option" /></SelectTrigger>
  <SelectContent>
    <SelectItem value="option1">Option 1</SelectItem>
    <SelectItem value="option2">Option 2</SelectItem>
  </SelectContent>
</Select>
```

### Checkbox
For selecting one or more options, or for confirming a statement.

- **Props (`ui/checkbox`, Radix):** `checked`, `onCheckedChange`, `disabled`, `id`. Wrapper `CheckboxComponent` props: `label`, `checked`, `onCheckedChange`, `id`.
- **Style:** `size-4 rounded-[4px] border bg-input-background shadow-xs`. When checked: `bg-primary border-primary text-primary-foreground` with a check icon. Focus ring and invalid state match Input.
- **Do:** put the label on the right. Make both box and label clickable. Stack vertically, at least 16px apart. Show validation states. Offer "Select all" for long lists.
- **Don't:** use it when only one option can be chosen (use Radio). Use it for instant on/off (use a switch). Use it to navigate.

```tsx
import { Checkbox } from "./ui/checkbox";

<div className="flex items-center space-x-2">
  <Checkbox id="terms" checked={checked} onCheckedChange={setChecked} />
  <label htmlFor="terms" className="text-sm leading-none cursor-pointer">Accept terms and conditions</label>
</div>
```

### Radio
Choose exactly one option from a small set while seeing every option.

- **Visual spec (preview):** 18×18 circle. Selected: fill `#193055` with a white 4px-radius dot. Unselected: white fill with a `rgba(0,0,0,0.25)` border. Label `text-sm` `#343940` (`--brand-grey-900`), with `gap-3` between control and label.
- **Do:** use for 2–5 options. Put the group under a clear question. Stack vertically (horizontal only for 2–3 short options). Space options at least 16px apart. Pre-select a safe default where it makes sense. Make the label clickable. Validate when nothing is selected.
- **Don't:** use it for multi-select. Use it for more than 5–6 options (use Dropdown). Use it for immediate actions. Leave an option without a label.

```tsx
<fieldset className="flex flex-col gap-4">
  <legend className="text-base font-medium">Have you applied before?</legend>
  <label className="flex items-center gap-3 text-sm text-[var(--brand-grey-900)]">
    <input type="radio" name="prior" value="no" defaultChecked className="accent-primary size-[18px]" />
    No, this is my first application
  </label>
  <label className="flex items-center gap-3 text-sm text-[var(--brand-grey-900)]">
    <input type="radio" name="prior" value="yes" className="accent-primary size-[18px]" />
    Yes, for a different project
  </label>
</fieldset>
```

### Card
A container surface (`ui/card`).

- **Parts:** `Card`, `CardHeader`, `CardTitle` (renders `h4`), `CardDescription` (`text-muted-foreground`), `CardAction`, `CardContent` (`px-6`), `CardFooter`.
- **Style:** `bg-card text-card-foreground flex flex-col gap-6 rounded-xl border` (14px radius). Pages add `p-6` (or `p-6 md:p-8`). The hover state is `hover:shadow-lg transition-shadow`, or `hover:shadow-md` for list cards.

```tsx
import { Card } from "./ui/card";

<Card className="p-6">
  <h3 className="mb-4 text-[32px] font-bold">Section title</h3>
  <p className="text-muted-foreground">Supporting text.</p>
</Card>
```

### Tabs
Switch between sibling views (`ui/tabs`, Radix).

- **Style:** list `bg-muted rounded-xl p-[3px] h-9`, trigger `rounded-xl text-sm font-medium`, active `bg-card`. Foundation pages use a full-width list on `bg-[#FAFAFA]` with `data-[state=active]:shadow-sm data-[state=active]:font-semibold`.

```tsx
import { Tabs, TabsList, TabsTrigger, TabsContent } from "./ui/tabs";

<Tabs defaultValue="typography">
  <TabsList>
    <TabsTrigger value="typography">Typography</TabsTrigger>
    <TabsTrigger value="colors">Color Scheme</TabsTrigger>
  </TabsList>
  <TabsContent value="typography">…</TabsContent>
  <TabsContent value="colors">…</TabsContent>
</Tabs>
```

### Accordion
Stacked headings that each reveal a section of content.

- **Props:** Radix `type="single" | "multiple"`, `collapsible`, `defaultValue`. Items are `border-b border-border`. The trigger is `py-4` with a **Plus** icon that becomes **Minus** when open. Content is `text-sm pb-4`.
- **Do:** use it for FAQs and long sectioned content. Keep titles short and scannable. Consider opening the first item when it holds key context.
- **Don't:** hide critical information. Nest accordions. Use one for short content that already fits on screen.

```tsx
import { Accordion, AccordionContent, AccordionItem, AccordionTrigger } from "./ui/accordion";

<Accordion type="multiple" className="w-full max-w-2xl mx-auto">
  <AccordionItem value="item-1">
    <AccordionTrigger>Title for accordion item1</AccordionTrigger>
    <AccordionContent><p className="text-sm text-muted-foreground">Content…</p></AccordionContent>
  </AccordionItem>
</Accordion>
```

### Breadcrumb
Shows where the user is in the site and links back to parent pages.

- **Parts:** `Breadcrumb`, `BreadcrumbList`, `BreadcrumbItem`, `BreadcrumbLink`, `BreadcrumbPage` (current page, not a link), `BreadcrumbSeparator`.
- **Do:** place it below the main nav. Make every item except the current page a link. Underline on hover. Use chevron or slash separators. Allow 5–7 levels at most. Add ARIA labels.
- **Don't:** use it on single-level sites or landing pages, when there are fewer than 3 levels, or on mobile when space is tight.

```tsx
<Breadcrumb>
  <BreadcrumbList>
    <BreadcrumbItem><BreadcrumbLink href="/">Home</BreadcrumbLink></BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem><BreadcrumbLink href="/components">Components</BreadcrumbLink></BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem><BreadcrumbPage>Breadcrumb</BreadcrumbPage></BreadcrumbItem>
  </BreadcrumbList>
</Breadcrumb>
```

### Close button
Dismisses table rows or surfaces such as modals, drawers, alerts and toasts.

- **Spec:** 48×48 box, white background, `1px #D6DADF` border, `rounded-[6px]`. 16px X icon with 1.5 stroke: `#7B8798` at rest. Hover: `bg-gray-100` with the icon at `#344054`.
- **Do:** always set an `aria-label` ("Close dialog", "Remove row"). Put it top-right on overlays and at the end of a row in tables. Hit target at least 44×44. Escape should also close the surface. Return focus to the trigger afterwards.
- **Don't:** use it as a form Cancel, as a Back button, or where it overlaps content. Don't use it when dismissed content can't be recovered.

```tsx
import { X } from "lucide-react";

<button aria-label="Close dialog"
  className="flex size-12 items-center justify-center rounded-[6px] border border-[var(--brand-grey-100)] bg-white text-[var(--brand-grey-500)] hover:bg-gray-100">
  <X className="h-4 w-4" strokeWidth={1.5} />
</button>
```

---

## E-service / form components

### Form
A multi-field form with validation and layout options, based on SGDS. The Make demo is a 3-step wizard ("Personal Information" → "Preferences" → "Review & Submit") with a stepper, text/email inputs, selects, radio groups, checkbox lists, a textarea, terms checkboxes and Previous/Next buttons.

- **States:** Next and Submit stay disabled until the step is valid (`canProceed`). Previous is disabled on step 1. Required fields show a red `*`.
- **Do:** keep it short. Label every field clearly. Group related fields. Choose input types that suit mobile. Mark required vs optional. Validate inline. Make it fully keyboard accessible. Use placeholders sparingly.
- **Don't:** build a form where a button or link would do, make it very long, or collect non-essential data.
- **Layout:** see `patterns.md` → E-service form.

```tsx
<form className="space-y-6">
  <div className="space-y-2">
    <label htmlFor="name" className="block text-base font-medium">Full Name <span className="text-destructive">*</span></label>
    <Input id="name" required placeholder="Enter your full name" />
  </div>
  <div className="flex justify-between pt-6 border-t">
    <Button type="button" variant="outline">Previous</Button>
    <Button type="submit">Next</Button>
  </div>
</form>
```

### Stepper
Shows where the user is in a multi-step process.

- **States:** `completed` is a Jet Blue fill with a white number. `active` is white with a `3px` Jet Blue border and Jet Blue number. `inactive` is white with a `2px #C2C8D0` border (`--brand-grey-200`) and `#707B8A` text (`--brand-grey-600`). The circle is `w-9 h-9` and the connector is `2px` high, Jet Blue up to the current step and grey after it. Labels are `text-base font-medium`, `#111827` when completed or active.
- **Do:** show all steps up front. Use descriptive labels ("Personal details", not "Step 2"). Make only completed steps clickable. Validate a step before moving on. Auto-save long processes.
- **Don't:** use it for short tasks, non-linear flows (use tabs or side nav) or steps that can be done in any order.

```tsx
const steps = [
  { id: 1, title: "Personal details", state: "completed" },
  { id: 2, title: "Review & confirm", state: "active" },
  { id: 3, title: "Submission",       state: "inactive" },
];
// circle style per state:
// completed → { backgroundColor: "var(--primary)", color: "white" }
// active    → { backgroundColor: "white", border: "3px solid var(--primary)", color: "var(--primary)" }
// inactive  → { backgroundColor: "white", border: "2px solid var(--brand-grey-200)", color: "var(--brand-grey-600)" }
```

### Date picker
Select a date from a calendar popup or by typing it. Supports a single date or a range. SGDS tag: `sgds-datepicker`.

| Prop | Type | Description |
|---|---|---|
| `value` | string | Pre-selected date (ISO). Replaces the deprecated `initialValue`. |
| `minDate` / `maxDate` | string | Earliest / latest selectable date (ISO) |
| `mode` | `'single' \| 'range'` | Single date or range |
| `hintText` | string | Helper text below the input |
| `required` / `disabled` / `invalid` | boolean | Required / disabled / error styling |
| `noFlip` | boolean | Turns off auto-flipping of the calendar direction |
| `noValidate` | boolean | Turns off built-in validation (v3.20.0+) |

- **Keyboard:** Tab / Shift+Tab move focus. ↓/↑ open or navigate the calendar. Enter selects. Esc closes.
- **Visual spec (demo):** `dd/mm/yyyy` placeholder. Input `px-4 py-3 rounded-md` with a `1px #D6DADF` border and a calendar button separated by a left border. Popup is `w-[320px] rounded-lg shadow-lg p-4`. Selected day fill and today's border are `#0C3763`.
- **Do:** set `minDate`/`maxDate`. Use `hintText` for the format. Default to single mode. Return focus to the input when the calendar closes.
- **Don't:** use it when users can type a known date faster, for year/month-only fields, or for time-only fields.

```html
<sgds-datepicker mode="single" hintText="DD/MM/YYYY" minDate="2026-01-01" required></sgds-datepicker>
```

### File upload
Attach documents or images, show the selected files (name and size), and state file requirements up front. SGDS tag: `sgds-file-upload`.

| Prop | Type | Description |
|---|---|---|
| `variant` | `'default' \| 'dragdrop'` | Button trigger or drag-and-drop zone |
| `accept` | string | Restrict file types, e.g. `'.pdf,.jpg'` |
| `multiple` | boolean | Allow more than one file |
| `required` / `disabled` / `invalid` | boolean | |
| `hasFeedback` | boolean | Inline validation feedback |
| `invalidFeedback` | string | Message shown when validation fails |
| `ariaLabel` | string | Accessible label (v3.21.0+) |

- **Keyboard:** Tab / Shift+Tab move focus. Enter / Space activate the button (open the picker or remove a file).
- **Visual spec (demo):** label `text-base font-medium`. Outline button with `#193055` border and text, `padding: 10px 16px`, Upload icon. Hint `14px #57606C`. File rows: `1px #D6DADF` border, `#F2F3F5` background, name plus size (`#707B8A`), remove X. Drag-drop zone: `1.5px dashed #C2C8D0` on `#F2F3F5`, which becomes `#193055` on `#EBF1F8` while dragging.
- **Do:** state accepted types and size limits before upload ("Accepted: PDF, JPG, PNG. Max 5 MB per file."). Use verb-led labels ("Upload report"). Set `accept`. Turn on `multiple` only when needed. Pair `required` with `hasFeedback` and `invalidFeedback`. Use `dragdrop` when users will upload several files.
- **Don't:** use it for short text values, or when the task doesn't really need a file.
- **Accessibility:** every upload and remove button must be reachable by keyboard. Use `ariaLabel` when the visible label isn't enough.

```html
<sgds-file-upload accept=".pdf,.jpg,.jpeg,.png" multiple required hasFeedback
  invalidFeedback="Please upload at least one file">Upload supporting documents</sgds-file-upload>
```

### Quantity toggle
A number field with − and + buttons.

- **Spec:** 48×48 buttons. Left button `rounded-l-lg`, right button `rounded-r-lg`. Enabled: `#193055` background, white icon, hover `#0C3763`. Disabled at min/max: `#C2C8D0` background, `#A7AFBA` icon, `not-allowed`. Centre value: `h-12 min-w-[60px] px-6`, `#D6DADF` top/bottom border, `text-base font-medium`. Demo range is 0–100.
- **Do:** set min and max. Disable − at min and + at max. Show the value clearly. Make the buttons large enough to tap on mobile.
- **Don't:** use it for large ranges, decimals or single-click actions.

```tsx
<div className="flex w-fit items-center">
  <Button size="icon" className="size-12 rounded-none rounded-l-lg" disabled={qty <= 0} onClick={() => setQty(qty - 1)} aria-label="Decrease"><Minus className="h-5 w-5" /></Button>
  <div className="flex h-12 min-w-[60px] items-center justify-center border-y border-[var(--brand-grey-100)] bg-white px-6 text-base font-medium">{qty}</div>
  <Button size="icon" className="size-12 rounded-none rounded-r-lg" disabled={qty >= 100} onClick={() => setQty(qty + 1)} aria-label="Increase"><Plus className="h-5 w-5" /></Button>
</div>
```

---

## Content blocks

These are the CAAS (Isomer-style) page blocks. Each lives in its own file in the Make project, for example `components/Hero.tsx`. Props are listed exactly as typed in the source. Most blocks sit in a `max-w-6xl mx-auto` container with a block title of `text-[36px]` (`font-semibold` or `font-medium`).

### Hero
A full-width banner: background image, dark gradient overlay (`bg-gradient-to-r from-black to-transparent`), title, subtitle, and up to 2 CTAs.

- **Props:** `backgroundImage?: string` (defaults to an airport photo) · `title: string` · `subtitle: string` · `primaryCTA?: { text; href?; onClick? }` · `secondaryCTA?: { text; href?; onClick? }`.
- **Spec:** `h-[500px]`. Content `max-w-7xl px-6`. Title `text-[48px] leading-tight text-white w-[60%]`. Subtitle `text-[18px] text-white/90`. CTAs are `Button size="lg" rounded-[4px] px-8`, and the secondary is a white 2px outline (`hover:bg-white/10`). The demo's primary CTA is `bg-blue-600`; use `bg-primary` instead.
- **Do:** one hero per page, at the top. Use a short message, sufficient contrast and optimised images. Limit it to 1–2 CTAs.
- **Don't:** stack several heroes, use one on reading-focused pages, or use one without a good image.

```tsx
<Hero
  title="Ministry of Trade and Industry"
  subtitle="A leading global city of enterprise and talent, a vibrant nation of innovation and opportunity"
  primaryCTA={{ text: "Main CTA", href: "/about" }}
  secondaryCTA={{ text: "Sub CTA", href: "/contact" }}
/>
```

### Infobar
A full-width call-to-action banner: title, description, primary and secondary buttons.

- **Props:** `title: string` · `description?` · `buttonLabel?` · `buttonUrl?` · `secondaryButtonLabel?` · `secondaryButtonUrl?`.
- **Spec:** section `w-full bg-neutral-50 py-12 px-6 md:px-12`, inner `max-w-6xl mx-auto`. Description `text-foreground mb-6`. Buttons `flex flex-col sm:flex-row gap-3`: primary is filled `bg-neutral-800 hover:bg-neutral-900 text-white`, secondary is outline `border-neutral-800 text-neutral-800 hover:bg-neutral-100`.
- **Do:** keep the title to 1–2 lines and the description to 1 sentence. Use 1–2 CTAs with contrasting styles. Place it after key content.
- **Don't:** use several on one page, or use one without a clear action.

```tsx
<Infobar title="This is a place where you can put nice content" description="About a sentence worth of description here"
  buttonLabel="Primary CTA" buttonUrl="/" secondaryButtonLabel="Secondary CTA" secondaryButtonUrl="/" />
```

### Infopic
A split screen with text on one side and a full-height image on the other.

- **Props:** `title: string` · `description: string` · `buttonLabel?` · `buttonUrl?` · `imageSrc: string` · `imageAlt: string` · `imagePosition?: "left" | "right"`.
- **Spec:** `grid grid-cols-1 md:grid-cols-2`. Text column `max-w-lg`, title `text-[36px] font-semibold mb-6`, description `text-muted-foreground mb-8`. Image `aspect-[4/3] md:aspect-auto object-cover`. The demo button is `bg-[#1f3a5f]`; use `bg-primary`.
- **Do:** use a relevant, high-quality image. Keep the title to 2–3 lines and the description to 2–3 sentences. Alternate `imagePosition` between consecutive Infopics. Check focal points across breakpoints.
- **Don't:** use it for long content or with weak imagery.

```tsx
<Infopic title="Explore your great neighbourhood with us" description="…" buttonLabel="Sign up" buttonUrl="/"
  imageSrc="https://images.unsplash.com/photo-1527436826045-8805c615a6df?w=1280" imageAlt="Two rhinos grazing in a field"
  imagePosition="right" />
```

### InfoCards
A grid of image cards, each with a title, description and optional link.

- **Props:** `title?` · `subtitle?` (max 150 chars) · `cards?: { title: string; description?: string; imageUrl: string; imageAlt: string; url?: string }[]` · `maxColumns?: "2" | "3" | "4"`.
- **Spec:** title `text-[36px] font-semibold mb-3`, subtitle `text-muted-foreground max-w-3xl`, header `mb-10`. Grid `grid-cols-1` up to `maxColumns`, `gap-16`. Image `aspect-[4/3] rounded-lg border border-gray-200 bg-gray-50 object-cover`. Card title `text-[16px] font-medium`, which turns `#193055` on hover, with an arrow that nudges right (`group-hover:translate-x-1`). Description `text-[14px] text-muted-foreground`.
- **Do:** 2–3 columns on desktop. Titles of 1–2 lines, descriptions under 200 characters. Consistent image ratios. Link each card.
- **Don't:** use it for a single item (use Hero or Infopic) or for data comparison.

```tsx
<InfoCards title="Section title ministry highlights" subtitle="Section subtitle, maximum 150 chars." maxColumns="3"
  cards={[{ title: "Grants", description: "…", imageUrl: "/img/grants.jpg", imageAlt: "…", url: "/grants" }]} />
```

### InfoCols
Columns with an icon, title, description and action link.

- **Props:** `title?` · `subtitle?` · `infoBoxes?: { icon: string; title: string; description: string; buttonLabel: string; buttonUrl: string }[]`.
- **Spec:** header like InfoCards. Grid `grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8`. Icon `h-8 w-8 text-foreground mb-4`. Description `text-sm text-muted-foreground mb-4`. Link `text-sm inline-flex gap-2 hover:underline` with a `h-4 w-4` arrow that moves right on hover.
- **Do:** 6–9 items. Use clear icons and action-oriented link labels. Keep description lengths consistent.
- **Don't:** use it for image-heavy content or a single item.

```tsx
<InfoCols title="MTI Highlights" subtitle="These are some of the things we are working on." infoBoxes={[
  { icon: "…", title: "Trade", description: "…", buttonLabel: "Learn more", buttonUrl: "/trade" },
]} />
```

### KeyStatistics
Shows key metrics in a scannable row.

- **Props:** `title?` · `statistics: { value: string; label: string }[]`.
- **Spec:** section `bg-white border-b border-neutral-200`, inner `max-w-6xl md:px-12 py-[48px]`. Title `text-[36px] font-medium mb-12`. Grid `grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-12`. Value `text-[2.25rem] lg:text-[3rem] leading-none font-weight 500 mb-4`. Label `text-sm text-muted-foreground leading-relaxed`.
- **Do:** 4–6 stats. Large numbers, labels of 1–2 lines, consistent formats, readable units (1.2M). Use credible sources and add a timestamp.
- **Don't:** use it for complex data (use a chart) or a single stat.

```tsx
<KeyStatistics title="Work life in numbers" statistics={[
  { value: "3", label: "Average all nighters pulled in a typical calendar month" },
  { value: "+12.2%", label: "Growth in tasks assigned Q4 2024 (YoY)" },
]} />
```

### Contentpic
Rich content beside an image. Use it for profiles and case studies.

- **Props:** `content: { type: "prose"; content: (Paragraph | UnorderedList)[] }`, where a paragraph holds `text` nodes (with `marks: [{ type: "bold" }]`) and `hardBreak` nodes, and an unordered list holds `listItem → paragraph` · `imageSrc: string` · `imageAlt: string`.
- **Spec:** `max-w-6xl`, `flex flex-col sm:flex-row gap-7`. Image `aspect-[5/6] rounded object-cover sm:h-[240px] sm:w-[200px]`, with text on the right. Links in the prose are underlined.
- **Do:** structure the text with bold lines and bullets. Make it responsive.
- **Don't:** use it for text-only content or where the image adds nothing.

```tsx
<ContentPic
  content={{ type: "prose", content: [{ type: "paragraph", content: [
    { type: "text", text: "Professor Rhino Bean", marks: [{ type: "bold" }] },
    { type: "hardBreak" },
    { type: "text", text: "Executive Bean", marks: [{ type: "bold" }] },
  ] }] }}
  imageSrc="https://images.unsplash.com/photo-1527436826045-8805c615a6df?w=1280"
  imageAlt="Two rhinos grazing in a field"
/>
```

### CollectionBlock
A grid of related items (articles, resources) with a "View all" button.

- **Props:** `title?` · `description?` · `items?: { title: string; date?; category?; imageUrl?; imageAlt?; url? }[]` · `buttonLabel?` · `buttonUrl?` · `displayThumbnail?: boolean` · `displayCategory?: boolean` · `collectionReferenceLink?: string`.
- **Spec:** `max-w-4xl my-8`. Title `text-[32px] font-medium mb-3`. Grid `grid-cols-1 md:grid-cols-3 gap-16`. Thumbnail `aspect-[3/2] rounded-lg border border-gray-200 bg-gray-50`. Date/category `text-xs text-muted-foreground`. Item hover: title turns `#193055` and a `2px #193055` outline appears (offset 2). The "View all" button is centred, outline, `rounded border-current px-4 py-2 hover:bg-gray-100`.
- **Do:** keep card sizes consistent. Show dates and categories. Link to "View all" or paginate.
- **Don't:** use it for a single item, for mixed content types, as navigation, or where complex filtering is needed.

```tsx
<CollectionBlock title="Corrections and Clarifications" description="…"
  items={[{ title: "Where does Government revenue come from? →", date: "1 January 2021", category: "Policy",
            imageUrl: "/img/1.jpg", imageAlt: "…", url: "/collection-1/item-3" }]}
  buttonLabel="View all corrections" buttonUrl="#" displayThumbnail displayCategory />
```

### Collection Card
A horizontal card: date column, then title, description and tag, then a thumbnail on the right.

- **Props:** `date`, `title`, `description`, `tag`, `image` (all required strings).
- **Spec:** `flex gap-6 p-6 bg-white border`. Date column `w-32 text-sm`. Title `font-semibold`, underlined on group hover. Image `w-[200px] h-[160px] object-cover rounded-[4px]`. The usage guide recommends 140×140px thumbnails.
- **Do:** titles of 2–3 lines and descriptions of 2–3 sentences. Make the whole card clickable. Stack it on mobile.
- **Don't:** use it without images or with multiple images.

```tsx
<CollectionCard date="2 December 2023" title="A journal on …" description="We've looked at …" tag="Research" image="/img/journal.jpg" />
```

### Blog Card
A vertical card: square image, date, title, 3-line description, category.

- **Props:** `image?`, `date?`, `title?`, `description?`, `category?`, `href?`.
- **Spec:** an `<a>` block that is `bg-white border`. Image `aspect-square object-cover`. Body `p-6`: date `text-sm mb-3`, title underlined on hover, description `text-sm line-clamp-3`.
- **Do:** titles of 50–70 characters. Show the date and category. Make the whole card clickable. Add a hover effect. Grid of 1 column on mobile, 2–3 on desktop. Include alt text.
- **Don't:** use it when there are no images or space is tight.

```tsx
<BlogCard image="/img/post.jpg" date="2 December 2023" title="…" description="…" category="Research" href="/blog/post" />
```

### Blockquote
A quotation with attribution and an optional image.

- **Props:** `quote: string` · `source: string` · `imageSrc?` · `imageAlt?`.
- **Spec:** `max-w-3xl mx-auto my-8`. Box `border-l-4 flex gap-[16px] py-[16px] px-[20px]` with a 32px-wide speech-mark icon (`w-[32px] h-[24px]`). Optional image `max-w-md rounded-md mb-4`. Quote `italic text-muted-foreground mb-4`. Source `text-sm text-foreground`. The demo uses `border-blue-600 bg-blue-50`, which is off-palette (`tokens.md` §11 D).
- **Do:** always attribute the quote. Keep it concise and accurate.
- **Don't:** use it for body text or disclaimers (use Callout), or overuse it.

```tsx
<Blockquote
  quote="When I was a rookie, I had trouble overcoming the low rope at first…"
  source="— ME2 Jenny Teng, Recipient of SAF Polytechnic Sponsorship, Army Medical Services"
/>
```

### Callout
Highlights an announcement, note or warning. There is no separate component file. It is markup.

- **Spec (preview):** `rounded-md border p-4 text-sm` on a tinted background (demo: `bg-blue-50 border-blue-200`), with an underlined inline link.
- **Do:** pick the colour by message type (info, warning, success). Keep it concise. Link to resources. Place it near the content it relates to. E-services use callouts for important reminders.
- **Don't:** use it for ordinary content, use several on one page, or use it for critical errors (use an alert dialog).

```tsx
<div className="rounded-md border border-[var(--brand-sky-blue-100)] bg-[var(--brand-sky-blue-50)] p-4">
  <p className="text-sm">As of December 1, 2024, the scheme is being reviewed. Please refer to{" "}
    <a href="#" className="text-[var(--sgds-link-color-default)] underline">New Idea Scheme Proposal</a>.</p>
</div>
```

The Sky Blue tints are a token mapping of the demo's `blue-50` / `blue-200`.

### ContactInformation
Contact details with icons: address, phone, email, hours.

- **Props:** `title: string` · `subtitle?` · `contactItems: { icon: "phone"|"fax"|"globe"|"email"|"clock"|"address"|"user"; label: string; content: string | string[]; link?: string }[]` · `additionalInfo?: { title: string; description: string }`.
- **Spec:** `bg-white p-8`. Title `text-[36px] font-medium mb-2`. Items grid `grid-cols-1 md:grid-cols-2 gap-8`, each with a `w-5 h-5` icon, a `font-semibold` label and the content. Links are truncated and underlined on hover. Additional info sits below `border-t pt-6` with a `text-[24px] font-medium` title.
- **Do:** group related details. Make phone, email and web links clickable (`tel:`, `mailto:`). Include opening hours. Format addresses consistently.
- **Don't:** use it for contact forms (use FormSG), for only 1–2 contact methods, or for maps.

```tsx
<ContactInformation title="Sentosa" subtitle="Embassy of the Republic of Singapore" contactItems={[
  { icon: "phone", label: "Tel", content: "+41-22-795-0101", link: "tel:+41227950101" },
  { icon: "clock", label: "Operating Hours", content: ["Mon - Fri", "8.30 am to 1.00 pm", "2.00 pm to 5.00 pm"] },
]} additionalInfo={{ title: "Other Information", description: "…" }} />
```

### Image
A single image with alt text.

- **Props:** `src: string` · `alt: string` · `site?` (Isomer config, unused in preview).
- **Spec:** `w-full max-w-md mx-auto`, image `w-full h-auto rounded-lg`.
- **Do:** write meaningful alt text. Optimise the file and keep its aspect ratio. Add a caption where it helps.
- **Don't:** use decorative images, images of text, or images in place of text.

```tsx
<Image src="https://images.unsplash.com/photo-1598567529331-9cf5f653e42a?w=800" alt="Aircraft flying in blue sky" />
```

### ImageGallery
A 16:9 carousel with arrows, a caption bar and a thumbnail strip.

- **Props:** `images?: { src: string; alt: string; caption: string }[]`.
- **Spec:** stage `relative aspect-[16/9] bg-neutral-100`. Prev and next buttons are `w-12 h-12 rounded-full bg-neutral-700/80 hover:bg-neutral-700 text-white` and only appear on hover. They have `aria-label="Previous image"` / `"Next image"`. Caption bar `absolute bottom-0 bg-neutral-700/90 text-white p-4 text-sm`. Thumbnails `w-32 h-24 rounded border-4`: active `border-neutral-800`, inactive `border-transparent hover:border-neutral-400`. The thumbnail row scrolls horizontally.
- **Do:** keep aspect ratios consistent, write descriptive captions and alt text, and order images logically.
- **Don't:** use it for a single image (use Image), for critical information, or for more than 20 images (paginate).

```tsx
<ImageGallery images={[
  { src: "/img/terminal.jpg", alt: "Changi Airport terminal interior", caption: "Terminal 3 departure hall" },
  { src: "/img/runway.jpg",   alt: "Aircraft on the runway at dusk",  caption: "Runway operations" },
]} />
```

### Paragraph
Body text with links, code, sub- and superscript.

- **Props:** `children?: ReactNode` (renders sample content when empty).
- **Spec:** wrapper `max-w-2xl`. Internal links are `underline`. External links are `underline inline-flex gap-0.5` with a `w-3 h-3` external-link icon. Inline code is `px-1.5 py-0.5 bg-gray-100 rounded text-sm font-mono`. The demo link colour is `blue-600` → use `--sgds-link-color-default`.
- **Do:** 2–4 sentences per paragraph. Lead with a topic sentence. Mark external links with an icon (internal links get none). Line height 1.5–1.6.
- **Don't:** use it for lists or headings, or write paragraphs longer than 4–5 sentences.

```tsx
<Paragraph>
  Read the <a href="/guide" className="underline text-[var(--sgds-link-color-default)]">guide</a>. Run{" "}
  <code className="px-1.5 py-0.5 bg-gray-100 rounded text-sm font-mono">npm install</code>. H<sub>2</sub>O, x<sup>2</sup>.
</Paragraph>
```

### OrderedList
A numbered list for sequential or ranked items.

- **Props:** `items?: string[]`.
- **Spec:** `max-w-2xl`, `list-decimal list-outside ml-6 space-y-2`, items `pl-2`.
- **Do:** 1–2 lines per item. Consistent capitalisation and punctuation. Nest at most 2–3 levels.
- **Don't:** use it when order doesn't matter, or for very long lists (use a table).

```tsx
<OrderedList items={["Check your eligibility", "Prepare your documents", "Submit the application"]} />
```

(The Components page preview passes a prose-JSON `content` prop, but the component only reads `items`.)

### UnorderedList
A bulleted list where order doesn't matter.

- **Props:** `items?: { text: string; hasLink?: boolean; linkText?: string; linkUrl?: string; hasBold?: boolean; boldText?: string; hasLineBreak?: boolean }[]`.
- **Spec:** `max-w-2xl`, `list-disc list-outside ml-6 space-y-2`, items `pl-2`. Links are underlined with the external-link icon.
- **Do:** keep items parallel in structure. 7–10 items at most. Add an intro sentence. Mark external links with an icon.
- **Don't:** use it for steps or ranked content.

```tsx
<UnorderedList items={[
  { text: "Apply online via " , hasLink: true, linkText: "CAAS e-services", linkUrl: "https://example.gov.sg" },
  { text: "Processing takes", hasBold: true, boldText: "5 working days" },
]} />
```

### Divider
A horizontal line between sections.

- **Spec:** `<hr className="w-full border-0 border-t border-gray-200 my-8" />`. No props.
- **Do:** keep the same spacing above and below (`my-8`). Keep it subtle. Pair it with headings.
- **Don't:** overuse it, stack several in a row, or use it where whitespace is enough.

```tsx
<p>This paragraph appears before the divider.</p>
<Divider />
<p>This will appear after the divider.</p>
```

### Table
A responsive data table with a caption.

- **Props:** `content: TableContent[]` (prose JSON `tableRow` → `tableHeader` / `tableCell` → `paragraph` / lists) · `attrs?: { caption?: string }` · `site?` (unused). Primitive alternative: `ui/table` (`Table`, `TableHeader`, `TableBody`, `TableRow`, `TableHead`, `TableCell`).
- **Spec:** wrapper `w-full overflow-x-auto`. Caption `text-sm text-muted-foreground mb-4`. Table `w-full border-collapse border border-gray-200 table-fixed`. Header cells `p-4 bg-[#F3F4F6] text-sm font-semibold text-left border-r border-gray-200`. Body cells `p-4 align-top text-sm bg-[#F9FAFB] border-r border-gray-200`. Rows `border-b border-gray-200`. Cells can hold lists.
- **Do:** clear headers. Right-align numbers. Use zebra striping or hover. Scroll horizontally on mobile. Give it a caption. Use 0px radius.
- **Don't:** use it for small list-like data, many columns on mobile, or hierarchical data.

```tsx
<Table attrs={{ caption: "A table of IIA countries (2024)" }} content={[
  { type: "tableRow", content: [
    { type: "tableHeader", content: [{ type: "paragraph", content: [{ type: "text", text: "Countries" }] }] },
    { type: "tableHeader", content: [{ type: "paragraph", content: [{ type: "text", text: "Date of Entry into Force" }] }] },
  ] },
  { type: "tableRow", content: [
    { type: "tableCell", content: [{ type: "paragraph", content: [{ type: "text", text: "Some text" }] }] },
    { type: "tableCell", content: [{ type: "paragraph", content: [{ type: "text", text: "Some text" }] }] },
  ] },
]} />
```

### SearchableTable
A table with live search and pagination.

- **Props:** `title?` (default "Sample DGS Table") · `data?: Row[]`.
- **Behaviour:** the search input (Search icon, placeholder "Enter a search term", `rounded-[4px]`) filters every column and resets to page 1. 5 rows per page. Header row filled (demo `#1e3a5f` → use `bg-primary`) with white text. Rows stripe white / `bg-gray-50`. Numbers are right-aligned. Pagination (Previous, up to 5 page numbers, …, Next) is right-aligned. **Empty state:** one row spanning all columns that reads "No results found" (`text-center text-muted-foreground`).
- **Do:** use it for datasets of 10 or more rows. Give feedback when nothing matches. Debounce on very large data.
- **Don't:** use it for fewer than 10 rows, or when complex filters are needed.

```tsx
<SearchableTable title="Licensed operators" data={rows} />
```

### TableOfContents
In-page links under the heading "On this page".

- **Props:** `items?: { id: string; text: string; href?: string }[]`.
- **Spec:** `py-4`, heading `mb-4`, list `space-y-2`. Each item has an `11×2px` dash marker and a link (`rgb(26,86,229)`, underlined on hover).
- **Do:** 5–10 items. Match the section headings exactly. Consider sticky positioning and highlighting the current section. Use smooth anchor scrolling.
- **Don't:** use it on short pages or alongside a sidebar nav.

```tsx
<TableOfContents items={[{ id: "1", text: "What does the scheme support?", href: "#section-1" }]} />
```

---

## Site chrome

### Navbar
A site navigation bar with logo, top-level items that open mega-menu panels, and search.

- **Props:** `logo?: ReactNode` · `brandName?: string` (default "isomer") · `menuItems?: { label: string; href?: string; description?: string; dropdown?: { label: string; href: string; description?: string; subtitle?: string; isExternal?: boolean }[] }[]` · `onSearch?: () => void`.
- **Spec:** bar `bg-white`, inner `max-w-screen-xl h-16 md:px-10 flex justify-between gap-x-4`. Logo `w-[128px]`. Top-level items are `text-sm pt-6 pb-5 border-b-2 border-transparent hover:border-gray-300` with a chevron that rotates 180° when open. Search is a `h-12 w-12` icon button (`aria-label="Search"`, `h-6 w-6 text-gray-600` icon, `hover:bg-gray-100`).
- **Dropdown panel** (click to toggle; only one open at a time): full width `bg-white shadow-md`, inner `max-w-screen-xl px-10 pt-12 pb-16 gap-8`. Header shows the item label (`text-[24px] font-medium`) and description (`text-sm text-gray-600`), plus a close button (`h-12 w-12`, `aria-label="Close menu"`). Links sit in `grid grid-cols-3 gap-x-16 gap-y-8`: each is `text-[16px] text-[#193055] hover:text-[#0087BF]` with an arrow that nudges right, then its description `text-sm text-gray-600`. External items show a subtitle.
- **Make app header** (the docs site's own header, see `patterns.md` §5.1): sticky, hover dropdowns, ⌘K search, hamburger below 900px.
- **Do:** 5–7 top-level items. Labels of 70 characters or fewer. Sticky. Mark the active item clearly. Keyboard-accessible dropdowns. Keep dropdowns shallow.
- **Don't:** use it for single-page apps or as a footer.

```tsx
<Navbar
  logo={<img src={caasLogo} alt="CAAS - Civil Aviation Authority of Singapore" className="h-12 w-auto" />}
  menuItems={[
    { label: "About us", description: "Who we are and what we do", dropdown: [
      { label: "Our team", href: "/about/team", description: "This is our leadership and senior management team" },
      { label: "Careers", href: "https://careers.example.gov.sg", subtitle: "Careers portal", isExternal: true },
    ] },
  ]}
  onSearch={() => setIsSearchOpen(true)}
/>
```

### Footer
A dark footer with two link columns, a "Reach us" column (social icons, contact, feedback) and a bottom bar.

- **Props:** `title?` · `leftLinks?` / `middleLinks?` / `bottomLinks?: { label: string; href: string; external?: boolean }[]` · `socialLinks?: { facebook?, twitter?, instagram?, linkedin?, telegram?, youtube?, github?, tiktok?, whatsapp?, flickr?, threads? }` · `contactLink?` · `feedbackLink?` · `copyrightText?` (default "© 2025 Government of Singapore") · `lastUpdated?`.
- **Spec:** `bg-[#1F1F1F] text-white`. Inner `max-w-7xl px-6 py-12`. Title `text-[24px] font-medium mb-6`. Grid `grid-cols-1 md:grid-cols-3 gap-12`. Links `text-[14px] text-white hover:underline`, and external links get a `w-4 h-4` icon. Social icons are `w-6 h-6`, `hover:opacity-80`, each with an `aria-label` ("Facebook", "LinkedIn" …), in rows of `gap-4`. The bottom bar sits above a `border-t border-gray-700`, `py-6`, `flex-col md:flex-row justify-between`: copyright and last updated date, legal links (`text-sm`, external ones with a `w-3 h-3` icon), and the "Made with / Built by" attribution.
- **Default bottom links:** Report Vulnerability (external), Privacy Statement, Terms of Use.
- **Do:** group links logically. Mark external links. Label social icons for screen readers. Keep the year and last-updated date current. Ensure contrast on dark backgrounds. Stack columns on mobile.
- **Don't:** use more than one footer, put one in modals, or add time-sensitive content.

```tsx
<Footer
  title="Civil Aviation Authority of Singapore"
  leftLinks={[{ label: "About us", href: "/about" }, { label: "Contact us", href: "/contact" }]}
  middleLinks={[{ label: "Careers", href: "/careers" }, { label: "Our Corp Site", href: "https://example.com", external: true }]}
  socialLinks={{ facebook: "https://facebook.com/…", linkedin: "https://linkedin.com/…" }}
  contactLink="/contact" feedbackLink="https://feedback.example.com"
  copyrightText="© 2025 Government of Singapore" lastUpdated="11 March 2024"
  bottomLinks={[{ label: "Report Vulnerability", href: "https://vulnerability.example.com", external: true },
                { label: "Privacy Statement", href: "/privacy" }, { label: "Terms of Use", href: "/terms" }]}
/>
```

---

## Tools

### Contrast checker
Tests colour contrast against WCAG.

- **Targets:** AA 4.5:1 for normal text and 3:1 for large text (18px+, or 14px+ bold). AAA 7:1.
- **Do:** test every text and background pair, in light and dark modes, and document the pairs that pass.
- **Don't:** treat it as full accessibility testing or use it on images.
