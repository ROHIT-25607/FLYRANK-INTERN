# Prompt Ladder — from "Explain this data" to a reusable refresh-queue prompt

**Track:** Machine Learning · **Context:** FlyRank refresh lane (the capstone)
**Method:** one weak baseline + five versions, each adding exactly ONE named layer.
Each version was run and excerpted below so changes to the **output** can be compared, not remembered.
Outputs are lightly condensed for reading length (headers/verbatim rows kept).

---

## V0 — Baseline (weak prompt)

> Explain this data.

**Excerpt of the output:**
> This looks like a spreadsheet of pages with columns for impressions, clicks, and average position,
> plus a date. Impressions means how many times your page was shown in search, clicks means how often
> people clicked it, and position is roughly where it ranks… You could use this to see which pages get
> the most attention, or maybe spot pages with high impressions but low clicks. If you want, I can make
> a chart or help you decide what to look at.

**Notes**
- **Changed in the prompt:** nothing — this is the baseline. One sentence, no goal, no audience, no shape.
- **Improved in the output:** nothing yet; it defined the columns I already knew and offered to "help" without doing anything.
- **Still failed:** no decision, no ranking, no honesty problem even identified; it hedged with "maybe/roughly."
- **Next:** give it a goal — the decision I actually wanted help with (what do I do with these pages).

---

## V1 — Layer added: clearer goal

> Explain this data to help me decide **which pages deserve a content-refresh review first**.

**Excerpt of the output:**
> Based on this table, the pages you should consider for a refresh review first are the ones with a
> combination of solid impressions and a low click-through rate — those pages are being shown but not
> earning clicks. For example, the row at position ~7 with 5,000+ March impressions and ~0.1% CTR would
> be a good first candidate. You could also prioritize high-impression pages whose position recently
> slipped.

**Notes**
- **Changed in the prompt:** added the goal (pick the pages to review first).
- **Improved in the output:** it stopped describing columns and started recommending candidates; it pointed at one concrete row instead of asking permission to help.
- **Still failed:** no audience, so it explained the obvious ("being shown but not earning clicks"); no method — the ranking is vibes, and the second suggestion ("position recently slipped") was invented, not in the data.
- **Next:** define the reader so the language matches what an editor actually thinks in.

---

## V2 — Layer added: defined audience

> Same goal, but write for a **FlyRank content editor** who lives in Google Search Console every day
> and has never trained a model.

**Excerpt of the output:**
> For the editor: the actionable signal is a page sitting in the page-one band that is under-earning
> clicks for its position, and a page whose recent traffic is decelerating. Those two patterns — not raw
> impression drops — are where the review time pays. I'd start there before touching deep pages.

**Notes**
- **Changed in the prompt:** added the audience and its constraints (knows Search Console, not ML).
- **Improved in the output:** dropped all "impressions means…" filler; switched to editor vocabulary ("page-one band", "under-earning clicks", "deep pages"); it now reads like a peer note, not a tutorial.
- **Still failed:** it had no real data context, so it guessed thresholds and confidence; "recent traffic is decelerating" was speculation, not something measureable from the columns it had been handed.
- **Next:** feed it the actual data contract so the claims are grounded, not plausible.

---

## V3 — Layer added: real context

> Add this context: one row per page; March 2026 features (impressions, clicks, avg position, last-7-day
> share of monthly impressions, days since last active); label is "declined" if April impressions
> < 0.8 × March impressions; the pool is 100,893 pages across 43 clients, base decline rate 0.515;
> validation is a seeded group-by-client split (30 train / 13 held-out); the model's measured held-out
> claim is precision@50 = 0.86, AUC 0.739. Now explain which pages deserve a refresh review first.

**Excerpt of the output:**
> The honest answer starts with the base rate: 51.5% of pages decline next month, so "decline" is not a
> rare event and a random review is an 51% coin-flip. The validated model measured on held-out clients
> reaches precision@50 0.86 and AUC 0.739, so ranking is the part that is actually supported. The top
> permutation-importance drivers were momentum_last7 and ctr. Feature importance for the forest across
> the 13 held-out clients: momentum_last7 +0.1354, ctr +0.0120, pos_march +0.0072. The pages to review
> first are therefore those with a strong position but failed recent momentum…

**Notes**
- **Changed in the prompt:** added the full data contract and the measured numbers.
- **Improved in the output:** it finally reasoned from the real base rate (0.515) and used the real measured claim (P@50 0.86) with correct caveats; it correctly distinguished "measured" from "support." This is the first version whose claims are true to the repo's receipts.
- **Still failed (the honest regression):** it became a **context dump** — the actionable ranked list was buried two paragraphs deep under repeated statistics; the output got longer and *less* usable than V2, exactly the failure this ladder exists to catch.
- **Next:** stop letting it choose the shape. Force a structure so the queue comes first.

---

## V4 — Layer added: specified output format

> Keep all context. Output exactly this shape, nothing else: a ranked table with at most 50 rows and
> columns **rank | action | reason code | one line of supporting evidence | what to check by hand**,
> then one short "Limits" paragraph, then one "Not claimed" sentence.

**Excerpt of the output:**
> | rank | action | reason code | evidence (March only) | hand-check |
> |---|---|---|---|---|
> | 1 | investigate_first | ctr_gap+momentum_loss | p1 band, CTR 0.09× band median, last-7 share 0.03 | open page; confirm meta/title; check changelog for recent refresh |
> | 2 | investigate_first | momentum_loss+inertia | last-7 share 0.05, no imp day in final week | confirm the quiet week is real, not tracking |
> …
> **Limits:** features end 2026-03-31; the label is April-only; ranking is decision-support, and P@50 0.86 was measured only on the 13 held-out clients.
> **Not claimed:** that refreshing these pages causes them to recover — that needs an experiment.

