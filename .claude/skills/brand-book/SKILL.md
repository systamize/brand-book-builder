---
name: brand-book-generator
description: Generates a comprehensive brand book, shareable brand summary, and PDF from a website URL plus any supplemental assets the user provides. Use this skill whenever the user wants to create brand guidelines, a brand kit, a visual identity document, a style guide, or brand standards documentation. Trigger when the user shares a website URL and asks for brand documentation, mentions "brand book", "brand guidelines", "style guide", "brand identity", "brand kit", "brand standards", or wants to turn their website into a shareable design reference. Also trigger when the user needs to onboard a designer, contractor, or agency on their brand, or wants a PDF they can share that documents how their brand looks and sounds.
---

# Brand Book Generator

This skill turns a website into a polished, self-contained brand guidelines document — something a designer, copywriter, or contractor can pick up and immediately understand. The outputs are a comprehensive HTML brand book, a quick-share one-pager, and a PDF.

Good brand books don't just extract colors — they synthesize the brand's personality, voice, and visual logic into actionable rules. Aim for a document that makes decisions easier for anyone representing the brand, not just a visual inventory.

---

## Step 1: Gather Inputs

Before starting, collect:

**Required:**
- Website URL

**Optional (ask if not provided):**
- Additional logo files — file paths to SVG/PNG/JPG variants (horizontal, dark, icon-only, monochrome), or SVG code pasted inline
- Additional brand colors not visible on the website — hex, RGB, or CMYK values
- Output directory path (default: current working directory)
- Any known brand values, personality traits, or audience description the user wants to lock in rather than infer

Tell the user upfront what's required vs. what you'll extract from the website so they know what to prepare.

---

## Step 2: Research the Website

Visit the website and document systematically. Use every page available — especially About, Services/Products, and any blog or social links.

**Extract visually:**
- All colors in use: backgrounds, text, buttons, links, hover states, borders, accents. Record exact hex values. Don't guess — inspect CSS or computed styles if possible.
- Typography: font families (from CSS font-family declarations), weights used, size hierarchy from large headings to body to captions.
- Logo: how many variants are visible, their placement, clearspace, color versions.
- Imagery style: photography tone (warm/cool/neutral), subject matter, editing style, presence of illustration or graphics.
- Iconography: line vs. filled vs. duotone, stroke weight, visual consistency.
- Spatial feeling: tight/airy, grid-based, generous padding — this communicates brand personality too.

**Extract from copy:**
- About page: mission, origin story, who they serve, how they describe themselves. Synthesize — don't just copy-paste.
- Headline and tagline: vocabulary choices, sentence structure, tone (confident, nurturing, irreverent, technical, warm).
- CTA copy: "Get started free" vs. "Apply now" vs. "Talk to us" signals a lot.

The copy analysis shapes the Voice & Tone section even when the user hasn't written brand guidelines. A site saying "trusted for 30 years, certified professionals" is a completely different brand from one saying "no fluff, built for builders." Let that signal everything.

---

## Step 3: Handle Supplemental Assets

**Logo — SVG required for accurate reproduction:**

Before asking the user for anything, check whether the website's source code contains the logo as an inline SVG or references an `.svg` file. If it does, use that — it's the authoritative source and no further action is needed.

If the logo is only available as a rasterized image (PNG, JPG, WebP) on the website — or if the website is inaccessible — do not attempt to reconstruct the logo geometry by eye. Reconstructing paths from a rendered image produces inaccurate results. Instead, ask the user:

> "I can see your logo on the website but only as a raster image (PNG/JPG). For accurate reproduction in the brand book, could you share an SVG file? Any variant works — even just the icon."

Only ask for an SVG if:
1. The website source doesn't expose an SVG version, AND
2. The user hasn't already provided one

If the user can't provide an SVG, embed the raster image as a base64 data URI and note in the brand book that a vector source file should replace it.

Once you have an SVG file path, read it and embed the raw `<svg>` element inline in the HTML. Do not base64-encode SVGs — inline SVG is smaller and lets CSS target the fill colors for different background variants (white, dark, colored).

