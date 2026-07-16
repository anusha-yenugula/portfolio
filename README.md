# Handoff: Anusha Yenugula — Design Portfolio Site

## Overview
A personal portfolio site for Anusha Yenugula, Head of Design at Smart Pension. Five pages: homepage (hero, selected work, writing, connect/footer), an About page, and three password-protected case study pages. Editorial, minimal, plant-green palette.

## About the Design Files
The files in this bundle (`*.dc.html`) are **design references built in a proprietary HTML component format** — they render and behave correctly in a browser (including the search, dark mode, and password-gate logic) but are **not production code to copy directly**. They use a custom templating syntax (`{{ }}` bindings, `<sc-if>`, `<sc-for>`, a `DCLogic` class) that only runs inside their original authoring tool via `support.js`.

**The task is to recreate these designs in Next.js** (App Router recommended), translating the layout, styling, and interaction logic into React components and Tailwind/CSS. Do not attempt to ship `support.js` or the `.dc.html` files as-is — treat this bundle purely as a visual + behavioral spec.

## Fidelity
**High-fidelity.** Colors, type, spacing, and copy in the HTML files are final (aside from placeholders explicitly marked `[Placeholder]`). Recreate pixel-close using the values below.

## Recommended stack for Vercel + Claude Code
- Next.js 14+ (App Router), TypeScript
- Tailwind CSS (all styling below is expressible as utility classes)
- Deploy target: Vercel (static/SSG is sufficient — no backend needed)

## Pages / Routes

### 1. `/` — Portfolio (home)
File: `Portfolio.dc.html`
- **Header** (sticky, blurred bg): logo/name link → `#top`, search input (220px wide; filters case studies + Connect only — Work/Writing excluded from results), "Work" nav link (no "About" link currently — removed for now), dark-mode toggle (sun/moon icon button), "Let's Connect!" pill button (dark bg, cream text) linking directly to `https://www.linkedin.com/in/anusha-yenugula` (external).
- **Hero** (`#top`): eyebrow label "DESIGN LEADER AT SMART PENSION", large serif H1 (64px) reading "I help make complex things feel simple, achievable and a little more human" (confirm exact current copy in file), intro paragraph (max-width 560px), two CTAs ("View my work →" filled green pill scrolling to `#work`, "Get in touch" outlined pill linking directly to LinkedIn, external), photo (rounded-rect 4:5, ~260px wide, right-aligned, vertically centered with text, object-position centered, no border).
- **Selected Work** (`#work`): section label + note "Password protected. Access details are available on my resume." (no reach-out CTA — resume is the access path). 3-column card grid: each card = gradient cover image (aspect 4:3) with large number ("01"/"02"/"03") and a small lock-icon badge (top-right), then padding block with tag ("Smart Pension" — company name, not the internal project category), title (serif 26px), one-line non-confidential teaser, "View case study →". Cards lift + shadow on hover. Card 01 teaser: "Reimagining Smart Pension's mobile experience, moving millions of members from a web app into an engaging native-first journey."
- **Writing section removed** (was previously a Substack teaser card under `#writing` — currently hidden; Substack link still lives in the footer Connect row).
- **Connect / Footer** (`#connect`, dark bg `#191714`, cream text): section label, large serif headline, LinkedIn (`https://www.linkedin.com/in/anusha-yenugula`) + Substack buttons (external links), bottom row: "© 2026 Anusha Yenugula" / "Built with Claude Code".

### 2. `/about` — About
File: `About.dc.html` — bio, values/mantras, personal geo-journey infographic. (Copy is placeholder — see file for structure.)

### 3–5. Case studies (password-protected, noindex)
Files: `Case Study - Native Member App.dc.html`, `Case Study - Profile Project.dc.html`, `Case Study - Individual Pensions.dc.html`
- Each page **gates content behind a password form** until unlocked (session-persisted via `sessionStorage`), and carries `<meta name="robots" content="noindex, nofollow, noarchive">` to keep them out of search engines.
- **Lock screen**: centered card, lock icon, "This case study is private", password input, error message on wrong password, "Unlock" button, "← Back to portfolio" link.
- **Unlocked content structure** (consistent across all three): sticky header (logo + "← Back to work"), hero (case number/category label, serif H1, intro paragraph, 4-stat metrics strip in a card), then stacked sections — Overview, My Role (2–3 column card grid), The Challenge (3-column card grid), Strategic Shift (dark diagram panel, two-column before/after), Strategy/Design Principles (2-column card grid), Business Impact/Outcome (3-column card grid), closing pull-quote, footer with "Next" case study link.
- **In Next.js**: implement the password gate client-side exactly as here (a simple string match is fine for a portfolio — no real security requirement), and keep `noindex` meta + `sessionStorage` unlock persistence per case study (or shared across all three, current behavior is shared via one `cs_unlocked` key).

