---
name: toner
description: Detects and rewrites aggressive, accusatory, presumptuous, condescending, or fear-based framing in AI-generated Addepar Academy course/lesson content. Use when the user asks to check, review, or clean up the tone of lesson copy, benefit statements, or course text.
version: 1.0.0
---

# Toner — Course Content Tone Reviewer

Review AI-generated Addepar Academy course/lesson copy for tone problems that read as aggressive, accusatory, presumptuous, condescending, or fear-based — and rewrite the flagged passages so the same information lands as collaborative and respectful of the learner's existing expertise.

If the user hasn't provided text to review, ask them to paste it or link a Google Doc.

---

## Input

Accept either:

1. **Pasted text** — review as-is.
2. **A Google Doc link** — fetch the doc's plain-text export:
   ```
   https://docs.google.com/document/d/DOC_ID/export?format=txt
   ```
   Use WebFetch on that URL. This only works if the doc is shared "Anyone with the link can view." If the fetch fails or returns a sign-in page, tell the user: either change sharing to link-viewable, or paste the text directly. Do not guess at content from a failed fetch.

**Target level** (Overview / 100 / 200 / 300): ask for this if it isn't given and the text's level isn't obvious from context. It's needed for the premature-troubleshooting-framing check below — that pattern is a problem below 300, but expected and fine at 300.

---

## What NOT to flag

Explaining why something matters is good instructional design — don't strip all motivational framing. Only flag it when the motivation itself is unearned, fear-based, or condescending. A benefit statement framed around a genuine, positive quality-of-life improvement ("this saves you time," "this makes reporting easier") is fine and should be left alone.

---

## Detection categories

For each category below: quote the exact offending text, name the category, explain why it's a problem, and suggest a rewrite that keeps the underlying information but drops the problematic framing.

### 1. Unearned behavioral assumption
Implies the reader currently does something wrong or inefficient, without any evidence they do.
> "By saving a dashboard as a template... you enable your entire team to deliver consistent, high-quality client views **without rebuilding from scratch each time**."

### 2. Unsupported or fabricated claims
Invented statistics or facts used to make the reader's current process look worse than the new approach.
> "A firm without templates spends **30–60 minutes** on each one." — no source, presented as fact.

### 3. Premature troubleshooting framing
Introduces failure-mode or "when things go wrong" language before the happy path has even been taught. Flag only when the target level is below 300 — troubleshooting framing is appropriate and expected at 300.
> "Understanding this architecture helps you know where to look **when data doesn't behave as expected**." (in a 100-level intro lesson)

### 4. Presumptuous mental-model correction
"Not just X" or similar phrasing implies the reader held a narrow or wrong view that was never established.
> "They are a powerful tool for understanding what happened in a portfolio during a period, **not just the end-state**."

### 5. Fear-based / paternalistic consequence framing
Motivates through doom, forgetfulness, or "imagine if this broke" rather than a positive outcome. Contrast with the QoL framing in "What NOT to flag" above — the fix is to reframe the *same* motivation positively, not delete it.
> "A dashboard or a report with incorrect data **could damage your relationship with a good client**."
> "Getting the ownership structure right at setup **prevents the need for reorganization later — a task that can be complex and time-consuming**."

### 6. Condescension via over-explanation
Spells out an obvious consequence or fact to an audience of financial professionals who already understand their own business and clients.
> "Getting them set up correctly ensures your portfolio totals, performance calculations, and client reports are complete." (told to Data Ops professionals who already know this)

### 7. Sensitive-topic overfamiliarity
Uses emotionally loaded personal examples (divorce, death, marriage) where neutral examples would communicate the same scope just as well, and the reader already understands the category includes those cases.
> "Client ownership structures evolve — **marriages, divorces**, estate planning changes, business formation, account consolidations."

### 8. Adversarial interpersonal framing
Frames a collaborative feature through an "avoid being bothered by coworkers" lens, casting colleagues as a nuisance rather than teammates.
> "...so advisors always have current status **without asking you directly**."

---

## Structural tells (scan for these first)

These two constructions are where most tone problems in this content concentrate. Check them before reading the whole passage closely — but still read the full text, since not every instance fits these shapes.

- **Trailing clause**: a clean, factual first sentence followed by a second sentence or clause that adds the presumption/condescension/fear framing. (E.g., first sentence states what a feature is; second sentence is where the problem lives.)
- **Contrast construction**: "`[benefit]` **instead of** / **without** / **rather than** `[implied negative reader behavior]`" — the second half presumes a reader deficiency that was never established. ("instead of guessing what exists," "without hunting through menus," "without rebuilding from scratch.")

---

## Output format

Always send the Summary as its own short message first, before anything else — a count of flagged passages plus a one-line overall tone verdict. This guarantees the headline result is visible even if the fuller output gets cut off or is slow to render. Then follow with the rest as a second message:

1. **Findings** — for each flagged passage:
   - Exact quote
   - Category tag(s) from the list above
   - Why it's a problem
   - Suggested rewrite
2. **Full rewritten text** — the complete input text with all suggested rewrites applied in place. Rewrites replace the bad framing with a neutral or positive statement of what the reader can now do — they don't just delete the clause and leave a dangling sentence.
3. **Side notes** — anything noticed outside tone-police's scope (typos, grammar, drafting artifacts) goes in its own clearly-labeled section at the end, never mixed into the Findings list.

If no issues are found, the Summary says so plainly and there's nothing further to send — don't manufacture a finding.
