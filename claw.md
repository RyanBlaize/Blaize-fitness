# Blaize Fitness Website — Build Notes (claw.md)

This document records every design and technical decision made for the Blaize Fitness website so future updates can be made quickly and consistently.

---

## Project Overview

**Site:** Blaize Fitness promotional website  
**File:** `blaize-fitness.html`  
**Stack:** Single-file HTML — no frameworks, no dependencies beyond one Google Font  
**Purpose:** Promote, build, and scale a mixed in-person + online fitness coaching business

---

## Design System

### Color Palette
| Name | Hex | Usage |
|------|-----|-------|
| Black | `#0a0a0a` | Page background |
| Near Black | `#111111` | Section backgrounds, card backgrounds |
| Dark Grey | `#1c1c1c` | Card surfaces, input backgrounds |
| Mid Grey | `#3a3a3a` | Borders, subtle elements |
| Light Mid Grey | `#6e6e6e` | Secondary labels, hints |
| Soft Grey | `#a0a0a0` | Body text, subheadings |
| Off White | `#f5f5f5` | Hover state for white elements |
| White | `#ffffff` | Primary CTAs, headings, featured cards |

### Typography
- **Font:** Inter (Google Fonts) — clean, modern, highly legible
- **Fallback:** -apple-system, BlinkMacSystemFont, sans-serif
- **Weights used:** 300 (body), 400 (labels), 500 (nav), 600 (headings), 700 (display)
- **Heading scale:** 86px hero → 52px section → 22px card → 19px subcard
- **Letter spacing:** Tight negative tracking on large headings (–2.5px hero, –1.5px section)

### Layout Principles
- Inspired by Apple.com: full-bleed sections, generous whitespace, precise type hierarchy
- Content width: unconstrained with `padding: 0 5%` side gutters
- Section vertical rhythm: `120px` top and bottom padding
- Grid system: CSS Grid throughout (`repeat(3, 1fr)` for services/packages/testimonials)
- Mobile breakpoints: 900px (2-column collapses) and 600px (single column)

### Motion
- Scroll-reveal on cards: IntersectionObserver fades in + slides up (opacity 0→1, translateY 24px→0)
- Gallery image zoom on hover (scale 1.06, 0.7s ease)
- Nav scroll blur: backdrop-filter on fixed nav
- Hero scroll indicator: pulsing opacity animation

---

## Site Structure

```
1. Navigation (fixed, blur bg)
2. Hero           — Full-bleed image, headline, two CTAs
3. Stats Bar      — Four key social-proof numbers
4. About          — Split image + copy layout
5. Gallery        — 5-photo asymmetric grid (2+2 columns, tall main image)
6. Services       — 6-card grid (2×3), icon + description + tag
7. Packages       — 3-tier pricing (Starter / Elite / Online Only)
8. Testimonials   — 3 cards with avatar, star rating, quote
9. Intake Form    — Google Form button + embed instructions
10. Contact        — Split layout: contact info + quick message form
11. Footer         — Copyright + nav links
```

---

## Things to Customize (Action Items)

### Must Do Before Launch

**1. ✅ Google Form — Intake Section (DONE)**
Form is live and linked:
```
https://docs.google.com/forms/d/e/1FAIpQLSeJ-NDgC44J93nxFnM6bWZ80dmqBAoRpk14mrwhfDe7b9k8tw/viewform?usp=header
```

**Optional — Embed the form inline instead of linking out:**
In your Google Form → click Send → click the `<>` embed tab → copy the `<iframe>` code.
Replace the entire `<div class="intake-embed">...</div>` block with the iframe. Add `style="width:100%; border:none; min-height:600px;"` to the iframe tag.

**2. ✅ Instagram Link (DONE)**
```
https://www.instagram.com/blaizefitness.ws
```

**3. ✅ Email Address (DONE)**
```
BlaizeFitnessinc@gmail.com
```

**4. ✅ Location (DONE)**
```
4617 36th Ave SW, Seattle, WA 98126
```

**5. ✅ Stats (DONE)**
```html
<div class="stat-num">100+</div>  ← clients trained
<div class="stat-num">5★</div>    ← rating
<div class="stat-num">4yr</div>   ← years coaching experience
<div class="stat-num">100%</div>  ← personalized
```

