# CLAUDE.md — camini-legal

## Repository Purpose

This repository hosts the static legal/compliance web pages for **Camini**, an AI-powered photo art mobile app. These HTML files are served directly (e.g. via GitHub Pages or a static host) and linked from the app and the Play Store listing.

## Files

| File | Purpose |
|---|---|
| `privacy-policy.html` | Full GDPR/Play Store–compliant privacy policy |
| `delete-account.html` | Account deletion instructions (Play Store requirement) |
| `play_store_512.png` | App icon asset (512×512, used for Play Store) |
| `README.md` | Minimal repository title |

There is no build step, bundler, package manager, or test suite. All pages are plain HTML + inline CSS.

## App Context

Camini collects and processes:
- **Name, email, country** — Firebase Authentication + Firestore
- **Profile photo** — Firebase Storage
- **Photos for transformation** — sent to Google Gemini API (not stored server-side)
- **Anonymous analytics** — Firebase Analytics
- **Subscription status** — RevenueCat

Third-party services referenced in the policy: Google Firebase, Google Gemini API, RevenueCat, Google Play.

Contact email: `contact@camini.app`

## Design System

Both pages share a consistent dark-theme design implemented entirely with inline CSS and CSS custom properties:

```css
:root {
  --bg:      #080810;   /* page background */
  --surface: #12122A;   /* card/section background */
  --gold:    #FFD700;   /* primary accent */
  --purple:  #7C3AED;   /* secondary accent */
  --text:    #E8E6F0;   /* body text */
  --muted:   #8A84A8;   /* secondary text */
  --border:  rgba(255,255,255,0.07);
}
```

Key layout patterns:
- Centered single-column layout, `max-width: 720px`
- Top radial gradient glow: `rgba(124,58,237,0.28)` at `50% -10%`
- Section cards: `background: var(--surface)`, `border-radius: 20px`, 3px gold-to-purple left border via `::before`
- Numbered section headers use a `32×32px` rounded badge with gold border
- Bullet lists use a 6px gold dot `<span class="dot">`
- Contact CTA: gold `#FFD700` button, black text, `border-radius: 14px`
- Mobile breakpoint: `max-width: 480px` — reduces padding and font sizes

Logo mark: `✦` (Unicode U+2736, six-pointed black star)

## Conventions

### Dates
The "Last updated" date appears in two places per page — the `<span class="updated-badge">` in the header. Format: `Month DD, YYYY` (e.g. `June 25, 2026`). Update this whenever the content changes.

### Email links
Use `mailto:` hrefs with pre-populated subject and body where appropriate (see `delete-account.html`). URL-encode the subject and body strings.

### No JavaScript
Both pages are pure HTML/CSS with no JavaScript. Keep it that way unless there is a compelling reason.

### Responsive
Always verify changes look correct at ≤480px (the defined mobile breakpoint). Padding tightens from `64px 24px 80px` to `40px 16px 60px` on `.page`, and heading font sizes reduce.

### Cross-linking
`delete-account.html` links to `privacy-policy.html` in the footer. Maintain this link if files are renamed.

## Git Workflow

- Default development branch for AI-assisted changes: `claude/claude-md-docs-wm7c1u`
- Production content lives on `main`
- Commit messages follow an imperative style describing what changed (e.g. `Update privacy policy last updated date`)
- No linter, formatter, or CI pipeline — commits go straight to the branch

## Common Tasks

**Update the "Last updated" date**
Edit the `<span class="updated-badge">` text in the `<div class="header">` of the relevant file.

**Add a new policy section**
Copy an existing `.section` block, increment the `.section-num` number, update the `<h2>` title, and add content inside. No renumbering needed for semantic HTML, but keep numbers visually sequential.

**Change contact email**
Search both HTML files for `contact@camini.app` and update all occurrences (appears in `href` attributes and visible text).

**Add a new third-party service to the privacy policy**
Add a `<div class="third-party-item">` block inside the `.third-party-grid` in section 3 of `privacy-policy.html`.