**Notes**
- **Changed in the prompt:** added a mandated output format (table + Limits + Not-claimed).
- **Improved in the output:** the queue came first; each row carried a falsifiable evidence string and a human check; the honesty framing survived in two tight paragraphs. This is the version that starts to look like the product the repo actually shipped.
- **Still failed:** no constraints on the rows themselves — some evidence lines were vague ("CTR seems low" without the band comparison), and the hand-check column was skipped on several rows.
- **Next:** constrain the content per row and give one worked example of "what good looks like."

---

## V5 — Layer added: constraints

> Keep everything. Add these constraints: evidence must be a concrete number from a March feature (a band,
> a ratio to the band median, or a momentum share) — never vague words; if a row can't back its reason
> with a number, delete the row; no client names, domains, or URLs anywhere; max 50 words per row;
> before you answer, re-read your own table and delete any row whose evidence could have come from the
> April label window.

**Excerpt of the output:**
> | rank | action | reason code | evidence (March only) | hand-check |
> |---|---|---|---|---|
> | 1 | investigate_first | ctr_gap+momentum_loss | p1 band; CTR 0.09× band median; last-7 share 0.03 | open page; confirm meta/title; check changelog |
> | 2 | investigate_first | momentum_loss+volume_exposure | last-7 share 0.05; 12.4k March imps at stake | confirm quiet week is real; size the exposure |
> | 3 | investigate_first | ctr_gap | p1 band; CTR 0.22× band median; 21 active days | compare to band; confirm the CTR story with clicks>0 |
> …
> **Limits:** decision-support; features end 2026-03-31; label is April-only; measured only on held-out clients.
> **Not claimed:** refreshing causes recovery.

**Notes**
- **Changed in the prompt:** added hard constraints (numeric evidence or drop the row, no identifiers, word cap, label-window self-check).
- **Improved in the output:** every retained row now has a number that can be checked against a March column; the vague-CTR row from V4 got deleted (exactly what the constraint asked); the requested post-answer self-review ("did any evidence smuggle in the label?") found nothing and said so.
- **Still failed:** it can't decide *how many* pages per action — the banding rule (top 1% investigate_first, etc.) is a judgment the prompt's author still has to supply; and the prompt is now long.
- **Next:** fold in the action-banding rule + a worked example row, then clean the whole thing into the reusable prompt below.

---

## Side-by-side (what earned its place)

| Layer added | Did the OUTPUT improve? | Verdict |
|---|---|---|
| V1 clearer goal | stopped explaining columns; recommended candidates | helped |
| V2 audience | killed the tutorial filler; editor vocabulary | helped |
| V3 real context | true base-rate + measured-claim reasoning, but **context dump** | helped AND hurt (longer, queue buried) |
| V4 output format | queue-first, falsifiable rows, tight honesty | helped most |
| V5 constraints | deleted weak rows; every reason now numeric | helped |

One note for the discipline: V3 is the version that would be easy to call "an improvement" if you only
looked at the prompt. Side-by-side with V2, the output regressed. That is the exact case the ladder exists for.

---

## Final reusable prompt

> You are helping me build a content-refresh review queue for the FlyRank refresh lane. Data: one row per
> page; March 2026 features (impressions, clicks, CTR, avg position, last-7-day share of March
> impressions, days since last active day, position band). Label for any internal check: April
> impressions < 0.8 × March = "declined" (decision-support only — never output, never use as a reason).
> Pool: 100,893 pages / 43 clients, base decline 0.515; the only measured claim is the model's held-out
> P@50 0.86, AUC 0.739 (13 clients, seeded grouped split). Audience: a content editor who lives in
> Search Console and doesn't need anything explained.
>
> Act as the editor's analyst. Produce exactly:
> 1. **Ranked table**, max 50 rows, columns: `rank | action | reason code | evidence (March only) |
>    hand-check`. Action band: top 1% investigate_first, next 1.5% watch_prepare, next 7.5% monitor,
>    rest no_action. Reason codes are feature flags, up to two, priority order:
>    `ctr_gap` (top3/p1 band, CTR ≤ half the band median) · `momentum_loss` (last-7 share ≤ 0.15) ·
>    `inertia` (no impression day in final week) · `volume_exposure` (≥ 10k March impressions).
> 2. **Limits** (2–4 sentences): decision-support; features end 2026-03-31; label is April-only;
>    measured numbers are held-out only.
> 3. **Not-claimed** (one sentence): no causal claim that refreshing pages recovers traffic.
>
> Constraints: every evidence cell is a concrete number from a March feature (band, ratio to band median,
> or a momentum share) — if a row can't produce one, delete the row; no client names, domains, URLs, or
> raw queries; ≤ 50 words per row; before answering, re-read the table and delete any row whose evidence
> could have come from the April label window, then say "checked". Example of a good row:
> `1 | investigate_first | ctr_gap+momentum_loss | p1; CTR 0.09× band median; last-7 share 0.03 |
> confirm meta/title; check changelog for recent refresh`.

---

## What I learned (the one-layer discipline)

The biggest output jump came from **format** (V4), not from feeding more **context** (V3) — context raised
the truthfulness but lowered the usability until the format tamed it. If I ran this again I'd put the
audience and format in earlier and spend V3/V4 instead on a worked example plus constraints, because "here's
one row done right" did more for the output than "the full data contract" did. The layer that genuinely did
not earn its place in this ladder was none of them individually — it was the order: real context before
format made the middle version worse, and that regression is the evidence this assignment was after.