**Other logo assets:**
- Label each variant clearly (e.g., "Primary — Horizontal", "Icon Only", "Monochrome — White").

**Additional colors:**
- Add to the palette alongside website-extracted colors.
- Label source: "Provided by client" vs. "Extracted from website" — this matters when values differ.

If the user provides values that conflict with what's on the website (e.g., a different blue than what appears), note the discrepancy and ask which is canonical before proceeding.

---

## Step 4: Generate `brand-book.html`

Create a comprehensive, self-contained HTML file. The brand book should visually embody the brand it documents — use the brand's colors, fonts, and energy within the layout itself. No external dependencies except Google Fonts CDN (always include a complete local fallback stack).

### Navigation
Include a sticky sidebar (desktop) or top nav (mobile) that links to each section. The document is long — navigation is essential.

### Sections

**Cover Page**
Brand name, primary logo, and "Brand Guidelines [Current Year]". Clean, full-bleed, brand-colored.

---

**1. Brand Foundation**

Why this matters: this section tells people *how* to think about the brand, not just how it looks. Without it, every decision becomes a guessing game.

- **Brand overview** — 2-4 sentences on origin, mission, and positioning. Write this from what you found on the About page; note if inferred.
- **Brand values/pillars** — 3-5 core principles that guide decisions. Each gets a short name and a 1-2 sentence description.
- **Brand personality** — 8-10 descriptive keywords/attributes rendered as styled tags or tiles (e.g., Bold · Direct · Warm · Modern · Trustworthy). These should be specific enough to be useful — "professional" alone is too vague, "quietly confident" is better.
- **Target audience** — Who they're speaking to and what that audience values. 2-3 sentences.

---

**2. Logo Guidelines**

- Display all logo variants with labels (primary, secondary, icon-only, horizontal, monochrome, reversed).
- **Clearspace rule** — show minimum breathing room around the logo. Use the logo's cap-height or a simple grid unit as the clearspace standard. Display it visually.
- **Minimum size** — specify minimum width in pixels (digital) and millimeters (print).
- **Color backgrounds** — show the logo on approved backgrounds (light, dark, brand-color). Use CSS-rendered swatches with the logo overlaid.
- **Do's and Don'ts** — render actual visual examples in HTML/CSS, not just text rules. Show: stretched/squished, outlined, color-altered, placed on busy/low-contrast backgrounds as don'ts.

---

**3. Color Palette**

- Organize as: Primary, Secondary, Accent, Neutral/Support.
- For each color: rendered swatch, color name (if any), HEX, RGB, CMYK.
- **Contrast accessibility** — for each text color, show its contrast ratio against its intended background. Flag any that fall below WCAG AA (4.5:1 for body text, 3:1 for large text). Don't lecture about accessibility — just display the numbers.
- **Combination rules** — show which pairings work and which to avoid, rendered as actual HTML examples.

---

**4. Typography System**

- List primary typeface(s) and secondary/accent typeface (if any), with web-safe fallback stacks.
- **Typographic scale** — render actual HTML specimens for H1 through H6, body text, caption, and any special styles (pull quotes, labels). Show the actual font rendered at the right size and weight.
- Weight usage: which weight for headings, subheadings, body, captions.
- Line height and letter spacing recommendations.
- **Do's and Don'ts** — e.g., don't center-align body text, don't use more than 2 typefaces, don't set body text below 16px.

---

**5. Imagery & Visual Style**

- **Photography direction** — tone (warm/cool/moody/bright), composition (candid/posed/product-focused), color grading style, what subjects/contexts feel on-brand. 2-4 visual descriptors per dimension.
- **What to avoid** — generic stock photo clichés, visual treatments that feel off-brand.
- **Illustration style** (if present) — line weight, color palette used, level of detail.
- **Iconography** — style family (line, filled, duotone), stroke weight, sizing grid, whether to use rounded or sharp corners.

If the site has few imagery signals, note this and provide directional guidance based on the brand personality.

---

**6. Voice, Tone & Messaging**

