# Project: ShopFloorCalc (shopfloorcalc.com)

## What This Site Is
A collection of free professional calculators and reference tools for tradespeople and engineers. Covers metalworking, welding, HVAC, plumbing, electrical, and general industrial calculations. Every tool is a standalone page designed to be found through Google search and used on a phone at a job site.

---

## Architecture

- Every tool is a single HTML file with all CSS and JS embedded inline.
- Each tool lives in its own folder: `tool-name/index.html` (produces clean URLs like `shopfloorcalc.com/tool-name/`).
- No build step, no frameworks, no bundlers. Push to GitHub, Cloudflare Pages deploys automatically.
- Google Fonts is the only allowed external resource. Everything else is self-contained.

---

## Design System

### Aesthetic Direction
The site should feel like a well-made modern reference tool: the kind of thing an engineer bookmarks and trusts. Think of a high-end technical manual redesigned for screens. Dense with information, generous with whitespace around that density, and confident in its typography.

Dark headers and navigation contrast with light content areas. Data is king. The design stays out of the way and lets the tool do its job.

### Typography
Load from Google Fonts in every page `<head>`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

- **Headings and UI labels:** Inter 600/700
- **Body text:** Inter 400/500
- **Data, numbers, inputs, outputs, table cells, code:** JetBrains Mono 400/500
- Base font size: 16px body, 15px for dense table content
- Line height: 1.6 for body text, 1.4 for headings, 1.3 for table rows

### Color Palette
Use CSS custom properties defined in `:root` on every page:

```css
:root {
  /* Core */
  --color-bg: #f8f9fa;
  --color-surface: #ffffff;
  --color-text: #1a1d23;
  --color-text-secondary: #5a6069;
  --color-border: #d1d5db;

  /* Header and navigation */
  --color-header-bg: #111318;
  --color-header-text: #f0f1f3;

  /* Accent — steel blue */
  --color-accent: #2563eb;
  --color-accent-hover: #1d4ed8;
  --color-accent-light: #dbeafe;
  --color-accent-subtle: #eff6ff;

  /* Functional */
  --color-success: #16a34a;
  --color-warning: #d97706;
  --color-error: #dc2626;

  /* Table */
  --color-table-header-bg: #1e2028;
  --color-table-header-text: #f0f1f3;
  --color-table-row-alt: #f1f3f5;
  --color-table-hover: #e8ecf0;

  /* Shadows */
  --shadow-sm: 0 1px 2px rgba(0,0,0,0.06);
  --shadow-md: 0 2px 8px rgba(0,0,0,0.08);
  --shadow-lg: 0 4px 16px rgba(0,0,0,0.10);
}
```

### Layout and Spacing
- Max content width: 960px, centered with `margin: 0 auto`
- Page padding: 24px on mobile, 40px on desktop
- Section spacing: 48px between major sections, 24px between subsections
- Card/panel style for the interactive tool area: white background, 1px border `var(--color-border)`, border-radius 8px, `var(--shadow-md)`, padding 24px
- All content must work at 380px viewport width minimum

### Interactive Tool Area
- Inputs: height 44px minimum (touch-friendly), border-radius 6px, 1px solid border, JetBrains Mono for the input text
- Labels: Inter 500, `var(--color-text-secondary)`, placed above inputs
- Buttons: `var(--color-accent)` background, white text, Inter 600, height 44px, border-radius 6px, subtle hover transition (darken background)
- Output/result area: light accent background `var(--color-accent-subtle)`, border-left 3px solid `var(--color-accent)`, JetBrains Mono for values, padding 16px

### Tables
- Dark header row: `var(--color-table-header-bg)` background, `var(--color-table-header-text)` text, Inter 600, uppercase small text with letter-spacing
- Alternating row colors using `var(--color-table-row-alt)`
- Hover highlight on rows using `var(--color-table-hover)`
- Cell padding: 10px 14px
- Number/data cells in JetBrains Mono
- On mobile: wrap the table in a horizontally scrollable container with `-webkit-overflow-scrolling: touch`

