# Partnership readiness — what's left before you apply

## TL;DR

- **Thumbtack** is realistically reachable in **1–2 weeks** if you push.
- **Yelp** is **2–4 months** minimum — they're extremely selective and favor partners with existing scale.
- The landing page is necessary but **about 30% of the bar.** The other 70% is below.

---

## Tier 1 — must-have before you send the partnership email (cost: ~$300, time: 3–5 days)

These are non-negotiable. Apply without them and you go to the bottom of the queue.

### Legal entity
- [ ] **LLC or Inc registered.** Wyoming or Delaware are fine for an integrator. Use Stripe Atlas ($500) or Firstbase ($400) — 24h turnaround. Cheaper option: file directly on your state's Secretary of State site (~$100–200).
- [ ] **EIN from the IRS** (free, takes 10 minutes via irs.gov).
- [ ] **Business address.** Wyoming registered-agent address (~$100/yr) is fine. Don't use a P.O. box.

### Domain + email
- [ ] **Buy `replyhawk.ai`** before anything else (~$70/yr on Cloudflare or Porkbun).
- [ ] **WHOIS shows your company name** — don't use "Domains By Proxy." This is the #1 thing reviewers check.
- [ ] **Google Workspace** ($6/user/month) — get `partnerships@`, `founders@`, `support@` working with proper SPF/DKIM/DMARC. **Apply from a Google Workspace address, never from Gmail.**

### Site polish
- [ ] **Deploy the landing page to HTTPS** — Vercel/Netlify/Cloudflare Pages, all free.
- [ ] **Privacy Policy + Terms of Service** that actually exist. Use **Termly** (free tier) or **Iubenda** ($10/mo) — auto-generated, reviewer-acceptable. Wire them into the footer links.
- [ ] **DPA stub (Data Processing Agreement)** — single page describing what data you process and your sub-processors (Anthropic, Twilio, your DB host). Termly can generate this too.

### Team page
- [ ] Add an **About / Team** section to the site with your real name, photo, and LinkedIn link. Even just you is fine. Solo-founder partnerships get approved — pseudonymous ones don't.
- [ ] **LinkedIn company page** for ReplyHawk — takes 5 minutes, instantly increases legitimacy.

---

## Tier 2 — closes Thumbtack approval (cost: ~$0, time: 3–7 days)

These take you from "we're considering you" to "test credentials issued."

- [ ] **Demo video or live demo capability.** A 90-second Loom of the dashboard + the parse-lead/send-reply scripts running against a real lead. This is the single highest-leverage thing you can have.
- [ ] **Your anchor customer in writing.** Get a short email from the construction firm owner stating: "We are working with ReplyHawk to integrate with our Thumbtack Pro account. We authorize ReplyHawk to apply for partner API access on our behalf." Attach to the application.
- [ ] **Thumbtack Pro URL of the customer** — partnerships will spot-check the pro account exists, is in good standing, has paid leads in the last 30 days, has good reviews.
- [ ] **List the scopes you need** in the email: `supply::negotiations.read`, `supply::negotiations.write`, `supply::messages.read`, `supply::messages.write` (they'll correct if needed).
- [ ] **Webhook endpoint ready.** Even just an ngrok URL pointing to a Node server that logs the payload — they want to verify your endpoint can receive `NegotiationCreatedV4`.

---

## Tier 3 — unlocks Yelp eventually (cost: variable, time: weeks–months)

Yelp's bar is genuinely high. To get past it you need:

- [ ] **Multiple customers** — ideally 5+ pros under contract, even at $0 during pilot.
- [ ] **Existing integrations as social proof** — e.g. "we already integrate with Twilio for SMS, Stripe for billing." Doesn't matter that this is just plumbing; it shows you ship.
- [ ] **Security one-pager** — short PDF describing your data handling, encryption at rest/in transit, access controls. SOC 2 not required for v1 but a stated intent ("SOC 2 Type 1 by Q4") helps.
- [ ] **Press or notable customers** — if you can land one well-known service business or get covered by a trades publication, lead with that in the cover note.
- [ ] **Reference customer who will vouch on a call** — Yelp sometimes asks to talk to one of your customers before approving.

Don't apply to Yelp until you've already got Thumbtack live and 3–5 pros on the platform. Doing it earlier makes the rejection a permanent mark on your file.

---

## Specifically what's broken right now in the site

Reviewers WILL click these:

| Issue | Fix | Time |
|---|---|---|
| Privacy + Terms link to `#` | Generate via Termly, deploy as `/privacy.html`, `/terms.html` | 30 min |
| Demo form doesn't actually capture leads | Wire to Formspree (free) or Resend webhook | 15 min |
| No team / about page | Add a `/about.html` with your photo + LinkedIn | 1 hr |
| Footer contact links go to `mailto:` on a domain you don't own yet | Buy domain + set up Workspace | 1 hr |
| No customer logos or testimonials | Add a one-line testimonial from your construction client once he agrees | 5 min |
| No screenshots of the product | Take 3 dashboard screenshots once the v1 is running | 1 hr |
| No demo video | Record a 90-sec Loom showing a real lead being replied to | 30 min |

---

## What this costs you to get partnership-ready

| Item | One-time | Recurring |
|---|---|---|
| LLC registration (Stripe Atlas) | $500 | — |
| Domain (replyhawk.ai) | — | $70/yr |
| Google Workspace | — | $6/user/mo |
| Termly (Privacy + ToS + DPA) | — | $10/mo |
| Hosting (Vercel free tier) | — | $0 |
| Wyoming registered agent (optional, for address) | — | $100/yr |
| **Total** | **~$500** | **~$25/mo** |

For five hundred bucks and a long weekend, you go from "guy with a landing page" to "credible early-stage SaaS the partnerships team will respond to."

---

## Recommended order of operations this week

**Day 1 (today):**
1. Register `replyhawk.ai` on Cloudflare. Don't enable WHOIS privacy yet — set the registrant to your name + your LLC's intended name. You'll update once the LLC is filed.
2. File the LLC on Stripe Atlas or directly on Delaware/Wyoming SOS site.
3. Apply for EIN on irs.gov (instant if you have an SSN, 4 weeks by mail otherwise).

**Day 2–3:**
4. Once LLC is approved, set up Google Workspace with the domain.
5. Deploy the landing page to Vercel/Netlify with HTTPS.
6. Generate Privacy + Terms + DPA via Termly, wire into the footer.
7. Add a simple `/about.html` with your photo and LinkedIn.

**Day 4–5:**
8. Record a 90-second Loom showing: opening a real Yelp lead in Chrome → running `parse-lead.mjs` in a terminal → showing the parsed JSON → showing the AI draft → showing the reply land in Yelp. This single video is worth more than 10 cold emails.
9. Have your construction client write the 3-line authorization email.

**Day 6:**
10. Send the Thumbtack partnership email from `partnerships@replyhawk.ai` with the Loom link, the customer authorization, and your Thumbtack scope list. Expected response: 2–10 business days.

**Day 7+:** Build the product against the test environment they'll give you while waiting for production credentials.

---

## A bluntly honest read of your odds

- **Thumbtack approval in <30 days: ~70%** if you do everything in Tier 1 + Tier 2.
- **Yelp approval in <90 days: ~10%** even with everything. Plan for Yelp via email-parse + Playwright until you have real scale.

Don't let Yelp approval block the rest of the build. The system works fine on email-parse + Playwright for Yelp, the Thumbtack API for Thumbtack, and email-parse for LSA. By the time you've earned Yelp's attention, those other channels are already paying.