- **Brand voice** — 3-5 adjectives with a brief explanation of what each means in practice (not just "friendly" — "friendly: we sound like a smart peer, not a corporate FAQ").
- **Tone by context** — how the voice shifts: formal communications vs. social media vs. error messages vs. sales copy. Use a simple 2-column table: context | how the tone shifts.
- **Key messaging** — tagline, 1-sentence elevator pitch, 3-5 core phrases or messages.
- **On-brand vs. off-brand writing examples** — show 2-3 side-by-side pairs. Make the contrast obvious and instructive.
- **Vocabulary** — words and phrases to use; words to avoid.

---

**7. Brand in Use**

Show the brand applied in context. Build these as rendered HTML/CSS mockups — not screenshots, not stock images. They live in the brand book as interactive examples.

Include at least:
- Social media post (square, with brand colors and typography)
- Email header / newsletter header
- Button/CTA styles (primary, secondary, ghost/outline)
- Pull quote / testimonial card

If the brand has specific applications (packaging, slide deck cover, business card), add those too. Keep them proportional and accurate to real-world usage.

---

### HTML Technical Requirements

- Fully self-contained: all CSS inline or in `<style>`. All images as base64 data URIs.
- `@media print` CSS: hide sidebar, break at section boundaries, preserve color backgrounds (`-webkit-print-color-adjust: exact`), remove decorative elements that don't print well.
- Responsive: sidebar collapses to top nav on mobile.
- Smooth scroll to anchored sections.
- A subtle "Back to top" link at each section end.

---

## Step 5: Generate `brand-summary.html`

A quick-reference one-pager — 2-3 screens tall, 1-2 printed pages. Think of it as a reference card someone can screenshot or print and pin to their wall.

Include:
- Brand name + primary logo (large)
- Full color palette (swatches + hex values)
- Typography specimens (font name + sample sentence in each weight)
- 6-8 brand personality keywords as styled tags
- 2-3 sentence brand voice description
- One "This is us / This isn't us" contrast row — a pair of short copy examples

Make it beautiful and compact. The layout should feel like the brand.

Technical: same self-contained HTML requirements. `@media print` should produce a clean 1-2 page PDF.

---

## Step 6: Generate `brand-book.md`

Create a structured Markdown file for LLM consumption — a machine-readable reference that any AI tool can load as context to produce on-brand output. It should be comprehensive but scannable: clear headings, short prose, and values expressed as lists or key-value pairs rather than narrative paragraphs.

Include all core brand data:

```markdown
# [Brand Name] — Brand Reference

## Brand Foundation
- **Mission:** ...
- **Positioning:** ...
- **Target audience:** ...
- **Brand values:** [value 1], [value 2], [value 3]
- **Brand personality:** [keyword 1] · [keyword 2] · ...

## Logo
- Primary logo: [description, file name if available]
- Variants: [list]
- Clearspace: [rule]
- Minimum size: [px digital / mm print]
- Approved backgrounds: [list]
- Never: [don'ts as a list]

## Color Palette
### Primary
- [Name]: HEX #XXXXXX | RGB (r, g, b) | CMYK (c, m, y, k)

### Secondary
...

### Neutral / Support
...

## Typography
- **Primary typeface:** [name] — [weights used]
- **Secondary typeface:** [name] — [weights used] (if any)
- **Fallback stack:** [CSS font-family string]
- **Scale:** H1 [size/weight], H2 [...], Body [...], Caption [...]
- **Line height:** [value] | **Letter spacing:** [value]
- **Rules:** [short do/don't list]

## Voice & Tone
- **Voice:** [adjective]: [what it means in practice] (repeat per trait)
- **Tone shifts by context:**
  - [Context]: [how tone adjusts]
- **Use:** [vocabulary/phrases to use]
- **Avoid:** [vocabulary/phrases to avoid]
- **Tagline:** "..."
- **Elevator pitch:** "..."

## Imagery & Visual Style
- **Photography:** [descriptors]
- **Avoid:** [what feels off-brand]
- **Iconography:** [style, weight, corner style]

## On-Brand vs Off-Brand Writing
- ON: "[example]"
- OFF: "[example]"
(repeat 2-3 pairs)
```

