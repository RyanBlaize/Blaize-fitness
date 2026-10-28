# Blaize Fitness Website — Build Notes (claw.md)

Updated October 28, 2026 (second update). Replaces the previous build notes.

## What changed in this version
- Full redesign: Big Shoulders Display headlines + Hanken Grotesk body, black and white with blush pink (#e9b9bd) taken from the logo wordmark.
- New hero built on the brand story: "I was the client before I was the coach."
- New "Two ways to train" section that splits visitors into In person or Online.
- New Online Coaching section on a light background: 4-step "how it works", the three online plans, and an online FAQ.
- The Google Form link is replaced by an in-site, 4-step application (format and plan, contact info, goals, story). Every "Apply" button on a plan pre-fills that plan.
- Quick-question form now actually sends (the old one only cleared the fields).
- SEO: real page title and description, Open Graph tags, and LocalBusiness structured data (helps Google and AI search).
- Images recompressed; page is about 920 KB instead of 1.08 MB.
- Footer year updates automatically.
- My Story photo: the overhead press shot is replaced by a full-frame side-by-side of Ryan before and now, labeled "Before" (black tag) and "Now" (pink tag) in the top-left corner, with the caption "Same guy, 100 pounds apart." The overhead press photo is no longer used anywhere on the site.

## Section order
Nav → Hero → Proof row → My story → Two ways to train → In person (plans) → Online coaching (steps, plans, FAQ) → Results → Gallery → Apply → Quick question → Footer

## How applications reach you
Both forms post to FormSubmit (formsubmit.co), which works on GitHub Pages with no account and emails each submission to BlaizeFitnessinc@gmail.com.

ONE-TIME SETUP after you publish:
1. Go to blaizefitness.com and send yourself one test application.
2. Open the "Activate form" email from FormSubmit in BlaizeFitnessinc@gmail.com and click the button.
3. Every application after that arrives as an email titled "New application: Name (format)", with Reply-To set to the applicant.

Optional: FormSubmit's activation email gives you a random alias string. Swap it in for the email address in `FORM_ENDPOINT` near the top of the script so your address isn't visible in the page source.

If sending ever fails, the applicant sees an error with a one-click "send it by email instead" link, and their answers stay filled in.

Fields sent: format, plan, first/last name, email, phone, city/state, contact preference, main goal, experience, days per week, equipment, what's gotten in the way, injuries/health (optional), start timing, lead source ("How did you find me?" for your KPI tracking), consent, time zone.

## Pricing (unchanged from the live site)
In person: Beginner $250, Moderate $545 (Most popular), Gym Rat $815 per month, 55-min sessions, plus WA sales tax.
Online: Starter $139, Pro $215 (Best value), Elite $350, shown as per month.

## Photos on the page
- Hero: Ryan in the gym (portrait, tank top)
- My Story: Before (prom photo) and Now (graduation photo), full frame, 600×800 each
- Gallery: Ryan with a client, two gym floor shots, West Side Fitness team photo

To swap the before/now photos, keep both the same shape (3:4 portrait). Both photos include other people, so get their OK before the site goes live.

## Things to confirm
- Online prices are now labeled "/ month". Change the `<small>/ month</small>` text if they're billed differently.
- Online cards say "Plus WA sales tax where it applies". Confirm with a CPA how WA treats remote coaching, then edit that line.
- The online FAQ refers to "a coaching app". Name the app (Trainerize, TrueCoach, etc.) once you pick one.

## Editing tips
- Colors live in `:root` at the top of the `<style>` block.
- Images are embedded as base64. To swap one, re-export at ~800–900px wide, JPEG quality ~72, and replace the `src="data:image/jpeg;base64,…"` string.