## Interactions & Behavior
- **Dark mode**: toggled via button in header; swaps a small palette object (bg/text/muted/border/card/input colors + accent + eyebrow color) app-wide. No system-preference detection currently — manual toggle only, state not persisted across reloads (add `localStorage` persistence if desired).
- **Search**: live-filters a static list of {title, subtitle, href} items as you type; shows dropdown under the input; empty state "No results for "…""; currently includes case studies + Connect (Work/Writing intentionally excluded from search results per latest edit).
- **Case study password gate**: form submit checks against a hardcoded string (`PASSWORD` constant per file, currently `"smartpension2026"` — **change before shipping**), persists unlock via `sessionStorage.setItem("cs_unlocked","true")`.
- **Hover states**: nav links → accent color; buttons → darken/lighten; work cards → lift + shadow; Substack card → lift + shadow.
- **Focus states**: visible 2px outline (accent color) with offset on all interactive elements, for accessibility.
- **Anchor scrolling**: `scroll-behavior: smooth`, sections have `scroll-margin-top: 96px` so the sticky header doesn't cover the target.

## Design Tokens

### Light mode
- Background: `#F6F4EF`
- Text: `#191714`
- Muted text: `#4a453e`
- Border: `rgba(25,23,20,0.12)`
- Card background: `#FBF7EF`
- Input background: `#FFFFFF`
- Accent (labels, tags, links): `#3d5636` (darkened green — AA-compliant on light bg)
- Eyebrow color: `#5D6D22`

### Dark mode
- Background: `#17160F`
- Text: `#F5F0E6`
- Muted text: `#b3ad9d`
- Border: `rgba(245,240,230,0.15)`
- Card background: `#221F16`
- Input background: `#221F16`
- Accent (labels, tags, links): `#A9C08F` (sage — AA-compliant on dark bg)
- Eyebrow color: `#A9C08F`

### Fixed (both modes)
- Deep green (buttons, selection): `#4A6741`
- Button green: `#5D6D22` / hover `#4a5a1b`
- Sage: `#A9C08F`
- Dark footer bg: `#191714`
- Cream (on dark): `#F5F0E6`

All accent/label colors are **theme-aware** — they were adjusted so both light and dark variants pass WCAG AA contrast (4.5:1) against their respective backgrounds. Keep them theme-aware in the rebuild; don't hardcode a single accent color across both modes.

**Font pairing is settled as Newsreader + IBM Plex Sans** — this was tested against a Lora swap and reverted; keep Newsreader for all serif/display headings.

### Typography
- Display/headings: `Newsreader` (serif), weights 400/500/600, italic 400/500 — Google Fonts
- Body/UI: `IBM Plex Sans`, weights 400/500/600 — Google Fonts
- H1 hero: 64px/1.08, weight 500, letter-spacing -0.01em
- Section labels: 15px, weight 600, uppercase, letter-spacing 0.08em
- Case study H1: 56px/1.12, weight 500
- Card titles: serif 26px weight 500; body copy 15–18px, line-height 1.5–1.75

### Spacing / Radius
- Page max-width: 1400px (case studies: 1100px content column, 900px text column)
- Section padding: 100px vertical, 64px horizontal
- Card radius: 20px; buttons: 100px (pill); photo: 14px

## Assets
- Hero photo: `uploads/D1CB5267-3A93-44A2-BED0-DAA6266C52EA.png` (copied into this folder as `photo.png`)
- Work card covers: currently CSS gradients (no real images yet) — swap in real product screenshots when available
- Icons: inline SVG (search, lock, sun/moon, arrows) — recreate as SVG or icon library (e.g. lucide-react)

## Files in this bundle
- `Portfolio.dc.html` — homepage
- `About.dc.html` — about page
- `Case Study - Native Member App.dc.html`
- `Case Study - Profile Project.dc.html`
- `Case Study - Individual Pensions.dc.html`
- `photo.png` — hero photo asset

## Known placeholders to fill in before launch
- About page bio/values/journey copy
- Case study Role/Team/Timeline/Platform fields (Native Member App page)
- Real product screenshots for case study hero/gallery images and homepage work cards
- Case study passwords (currently a shared demo password — replace per-page or with real auth if needed)