Write this file as `brand-book.md` in the output directory. It is meant to be dropped into an AI tool's context or stored as a memory — keep prose tight and values explicit.

---

## Step 7: Generate `brand-summary.pdf`

Generate the PDF from `brand-summary.html` (the shareable one-pager), not the full brand book. Use Playwright with a local HTTP server — the server is required so Google Fonts CDN loads in headless Chrome (file:// blocks external resources).

Create `generate_pdf.js` in the output directory:

```javascript
const http = require('http');
const fs = require('fs');
const path = require('path');
const { chromium } = require('playwright');

const DIR = __dirname;
const HTML_FILE = 'brand-summary.html';
const OUTPUT = path.join(DIR, 'brand-summary.pdf');
const PORT = 4321;

(async () => {
  const server = http.createServer((req, res) => {
    const filePath = path.join(DIR, req.url === '/' ? HTML_FILE : req.url);
    try {
      const data = fs.readFileSync(filePath);
      const ext = path.extname(filePath).slice(1);
      const types = { html: 'text/html', css: 'text/css', js: 'application/javascript', svg: 'image/svg+xml', png: 'image/png' };
      res.writeHead(200, { 'Content-Type': types[ext] || 'application/octet-stream' });
      res.end(data);
    } catch {
      res.writeHead(404); res.end();
    }
  });

  server.listen(PORT, async () => {
    const browser = await chromium.launch({ channel: 'chrome' });
    const page = await browser.newPage();
    await page.goto(`http://localhost:${PORT}`, { waitUntil: 'networkidle', timeout: 15000 });
    await page.waitForFunction(() => document.fonts.ready, { timeout: 10000 });
    await page.waitForTimeout(500);
    await page.pdf({
      path: OUTPUT,
      format: 'A4',
      printBackground: true,
      displayHeaderFooter: false,
      margin: { top: '12mm', right: '12mm', bottom: '12mm', left: '12mm' },
    });
    await browser.close();
    server.close();
    console.log(`PDF saved to ${OUTPUT}`);
  });
})();
```

Then install and run:

```bash
cd /path/to/output/directory
npm install playwright
node generate_pdf.js
```

Key notes:
- `channel: 'chrome'` uses the user's installed system Chrome — no headless browser download needed
- `displayHeaderFooter: false` removes timestamps, URLs, and page numbers (Chrome CLI adds these by default; this suppresses them reliably)
- `waitUntil: 'networkidle'` + `document.fonts.ready` ensures all fonts load before capture
- If Chrome is not installed, tell the user to install it or switch `channel: 'chrome'` to `executablePath: '/path/to/chromium'`

After the PDF is generated, clean up — these files shouldn't persist in the output folder:

```bash
rm generate_pdf.js package.json package-lock.json
rm -rf node_modules
```

Tell the user where the PDF was saved. If Playwright fails entirely, the HTML files print cleanly to PDF via browser (Cmd+P → Save as PDF → disable headers/footers in the print dialog).

---

## Quality Check Before Finishing

Before reporting completion, ask:

- Does the brand book itself look like the brand it's documenting? Does it use the right colors, fonts, and visual energy?
- Are all color hex values accurate and cross-checked against the website?
- Do the typography specimens actually render in the brand fonts (not system fallbacks)?
- Are the do's and don'ts specific enough to be actionable for someone who's never seen this brand before?
- Are the Voice & Tone examples concrete enough that a copywriter could use them without guessing?

If any section relied heavily on inference rather than direct evidence from the website, note that for the user so they can confirm or correct.

---

## Deliverables

Tell the user the four files that were created:

- `brand-book.html` — comprehensive brand guidelines, ~7 sections, full interactive HTML
- `brand-book.md` — structured Markdown reference for LLM consumption
- `brand-summary.html` — shareable one-pager reference card
- `brand-summary.pdf` — clean print-ready PDF generated from the summary (no headers, footers, timestamps, or page numbers)

And note anything that needed inference or might need their review (e.g., "I synthesized the brand values from your About page — take a look and adjust if they don't feel right").