**6. ✅ Pricing (DONE)**
In-person packages (Beginner $190, Moderate $545, Gym Rat $815, all + WA state tax) and online coaching packages (Starter $139, Pro $215, Elite $350) are live, with 55 minute session length and weekly cadence (1/2/3 sessions per week) called out on each card.

**7. ✅ Testimonials (DONE)**
Real client testimonials from Kristin G., Verne J., and Adele L. are live.

**8. ✅ About Section Story (DONE)**
Rewritten with Ryan's personal journey (losing 100 pounds, 4 years coaching, mission to create a safe training environment), in a warm conversational tone. The About section uses a single portrait photo (solo tricep pose).

**9. ✅ TikTok + YouTube Links (DONE)**
```
https://www.tiktok.com/@blaize.fitness
https://www.youtube.com/@BlaizeFitness
```
Added to the contact section's social links, alongside Instagram.

**10. ✅ Hero Section (DONE)**
Background photo replaced with the Blaize Fitness logo, displayed large and centered over a dark radial gradient.

**11. ✅ Gallery (DONE)**
The overhead press action photo now lives in the gallery grid (6 photos total), above the packages/offer section.

---

## Photography

All images are now **real Blaize Fitness photos**, embedded directly in the HTML as base64 data so no external image hosting is needed:

| Section | Source |
|---------|--------|
| Hero background | Solo trainer dramatic shot (Ryan) |
| About — portrait | Same solo trainer shot, reused |
| About — second photo | Ryan coaching an overhead press |
| Gallery (5 images) | Gym interior shots, team photo outside Westside Fitness, trainer + client mirror selfie, solo trainer shot |
| Testimonial avatars | Initial letter circles (K, V, A) — no photos needed |

To swap in a new photo for any section, replace the `src="data:image/jpeg;base64,..."` string with a freshly base64 encoded image. Recommended sizing before encoding to keep file size reasonable:
- Hero: ~1200px wide
- About / gallery images: ~800 to 900px wide
- Always re-export at JPEG quality 70 to 80 for a good size to quality balance

---

## Contact Form Notes

The quick-message form in the Contact section is currently **front-end only** — it clears the fields and shows a success message, but does not actually send an email. To make it functional, choose one of these options:

**Option A — Formspree (easiest, free tier)**
1. Sign up at formspree.io
2. Create a form and copy your endpoint (e.g. `https://formspree.io/f/abc123`)
3. In the `handleContactSubmit()` function, replace the content with:
```javascript
async function handleContactSubmit() {
  const data = {
    name: document.getElementById('cf-name').value,
    email: document.getElementById('cf-email').value,
    interest: document.getElementById('cf-interest').value,
    message: document.getElementById('cf-msg').value,
  };
  await fetch('https://formspree.io/f/YOUR_ID', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(data)
  });
  document.getElementById('form-success').style.display = 'block';
}
```

**Option B — EmailJS (no backend required)**
Works similarly to Formspree but sends directly via your email account.

**Option C — Link to your intake Google Form**
Replace the contact form with a second link to the same Google Form for simplicity.

---

## Hosting Options

Since this is a single `.html` file, it can be hosted anywhere:

| Platform | Steps | Cost |
|----------|-------|------|
| **GitHub Pages** | Push to a repo, enable Pages in settings | Free |
| **Netlify** | Drag-and-drop the HTML file at netlify.com/drop | Free |
| **Squarespace** | Paste as a code block or host the file separately | Paid |
| **Custom domain** | Point your DNS to any of the above | Domain cost only |

---

## Future Enhancements

These sections are ready to add when needed:

- **Blog / Content section** — Add below testimonials for SEO + credibility
- **Video embed** — Replace gallery with a hero reel or YouTube/Vimeo embed
- **Booking widget** — Embed Calendly directly in the intake section
- **Progress photos section** — Before/after grid with client permission
- **FAQ accordion** — Add above the footer to reduce intake questions
- **Additional social links** — Facebook (SVG icon pattern available, follow the same structure as Instagram/TikTok/YouTube)

---

## Rebuilding or Editing with Claude

To make changes to this site using Claude in the future, paste this prompt:

> "I have a fitness website at `blaize-fitness.html` built for Blaize Fitness. The design uses black (#0a0a0a), near-black (#111111), white, and grey (#a0a0a0) — Inter font, Apple-style layout. Here is my claw.md for context: [paste this file]. I want to [describe what you want changed]."

Claude will be able to maintain design consistency by referencing these tokens.
