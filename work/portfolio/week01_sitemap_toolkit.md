# Week 1 — Draw the Path: Portfolio Sitemap + Toolkit

Track: General AI Fluency · Assignment: Draw the Path · Phase: Setup

## The one person and the one action

- **One person:** the FlyRank mentor/lead who reviews this portfolio and decides
  "does this intern build honest ML end to end?" They know the internship, the lanes,
  and the grading rubric — the site does not need to explain FlyRank, it needs to
  prove *my* workflow.
- **One action:** **email / contact me.** Every page funnels to one "I have a
  suggestion — email me" CTA. Nothing else is asked of them.
- **Proof statement (claim) — primary draft:**
  > "I take a fuzzy business question and end with an honest, useful model — on the
  > FlyRank refresh lane that means a forest that ranks which pages to review first,
  > measured P@50 0.86 on held-out clients, with the data contract, leakage audit, and
  > claim rewrites committed in public every week."
- **Proof statement — short hero variant (if the full one is too long):**
  > "From fuzzy question to honest, shipped model — every step committed."

The claim lives in the hero, echoes in each section heading, and is the custom
instruction for the Claude Project below.

## Sitemap — 4 pages, nothing more

```
                ┌─────────────────────────────┐
                │  HOME (hero: the claim)     │
                │  "…honest, useful model."   │
                └──────────────┬──────────────┘
                               │
        ┌──────────────┬───────┴────────┬──────────────┐
        │              │                │              │
   ┌────▼─────┐  ┌─────▼──────┐   ┌────▼─────┐   ┌────▼──────┐
   │  WORK    │  │  ABOUT     │   │ CONTACT  │   │ (footer:  │
   │the case  │  │ who + how  │   │the action│   │ all pages)│
   │ study    │  │ I work     │   │ email CTA│   │ │
   └──────────┘  └────────────┘   └──────────┘   └───────────┘
```

Why each page earns its place against claim + one action:

| Page | Purpose in the path (land → believe → act) | What proves the claim here | It stays because |
|---|---|---|---|
| **Home** | Index + hero states the claim, one CTA below it | Claim + the single number (P@50 0.86, held-out clients) + link to Work | Without it no one knows who I am in one glance |
| **Work / case study** | The belief step — one case, not a gallery | Refresh Lane end-to-end: contract → baseline 0.74 → forest 0.86 → validation audit (before/after split, leak trap) → ranked queue. Notebook + metrics receipts linked | One deep, honest case out-proves five shallow ones; it *is* the claim |
| **About** | Trust: who I am + how I operate | Three sentences + the working method (one task per conversation, honest claims, commit each step) | Explains *why* the receipts exist — method is part of the proof |
| **Contact** | The one action: email/kick off a conversation | Direct email link, no forms, no signup before the click | The brief's "how to act" page; it is the conversion point |

**Pages deliberately NOT added** (resisted): a blog, a "projects" gallery with more
than this one case, a skills/resume dump, a pricing page, a testimonials page. Each
would give the mentor a reason to wander instead of acting — the one action is email.

## Toolkit checklist (free accounts)

All four are set up already; confirm each can sign in on signing-day:

- [x] **Claude** (claude.ai) — free — *the* primary assistant + Project tutor for all 8 weeks
- [x] **ChatGPT** (chatgpt.com) — free — second opinion / challenge my claims weekly
- [x] **Gemini** (gemini.google.com / aistudio.google.com) — free — cross-check + free API path
- [x] **Perplexity** (perplexity.ai) — free — factual checks, sources, current-version answers

Ground rules kept from `docs/intern-free-tooling-guide.md`: no private client data in
any third-party tool, no paid plans needed, and if a free limit blocks — switch tools,
do not add a payment method.

## Claude Project — setup, copy-paste

**1. Create the Project.** claude.ai → Projects → Create project.
**Name:** `FlyRank Portfolio Build`
(one Project, named for this build, it follows all eight weeks.)

**2. Custom instructions — paste this in** (project instructions field; genuine, not defaults —
the proof statement is the first line and Claude is cast as a tutor):

> You are my tutor for an 8-week job-search build. My proof statement — paste this
> into any page copy you review — is: "I take a fuzzy business question and end with
> an honest, useful model — on the FlyRank refresh lane that means a forest that ranks
> which pages to review first, measured P@50 0.86 on held-out clients, with the data
> contract, leakage audit, and claim rewrites committed in public every week."
>
> My portfolio's one person is the FlyRank mentor/lead; my one action is to get them
> to email me. Act as a writing and reasoning tutor: pressure-test my claims and my
> portfolio structure, keep my language honest (observed / measured / directional /
> decision-support — never causal, never a forecast), keep the portfolio small, and
> always answer with "what I'd change and why" rather than only praise.

**3. First real prompt — pressure-test the sitemap** (paste as the first message in the Project):

> Pressure-test my portfolio sitemap against my one action and my claim.
> Person: FlyRank mentor/lead. Action: they email me.
> Claim: "I take a fuzzy business question and end with an honest, useful model — on
> the FlyRank refresh lane, a forest that ranks which pages to review first, P@50 0.86
> on held-out clients, contracts/leakage audits/claim rewrites committed weekly."
> Sitemap: Home (hero: the claim + that number) → Work (the one case study,
> contract → baseline → model → validation → queue) → About (who I am, three
> sentences + method) → Contact (email CTA only).
> Tell me: (1) which page or text fights the claim or the action, (2) which page could
> be removed or merged, (3) the single highest-friction point between hero and email,
> and (4) one concrete thing I should change. Be the skeptical mentor, not the cheerleader.

**4. Save the answer.** Copy Claude's reply into a file named
`work/portfolio/claude-pressure-test-round1.md` (or paste it back into this repo in the
same folder). One saved answer per round.

## At least one thing I will change (filled after the pressure test)

- [ ] *(fill in the single concrete change Claude surfaced after the run — this is a
      graded pass condition.)*

## Deliverable checklist

- [ ] Photo of the sitemap sketch (the 4-page map above, redrawn by hand)
- [ ] Screenshot of the configured Claude Project (Custom Instructions visible, not defaults)
- [ ] Screenshot of the pressure-test prompt + Claude's output (saved to `work/portfolio/`)
- [ ] One "thing I'll change" noted above