### Header/Navigation
- Full-width dark bar: `var(--color-header-bg)`
- Site name "ShopFloorCalc" on the left in Inter 700, `var(--color-header-text)`
- Minimal nav links on the right: Tools, About (plain text links, no buttons)
- Height: ~56px
- Sticky position at top of page

### Footer
- Light gray background (#f1f3f5)
- Links to: Home, About, Privacy Policy
- Disclaimer text: "These values are general guidelines from published industry references. Always verify with manufacturer specifications for your specific application."
- Small, understated. Inter 400, `var(--color-text-secondary)`

---

## Page Structure for Tool Pages

Build every tool page with these sections in order:

1. **Site header** (sticky nav bar described above)
2. **Breadcrumb** — Home > Category > Tool Name (small, `var(--color-text-secondary)`)
3. **H1** with the tool name
4. **Subtitle** — one line describing what the tool does and who it's for
5. **The interactive tool** inside a card/panel (inputs, live-updating outputs or a calculate button, results display)
6. **Supporting content** (800–2,000 words) covering:
   - What this calculation or concept is and why it matters
   - The formulas used, cited to authoritative sources (ASM, AWS, ASHRAE, NEC, ASME, etc.)
   - Practical examples with real numbers
   - Common mistakes professionals make
   - Edge cases and related considerations
7. **Reference table** if applicable (common values, material properties, etc.)
8. **FAQs section** — 5–8 questions with detailed answers, using `<details>/<summary>` elements
9. **Related Tools** — links to 3–5 other tools on shopfloorcalc.com
10. **Site footer**

---

## SEO Requirements

- **Title tag:** under 60 characters, target keyword first, then "| ShopFloorCalc"
  Example: `Heat Treat Temperature Guide | ShopFloorCalc`
- **Meta description:** under 155 characters, describes what the tool does and who benefits
- **H1** matches the title tag closely (without the "| ShopFloorCalc" suffix)
- **Canonical URL:** include `<link rel="canonical" href="https://shopfloorcalc.com/tool-name/">` on every page
- Semantic HTML: proper heading hierarchy (one H1, logical H2/H3), `<label>` elements for form inputs, `alt` text on any images
- Google Analytics placeholder in the `<head>`:
  ```html
  <!-- Google Analytics: replace GA_MEASUREMENT_ID with actual ID -->
  ```

---

## Quality Requirements

- Every calculation must cite its source formula somewhere in the supporting content.
- Test all outputs against known references before deploying. Spot-check at least 3–5 values.
- Handle edge cases: zero inputs, negative numbers, out-of-range values should produce a clear message instead of broken output or NaN.
- Input fields should include placeholder text showing the expected format (e.g., `placeholder="e.g. 4140"`).
- Results should update live on input change for simple calculators. Use a "Calculate" button only when the computation involves multiple required fields that interact.
- Every input and output must show its units.

---

## Homepage (index.html)

- Same header and footer as tool pages
- Brief intro: "Free professional calculators and reference tools for tradespeople and engineers."
- Tools listed in a card grid, organized by trade category (Metalworking, Welding, HVAC, Plumbing, Electrical, General)
- Each card shows: tool name (linked), one-line description, and category tag
- Cards should have `var(--shadow-sm)` and a subtle hover effect (lift + stronger shadow)
- Responsive grid: 3 columns on desktop, 2 on tablet, 1 on mobile

---

## Cross-Linking Rules

- Every tool page must link to 3–5 related tools in its "Related Tools" section.
- Only link tools that serve the same audience or involve complementary calculations.
- Use descriptive anchor text that includes the tool's purpose (e.g., "Hardness Conversion Calculator" instead of "click here").
- When a new tool is added, update the Related Tools section on existing pages that share the same audience.

---

## File Naming

- Folder names: lowercase, hyphenated (e.g., `heat-treat/`, `weld-filler/`, `duct-sizing/`)
- Each folder contains a single `index.html`
- No spaces, no underscores, no capital letters in folder names